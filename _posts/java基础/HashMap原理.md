---
title: HashMap原理
category: java基础
date: 2022-06-14 22:16:40
updated: 2022-06-14 22:16:40
enname: hashmap
categories: java基础
tags:
	- 面试
	- java集合
keywords: hashmap
permalink:
thumbnail:
---

# 简介

- 是一个散列表，它存储的内容是键值对(key-value)映射<!--more-->
- 继承于AbstractMap，实现了Map、Cloneable、java.io.Serializable接口
- 不是同步的（不是线程安全的）
- key、value都可以为null（当HashMap的key为null时，HashMap会将其固定的插入table[0]位置(即HashMap散列表的第一个位置)；而且table[0]处只会容纳一个key为null的值，当有多个key为null的值插入的时候，table[0]会保留最后插入的value。）
- 映射不是有序的
- 数据结构由数组+单向链表组成，数组决定桶的数量
- **初始容量** ：容量是哈希表中桶的数量，默认为16
- **加载因子**：是哈希表在其容量自动增加之前可以达到多满的一种尺度。当哈希表中的条目数超出了加载因子与当前容量的乘积时，则要对该哈希表进行 rehash 操作（即重建内部数据结构），从而哈希表将具有大约两倍的桶数。**默认加载因子是 0.75**，过低则数据分布稀疏浪费空间且频繁rehash，过高则数据分布稠密增加查询成本
- 增加键值对时将key和链表中的每一个节点的key进行对比。若key已经存在Entry链表中，则用该value值取代旧的value值；若key不存在Entry链表中，则新建一个key-value节点，并将该节点插入Entry链表的表头位置
- hash算法采用自定义的哈希算法



</br>

# 构造函数

```java
// 默认构造函数。
HashMap()

// 指定“容量大小”的构造函数
HashMap(int capacity)

// 指定“容量大小”和“加载因子”的构造函数
HashMap(int capacity, float loadFactor)

// 包含“子Map”的构造函数
HashMap(Map<? extends K, ? extends V> map)
```



</br>

# 数据结构

- table是一个Entry[]数组类型，而Entry实际上就是一个单向链表（拉链法解决哈希冲突）。哈希表的"key-value键值对"都是存储在Entry数组中的
- size是HashMap的大小，它是HashMap保存的键值对的数量
- threshold是HashMap的阈值，用于判断是否需要调整HashMap的容量。threshold的值="容量*加载因子"，当HashMap中存储数据的数量达到threshold时，就需要将HashMap的容量加倍。
- loadFactor就是加载因子
- modCount是用来实现fail-fast机制的，原理是当多线程操作时会修改modCount，如果发现有变动则触发fail-fast机制



**<u>JDK1.7与1.8的区别</u>**

JDK7之前hashmap又叫散列链表：基于一个数组以及多个链表的实现，hash值冲突的时候，就将对应节点以链表的形式存储。JDK8中，当同一个hash值（Table上元素）的链表节点数不小于8时，将不再以单链表的形式存储了，会被调整成一颗红黑树。这就是JDK7与JDK8中HashMap实现的最大区别。



</br>

# 遍历方法

当我们通过entrySet()获取到的Iterator的next()方法去遍历HashMap时，实际上调用的是 nextEntry() 。而nextEntry()的实现方式，先遍历Entry(根据Entry在table中的序号，从小到大的遍历)；然后对每个Entry(即每个单向链表)，逐个遍历。



</br>

</br>

</br>

<script>
var _hmt = _hmt || [];
(function() {
  var hm = document.createElement("script");
  hm.src = "https://hm.baidu.com/hm.js?2f798e6b269c8a40f12bef25d7f1876d";
  var s = document.getElementsByTagName("script")[0]; 
  s.parentNode.insertBefore(hm, s);
})();
</script>

<div style="height:260px; background-color:rgb(238,240,244); padding:10px;border-radius:10px;">
    <p style="color:#f36c21;font:bold 16px/20px 'kaiTi';">
      版权声明：本文为博主原创文章，欢迎转载，转载请注明作者、原文超链接，感谢各位看官!!!
    </p>
    <p>
      <span style="font:bold 16px/20px 'kaiTi';">本文出自：</span><a href="https://monkeyGeek369.github.io">monkeyGeek</a> 
    </p>
    <p>
      <span style="font:bold 16px/20px 'kaiTi';">座右铭：</span><span>生于忧患，死于安乐</span> 
    </p>
    <p>
      <span style="font:16px/20px 'kaiTi';">欢迎志同道合的朋友一起交流、探讨！</span> 
    </p>
    <img style="height:auto; width:auto;flot:left;" src="../../../../image/monkey64.png" /><span style="font:16px/20px 'kaiTi';flot:left;">   monkeyGeek</span>


