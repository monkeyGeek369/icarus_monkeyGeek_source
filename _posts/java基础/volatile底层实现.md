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
	- 锁
keywords: volatile
permalink:
thumbnail:
---

# 作用

- Java关键字<!--more-->
- 确保内存可见性、禁止指令重排序



</br>

# 内存可见性

多线程共享变量，对于每个线程来说都是内存可见的

- 当写一个volatile变量时，JMM会把该线程本地内存中的变量强制刷新到主内存中去
- 写会操作会导致其他线程中的volatile变量缓存无效
- 只保证可见性，不保证原子性



</br>

# 指令重排序

### 什么是重排序

为了提高程序执行的性能，编译器和执行器(处理器)通常会对指令做一些优化(重排序)

重排序可以分为：

1、编译器重排序。编译器在不改变单线程程序语义的前提下，可以重新安排语句的执行顺序；

2、处理器重排序。如果不存在数据依赖性，处理器可以改变语句对应机器指令的执行顺序；



happen-before原则保证了程序的“有序性”，它规定如果两个操作的执行顺序无法从happens-before原则中推到出来，那么他们就不能保证有序性，可以随意进行重排序。（在不影响单线程运行结果的前提下java会进行重排序）



</br>

### 不禁止重排序会有什么问题

```java
class ReorderExample {
  int a = 0;
  boolean flag = false;
  public void writer() {
      a = 1;                   //1
      flag = true;             //2
  }
  Public void reader() {
      if (flag) {                //3
          int i =  a * a;        //4
          System.out.println(i);
      }
  }
}
```

以上面代码为例，如果flag没有加上volatile，那么由于指令重排序1和2的顺序可能颠倒，在多线程环境下A负责写入，B负责读取，则读取结果可能是0.



</br>

# 禁止重排序的原理

在JVM底层volatile是采用“内存屏障”来实现的

volatile变量在字节码级别没有任何区别，在汇编级别使用了lock指令前缀（相当于内存屏障）



- 内存屏障之前/之后的代码不会发生位置转换，确保有序执行
- LOCK 指令前缀促使多线程竞争的环境下互斥地使用这个内存地址



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


