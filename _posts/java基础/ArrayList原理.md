---
title: ArrayList原理
category: java基础
date: 2022-06-13 11:57:35
updated: 2022-06-13 11:57:35
enname: arraylist
categories: java基础
tags:
	- 面试
	- java集合
keywords: ArrayList
permalink:
thumbnail:
---

# 简介

- 是一个数组队列，容量能动态增长<!--more-->
- 继承于AbstractList，实现了List, RandomAccess, Cloneable, java.io.Serializable等接口
- 实现了RandmoAccess接口，即提供了随机访问功能，我们可以通过元素的序号快速获取元素对象
- 实现了Cloneable接口，能被克隆
- 实现java.io.Serializable接口，能通过序列化去传输
- 操作不是线程安全的



</br>

# 数据结构

- elementData：是"Object[]类型的数组"，支持动态增长。若使用默认构造函数，则ArrayList的**默认容量大小是10**。
- size：动态数组的实际大小



扩容方案：ArrayList会重新设置容量：**新的容量=“(原始容量x3)/2 + 1”**，比当前容量的一半多一个。数据会被拷贝到新数组中。



</br>

# 遍历方式

- 迭代器：效率低
- 随机访问：效率较高
- for循环



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


