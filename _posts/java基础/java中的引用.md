---
title: java中的引用
category: java基础
date: 2022-06-07 22:41:24
updated: 2022-06-07 22:41:24
enname: reference
categories: java基础
tags:
	- 面试
keywords: java、强引用、软引用、弱引用
permalink:
thumbnail:
---

Java中的引用，有点像C++的指针。通过引用，可以对堆中的对象进行操作。在某函数中，当创建了一个对象，该对象被分配在堆中，通过这个对象的引用才能对这个对象进行操作。<!--more-->

</br>

```java
String str=new String("abc"); // 强引用（abc被存放到堆中，str存放到栈中）
SoftReference<String> softRef=new SoftReference<String>(str); // 软引用
WeakReference<String> abcWeakRef = new WeakReference<String>(str);//弱引用
```

</br>

- 强引用
    - 对象默认的引用状态，任何时候都不会被JVM回收
    - 显式地设置str为null，或超出对象的生命周期范围，这时就可以回收这个对象
- 软引用
    - 内存不足即回收
    - 软引用可用来实现内存敏感的高速缓存
    - 可以设置一个引用队列，在对象被回收时放入引用队列
- 弱引用
    - 发现即回收
    - 可以用作高速缓存
    - 可以设置一个引用队列，在对象被回收时放入引用队列
- 虚引用
    - 不会对垃圾回收产生影响
    - 对象回收跟踪，唯一的目的是在对象被回收时收到一个系统通知
    - 可以设置一个引用队列，在对象被回收时放入引用队列
- 终结器引用
    - 首次gc会执行对象的finally方法并放入引用队列，第二次gc时进行回收

</br>

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


