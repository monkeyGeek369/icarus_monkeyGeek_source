---
title: CAS原理
category: java基础
date: 2022-06-18 16:25:59
updated: 2022-06-18 16:25:59
enname: cas
categories: java基础
tags:
	- 面试
	- 锁
keywords: cas原理、CAS
permalink:
thumbnail:
---

# 什么是CAS

CAS的全称是Compare And Swap 即比较交换，它将指定内存位置的内容与给定值进行比较，只有在相同的情况下，将该内存位置的内容修改为新的给定值。<!--more-->

CAS具有原子性，可以实现多线程同步

CAS就是乐观锁的典型代表



执行函数：CAS(V,E,N)

V表示要更新的变量

E表示预期值

N表示新值

</br>

</br>

# 更新逻辑

- V==E表明变量符合预期值，可以对其进行更新。将V设置为N
- V!=E证明有其他线程已经做了更新，本次线程不进行操作。可以读取E值重复一次

</br>

</br>

# ABA问题

如线程1从内存X中取出A，这时候另一个线程2也从内存X中取出A，并且线程2进行了一些操作将内存X中的值变成了B，然后线程2又将内存X中的数据变成A，这时候线程1进行CAS操作发现内存X中仍然是A，然后线程1操作成功。虽然线程1的CAS操作成功，但是整个过程就是有问题的。比如链表的头在变化了两次后恢复了原值，但是不代表链表就没有变化。

 所以JAVA中提供了AtomicStampedReference/AtomicMarkableReference来处理会发生ABA问题的场景，主要是在对象中额外再增加一个标记来标识对象是否有过变更。



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


