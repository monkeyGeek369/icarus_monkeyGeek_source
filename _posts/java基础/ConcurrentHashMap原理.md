---
title: ConcurrentHashMap原理
category: java基础
date: 2022-06-15 11:47:19
updated: 2022-06-15 11:47:19
enname: con-hashmap
categories: java基础
tags:
	- 面试
	- 并发容器
keywords: ConcurrentHashMap
permalink:
thumbnail:
---

# JDK1.7版本原理

## 分段锁机制

简而言之，ConcurrentHashMap在对象中保存了一个Segment数组<!--more-->，即将整个Hash表划分为多个分段；而每个Segment元素，即每个分段则类似于一个Hashtable；这样，在执行put操作时首先根据hash算法定位到元素属于哪个Segment，然后对该Segment加锁即可。因此，ConcurrentHashMap在多线程并发编程中可是实现多线程put操作。

Segment继承了ReetrantLock，表示Segment是一个可重入锁，因此ConcurrentHashMap通过可重入锁对每个分段进行加锁。



</br>

## 数据结构

- Segment数组：key/value数组，key存放hash值，value存放指向“分段”的指针，分段类似hashtable
- 分段：类似hashtable，是一个HashEntry的数组
- 分段元素HashEntry：内部包含hash值、key、value、next。单项链表



</br>

## 初始化

- 第一步初始Segment数组：构造函数中的参数concurrencyLevel用来初始化数组。在初始化时创建了两个中间变量ssize和sshift，它们都是通过concurrencyLevel计算得到的。

1. ssize表示了segments数组的长度，在初始化时通过循环计算出一个大于或等于concurrencyLevel的最小的2的N次方值来作为数组的长度。
2. sshift表示了计算ssize时进行移位操作的次数



- 第二步初始化分段：构造函数中的initialCapacity和loadFactor用来初始化分段。前者规定了每个分段的初始化长度，后者规定了加载因子。通过分段数组长度*loadFactor获得每个Segment的阈值threshold.



默认情况下，initialCapacity等于16，loadFactor等于0.75，concurrencyLevel等于16.



</br>

</br>



# JDK1.8版本原理

## 简介

通过分段锁机制其最大并发度受Segment的个数限制。因此，在JDK1.8中选择了与HashMap类似的数组+链表+红黑树的方式实现，而加锁则采用CAS和synchronized实现。



</br>

## 数据结构

- 数组：一个table数组，其类型是一个Node数组
- 链表：Node是一个继承自Map.Entry<K, V>的链表，果元素所在的红黑树节点个数小于6，则会触发红黑树向链表结构转换。
- 红黑树：链表结构中的数据大于8，则将数据结构升级为TreeBin类型的红黑树结构



</br>

## 初始化

- 通过初始容量initialCapacity，加载因子loadFactor和预估并发度concurrencyLevel三个参数计算table数组的初始大小sizeCtl的值
- hash表的初始化是在插入第一个元素时进行



</br>

## 其它说明

- Hashtable对get,put,remove都使用了同步操作
- ConcurrentHashMap只对put,remove操作使用了同步操作
- Hashtable在使用iterator遍历的时候，如果其他线程，包括本线程对Hashtable进行了put，remove等更新操作的话，就会抛出ConcurrentModificationException异常，但如果使用ConcurrentHashMap的话，就不用考虑这方面的问题了



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


