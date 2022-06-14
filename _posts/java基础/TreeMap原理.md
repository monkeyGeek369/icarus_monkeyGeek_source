---
title: TreeMap原理
category: java基础
date: 2022-06-14 22:36:11
updated: 2022-06-14 22:36:11
enname: treemap
categories: java基础
tags:
	- 面试
	- java集合
keywords: treemap
permalink:
thumbnail:
---

# 简介

- 一个**有序的key-value集合**，它是通过红黑树实现的<!--more-->
- 实现了NavigableMap接口，意味着它**支持一系列的导航方法。**
- 实现了Cloneable接口
- 实现了java.io.Serializable接口
- 适合读取操作频繁，插入、删除操作少的业务，因为红黑书在变动后必须保持平衡
- 基于**红黑树（Red-Black tree）实现**。该映射根据**其键的自然顺序进行排序**，或者根据**创建映射时提供的 Comparator 进行排序**，具体取决于使用的构造方法。
- 基本操作 containsKey、get、put 和 remove 的时间复杂度是 log(n) 。
- 是**非同步**的
- 它的iterator 方法返回的**迭代器是fail-fastl**的



</br>

# 构造函数

```java
// 默认构造函数。使用该构造函数，TreeMap中的元素按照自然排序进行排列。
TreeMap()

// 创建的TreeMap包含Map，构造函数会调用putAll()将m中的所有元素添加到TreeMap中
TreeMap(Map<? extends K, ? extends V> copyFrom)

// 指定Tree的比较器
TreeMap(Comparator<? super K> comparator)

// 创建的TreeSet包含copyFrom，通过buildFromSorted()来创建对应的Map
TreeMap(SortedMap<K, ? extends V> copyFrom)
```



</br>

# 数据结构

- root 是红黑数的根节点。它是Entry类型，Entry是红黑数的节点
- entry包含了红黑数的6个基本组成成分：key(键)、value(值)、left(左孩子)、right(右孩子)、parent(父节点)、color(颜色)。Entry节点根据key进行排序，Entry中的key比较大小是根据比较器comparator来进行判断的。
- size是红黑数中节点的个数



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


