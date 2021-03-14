---
title: Java基础 | 反射
date: 2021-02-22 21:48:20
postlink: Java-based-reflection
categories: Java
tags: Java基础
---
### 一、概述

本文主要介绍Java基础部分的反射，它的定义，如何访问字段、方法、继承关系以及动态代理。

### 二、什么是反射？

反射就是Java在运行期间，可以获取某个对象、实例的所有信息。（代检验）

在运行期内，对某个实例一无所知的情况下，去获取它的属性以及方法。
<!--more-->
### 三、反射

#### 3.1 反射在JVM的实现原理

Java的包装类型，或者我们自定义的Java类，在JVM加载一个类到内存的时候，会为其创造一个`Class`的实例，将这个类的信息都保存起来，比如什么名字，属于哪个包，父类是谁，实现了哪些接口，有哪些属性和方法等。

因此，只要我们获取JVM创建的那个`Class`实例，就可以获取它的所有信息了。

#### 3.2 获取一个类（接口）的`Class`实例方法

> 1、方法一：通过类/接口的静态变量`class`获取

```java
public static void main(String[] args) {
    Class cls = String.class;
    System.out.println(cls);
}
```

输出结果如下：

```java
class java.lang.String
```

有点好奇这个类的toString()方法，于是查看了下源码，发现输出分为三种情况，接口、基本类型和普通类。源码如下：

```java
public String toString() {
    return (isInterface() ? "interface " : (isPrimitive() ? "" : "class "))
        + getName();
}
```

测试三种情况，输出如下：

```java
public static void main(String[] args) {
    Class cls2 = HelloWorld.class; // HelloWorld是一个接口
    System.out.println(cls2); // interface Reflect.HelloWorld

    Class cls1 = int.class;
    System.out.println(cls1); // int

    Class cls = String.class;
    System.out.println(cls); // class java.lang.String
}
```

> 2、方法二：通过实例变量提供的`getClass()`方法获取

```java
Person person = new Person(); // Person自定义的一个类，实例化对象
Class cls = person.getClass(); // 通过对象实例来获取
System.out.println(cls); // 输出：class Reflect.Person
```

> 3、方法三：通过Class的静态方法获取，前提是要知道完整的类/接口名

```java
Class cls = Class.forName("java.lang.String");
System.out.println(cls); // class java.lang.String
```

JVM加载一个`class`之后，生成的`Class`实例是唯一的，我们可以通过上面三种方法获取`Class`验证一下。

```java
Class cls1 = String.class;
Class cls2 = new String().getClass();
Class cls3 = Class.forName("java.lang.String");
boolean result = (cls1 == cls2)&&(cls1 == cls3)&&(cls2 == cls3);
System.out.println(result); // true

String s1 = new String("hello");
String s2 = new String("world");
boolean rlt = s1.getClass() == s2.getClass();
System.out.println(rlt); // true

Person person1 = new Person();
Person person2 = new Person();
boolean rlt1 = person1.getClass() == person2.getClass();
System.out.println(rlt1); // true
```

我们获取到一个`Class`实例之后，就可以通过这个`Class`实例来创建对应类型的一个新实例。

**JVM的动态加载：**JVM在执行Java程序的时候，不会把所有的class都加载到内存中，而是需要用到的时候才会去加载，这就是JVM的动态加载特性。

**小结：** 通过JVM的`Class`实例获取class类信息的方法就是反射。

### 四、通过反射获取属性、方法

#### 4.1 通过`Class`实例获取字段信息的方法

```java
getFields() // 获取所有public字段（包括父类）
getField(String) // 根据字段名获取字段（包括父类）
getDeclaredFields() // 获取当前类的所有字段（不包括父类）
getDeclaredField(String) // 根据字段名获取当前类的字段（不包括父类）
```

由此，我们可以小结一下：

1、含有声明Declared的，都是获取当前类，包括public, private。

2、没有Declared的，也可以获取父类的字段，但是只能获取public的，连当前类的私有也不可以获取。

下面我们下一个例子验证一下：

```java
public class Person {
    public String name;
    private int age;
}

public class Student extends Person{
    public String grade;
    private String address;
}

    public static void main(String[] args) throws ClassNotFoundException, NoSuchFieldException {
        Class stuCls = Student.class;
        System.out.println(stuCls.getField("grade")); // 只能获取public的，private不可以获取
        System.out.println(stuCls.getFields()); // 继承获取父类的public属性
        System.out.println(stuCls.getDeclaredField("address")); // 这个可以获取private字段
        System.out.println(stuCls.getField("name")); // 继承获取父类的public属性
    }
```

我们通过class的getFields()方法获取Field对象之后，就可以打印它的诸多信息，举例如下：

```java
Class stuCls = Student.class;
Field field = stuCls.getField("grade");
System.out.println(field.getName()); // 获取字段名
System.out.println(field.getType()); // 获取字段类型，输出 class java.lang.String
```

在获取字段之后，我们可以修改字段内容。其中，私有字段需要设置成可访问，如下代码：

```java
Person person = new Person("Huang",18);
System.out.println("本来的名字："+person.getName());
Class cls = person.getClass();
Field nameField = cls.getField("name");
nameField.set(person,"Lin"); // 更改实例person的name属性，这是public的，可以更改
System.out.println("更改后的名字："+person.getName());

System.out.println("本来的年龄："+person.getAge());
Field ageField = cls.getDeclaredField("age"); // 获取私有字段age
// ageField.set(person,22); // 这里会报错 java.lang.IllegalAccessException
// 绕开控制权限，也可以访问private字段
ageField.setAccessible(true);
ageField.set(person,22);
System.out.println("更改后的年龄："+person.getAge());
```

输出结果：

```java
本来的名字：Huang
更改后的名字：Lin
本来的年龄：18
更改后的年龄：22
```

**小结：** 修改非public字段，需要先.setAccessible(true)，才可以更改。

#### 4.2 通过`Class`实例获取方法字段信息的方法

这个跟上面的获取字段方法非常类似，列举如下：

```
getMethod(name,Class);
getDeclaredMethod(name,Class);
getMethods();
getDeclareMethods();
```

#### 4.3 调用构造方法

我们可以通过反射来获取新的实例，比如：

```java
Person person = Person.class.newInstance();
```

但是这个方法只能根据类的public无参构造方法实例化一个类。

如果要调用任意的构造方法，需要使用`Constructor`对象来实现，如：

```java
Constructor constructor = String.class.getConstructor(String.class);
String str = (String)constructor.newInstance("hello world");
System.out.println(str);
```

注意：如果要调用非public的构造方法，则需要先调用setAccessible(true)；

#### 4.4 获取继承关系（父类以及接口）

> 1、获取父类

```java
Class cls = Integer.class;
Class superCls = cls.getSuperclass();
System.out.println(superCls); // class java.lang.Number
```

> 2、获取interface

```java
Class cls = Integer.class;
Class[] is =cls.getInterfaces();
```

### 五、动态代理

Java提供了一种动态代理机制：也就是可以在运行期动态创建某个interface的实例。

这个的意思就是我们写一个接口，但是不实现这个接口，由JDK提供`InvocationHandler`实例去实现接口方法的调用。

定义接口如下：

```java
public interface Say {
    void talking(String str);
}
```

编写一个测试方法：

```java
InvocationHandler handler = new InvocationHandler() {
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        System.out.println(method);
        if(method.getName().equals("talking")){
            System.out.println("I am talking "+args[0]);
        }
        return null;
    }
};

Say say = (Say)Proxy.newProxyInstance(Say.class.getClassLoader(), new Class[]{Say.class},
        handler);
say.talking("Hello World!");
```

结果为：

```java
public abstract void Reflect.Say.talking(java.lang.String)
I am talking Hello World!
```

即动态代理是通过Proxy创建对象，然后把接口方法交给InvocationHandler去实现，而不用像我们之前一样，写完接口后实现接口，再由其他地方调用已经实现的接口方法。