---
title: ReentrantReadWriteLock
category: java基础
date: 2022-06-19 12:25:13
updated: 2022-06-19 12:25:13
enname: reent-rw
categories: java基础
tags:
	- 面试
	- 锁
keywords: ReentrantReadWriteLock
permalink:
thumbnail:
---

# 简介

多线程环境下对资源的读操作和写操作会造成数据的不一致，因此读和写需要加锁，ReadWriteLock诞生。<!--more-->

ReentrantReadWriteLock是可重入读写锁，意味着已经获取到资源的线程可以重复的获取锁，获取次数+1，相应的也要释放对应的次数。

</br>

# 资源分配方式

- 公平性选择：默认的方式，按照阻塞队列的先后顺序将资源分配给对应的线程
- 非公平性选择：资源的分配与阻塞队列的先后顺序无关，很可能出现一个线程多次长期占用某资源，从而造成死锁。

</br>

# 特点

- "读-读" 不互斥
- "读-写" 互斥
- "写-写" 互斥
- 读锁是可以被多线程共享的，写锁是单线程独占的。也就是说写锁的并发限制比读锁高，这可能就是升级/降级名称的来源。

</br>

# 触发条件

- 读操作：可以有多个读操作，但是读操作与写操作互斥，因此读操作的前提是阻塞队列中不存在写操作及读操作时对一切写操作加锁禁止写操作
- 写操作：写操作与写操作互斥，因此同一时刻只允许一个写操作对资源的占用



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


