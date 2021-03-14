---
title: Java框架 => 手写我的第一个IOC容器
date: 2021-02-25 23:17:41
postlink: java-my-first-ioc-container
categories: Java
tags: Java框架
---
## 一、概述

说到Java的spring框架，绕不来的就是它的两个特性：IOC和AOP。

为了更好的理解spring框架中的IOC，今天决定通过手写一个简单的IOC容器，来梳理它的工作原理，已算是为自己加强技术专研开一个好头。
<!--more-->
## 二、IOC原理

IOC是控制反转的英文缩写，控制反转，从字面上来看，我们要控制的其实是JavaBean的生命周期，而反转的意思是这个JavaBean的生命周期控制权不再有使用它的对象来持有，而是交出去给IOC容器。

IOC容器就像一个代理中介一样，举个例子来说，加入说我今天要拥有一辆车，那么我不需要自己去创建一辆车，而是直接去到4s店购买，但是4s点也不会制造车，4s店会去联系制造车的厂商，由他们来生产，然后再卖给我们。

简单来讲，就是我们想要拥有一辆车，但是不需要自己new一辆，而是由别人给我们提供，这里的“别人”也就是今天所讲的IOC容器了。

## 三、在没有使用IOC思想之前

举个例子，有一个人想开一辆车，如果按照对象需要什么就new一个什么的思想。我们先定义一个Car的接口，有开启，左转，右转，停车的方法。

```java
public interface Car {

    public void start();

    public void turnLeft();

    public void turnRight();

    public void stop();
}
```

然后定义两个类（奥迪、别克）来实现这个接口。

```java
public class Audi implements Car{
    public void start(){
        System.out.println("Audi start");
    }

    public void turnLeft(){
        System.out.println("Audi turnLeft");
    }

    public void turnRight(){
        System.out.println("Audi turnRight");
    }

    public void stop(){
        System.out.println("Audi stop");
    }
}

public class Buick implements Car{
    public void start(){
        System.out.println("Buick start");
    }

    public void turnLeft(){
        System.out.println("Buick turnLeft");
    }

    public void turnRight(){
        System.out.println("Buick turnRight");
    }

    public void stop(){
        System.out.println("Buick stop");
    }
}
```

然后我们定义一个Person类，他需要用车，于是new一辆奥迪车给他。

```java
public class Person {
    private Car car = new Audi(); // 创建一个奥迪车
    public void driveCar(){
        // 使用奥迪车
        car.start();
        car.turnLeft();
        car.turnRight();
        car.stop();
    }
}
```

如果这个人突然想换成别克车，这时候需要把`new Audi();`改成`new Buick();`这时候看起来比较简单，好像无所谓。但是如果有好多个车的品牌，难不成每次都要改吗？还有，这个Person类不仅会有开车的方法，还有吃饭，穿衣服，吃什么饭，穿什么品牌的衣服，如果每次都需要Person类去实例化具体的类的话，显然代码量太多了，而且依赖性太强，比如Car类有一天取消了`turnRight();`方法，那么所有用到这个方法的地方都要修改，这就是我们说的耦合度太高了。

因此，针对这个问题，IOC容器就出现了，IOC容器主要负责创建JavaBean以及负责它的生命周期，换成上面那个例子，IOC主要负责给Person类放入一辆车供他使用，至于是什么车，Person类不用管，后期想改的话，只要通知IOC就可以了。让Person类专注于自己的业务逻辑。

对于依赖注入来说，Person需要开车的话，就必须依赖于Car，但是Person类不需要自己去实例化new一个Car出来，针对于它依赖的Car，可以通过依赖注入的方式，比如通过Setter方法注入，或者通过构造方法参数的形式注入一个Car，而不需要创建一个实实在在的Audi车或者Buick车，只需要通过IOC注入即可。

小结一下：IOC主要是为了解决对象和对象之间繁琐的依赖关系，解耦合。

## 四、手写一个简单的IOC容器

好的，说了这么多（后面还需要整理，现在先这样子吧，赶时间）。我们的场景是程序员用不同电脑进行编码，本来程序员和电脑是依赖和被依赖关系（程序员依赖电脑去写程序）。这里我们将电脑类交给IOC容器处理，也就是说后面需要还电脑型号什么的，都不需要改动什么代码。

> 前期准备

我们新建一个Maven工程，里面增加junit组件用来测试就可以了。然后定义也给接口Human，程序员Programmer类实现Human接口（很明显，程序员也是人），新建一个小黄类和小林类来继承Programmer类。新建一个电脑Computer接口，新建一个Mac类和Surface类实现电脑接口。

> 在pom.xml中添加junit依赖

```xml
<dependencies>
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>RELEASE</version>
        <scope>test</scope>
    </dependency>
</dependencies>
```

> 包结构

![image-20210225231333708](https://file.hjxlog.com//blog/images/20210225231333.png)

> 接口

```java
/**
 * Hunam接口
 */
public interface Human {
    public void Work(); // 工作
}

/**
 * 电脑接口
 */
public interface Computer {
    public void Coding(); // 编码动作
}
```

> 类

```java
public class Mac implements Computer {
    public void Coding() {
        System.out.println("大家好！我使用Mac电脑编程！");
    }
}

/**
 * 程序员类
 */
public abstract class Programmer implements Human {
    protected Computer computer; // 程序员工作，需要依赖一台电脑

    public Programmer(Computer computer) { // 通过构造方法注入
        this.computer = computer;
    }

    public abstract void Work(); // 因为每个程序员用的编程可能不一样，修改成抽象方法，供子类去重写
}

public class Surface implements Computer {
    public void Coding() {
        System.out.println("大家好！我使用微软Surface编程！");
    }
}

public class XiaoHuang extends Programmer{

    public XiaoHuang(Computer computer) {
        super(computer);
    }

    public void Work() {
        computer.Coding();
    }

}

public class XiaoLin extends Programmer{

    public XiaoLin(Computer computer) {
        super(computer);
    }

    public void Work() {
        computer.Coding();
    }
}
```

> IOC容器

```java
/**
 * 需要做的事情：
 * 1、实例化bean
 * 2、保存bean
 * 3、提供给bean
 * 4、每个bean要产生一个唯一的id与之相对应
 */
public class IOCContainer {

    // 创建一个Map对象来存储，保证一个bean和唯一的Id相对应
    // 这里的bean就是Java对象，id就是String
    // 使用ConcurrentHashMap的原因是为了保证线程安全
    private Map<String,Object> beansMap = new ConcurrentHashMap<String,Object>();

    /**
     *根据beanId返回JavaBean
     * @param beanId
     * @return
     */
    public Object getBean(String beanId){
        return beansMap.get(beanId);
    }

    /**
     * 委托IOC创建一个JavaBean
     * 通过反射获取这个class，然后循环调用它的构造方法，
     * 找到适合的构造方法创建一个JavaBean，并存放到beansMap中
     * String...代表可变参数，就是个数不定
     * @param clazz
     * @param beanId
     * @param paramBeanIds
     */
    public void setBean(Class<?> clazz,String beanId,String... paramBeanIds){

        // 1、组装构造方法需要的参数值
        Object[] paramValues = new Object[paramBeanIds.length]; // 构造一个与参数列表长度相等的数组
        for (int i = 0; i < paramBeanIds.length; i++) {
            // 按照约定，被依赖的对象是需要先创建的，所以这里是没有问题的
            paramValues[i] = beansMap.get(paramBeanIds[i]);
        }

        // 2、调用构造方法实例化bean
        Object bean = null;
        for (Constructor<?> constructor : clazz.getConstructors()) {
            try {
                bean = constructor.newInstance(paramValues); //传构造方法参数
            } catch (InstantiationException e) { // 不处理这些报错，因为构造方法可能不止一个，报错很正常
            } catch (IllegalAccessException e) {
            } catch (InvocationTargetException e) {
            }
        }
        if(bean == null) // 还是没找到
            throw new RuntimeException("找不到合适的构造方法去实例化bean");

        // 3、将创建好的Javabean放入beansMap中
        beansMap.put(beanId,bean);
    }
}
```

> 测试类

```java
public class ClassTest {

    private IOCContainer iocContainer = new IOCContainer();

    @Before
    public void before(){ // 统一管理bean的创建

        // 被依赖对象需要先创建

        iocContainer.setBean(Mac.class,"mac"); // ioc容器创建Mac对象，beanId是mac，这个是需要唯一的
        iocContainer.setBean(Surface.class,"surface"); // 不需要有参构造方法，所以只有两个参数

        // 创建XiaoHuang的Javabean，因为他依赖一个Computer，构造方法传入上面新建好的JavaBean的beanId “mac”
        iocContainer.setBean(XiaoHuang.class,"xiaohuang","mac");
        iocContainer.setBean(XiaoLin.class,"xiaolin","surface");
    }

    @Test
    public void test(){
        Human xiaohuang = (Human) iocContainer.getBean("xiaohuang");
        xiaohuang.Work();

        Human xiaolin = (Human) iocContainer.getBean("xiaolin");
        xiaolin.Work();
    }
}
```

> 输出结果

```java
大家好！我使用Mac电脑编程！
大家好！我使用微软Surface编程！
```

至此，大功告成！