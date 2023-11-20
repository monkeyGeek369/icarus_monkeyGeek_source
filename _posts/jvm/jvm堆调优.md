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

1. jpa：列出所有java进程，命令jps
2. jinfo：列出指定进程id的详细信息，命令jinfo 1180
3. jstat：内存区域占用情况，命令jstat -gc 1180
4. jstack:列出指定进程的所有线程，命令jstack 1180
5. top:linux命令，列出所有线程，命令top
6. jmap:进程内对象占比分析，命令jmap -histo 1180，或者实现dump功能
7. arthas：系统的jvm分析工具（仪表板/修改代码/反编译/trace跟踪）



# -XX常用参数设置

jvm调优本质上就是调-XX参数







### 内存设置

- 整个JVM内存大小=堆内存+ 非堆内存<!--more-->
- 堆（heap）：是Java代码可用的内存，是留给开发人员使用的；
- 非堆（non-heap）：是JVM留给自己用的，所以方法区、JVM内部处理或优化所需的内存(如JIT编译后的代码缓存)、每个类结构(如运行时常数池、字段和方法数据)以及方法和构造方法的代码都在非堆内存中。
- JDK1.8之前:堆内存=年轻代+年老代+持久代.JDK18之后：堆内存=年轻代+年老代

</br>

### 堆内存分配

堆的大小一般不超过现有空闲内存的80%

- -Xmx：yound代加上old代总和的初始最大值
- -Xms：yound代加上old代总和的最小值
- -XX:NewSize=<n>[g|m|k]  设置年轻代的初始值和最小值（NewSize与MaxNewSize必须同时设置）
- -XX:MaxNewSize=<n>[g|m|k]  设置年轻代的最大值（NewSize与MaxNewSize必须同时设置）
- -Xmn<n>[g|m|k] 固定年轻代始终为该大小，Sun官方推荐配置为整个堆的3/8
- old代需要借助yound来设置（总量-yound）

</br>

### 非堆内存分配

一般不去设置持久代而是通过JVM自动调整

- -XX:PermSize=<n>[g|m|k] 设置permanent代的初始值和最小值
- -XX:MaxPermSize=<n>[g|m|k] 设置permanent代的最大值

</br>

### 其它辅助设置

- -XX:NewRatio=4:设置年轻代与年老代的比值。设置为4，则年轻代与年老代所占比值为1：4，年轻代占整个堆栈的1/5
- -XX:SurvivorRatio=4：设置年轻代中Eden区与Survivor区的大小比值。设置为4，则两个Survivor区与一个Eden区的比值为2:4，一个Survivor区占整个年轻代的1/6
- -XX:MaxTenuringThreshold=0：设置垃圾最大年龄。如果设置为0的话，则年轻代对象不经过Survivor区，直接进入年老代。对于年老代比较多的应用，可以提高效率。如果将此值设置为一个较大值，则年轻代对象会在Survivor区进行多次复制，这样可以增加对象再年轻代的存活时间，增加在年轻代即被回收的概率。

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


