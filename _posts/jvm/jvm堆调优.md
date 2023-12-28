---
title: jvm堆调优
category: jvm
date: 2022-06-18 14:34:01
updated: 2022-06-18 14:34:01
enname: heap
categories: jvm
tags:
	- 面试
	- jvm
keywords: jvm堆调优
permalink:
thumbnail:
---

# 分析工具

1. Jps：列出所有java进程，命令jps
2. jinfo
   1. 列出指定进程id的详细信息，命令jinfo 1180
   2. 查看进程下的jvm设置参数，例如 jinfo -flag NewRatio 1180
3. jstat：内存区域占用情况，命令jstat -gc 1180
4. jstack:列出指定进程的所有线程，命令jstack 1180
5. top:linux命令，列出所有线程，命令top
6. jmap:进程内对象占比分析，命令jmap -histo 1180，或者实现dump功能
7. arthas：系统的jvm分析工具（仪表板/修改代码/反编译/trace跟踪）
8. java visualVM：jdk自带，可分析本地、远程的jvm信息



# 常用参数设置

- 堆空间常用参数
  - -XX:+PrintFlagsInitial查看所有参数默认初始值
  - -XX:+PrintFlagsFinal查看所有参数的最终值
  - -Xms100m设置堆初始大小（默认1/64）
  - -Xmx100m设置堆最大值（默认1/4）
  - -Xmn10m设置年轻代大小
  - -XX:NewRatio=2设置老年代/年轻代的数值（默认2）
  - -XX:SurvivorRatio=8设置eden/s的数值（默认8）
  - -XX:MaxTenuringThreshold=15设置新生代垃圾的最大年龄
  - -XX:+PrintGCDetails打印gc详细信息
  - -XX:+PrintGC打印gc简略信息
  - -XX:HandlePromotionFailure=true是否设置空间分配担保(minor GC之前jvm会检查老年代最大连续空间是否大于新生代所有对象的总空间，如果大于则可以minor gc，如果小于则看此参数设置。如果为true则看老年代最大连续空间是否大于历次晋升到老年代的对象的平均大小，如果大于则尝试minor gc否则执行进行full gc。如果为false则直接full gc)<u>JDK 7之后此参数已经失效</u>



# 如何解决OOM

1. 首先要确定是内存泄漏还是内存溢出
   1. 内存泄漏：对象即使不使用了但一直存在引用导致无法回收
   2. 内存溢出：对象所需大小超过了堆空间大小
2. 如果是内存泄漏：通过工具找到泄漏对象的引用链
3. 如果是内存溢出：找到存活时间长或者大量的对象，通过优化代码和设置堆大小来解决





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


