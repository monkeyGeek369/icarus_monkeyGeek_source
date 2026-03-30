---
title: StampedLock原理
category: java基础
date: 2022-06-19 12:33:47
updated: 2022-06-19 12:33:47
enname: stampedlock
categories: java基础
tags:
	- 面试
	- 锁
keywords: StampedLock
permalink:
thumbnail:
---

# 简介

ReentrantReadWriteLock中的读--写锁时互斥的，在读的时候都会悲观的去加锁，防止同时刻的写操作，如果读操作数量远远大于写操作则会造成写饥饿。<!--more-->

不幸的是大多数情况下读操作都远远大于写操作，因此StampedLock登场。

</br>

# 原理

- StampedLock有两种锁，一种是悲观锁，另外一种是乐观锁。
- 如果线程拿到乐观锁就读和写不互斥，每次去拿数据的时候，都没锁上，而是判断标记位是否有被修改，如果有修改就再去读一次。
- 如果拿到悲观锁就读和写互斥。



StampedLockd的内部实现是基于CLH锁的

CLH锁原理：锁维护着一个等待线程队列，所有申请锁且失败的线程都记录在队列。一个节点代表一个线程，保存着一个标记位locked,用以判断当前线程是否已经释放锁。当一个线程试图获取锁时，从队列尾节点作为前序节点，循环判断所有的前序节点是否已经成功释放锁。



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


