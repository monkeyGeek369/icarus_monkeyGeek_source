---
title: java的垃圾回收器
category: java基础
date: 2021-08-31 20:38:28
updated: 2021-08-31 20:38:28
enname: garbage-collection
categories: java基础
tags:
	- 面试
	- jvm
keywords: java、垃圾回收、面试
permalink:
thumbnail:
---

垃圾回收器的实现，跟厂商有很大关系。这里主要讨论基于JDK1.7 Update14之后的HotSpot虚拟机。<!--more-->新生代收集器使用的收集器：Serial、PraNew、Parallel Scaveng；老年代收集器使用的收集器：Serial Old、Parallel Old、CMS；以及G1收集器。

![](../../../../image/garbage.png)



</br>

# 串型回收器

Serial收集器（复制算法)

```
适用于新生代
单线程回收器
会产生STW(stop the world)
适用于几兆到几十兆内存回收
```



Serial Old收集器(标记-整理算法)

    适用于年老代
    其它Serial相同

一般Serial+Serial Old可以配合使用



</br>

# 并行回收器

<u>并行的含义是GC线程与业务线程可以切换执行，会产生STW</u>



Parallel Scavenge收集器(复制算法)

    适用于新生代
    多线程收集器：它追求的是达到可控的吞吐量。即吞吐量=运行用户代码时间/(运行用户代码时间+GC线程时间)
    会产生STW(stop the world)
    适用于几十兆到1G内存
    
    参数解析：
    1）Parallel Scavenge收集器提供了两个参数用于精确控制吞吐量，分别是控制最大垃圾收集停顿时间的-XX:MaxGcPauseMillis，以及直接设置吞吐量大小的-XX:GCTimeRatio。
    
    -XX:MaxGcPauseMillis参数允许一个大于0的毫秒数，收集器尽可能保证内存回收时间不超过设定值。但是这个时间的缩短是以牺牲吞吐量和新生代空间来换取的，过度缩小会导致垃圾收集更频繁，停顿时间减少，但吞吐量也会降低。
    
    -XX:GCTimeRatio参数的值应该是一个大于0小于100的整数，也就是运行用户代码时间和垃圾收集时间的比例。比如设置为19，表示运行用户代码时间：GC时间=19，所以吞吐量是19/(19+1)。参数默认值是99，也就是99%的吞吐量。
    
    2）收集器还有 “GC自适应调节策略”的开关参数-XX:+UseAdaptiveSizePolicy。参数打开后，不需要指定新生代大小(-Xmn)、Eden与Survivor比例(-XX:SurvivorRatio)、晋升老年代的对象大小(-XX:PretenureSizeThreshold)等细节参数，虚拟机会根据当前系统的允许情况、收集性能监控信息，动态调整这些参数以提供最合适的停顿时间或者最大吞吐量。



Parallel Old收集器(标记-整理算法)

    适用于年老代
    算法不同，其它与Parallel Scavenge相同

JDK 1.8默认的垃圾回收组合就是PS+PO



</br>

# 并发回收器

<u>并发（concurrent）的含义：GC线程和业务线程可以同时运行，不存在STW</u>



ParNew收集器(复制算法)　

    适用于新生代
    多线程收集器
    本质上是Parallel Scavenge的增强，为的是更好的与CMS收集器配合使用



CMS收集器（标记-清除算法）

    以获取最短回收停顿时间为目标的收集器

1）初始标记：用于标记GC Roots，速度很快，为了确保准确性需要“Stop the World”；

2）并发标记：根据GC Roots找到所有可达对象并标记，不需要“Stop the World”；

3）重新标记；针对新生代对象+GC Roots+被标记为“脏”区的对象进行扫描再标记。需要“Stop the World”；

4）并发清除

```
三色标记算法
目的：

```



</br>







4）并发清除：不需要“Stop the World”。

    由于整个过程中耗时最长的并发标记和并发清除过程，收集器线程都可以与用户线程以前工作，所以，总体来说，CMS收集器的内存回收过程是与用户线程以前并发执行的。

   CMS收集器的缺点：

1）CMS对CPU资源非常敏感：

    在并发阶段，CMS虽然不会导致用户线程停顿，但是还会因为占用了部分CPU资源导致应用程序变慢，总吞吐量降低。CMS默认启动的线程数是(CPU线程数+3)/4。

2）CMS无法处理浮动垃圾，可能出现“Concurrent Mode Failure”而导致另一次Full GC：

    浮动垃圾，指的是并发清理阶段产生的垃圾。因为并发清理阶段用户程序也在运行，产生的垃圾在标记过程之后，所以本次清理过程不会被清理，并且CMS还必须预留一部分空间提供给并发收集时的程序运作使用。要是CMS运行期间预留的内存无法满足程序需要，就会触发“Concurrent Mode Failure”，虚拟机将启动后备预案，临时启用Serial Old收集器，但停顿时间会明显增加。
    
    CMS提供了参数-XXCMSInitiatingOccupancyFraction来控制触发CMS的内存使用占比，设置太低会导致CMS触发过于频繁，设置太高则很容易出现大量的“Concurrent Mode Failure”。

3）基于标记-清除算法，收集结束会产生大量空间碎片：

    为解决这个问题，CMS提供了-XX:+UseCMSCompactAtFullCollection开关参数，默认打开。这个参数用于在CMS快要进行FullGC时开启内存碎片的合并整理过程，内存的整理过程无法并发，虽然减少了空间碎片，但是也增加了停顿时间。
    
    CMS还提供了参数-XX:CMSFullGCsBeforeCompaction，这个参数用于设置执行多少次不压缩的FullGC后，执行一次带碎片整理的FullGC。默认是0，表示每次Full GC都需要进行碎片整理。

7、G1收集器

    面向服务端应用的收集器
    一款并行与并发收集器
    能建立可预测的停顿时间模型
    不需要和其他收集器配合
    G1将内存分成多个大小相等的独立区域，虽然还保留着新生代和老年代的概念，但是新生代和老年代不再是物理隔离的，它们都是一部分Region(不需要连续)的集合。

G1回收器优点：

1）空间整合：

    G1从整体看是基于标记-整理算法实现的收集器，从局部看是基于复制算法。这两种算法都意味着G1运行期间不会产生大量内存空间碎片。

2）可预测的停顿：

    降低停顿时间是G1和CMS共同关注的，但G1能建立可预测的停顿时间模型，让使用者明确指定在一个长度为M毫秒的时间片段内，GC的时间不得超过N毫秒。

3）有计划的垃圾回收：

    G1可以有计划的在Java堆中进行全区域的垃圾收集。G1跟踪各个Region里面的垃圾堆的价值大小(回收所获得的空间大小，以及回收所需要的时间)，在后台维护一个优先列表，每次根据允许的收集时间，优先回收价值最大的Region。这就是Garbage-First的由来。


</br>

参考资料：

[1]https://blog.csdn.net/weixin_38569499/article/details/85645867

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


