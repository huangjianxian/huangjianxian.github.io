---
title: Java基础 | 多线程
date: 2021-02-23 22:18:27
postlink: java-base-thread
categories: Java
tags: Java基础
---
## 一、概述

本文主要介绍Java多线程的基础知识、如何创建新线程、线程状态、中断线程、同步以及常用的锁。

## 二、什么是多线程？

多线程是Java并发的基础，在说明多线程之前，我们务必需要先理清程序、进程和线程的概念。

### 2.1 三者概念区别

1、程序：程序是存储在磁盘中的文件，可以运行起来。

2、进程：进程是程序的一次执行过程，是动态的。比如下图为当前Windows系统正在执行的进程。

![image-20210223182706127](https://file.hjxlog.com//blog/images/20210223182710.png)
<!--more-->
3、线程：线程是比进程更小的单位，是操作系统调度的最小单位。

![image-20210223182810331](https://file.hjxlog.com//blog/images/20210223182814.png)

这里可以看到我们当前有3157个线程。

举个例子：我们现在打开IDEA编写Java程序，正在执行的IDEA就是一个进程，而我们在编写Java的同时，会有输入，拼写检查和编译等等，这些就是交给线程来执行的。

### 2.2 为什么需要线程？

因为CPU的指令是需要一条一条来执行的，为了让系统看起来像是可以同时执行多任务，我们通常需要用时间片切换的方式来给各个线程执行，每个线程占用一个很小的时间片，用完之后就给下一个线程用。我们可以写程序新建一个线程，但是如何调度，这个我们自己写的程序管不了，是由操作系统去进行调度的。

相比进程而言，创建一个线程和在切换的时候开销更小，线程之间的通信更快，但不如进程稳定。

在应用场景方面，有时候我们需要同步，比如听歌看歌词滚动，听歌是一个线程，看歌词是另外一个线程，这两个线程需要协调运行。

## 三、创建新线程的两种方法

### 3.1 新建类实现Thread，重写run()方法

我们实例化一个Thread实例，调用它的start()方法，即可启动一个线程。

```java
Thread thread = new Thread();
thread.start();
```

查看源码，我们会发现Thread是实现了Runnable接口的。

```java
public class Thread implements Runnable {}
```

上面的例子，我们啥也没做，只是实例化了一个线程，一般我们可以新建一个线程类继承Thread，然后重写它的run()方法，来实现我们自己的功能。

我们新建一个HelloThread类，继承于Thread，并且重写run()方法，打印一句话。

```java
public class HelloThread extends Thread{
    @Override
    public void run() {
        System.out.println("Hello World!");
    }
}
```

这时候我们在Main()方法里面实例化，调用它的start()方法。

```java
HelloThread helloThread = new HelloThread();
helloThread.start();
```

结果输出：Hello World!

**注意：**这里不能直接调用run()方法，这样的话就相当于直接调用方法而已，不会创建新线程。

### 3.2 创建Thread实例，然后传入Runnable实例

新建一个类，实现Runnable接口，重写run()方法。

```java
class HelloRunnable implements Runnable{
    @Override
    public void run() {
        System.out.println("This is HelloRunnable!");
    }
}
```

然后实例化一个HelloRunnable对象，再实例化一个Thread对象，把HelloRunnable对象作为参数传到Thread对象的构造方法里去。

```java
    public static void main(String[] args) {
        HelloRunnable runnable = new HelloRunnable();
        Thread thread = new Thread(runnable);
        thread.start();
    }
```

### 3.3 设置线程优先级

线程是可以设置优先级的，设置比较高的优先级，这样可能操作系统会优先调度我们这个线程，但是这个并不是绝对的。设置优先级代码如下：

```java
thread.setPriority(7);
```

通过查看源码可知，可以设置1-10级，默认为5级。

```java
public final void setPriority(int newPriority) {
    ThreadGroup g;
    checkAccess();
    if (newPriority > MAX_PRIORITY || newPriority < MIN_PRIORITY) {
        throw new IllegalArgumentException();
    }
    if((g = getThreadGroup()) != null) {
        if (newPriority > g.getMaxPriority()) {
            newPriority = g.getMaxPriority();
        }
        setPriority0(priority = newPriority);
    }
}
```

可以看出来，如果传入的参数newPriority不在MIN_PRIORITY和MAX_PRIORITY之间，就会报IllegalArgumentException异常。它们的默认值如下：

```java
 /**
  * The minimum priority that a thread can have.
  */
 public final static int MIN_PRIORITY = 1;

/**
  * The default priority that is assigned to a thread.
  */
 public final static int NORM_PRIORITY = 5;

 /**
  * The maximum priority that a thread can have.
  */
 public final static int MAX_PRIORITY = 10;
```

## 四、线程的状态

首先，我们先简要说明一下代码中的线程状态以及变化。在new一个线程对象的时候，我们就新建了一个线程，但是还没启动。在我们调用start()方法后，线程启动，但是线程可能由于其他原因，比如IO阻塞导致挂起，或因为某些操作在等待中，或调用了sleep()休眠，最后在run()方法执行完毕之后，线程就会终止。

> 1、 New

创建线程的时候，线程的状态是New，还没有执行。

> 2、Runnable

这是正在运行中，也就是调用了run()方法，正在执行过程中。

> 3、Blocked

因为某些操作被阻塞挂起。

> 4、Waiting

因为某些操作正在等待中。

> 5、Timed Waiting

运行中的线程，因为调用了sleep()主动休眠。

> 6、Terminated

线程已经执行完了run()方法，线程结束。



一个线程A还可以通过调用另一个线程B的join()方法来等B线程结束后，再继续执行A线程。比如下面这段代码，我们在执行main方法的时候，就是启动了一个主线程Main，然后执行第一个输出"Main start!"，之后新建一个线程thread，重写了run方法，并且休眠了3秒。之后启动线程thread，再调用thread.join();，让thread先执行完，再接着执行主线程。

```java
public static void main(String[] args) throws InterruptedException {
    System.out.println("Main start!");
    Thread thread = new Thread(new Runnable() {
        @Override
        public void run() {
            System.out.println("Thread start!");
            try {
                Thread.sleep(3000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println("Thread end!");
        }
    });
    thread.start();
    thread.join();
    System.out.println("Main end!");
}
```

运行这段代码，可以观察到先输出了Main start!和Thread start!，过了3秒后再输出Thread end!和Main end!

![image-20210223192740180](https://file.hjxlog.com//blog/images/20210223192740.png)

## 五、如何中断线程？

上面只是大概介绍了线程，在很多应用场景中，我们需要中断一个需要执行很长时间的线程。比如通过QQ传输一份大文件给好友，发现网速很慢，要传完需要花费很多时间，于是决定取消传输，通过别的方式把文件给好友。

这里面的“取消传输”，就是我们主动地中断线程。那么应该怎么中断呢？

### 5.1 调用thread.interrupt();中断

我们可以在其他线程中对目标线程调用thread.interrupt();方法来中断它。

```java
public class Test {
    public static void main(String[] args) throws InterruptedException {
        System.out.println("Main start!");
        MyThread thread = new MyThread();
        thread.start();
        thread.interrupt();
        System.out.println("Main end!");
    }
}
class MyThread extends Thread{
    @Override
    public void run() {
        System.out.println("Thread start!");
        int i=0;
        while(i<1000)
            System.out.println(i++);
        System.out.println("Thread end!");
    }
}
```

我们运行上面这段程序，结果如下：

```java
Main start!
Main end!
Thread start!
0
1
...输出到999
Thread end!
```

可以看出，主线程先执行完毕了，再开始执行thread线程，并输出到了999，这时候我们会发现thread.interrupt();并没有起到中断的作用。这是怎么回事呢？

因为这只是主线程向thread线程发送了中断请求而已，而我们的MyThread类并没有处理这个中断请求，我们需要在线程中加上处理中断请求的代码才可以生效。

我们修改代码如下：

```java
public class Test {
    public static void main(String[] args) throws InterruptedException {
        System.out.println("Main start!");
        MyThread thread = new MyThread();
        thread.start();
        Thread.sleep(10); // 休眠10毫秒，让线程可以打印出一点数字
        thread.interrupt(); // 请求中断
        System.out.println("Main end!");
    }
}
class MyThread extends Thread{
    @Override
    public void run() {
        System.out.println("Thread start!");
        int i=0;
        while(i<1000 && !isInterrupted()) // 接收到中断请求后退出
            System.out.println(i++);
        System.out.println("Thread end!");
    }
}
```

输出结果，打印了一部分的数字，这是因为thread线程开始执行了，而主线程调用了sleep()休眠了10毫秒后才发出中断请求，这时候thread线程已经跑了好一会了。

当join()方法遇到中断请求时，会立刻抛出InterruptedException异常。比如我们在MyThread线程类的run()方法中调用sleep()，这时候会强制捕获异常。

```java
class MyThread extends Thread{
    @Override
    public void run() {
        System.out.println("Thread start!");
        try {
            sleep(1000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("Thread end!");
    }
}
```

我们在Main主线程中发送了一个线程的中断请求，及时它当前处于join()或者休眠期，都必须中断返回。

### 5.2 设置标志位来中断线程

除了上面主动发送中断请求的方法来中断之外，还可以通过设置标志位来中断线程。

我们通常可以在线程类中设置一个公共字段，并加上volatile关键字，来判断线程当前是否需要中断，如以下代码：

```java
public class Test {
    public static void main(String[] args) throws InterruptedException {
        System.out.println("Main start!");
        MyThread thread = new MyThread();
        thread.start();
        Thread.sleep(10);
        thread.keepRunning = false;
        System.out.println("Main end!");
    }
}
class MyThread extends Thread{
    public volatile boolean keepRunning = true;
    @Override
    public void run() {
        System.out.println("Thread start!");
        int n = 0;
        while(keepRunning){
            n++;
            System.out.println(n);
        }
        System.out.println("Thread end!");
    }
}
```

其中，用volatile关键字标记的keepRunning变量，代表着它是线程间共享的。volatile关键字是告诉虚拟机，每次访问变量的时候，都要获取主存中的最新值，每次修改变量值的时候，要第一时间写回主存。

因为在Java内存模型中，当线程访问变量的时候，会拷贝一份副本回到自己的工作内存区，修改的时候也是修改的自己工作区的值，然后再写回主存。这个时候就有问题了，比如线程A读取了变量keepRunning=true;并修改为了false，但是还没来得及写回到主存中，这时候线程B读取keepRunning的值还是true。

volatile关键字就是用来解决可见性问题的，可见性问题就是说，如果一个线程修改了某个变量，那么其他线程可以立马看到。

## 六、线程同步以及同步方法

有时候我们需要用到线程同步机制，比如说，现在设计一个计数器，对于同一个变量，有一个线程A先从0开始，加一万次1，然后再有一个线程B，减1减一万次。

```java
public class Test {
    public static void main(String[] args) throws InterruptedException {
        System.out.println("Main start!");
        AddThread addThread = new AddThread();
        DecThread decThread = new DecThread();
        addThread.start();
        decThread.start();
        addThread.join();
        decThread.join();
        System.out.println(Counter.count);
        System.out.println("Main end!");
    }
}
class Counter{
    public static int count = 0;
}

class AddThread extends  Thread{
    @Override
    public void run() {
        for(int i=0;i<10000;i++){
            Counter.count +=1;
        }
    }
}

class DecThread extends Thread{
    @Override
    public void run() {
        for(int i=0;i<10000;i++){
            Counter.count -=1;
        }
    }
}
```

理论上是得到0才对，但是运行这段代码，发现每次的结果都不一样。

这是因为没有加锁，导致在加/减的过程中，有一些计算结果丢失了。打个比方，现在有n=10;然后A，B线程同时执行n=n+1;这时候应该等于12才对，但事实上，有可能线程A的n=n+1还没执行完，线程B就开始了，而此时线程B获取到的n的值也是10而不是线程A执行完的结果11。

我们加上同步锁之后，发现这时候的结果都是0了；

```java
class Counter{
    public static int count = 0;
    public static final Object lock = new Object();  // 锁
}

class AddThread extends Thread{
    @Override
    public void run() {
        for(int i=0;i<10000;i++){
            synchronized (Counter.lock){ // 加上同步锁
                Counter.count +=1;
            } // 释放锁
        }
    }
}

class DecThread extends Thread{
    @Override
    public void run() {
        for(int i=0;i<10000;i++){
            synchronized (Counter.lock){
                Counter.count -=1;
            }
        }
    }
}
```

两个不同的线程，如果需要同步操作，加锁的时候必须是同一个锁。

    synchronized (lock){ // 加上同步锁
    	// 这里写临界区代码
    } // 释放锁

## 七、死锁

死锁就是两个线程持有各自的锁，然后试图获取对方手里的锁，导致双方无限制的等待下去，而且没有任何机制能够解除死锁，除非强制结束JVM进程。

举个例子：

```java
public void doA(){
    synchronized (lockA){
        // do sth.
        synchronized (lockB){
            // do sth.
        }
    }
}
public void doB(){
    synchronized (lockB){
        // do sth.
        synchronized (lockA){
            // do sth.
        }
    }
}
```

线程1执行doA()方法，然后获取了lockA。同时线程2执行了doB()，获取了lockB。

接着线程1请求lockB，因为只有获取lockB，然后接着执行lockB下面的临界区代码，执行完之后才会释放lockB，最后再释放lockA。但是此时线程1获取不到lockB，因为它已经被线程2获取了，而同理，线程2因为没法获得lockA而没办法释放lockB。因此双方会一直等待下去...

平时写代码的时候，要特别注意获取锁的顺序，避免死锁。

## 八、多线程协调运行

### 8.1 wait()

在一个获得锁的线程中，如果此时因为没有资源或者等待某些资源到来，这时候为了避免因为占用锁而让其他线程不能够继续执行。可以调用this.wait();来释放锁，进入等待状态，等到资源到来之后，再重新获取this锁。

### 8.2 notify()和notifyAll()

上面我们调用this.wait();方法来释放锁，但是这时候要怎么通知别的线程这个锁已经释放了呢？答案就是在synchronized中调用notifyAll()，这是通知所有正在等待this锁的线程。

## 九、几种并发处理机制

### 9.1 ReentrantLock

ReentrantLock是比synchronized更加安全的获取锁的方式。它保证了只有一个线程可以执行临界区的代码。

它的流程是：先必须要获取锁，然后进入try{...}，最后在finally{}释放锁。

ReentrantLock可以用Condition对象来实现wait和notify功能。

```java
private final Lock lock = new ReentrantLock();
Condition condition = lock.newCondition(); // 必须从lock对象获取
condition.await();
condition.notify();
condition.notifyAll();
```

### 9.2 ReadWriteLock

ReadWriteLock只允许一个线程写入，当没有写入的时候，可以允许多个线程同时读，适合多读少写的场景。

但是这种是一个悲观锁，就是读的过程中不允许写。

### 9.3 StampedLock

StampedLock是一种乐观锁，也就是在读的过程中也允许写，如果读的数据不对了，再通过额外的代码判断是否有写入。

### 9.4 Java提供的并发集合

使用Java提供的并发集合，可以简化我们的多线程编程。如java.util.concurrent包下面的ConcurrentHashMap。

## 十、线程池

由于频繁创建、销毁线程会消耗大量系统资源和时间，这时候我们可以复用一组线程，来接受一些小任务并分发处理。

线程池维护了若干个线程，在没有任务的时候空闲着，如果有新任务到来，就分配一个处于空闲状态的线程去处理。

Java提供了ExecutorService实现了线程池，需要调用shutdown()关闭。