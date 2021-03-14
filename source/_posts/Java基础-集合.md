---
title: Java基础 | 集合
date: 2021-02-24 20:36:05
postlink: java-base-collection
categories: Java
tags: Java基础
---
## 一、集合的概念

集合我认为就是存放一堆东西的容器，很容易想到数组，数组也是一种容器。但是每次初始化之后，大小都是固定的，而且增删数据的时候，需要移动大量数据，耗时耗力。为了适应具体业务需求，Java提供了一些集合供我们使用。

## 二、集合的分类

Java集合主要包括三大巨头：List，Set，Map。

> 集合特性

List：存储有序列表的集合。

Set：存储没有重复数据的集合。

Map：存放key-value型的集合，其中key不可以重复。
<!--more-->
## 三、List

### 3.1 特性

List是一种有序列表，它按照我们添加的顺序，给每个元素一个下标索引，索引从0开始。它是一个接口，主要有ArrayList和LinkedList两种实现。

### 3.2 ArrayList

查看源码，可知ArrayList底层是由数组来实现的。

```java
transient Object[] elementData; 
```

其中transient字段的意思是不可序列化，这个变量不再是对象持久化的一部分，变量内容在序列化之后无法访问。

在数组中，如果我们要在某个位置插入或者删除元素，需要移动大量的元素，编写很多代码。ArrayList底层插入或者删除元素也是用的移动元素的方式，我们直接用ArrayList的话，就省去了自己编写移动元素的代码。

常用的方法如下：

```java
public static void main(String[] args) {
    ArrayList<Integer> list = new ArrayList<>();
    for(int i=0;i<5;i++){
        list.add(i);
    }
    list.add(5); // 添加一个元素
    list.size(); // 返回元素个数
    list.remove(1); // 删掉1
    list.get(2); //根据索引获取
    for (int s:list) { // 迭代
        System.out.println(s);
    }
}
```

一般迭代List，都使用Iterator，用iterator.hasNext();判断是否有下一个元素存在，这是遍历List最高效的方法。

### 3.3 LinkedList

LinkedList是List的另外一种实现，它的底层是由单链表实现的。获取指定索引的资源，需要从头开始遍历，速度比较慢，占用的内存也比较大（因为除了要保存元素值，还需要保存指针）。但是好处是在增删元素的时候，不需要移动元素，只需要修改指针即可。

## 四、Map

### 4.1 特性

Map是以key-value的形式存储元素的，是为了以最快的速度通过key找到value，并形成一个对应关系。Map是一个接口，通常用的是实现它的HashMap。Map并不保证顺序，只是保证key不重复。

### 4.2 HashMap

HashMap中，key是不能够重复的，但是value可以重复，且HashMap是无序的。

它的底层是用空间换时间来实现的，就是用一个大数组存储所有的value，根据Key的值计算出value应该存储在哪个索引，然后将value存储到对应索引位置。这样它在根据key取value的时候，直接计算key的hashCode()，然后找对应的value。

如果添加了重复的key，并不会报错，只是新的key对应的value会替代掉老的key对应的老value。如下面的例子：

```java
public static void main(String[] args) {
    Map<String,String> map = new HashMap<>();
    map.put("广东","广州");
    map.put("湖北","武汉");
    map.put("广西","南宁");
    map.put("广西","南宁111");
    System.out.println(map.get("广西")); // 南宁111
}
```

最后面输出的是南宁111，也就是覆盖掉了一开始的南宁。

迭代的使用可以用foreach迭代，用map.keySet()返回key集合，如下：

```java
public static void main(String[] args) {
    Map<String,String> map = new HashMap<>();
    map.put("广东","广州");
    map.put("湖北","武汉");
    map.put("广西","南宁");
    map.put("广西","南宁111");
    System.out.println(map.get("广西")); // 南宁111

    for(String key : map.keySet()){ // map.keySet() 返回Key的集合
        String value = map.get(key);
        System.out.println("key:"+key+"-value:"+value);
    }
}
```

### 4.3 EnumMap

如果Map的key是枚举类型的，使用EnumMap更加高效，不需要计算hashCode()，效率高，没有浪费空间。

### 4.4 TreeMap

我们注意到，上面的HashMap的key是无序的，如果想要key有序，那么可以使用实现了SortedMap接口的TreeMap类。但是它的顺序不是按照添加进来的顺序进行排序的，而是有一套自己的排序规则。比如：

```java
Map<String,Integer> map = new TreeMap<>();
map.put("C",1);
map.put("A",2);
map.put("B",3);

for(String key : map.keySet()){
    Integer value = map.get(key);
    System.out.println("key:"+key+"-value:"+value);
}
```

输出

```java
key:A-value:2
key:B-value:3
key:C-value:1
```

这是因为key是String，默认按照字母排序。这也是因为key已经实现了Comparable接口，如果是我们自己新建的类作为key，那么我们需要编写自己的排序算法（自己实现Comparable或者传入Comparator）。否则将不能够正确排序工作。

## 五、Set

### 5.1 特性

Set用来存储不重复的元素集合。于Map相比，Set相当于一个只存储key的集合，不存储value。事实上，Set很多实现都是通过Map来实现的，比如HashSet就是通过调用HashMap实现的，也就是说HashSet是HashMap的简单封装，源码如下：

```java
public HashSet() {
    map = new HashMap<>();
}
public HashSet(int initialCapacity) {
    map = new HashMap<>(initialCapacity);
}
```

Set的两个主要实现类是HashSet和TreeSet。

### 5.2 HashSet

HashSet是无序的，因为它没有实现SortedSet接口，继承关系如下：

![image-20210224202923868](https://file.hjxlog.com//blog/images/20210224202923.png)

HashSet常用API，如下：

```java
Set<String> set = new HashSet<>();
set.add("hello"); // 添加
set.add("world");
set.remove("hello"); // 删除
System.out.println(set.size()); //返回大小
for(String s : set){
    System.out.println(s);
}
```

### 5.3 TreeSet

TreeSet也是TreeMap的一个简单封装，要求跟TreeMap一样，加入的元素需要实现了Comparable接口才行。

```java
public TreeSet() {
    this(new TreeMap<E,Object>());
}
```

![image-20210224203215968](https://file.hjxlog.com//blog/images/20210224203216.png)

通过继承树可以看出TreeSet实现了SortedSet，因此它是有序的。