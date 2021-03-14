---
title: 浅谈Java的equals()与hashCode()方法
date: 2021-02-20 21:51:51
postlink: talk-java-equals-hashcode
categories: Java
tags: Java基础
---
## 一、概述

本文主要介绍equals()方法和hashCode()方法，讨论为什么重写equals()的同时也必须要重写hashCode()。

## 二、equals()

equals()这个方法是定义在基类Object中的，这也意味着所有的类都包含着这个方法，源码如下：

```java
    public boolean equals(Object var1) {
        return this == var1;
    }
```
<!--more-->
由上面的源码可知，equals()的默认实现是使用“==”的，也就是判断两个对象的内存地址是否一样。但问题是，很多时候我们想要的是，只要两个对象的内容相等，那就应该视为相等，这也就是为什么我们需要经常重写equals()的原因。

### 2.1 新建一个类举例说明

比如我们现在新建一个手机类，里面的属性是品牌和版本。

```java
/**
 * 手机类
 */
public class Phone {
    String brand; // 品牌
    String version; // 版本

    public Phone() {

    }

    public Phone(String brand,String version) {
        this.brand = brand;
        this.version = version;
    }
    // 省略setter getter
}
```

当我们实例化两个对象，即两台小米9手机，我们想要的是这两个对象其实是一样的，但是当我们调用equals()的时候返回的是false。

```java
    public static void main(String[] args) {
        Phone a = new Phone("xiaomi","9"); // 小米9
        Phone b = new Phone("xiaomi","9"); // 小米9
        boolean result = a.equals(b);
        System.out.println(result); // false
    }
```

显然，这并没有达到我们的预期，这是因为在JVM中，a和b是堆上的两个不同的对象，它们的内存地址是不一样的，而如果没有重写的equals()，调用的equals()默认方法是“==”，比较的是地址，因此上面的程序会得到false的结果。

那么，我们应该怎么做呢？答案就是重写equals()方法。

### 2.2 重写equals()方法

我们期望的是，上面那两个对象a, b，因为它们的值都是小米9，那么它们应该是相等的对象。因此我们需要重写equals()方法，比对品牌和版本即可。

```java
public class Phone {
    String brand; // 品牌
    String version; // 版本

    public Phone() {

    }

    public Phone(String brand,String version) {
        this.brand = brand;
        this.version = version;
    }
    // 省略setter getter

    @Override
    public boolean equals(Object obj){
        if(this == obj) // 内存地址都相等了，肯定是相同的对象
            return true;
        if(!(obj instanceof Phone)) // 如果要比对的对象都不是Phone类型的，则直接返回false
            return false;
        Phone phone = (Phone)obj; // 来到这里，肯定是Phone类型的，类型转换成Phone
        return this.brand.equals(phone.brand) && this.version.equals(phone.version);
    }
}
```

重新运行，这时候最上面的那个比对a.equals(b);返回的就是true了。

### 2.3 equals()方法的规范

Java SE定义了一些必须满足的规则，即：

* 自反性：一个对象必须等于自己。

* 对称性：x.equals(y)必须返回与y.equals(x)相同的结果。

* 可传递性：如果x.equals(y)和y.equals(z)则也x.equals(z)。

* 一致性：仅当equals()中包含的属性发生更改时，equals()的值才应更改（不允许随机性）。

## 三、hashCode()

hashCode()的作用是找到当前对象在哈希表中的索引位置，它返回的是一个整型的数值。也是定义在基类Object中的方法，还是个本地方法。

```java
public native int hashCode();
```

### 3.1 为什么要重写hashCode()？

还是上面的例子，我们重写过了equals()，返回的结果也符合了我们的预期，就是两个小米9比较，返回了true。但是为什么很多人都说重写equals()的同时也必须要重写hashCode()呢？

首先，我们用上面这个例子，查看两个对象的hashCode值。

```java
public static void main(String[] args) {
    Phone a = new Phone("xiaomi","9");
    Phone b = new Phone("xiaomi","9");
    boolean result = a.equals(b);
    System.out.println(result);

    int aCode = a.hashCode();
    int bCode = b.hashCode();
    System.out.println(aCode); // 返回结果：460141958
    System.out.println(bCode); // 返回结果：1163157884
}
```

显然，a和b对象的hashCode值不一样。它当然应该不一样啦，因为我们没有重写过hashCode()，而不同的对象的哈希值是不一样的。

只看到这里的话，好像重不重写hashCode()好像没有什么关系嘛。但是！在一些集合操作中，如果我们没有重写hashCode()，就会发生一些意料之外的结果。

### 3.2 HashMap举例

下面我们用HashMap来举例，为什么不重写hashCode()会有问题。

```java
Map<Phone,Integer> map = new HashMap<>();
map.put(new Phone("xiaomi","9"), 3000);
map.put(new Phone("xiaomi","10"), 3500);
map.put(new Phone("iPhone","12"), 6000);

Phone myPhone = new Phone("xiaomi", "10");
int myPhonePrice = map.get(myPhone);
System.out.println(myPhonePrice);
```

我们的期望是，通过myPhone可以获取Map中添加的那个小米10的value，返回3500。但是当我们运行这段程序的时候，会抛出空指针异常java.lang.NullPointerException。

这是为什么呢？因为我们没有重写hashCode()，myPhone和map里面的小米10不是同一个对象，两者返回了hashCode()不一致，因此会被判定为不是同一个对象，自然在map里面找不到对应的key和value了。

### 3.3 重写hashCode()

我们重写hashCode()如下，我们一般用31这个数值的原因是：31可以用移位和减法来代替乘法，以获得更好的性能：31 * i == (i << 5) - i，现代JVM自动进行这种优化。

```java
    @Override
    public int hashCode() {
        int result = 17;
        if (brand != null) {
            result = 31 * result + brand.hashCode();
        }
        if (version != null) {
            result = 31 * result + version.hashCode();
        }
        return result;
    }
```

我们再次运行获取上面a，b对象的hashCode()，发现返回的值是一样的了，map也获取到了值。

所以，当我们重写equals()方法的时候，为了遵守协定：相等的对象返回相同的hashCode。因此，我们也需要重写hashCode()。

## 四、结论

在本文中，我们讨论了equals()和hashCode()的一些作用和约定，需要记住的是：如果我们重写equals()方法，也同时必须重写hashCode()。