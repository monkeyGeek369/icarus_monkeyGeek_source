---
title: WeakHashMap原理
category: java基础
date: 2022-06-14 22:52:22
updated: 2022-06-14 22:52:22
enname: weakhashmap
categories: java基础
tags:
	- 面试
	- java集合
keywords: WeakHashMap
permalink:
thumbnail:
---

# 简介

-   是一个**散列表**，它存储的内容是**键值对(key-value)映射**<!--more-->
- **键和值都可以是null**
-   **键是“弱键”**
- 数据结构同HashMap
- 是不同步的



</br>

# 弱键实现原理

 **通过WeakReference和ReferenceQueue实现的**。 WeakHashMap的key是“弱键”，即是WeakReference类型的；ReferenceQueue是一个队列，它会保存被GC回收的“弱键”。

实现步骤是：

1. 弱键被GC回收时这个“弱键”也同时会被添加到ReferenceQueue(queue)队列
2. 当下一次我们需要操作WeakHashMap时，会先同步table和queue。同步它们就是**删除table中被GC回收的键值对**。



</br>

# 构造函数

```java
// 默认构造函数。
WeakHashMap()

// 指定“容量大小”的构造函数
WeakHashMap(int capacity)

// 指定“容量大小”和“加载因子”的构造函数
WeakHashMap(int capacity, float loadFactor)

// 包含“子Map”的构造函数
WeakHashMap(Map<? extends K, ? extends V> map)
```



</br>

# 数据结构

- table是一个Entry[]数组类型，而Entry实际上就是一个单向链表。哈希表的"key-value键值对"都是存储在Entry数组中的。
- size是Hashtable的大小，它是Hashtable保存的键值对的数量。
- threshold是Hashtable的阈值，用于判断是否需要调整Hashtable的容量。threshold的值="容量*加载因子"。
- loadFactor就是加载因子。
- modCount是用来实现fail-fast机制的
- queue保存的是“已被GC清除”的“弱引用的键”。



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


