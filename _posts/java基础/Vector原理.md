---
title: Vector原理
category: java基础
date: 2022-06-13 21:26:36
updated: 2022-06-13 21:26:36
enname: vector
categories: java基础
tags:
	- 面试
	- java集合
keywords: vector
permalink:
thumbnail:
---

# 简介

- 矢量队列<!--more-->
- Vector 继承了AbstractList，实现了List；所以，**它是一个队列，支持相关的添加、删除、修改、遍历等功能**。
- Vector 实现了RandmoAccess接口，即提供了随机访问功能
- Vector 实现了Cloneable接口，它能被克隆。
- **Vector中的操作是线程安全的**



</br>

# 构造函数

```java
// 默认构造函数
Vector()

// capacity是Vector的默认容量大小。当由于增加数据导致容量增加时，每次容量会增加一倍。
Vector(int capacity)

// capacity是Vector的默认容量大小，capacityIncrement是每次Vector容量增加时的增量值。
Vector(int capacity, int capacityIncrement)

// 创建一个包含collection的Vector
Vector(Collection<? extends E> collection)
```



</br>

# 数据结构

-  elementData ："Object[]类型的数组"，未设置初始大小时使用默认大小10
- elementCount：动态数组的实际大小。
- capacityIncrement：动态数组的增长系数。**若容量增加系数 >0，则将容量的值增加“容量增加系数”；否则，将容量大小增加一倍。**



</br>

# 遍历方式

- 迭代器：效率最低
- 随机访问：vec.get(i)，效率最高
- for循环
- Enumeration遍历





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


