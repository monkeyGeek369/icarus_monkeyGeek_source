---
title: jvm内存模型
category: java基础
date: 2022-06-18 15:02:04
updated: 2022-06-18 15:02:04
enname: memory
categories: java基础
tags:
	- 面试
	- jvm
keywords: jvm内存模型
permalink:
thumbnail:
---

# 简介

- 程序计数器，虚拟机栈，本地方法栈是线程私有的<!--more-->
- 堆是线程共有的
- 方法区可以被各个线程共享



</br>

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

- 程序计数器：记录当前线程所执行的字节码的行号
- 本地方法栈： 为Native方法服务(栈的空间大小远远小于堆)
- 虚拟机栈：描述的是***java方法执行的内存模型***

</br>

### 虚拟机栈

- 每个方法被执行的时候都会创建一个"栈帧"
- 每个方法被调用到执行完的过程，就对应着一个栈帧在虚拟机栈中从入栈到出栈的过程
- 栈帧生命周期与线程相同，是线程私有的

</br>

### 栈帧组成

- 局部变量区：以一个字长为单位、从0开始计数的数组。用于存储局部变量表，通过索引来访问
- 操作数栈：以一个字长为单位、从0开始计数的数组。通过入栈和出栈来访问
- 帧数据区：存储数据用来支持常量池解析、正常方法返回以及异常派发机制

</br>

### 局部变量表

- 存放了编译器可知的各种基本数据类型、对象引用(引用指针，并非对象本身)
- 局部变量表所需的内存空间在编译期间完成分配
- 在运行期间\****栈帧***\*不会改变局部变量表的大小空间



</br>

# 直接内存

JVM控制内存之外的本机内存。 

jdk的NIO引入了通道与缓冲区的IO方式，它可以调用Native方法直接分配堆外内存



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


