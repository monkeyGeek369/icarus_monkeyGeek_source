---
title: Set原理
category: java基础
date: 2022-06-14 21:43:21
updated: 2022-06-14 21:43:21
enname: set
categories: java基础
tags:
	- 面试
	- java集合
keywords: set
permalink:
thumbnail:
---

set是一个不存在重复元素的集合。<!--more-->接口的底层实现有两个hashset和treeset。

</br>

# HashSet

- 一个**没有重复元素的集合**。
- 由HashMap实现**不保证元素的顺序**
- 允许使用 null 元素
- 是**非同步的**
- **迭代器是fail-fast的**

</br>

### 数据结构

- 它是通过HashMap实现的。HashSet中含有一个"HashMap类型的成员变量"map
- 实际上的存值是将值存在map的key中，而value默认创建一个object代替
- 构造函数支持传递“加载因子”和“初始容量”，当实际大小>最大容量*加载因子，容量将翻倍



</br>

### 遍历方式

- 迭代器
- for-each



</br>

</br>

# TreeSet

- 一个有序的且没有重复元素的集合
- 继承于AbstractSet，所以它是一个Set集合，具有Set的属性和方法。
- 实现了NavigableSet接口，意味着它支持一系列的导航方法。比返回小于、小于等于、大于等于、大于给定元素的元素
- 实现了Cloneable接口
- 实现了java.io.Serializable接口
- 基于TreeMap实现的
- 是非同步的
- 它的iterator 方法返回的迭代器是fail-fast的



</br>

### 数据结构

- 它是通过TreeMap实现的。TreeSet中含有一个"NavigableMap类型的成员变量"m
- 实际上的存值是将值存在map的key中，而value默认创建一个object代替
- 构造函数支持比较器传递，无参构造函数默认自然比较器，否则可传入自定义比较器



</br>

### 遍历方式

- 迭代器
- for-each



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


