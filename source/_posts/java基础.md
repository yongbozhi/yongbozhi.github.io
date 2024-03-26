---
title: java基础
date: 2024-03-24 19:08:54
tags: [java面试,java基础]
categories: java面经
---

## 接口和抽象类的区别和相同点？

不同点：

1.类可以实现很多个接口，但是只能继承一个抽象类

2.Java接口中声明的变量默认都是final的。抽象类可以包含非final的变量。

3.接口中所有的方法隐含的都是抽象的。而抽象类则可以同时包含抽象和非抽象的方法。

4.Java接口中的成员函数默认是public的。抽象类的成员函数可以是private，protected或者是public。

 

两者相同点: 

1.抽象类和接口都不能直接实例化，如果要实例化，抽象类变量必须指向实现所有抽象方法的子类对象，接口变量必须指向实现所有接口方法的类对象。

2.抽象类里的抽象方法必须全部被子类所实现（若不是抽象方法则不用全部实现），如果子类不能全部实现父类抽象方法，那么该子类还只能是抽象类。同样，一个类实现接口的时候，如不能全部实现接口方法，那么该类也只能为抽象类。

扩展：对于一个final变量，如果是基本数据类型的变量，则其数值一旦在初始化之后便不能更改；如果是引用类型的变量，则在对其初始化之后便不能再让其指向另一个对象。

 

两种的使用场景：

abstract class在Java语言中体现的是一种继承关系，父类和派生类之间必须存在“is a”关系，即父类和派生类在概念本质上应该是相同的。对于interface 来说则不然，并不要求interface的实现者和interface定义在概念本质上是一致的，仅仅是实现了interface定义的契约而已。为了使论述便于理解，下面将通过一个简单的实例进行说明。

考虑这样一个例子，假设在我们的问题领域中有一个关于Door的抽象概念，该Door具有执行两个动作open和close，此时我们可以通过abstract class或者interface来定义一个表示该抽象概念的类型，其他具体的Door类型可以extends使用abstract class方式定义的Door或者implements使用interface方式定义的Door。看起来好像使用abstract class和interface没有大的区别。

如果现在要求Door还要具有报警的功能。我们该如何设计针对该例子的类结构呢），下面将罗列出可能的解决方案，并从设计理念层面对这些不同的方案进行分析。

解决方案一：

简单的在Door的定义中增加一个alarm方法，这种方法违反了面向对象设计中的一个核心原则ISP（Interface Segregation Priciple），在Door的定义中把Door概念本身固有的行为方法和另外一个概念“报警器“的行为方法混在了一起。这样引起的一个问题是那些仅仅依赖于Door这个概念的模块会因为“报警器“这个概念的改变（比如：修改alarm方法的参数）而改变。

解决方案二：

既然open、close和alarm属于两个不同的概念，根据ISP原则应该把它们分别定义在代表这两个概念的抽象类中。定义方式有：这两个概念都使用abstract class方式定义；两个概念都使用interface方式定义；一个概念使用abstract class方式定义，另一个概念使用interface方式定义。

显然，由于Java语言不支持多重继承，所以两个概念都使用abstract class方式定义是不可行的。后面两种方式都是可行的，但是对于它们的选择却反映出对于问题领域中的概念本质的理解、对于设计意图的反映是否正确、合理。

如果两个概念都使用interface方式来定义，那么就反映出两个问题：1、我们可能没有理解清楚问题领域，AlarmDoor在概念本质上到底是Door还是报警器？2、如果我们对于问题领域的理解没有问题，比如：我们通过对于问题领域的分析发现AlarmDoor在概念本质上和Door是一致的，那么我们在实现时就没有能够正确的揭示我们的设计意图，因为在这两个概念的定义上（均使用interface方式定义）反映不出上述含义。

如果我们对于问题领域的理解是：AlarmDoor在概念本质上是Door，同时它有具有报警的功能。我们该如何来设计、实现来明确的反映出我们的意思呢？前面已经说过，abstract class在Java语言中表示一种继承关系，而继承关系在本质上是“is a”关系。所以对于Door这个概念，我们应该使用abstarct class方式来定义。另外，AlarmDoor又具有报警功能，说明它又能够完成报警概念中定义的行为，所以报警概念可以通过interface方式定义。

这种实现方式基本上能够明确的反映出我们对于问题领域的理解，正确的揭示我们的设计意图。其实abstract class表示的是“is a”关系，interface表示的是“like a”关系，大家在选择时可以作为一个依据，当然这是建立在对问题领域的理解上的，比如：如果我们认为AlarmDoor在概念本质上是报警器，同时又具有Door的功能，那么上述的定义方式就要反过来了。

具体参考： https://cloud.tencent.com/developer/article/1434229

## 经典排序算法

![sort](java基础/sort.png)

![2](java基础/2.png)

https://www.runoob.com/w3cnote/ten-sorting-algorithm.html

## HashMap 和 Hashtable 的区别

\1. 线程是否安全： HashMap 是⾮线程安全的， HashTable 是线程安全的,因为 HashTable 内 

部的⽅法基本都经过 synchronized 修饰。（如果你要保证线程安全的话就使⽤

ConcurrentHashMap 吧！）； 

2. 效率： 因为线程安全的问题， HashMap 要⽐ HashTable 效率⾼⼀点。另外， HashTable 

基本被淘汰，不要在代码中使⽤它；

\3. 对 **Null key** 和 **Null value** 的⽀持： HashMap 可以存储 null 的 key 和 value，但 null 作为 

键只能有⼀个，null 作为值可以有多个；HashTable 不允许有 null 键和 null 值，否则会抛出 

NullPointerException 。 

\4. 初始容量⼤⼩和每次扩充容量⼤⼩的不同 ： ① 创建时如果不指定容量初始值， Hashtable 

默认的初始⼤⼩为 11，之后每次扩充，容量变为原来的 2n+1。 HashMap 默认的初始化⼤ 

⼩为 16。之后每次扩充，容量变为原来的 2 倍。② 创建时如果给定了容量初始值，那么 

Hashtable 会直接使⽤你给定的⼤⼩，⽽ HashMap 会将其扩充为 2 的幂次⽅⼤⼩ 

（ HashMap 中的 tableSizeFor() ⽅法保证，下⾯给出了源代码）。也就是说 HashMap 总 

是使⽤ 2 的幂作为哈希表的⼤⼩,后⾯会介绍到为什么是 2 的幂次⽅。 

\5. 底层数据结构： JDK1.8 以后的 HashMap 在解决哈希冲突时有了重⼤的变化，当链表⻓度 

⼤于阈值（默认为 8）（将链表转换成红⿊树前会判断，如果当前数组的⻓度⼩于 64，那么 

会选择先进⾏数组扩容，⽽不是转换为红⿊树）时，将链表转化为红⿊树，以减少搜索时

间。Hashtable 没有这样的机制

## 关于JAVA中接口存在的意义