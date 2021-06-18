---
title: ThreadLocal详解
category: java基础
date: 2021-05-21 20:12:34
updated: 2021-05-21 20:12:34
enname: threadlocal
categories: java基础
tags:
	- java
	- 线程与进程
keywords: java、ThreadLocal
permalink:
thumbnail:
---

**ThreadLocal是什么**

ThreadLocal是线程本地副本变量工具类。<!--more-->主要用于将私有线程和该线程存放的副本对象做一个映射，各个线程之间的变量互不干扰，在高并发场景下，可以实现无状态的调用，特别适用于各个线程依赖不通的变量值完成操作的场景。





</br>

```java
//在自定义类中创建ThreadLocal对象，EngineContext为我们自定义的Value对象类
//java是多线程的，父类初始化后，内部定义的ThreadLocal对象即在本线程内起效
    private static final ThreadLocal<EngineContext> CONTEXT_HOLDER = new ThreadLocal<>();

    public void initalize(EngineContext context) {
        CONTEXT_HOLDER.set(context);
    }

    public void clean() {
        CONTEXT_HOLDER.remove();
    }

    public EngineContext get() {
        return CONTEXT_HOLDER.get();
    }
```









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


