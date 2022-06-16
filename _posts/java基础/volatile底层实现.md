---
title: volatile底层实现
category: java基础
date: 2022-06-16 23:17:52
updated: 2022-06-16 23:17:52
enname: multithread
categories: java基础
tags:
	- 面试
	- 多线程
keywords: volatile
permalink:
thumbnail:
---

# 作用

- Java关键字<!--more-->
- 内存可见性：多线程共享变量，对于每个线程来说都是内存可见的
- 禁止指令重排序：为了提高程序执行的性能，编译器和执行器(处理器)通常会对指令做一些优化(重排序)



</br>

# 什么是重排序

本质上是为了提高程序的执行效率

重排序可以分为：

1、编译器重排序。编译器在不改变单线程程序语义的前提下，可以重新安排语句的执行顺序；

2、处理器重排序。如果不存在数据依赖性，处理器可以改变语句对应机器指令的执行顺序；



</br>

# java中的重排序

happen-before原则保证了程序的“有序性”，它规定如果两个操作的执行顺序无法从happens-before原则中推到出来，那么他们就不能保证有序性，可以随意进行重排序。（在不影响单线程运行结果的前提下java会进行重排序）

多线程下，重排序会造成指令执行顺序的不可控，影响到多线程执行的正确性。

Java提供给我们禁止重排序能力的操作——就是volatile。



</br>

# 禁止重排序的原理

- volatile变量在字节码级别没有任何区别，在汇编级别使用了lock指令前缀。
- lock后就是一个原子操作。原子操作是指不会被线程调度机制打断的操作；这种操作一旦开始，就一直运行到结束，中间不会有任何 context switch （切换到另一个线程）。
- 当使用 LOCK 指令前缀时，它会使 CPU 宣告一个 LOCK# 信号，这样就能确保在多处理器系统或多线程竞争的环境下互斥地使用这个内存地址。当指令执行完毕，这个锁定动作也就会消失。
- volatile底层使用多核处理器实现的lock指令，更底层，消耗代价更小



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


