---
title: Java三大特性（继承、封装、多态）
date: 2021-02-21 17:56:14
postlink: java-three-major-character
categories: Java
tags: Java基础
---
### 一、概述

本文简单描述Java的三大特性，继承、封装、多态，使用例子说明它们的原理。

### 二、封装

封装就是把类的属性隐藏起来，不允许外面直接访问类的内部属性，而是提供可以访问的方法供外界调用。举例子，就像我们笔记本，把内部的实现细节，CPU，主板那些包装好，只提供键盘、USB接口、耳机口给我们用。
<!--more-->
比如我们现在新建一个Person类。

```java
public class Person {
    public String name;
    public String sex;

    public Person(String name, String sex) {
        this.name = name;
        this.sex = sex;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", sex='" + sex + '\'' +
                '}';
    }
}
```

现在，我们编写一段测试代码，new一个Person类，然后发现可以直接通过 “实例.属性” 给属性重新赋值，而且属性值被篡改后已经失去了原来的意义。

```java
public static void main(String[] args) {
    Person person = new Person("小明","男");
    System.out.println(person.toString());
    person.sex = "嘿嘿嘿"; // 性别被随意的修改了，显然不符合我们的期望
    System.out.println(person.toString());
}
```

输出结果如下：

```java
Person{name='小明', sex='男'}
Person{name='小明', sex='嘿嘿嘿'}
```

为了防止这种情况的出现，封装的概念就很重要了，我们把属性私有化，只对外提供一些方法来操作这些属性。

比如上面这个例子，我们通过setter/getter来对属性进行赋值，用户想要修改实例的属性值，那么需要通过我们的setter方法来实现，这个时候，我们就可以在setter中做一些卡控，不允许用户设置除了“男”和“女”之外的属性值。

我们重新修改一下Person类。

```java
public class Person {
    private String name; // 将public修改为private
    private String sex;

    // 无参构造方法
    public Person(){

    }

    public Person(String name, String sex) {
        this.name = name;
        this.sex = sex;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getSex() {
        return sex;
    }

    public void setSex(String sex) throws Exception {
        try{
            if (!(sex.equals("男")||sex.equals("女"))) // 做一些卡控
                throw new Exception("性别只允许输入“男”或“女”");
            this.sex = sex;
        }catch (Exception e){
            throw e;
        }
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", sex='" + sex + '\'' +
                '}';
    }
}
```

我们在setSex()方法中做了一些卡控，只允许输入“男”或“女”，其他的会抛出异常。这就体现了封装的特性，外部无需关系我们内部的实现细节，而且我们可以把控自己的属性，不会被随意篡改。

运行下面这段代码，就会抛出异常[Exception in thread "main" java.lang.Exception: 性别只允许输入“男”或“女”]了。

```java
public static void main(String[] args) throws Exception {
    Person person = new Person("小明","男");
    System.out.println(person.toString());
    // person.sex = "嘿嘿嘿"; // 性别被随意的修改了，显然不符合我们的期望
    person.setSex("嘿嘿嘿"); // 上面的person.sex = "嘿嘿嘿";已经不能被访问了
    System.out.println(person.toString());
}
```

### 三、继承

我们定义一个学生类，一共有三个属性，名字、性别、电话号码，新建如下：

```java
public class Student {
    private String name;
    private String sex;
    private String phoneNum; // 电话号码
    // 省略setter/getter
}
```

我们会发现，学生类和Person类有很多相似的地方，其中name，sex属性是一致的。我们同样也需要对学生性别做一些卡控，这时候我们可以选择和Person类一样，也修改一下setSex()。但是这也太麻烦了，如果系统庞大的时候，要修改很多地方，也要写很多重复代码。

这时候我们就可以用到继承了，继承就是子类继承于父类，这时候子类就拥有了父类的所有属性和方法（虽然不一定能使用，但是理论上全都拥有了）。

我们修改Student类继承于Person。

```java
public class Student extends Person{
    private String phoneNum; // 电话号码
    // 省略setter getter
}
```

这时候，我们新建一个学生实例，赋值性别，发现执行的正是父类的方法，也意味着我们不需要重新写一遍卡控代码了。

```
    public static void main(String[] args) throws Exception {
        Student student = new Student();
        student.setSex("嘿嘿嘿");
    }
```

运行这段代码，同样会抛出异常[Exception in thread "main" java.lang.Exception: 性别只允许输入“男”或“女”]。

其中，子类也是不能够访问父类的private字段的，但是如果改成protected，子类是可以访问的。也就是说private的范围是当前类，protected的范围是继承树。

### 四、多态

多态是同一行为，对象不同，就具有不同的表现形态。比如我们现在定义一个动物类，有发出声音的方法。有猫和狗都继承于动物类，但是同样是发出声音，猫和狗是不一样的。

```java
public class Animal {
    public void voice(){
        System.out.println("动物发出声音");
    }
}

public class Cat extends Animal{
    @Override
    public void voice() {
        System.out.println("猫发出声音，喵喵喵");
    }
}

public class Dog extends Animal{
    @Override
    public void voice() {
        System.out.println("狗发出声音，汪汪汪");
    }
}
```

这时候，我们编写一些测试代码，如下：

```java
    public static void main(String[] args) throws Exception {
        Animal animal = new Dog();
        animal.voice();
    }
```

这里，我们声明的引用类型是Animal类，但是实际类型是Dog。这时候调用animal.voice();应该是直接打印父类的voice()还是子类Dog的voice()呢？

我们执行程序，发现是调用的子类的voice()。因此，Java实例方法的调用实际上是调用的运行时的实际类型的方法，而不是我们声明的方法。

这样看起来还不是那么明显，如果我们写一个方法，传入的参数是Animal类，内部执行voice();方法。

```java
public static void ShowVoice(Animal animal){
    animal.voice();
}
```

这时候就体现多态的作用了，这个方法可以根据不同的具体子类实例，来调用不同的方法，也就是同一个发出叫声的方法，但是有多种不同的形态实现。

编写测试代码如下：

```java
    public static void main(String[] args) throws Exception {
        Animal dog = new Dog();
        ShowVoice(dog);
        Animal cat = new Cat();
        ShowVoice(cat);
    }
```

执行结果：

```java
狗发出声音，汪汪汪
猫发出声音，喵喵喵
```

### 五、总结

封装：禁止外部直接修改属性值，而是通过我们对外提供的方法来操作属性，增加安全性。

继承：以父类为模板，继承父类的所有属性和方法，可以做一些拓展，减少冗余代码，易于维护。

多态：针对声明类的同一个行为，不同的实现类有不同的行为结果。