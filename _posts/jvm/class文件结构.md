---
title: class文件结构
category: jvm
date: 2023-11-20 00:06:57
updated: 2023-11-20 00:06:57
enname: jvmClass
categories: jvm
tags:
	- 面试
	- jvm
keywords: jvm，class文件结构
permalink:
thumbnail:
---

# 基本概念
* 前端编译器：将符合规范的java代码编译为符合jvm规范的字节码文件。
    * 默认如javac前端编译器
    * 只进行全量编译，不进行编译优化
* 即时编译器：如JIT负责编译优化，在代码的执行过程中发现重复执行的代码段时会被编译成效率更高的机器指令
* 字节码文件：经过前端编译器编译后的二进制文件，包含字节码指令即jvm指令
* 字节码指令：由一个字节的操作码+0个/多个操作数的jvm指令

<!--more-->

# 分析工具

用于反编译分析class二进制文件的组成内容
作用：通过分析字节码文件的指令来更好的理解代码执行过程，排查问题

- javap命令：javap -v class文件路径
- JClassLib插件：idea插件，将字节码文件翻译为字节码指令
- binary viewer：桌面工具，解读字节码文件


# 组成结构
* 魔数：magic number用于标识字节码文件开头，主要作用是安全验证
    * u4：采用无符号4字节表示
    * ca fe ba be
* 字节码文件版本号
    * u4大小，前两个字节存储小版本，后两个字节存储大版本
    * 不同的版本号对应不同的前端编译器，代表当前字节码文件的编译方式
* 常量池：class文件的基石
    * 常量池计数器：u2，用于记录常量池长度，实际长度=计数器数量-1
    * 常量池表：n字节，从1开始，存放各种字面量和符号引用。类加载后这部分内容会放入运行时常量池
* 


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


