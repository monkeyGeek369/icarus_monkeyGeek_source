---
title: java中的比较器Comparable与Comparator
category: java基础
date: 2020-04-07 21:48:37
updated: 2020-04-07 21:48:37
enname: compare
categories: java基础
tags:
	- java
	- java比较器
keywords: java,java比较器,Comparable,Comparator
permalink:
thumbnail:
---

# 简介

我们在java编码的过程中不可避免的会使用java中提供的集合对象,例如List,ArrayList等等,那么在使用集合的过程中有很大的几率会用到排序.回想一下在学习c语言的时候排序是如何实现的?<!--more-->没错,就是通过手写各种基础的排序算法实现排序(当然现在也有各种c语言的基础包可用),例如选择\冒泡\插入\希尔\二分法等等.在Java中我们当然也可以自己写排序算法实现排序,但是JDK已经提供一些基础的基于集合的排序接口供我们使用,因此无需特意自写算法.本文将介绍两种排序的实现方式Comparable与Comparator.

</br>

# Comparable





</br>

# Comparator







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


