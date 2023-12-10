---
title: jvm内存模型
category: jvm
date: 2022-06-18 15:02:04
updated: 2022-06-18 15:02:04
enname: memory
categories: jvm
tags:
	- 面试
	- jvm
keywords: jvm内存模型
permalink:
thumbnail:
---

# 简介

运行时数据区整体结构如下：

- 程序计数器<!--more-->
- 虚拟机栈
- 本地方法栈
- 堆
- 方法区（永久代/metaspace）



</br></br></br>

# CPU基础知识

### CPU缓存

- 一级缓存：cup内核有独立一级缓存
- 二级缓存：cup内核有独立二级缓存
- 三级缓存：cup内核共享三级缓存，会存在一致性问题



### 缓存一致性问题解决

- 总线锁：cpu取值时对总线上锁
- 缓存一致性协议MESI协议：modify/exclusive/shared/invalid，简单来讲cpu会对读写的数据进行四种状态的标记，不同的状态处理的方式不同。（无法被缓存的数据或者夸缓存行的数据MESI无法解决，还是要靠总线锁）



### 缓存行

- 目的：读区数据时进行缓存行cache line整行读区（主流一行64字节），提高效率。
- 伪共享问题：同一缓存行内的不同数据被不同cpu使用锁定，当数据被改动后，整个缓存行会被告知修改，不同cpu重新读区，造成互相影响
- 缓存行对齐提效：利用缓存行64字节以及整行读写的特点，利用“padding+真实数据 = 64字节”的方式提升真实数据对象的读写操作



### 乱序问题

- 并行执行：当两条指令没有依赖关系，cpu会并行执行
- 合并写：写操作有可能会放在一起合并操作
- 指令重排序：两条互相不影响的指令有可能被重新排序执行



### 有序性保证

- cpu内存屏障
  - sfence:写屏障，屏障前后的写操作不能重排
  - lfence:读屏障，屏障前后的读操作不能重排
  - mfence:读写屏障，屏障前后的读写操作不能重排
- lock指令



### 并行与并发

- 串行：以垃圾回收为例，串行回收器只有一个垃圾回收线程，执行时用户线程无法执行
- 并行：以垃圾回收为例，并行回收器可以有多个垃圾回收线程，执行时用户线程无法执行
- 并发：以垃圾回收为例，并发回收器可以有多个垃圾回收线程，执行时用户线程可以同时执行。例如采用时间片轮训的方式



</br></br></br>

# 线程共享

### 堆

是java虚拟机所管理的内存中最大的一块内存区域，也是被各个***线程共享***的内存区域，该内存区域***存放了对象实例及数组（但不是所有的对象实例都在堆中）***。

</br>

### 方法区

- 也称"***永久代***"，用于***\*存储虚拟机加载的类信息、常量、静态变量\****
- 默认最小值为16MB，最大值为64MB（64位JVM由于指针膨胀，默认是85M）
- 它是***\*一片连续的堆空间\****，并与堆空间连续
- 垃圾收集是和老年代(old generation)捆绑在一起的，无论谁满了都会一起回收
- 从JDK7***\*开始移除永久代（但并没有移除）\****



</br>

### 运行时常量池

- 是方法区的一部分
- 用于***\*存放编译器生成的各种符号引用\****

</br>

### 元数据区

- JDK8开始使用元空间（Metaspace），方法区存在于元空间
- 元空间不再与堆连续，而是存在于本地内存
- 本地内存（Native memory），也称为C-Heap，是供JVM自身进程使用的。当Java Heap空间不足时会触发GC，但Native memory空间不够却不会触发GC。
- 默认情况下元空间是可以无限使用本地内存的，但是也可以通过参数进行设置



</br>

# 线程私有

### 程序计数器

用来存储指向下一条指令的地址，由执行引擎读取下一条指令执行

- 程序计数器存在的意义？cpu在不停的切换线程执行，因此需要记录下一条可以执行的指令
- 为什么是线程私有的？如果不是私有会存在一致性问题



</br>

### 虚拟机栈

#### 特点

- 线程私有
- 生命周期与线程一致
- 不存在垃圾回收问题



#### 可能出现的异常

栈大小可以是固定的也可以是动态的

- Stackoverflowerror:当栈大小是固定的时候，有可能出现此问题
- Outofmemoryerror:当栈大小是动态的时候，请求增加栈空间有可能报此问题

可以通过-Xss设置栈大小，如-Xss1m



#### 栈的存储结构

栈的基本单位就是栈贞，java方法的执行就是栈贞入栈到出栈的过程，栈贞的具体结构如下

- 局部变量表
- 操作数栈
- 动态链接
- 方法返回地址
- 一些附加信息



#### 局部变量表

- 局部变量表在编译期就确定下来
- 定义为一个数字数组
- 存放方法参数、方法中的局部变量，例如基本数据类型、对象饮用、returnaddress类型等
- 基本存储单位是slot（槽）
- 32位以内类型占一个slot，如byte\short\char\int\boolen
- 64位占两个slot，如long\double



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


