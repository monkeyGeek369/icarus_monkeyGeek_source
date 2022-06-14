---
title: Hashtable原理
category: java基础
date: 2022-06-14 23:04:52
updated: 2022-06-14 23:04:52
enname: hashtable
categories: java基础
tags:
	- 面试
	- java集合
keywords: Hashtable
permalink:
thumbnail:
---

# 简介

- 是一个**散列表**，它存储的内容是**键值对(key-value)映射**<!--more-->
- **继承于Dictionary**，实现了Map、Cloneable、java.io.Serializable接口
- 是**同步的**，这意味着它是线程安全的
- key、value都不可以为null
- 映射不是有序的
- 数据结构由数组+单向列表组成，数组决定桶的数量，单向列表采用链表法避免hash冲突
- **初始容量** ：容量是哈希表中桶的数量，默认为11
- **加载因子**：对哈希表在其容量自动增加之前可以达到多满的一个尺度。**默认加载因子是 0.75**
- hash算法采用key的hashcode



</br>

# 构造函数

```java
// 默认构造函数。
public Hashtable()

// 指定“容量大小”的构造函数
public Hashtable(int initialCapacity)

// 指定“容量大小”和“加载因子”的构造函数
public Hashtable(int initialCapacity, float loadFactor)

// 包含“子Map”的构造函数
public Hashtable(Map<? extends K, ? extends V> t)
```



</br>

# 数据结构

- table是一个Entry[]数组类型，而Entry实际上就是一个单向链表。哈希表的"key-value键值对"都是存储在Entry数组中的
- count是Hashtable的大小，它是Hashtable保存的键值对的数量
- threshold是Hashtable的阈值，用于判断是否需要调整Hashtable的容量。threshold的值="容量*加载因子"，**当Hashtable的 “实际容量” >= “阈值”时，(阈值 = 总的容量 x 加载因子)，就将变为“原始容量x2 + 1”。**
- loadFactor就是加载因子
- modCount是用来实现fail-fast机制



</br>

# 遍历方式

- 实现了**Enumeration**接口，通过枚举接口实现遍历
- 实现了迭代器接口，通过迭代器遍历





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


