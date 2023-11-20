---
title: GC回收与算法
category: jvm
date: 2022-06-18 15:58:01
updated: 2022-06-18 15:58:01
enname: gc
categories: jvm
tags:
	- 面试
	- jvm
keywords: GC回收、GC回收算法、垃圾回收
permalink:
thumbnail:
---

# 如何发现垃圾

- 引用计数算法：缺点是无法解决循环引用问题
- 可达性分析算法：通过gc root开始，所有不可达的将被回收
  - GC Root有哪些？栈变量、常量池、静态变量



# 什么时候发生GC

**Minor GC（新生代GC）**：当jvm无法为新的对象分配空间的时候就会发生Minor gc，所以分配对象的频率越高，也就越容易发生Minor gc<!--more-->

**Full GC/Major GC（年老代GC）**：①当老年代无法分配内存的时候,②当发生Minor GC的时候可能触发Full GC

</br>

</br>

# GC回收算法

- Stop-the-world意味着 JVM由于要执行GC而停止了应用程序的执行，并且这种情形会在任何一种GC算法中发生。
- GC优化很多时候就是指减少Stop-the-world发生的时间

</br>

### 常见的回收算法

- 标记清除算法：对存活对象进行标记，对未存活对象进行回收。标记和清除两个过程效率不高且容易产生空间碎片。
- 复制算法：将可用内存按容量划分为大小相等的两块，每次只使用其中的一块。当这一块的内存用完了，就将还存活着的对象复制到另外一块上面，然后再把已使用过的内存空间一次清理掉。这种算法适用于对象存活率低的场景，比如新生代。

实践中会将新生代内存分为一块较大的Eden空间和两块较小的Survivor空间 ，每次使用Eden和其中一块Survivor。当回收时，将Eden和Survivor中还存活着的对象一次地复制到另外一块Survivor空间上，最后清理掉Eden和刚才用过的Survivor空间。虚拟机默认Eden和Survivor的大小比例是 8:1，也就是每次新生代中可用内存空间为整个新生代容量的90% ( 80%+10% )，只有10% 的内存会被“浪费”。

- 标记整理算法：标记存活对象然后让所有存活的对象都向一端移动，直接清理掉端边界以外的内存。适用于对象存活率高的场景（老年代）
- 分代回收算法：新生代对象存活率低，就采用复制算法；老年代存活率高，就用标记清除算法或者标记整理算法

</br>

</br>

# 对象存活划分

### 新生代（Young Generation）

- 新生代内存按照8:1:1的比例分为一个eden区和两个survivor(survivor0，survivor1)区
- 在进行垃圾回收时，先将eden区存活对象复制到survivor0区，然后清空eden区
- 当这个survivor0区也满了时，则将eden区和survivor0区存活对象复制到survivor1区，然后清空eden和这个survivor0区
- 此时survivor0区是空的，然后交换survivor0区和survivor1区的角色（即下次垃圾回收时会扫描Eden区和survivor1区），即保持survivor0区为空，如此往复。
- 当三个区域都存放满了后，就将存活对象直接存放到老年代
- 如果老年代也满了，就会触发一次FullGC，也就是新生代、老年代都进行回收。

</br>

### 老年代（Old Generation）

在新生代中经历了N次垃圾回收后仍然存活的对象就会被放到老年代中。

老年代的内存也比新生代大很多(大概比例是1:2)

当老年代满时会触发Major GC(Full GC)



</br>

### 永久代（Permanent Generation）

永久代主要用于存放静态文件，如Java类、方法等

永久代对垃圾回收没有显著影响，但是有些应用可能动态生成或者调用一些class，例如使用反射、动态代理、CGLib等bytecode框架时，在这种时候需要设置一个比较大的永久代空间来存放这些运行过程中新增的类。



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


