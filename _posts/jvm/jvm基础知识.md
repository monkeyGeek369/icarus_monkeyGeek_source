---
title: jvm基础知识
category: jvm
date: 2023-12-09 23:26:04
updated: 2023-12-09 23:26:04
enname: jvm-base
categories: jvm
tags:
	- 面试
	- jvm
keywords: jvm，基础知识
permalink:
thumbnail:
---

# 整体结构

<!--more-->

- class文件
- 类加载器
- 运行时数据区
  - 线程不共享
    - 虚拟机栈
    - 本地方法栈
    - 程序计数器
  - 线程共享
    - 方法区（永久代/metaspace）
    - 堆
      - 年轻代：eden/survive
      - 年老代
- 执行引擎
  - 解释器
  - JIT即时编译器
  - 垃圾回收器
- 本地方法接口
- 本地方法库

</br></br></br>

# 架构模型

- 基于栈的指令集架构
  - 设计、实现简单
  - 指令集小，执行指令多，编译器容易实现
  - 可移植性好，更容易垮平台
- 基于寄存器的指令集架构
  - 指令集依赖硬件，可移植性差
  - 性能优秀、执行效率高
  - 使用更少的指令完成一项操作

</br></br></br>

# 发展历程

- sun classic vm：第一款虚拟机，只提供解释器功能，目前hotspot中已内置
- exact vm：准确式内存管理、热点探测、编译器/解释器混合模式
- hotspot vm：jdk默认虚拟机、热点代码探测技术、编译器/解释器混合模式
- jrockit vm：专注于服务端应用、只有即时编译器（不关注启动时间）
- J9 vm：应用于桌面、服务、嵌入式，主要面向IBM内部产品中使用
- CDC/CLDC:oracle在java me产品线开发的虚拟机
- KVM：在更低端设备有一定应用、遥控器、功能机等
- azul vm：与特定硬件/软件环境绑定的虚拟机性能更高
- liquid vm：高性能虚拟机、与特定硬件/软件环境绑定的虚拟机性能更高
- apache harmony：类库代码被吸纳进了安卓jdk中
- Microsoft jvm：为了在ie中更好的支持java小程序而开发
- taobao jvm：基于open jdk hotspot深度定制的虚拟机，off-heap技术（即大对象移到堆外减少gc），严重依赖intel的cpu
- dalvik vm：谷歌开发，用于安卓系统，不是java虚拟机，执行dex文件，基于寄存器架构模型
- graal vm：垮语言全栈虚拟机，大体原理是各个语言代码编译成graal可以接受的中间代码，有graal解释器解释执行





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


