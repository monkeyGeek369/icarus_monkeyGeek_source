---
title: spring中AOP的实现原理
category: spring
date: 2022-06-12 15:48:49
updated: 2022-06-12 15:48:49
enname: aop
categories: spring
tags:
	- 面试
	- spring
keywords: aop
permalink:
thumbnail:
---

# 什么是AOP

面向切面编程，一种编程思想，是srping的核心之一。<!--more-->

本质是由 AOP 框架修改业务组件的源代码，为系统中的业务组件添加某种通用功能。

按照 AOP 框架修改源代码的时机，可以将其分为两类：

- 静态 AOP 实现：AOP 框架在编译阶段对程序源代码进行修改，生成了静态的 AOP 代理类，比如 AspectJ。
- 动态 AOP 实现：AOP 框架在运行阶段对动态生成代理对（借助动态代理实现），如 SpringAOP。

</br>

# AOP术语

- 通知/增强（Advice）: AOP 框架中的增强处理。描述切面何时执行以及如何执行。

1. 前置增强
2. 环绕增强
3. 异常增强
4. 返回值增强

- <u>连接点（join point）:增强执行的位置。在 Spring AOP 中，连接点总是方法的调用。</u>
- <u>切点（PointCut）: 具体的连接点。</u>
- <u>切面（Aspect）: 切面是增强和切点的结合。</u>
- 引入（Introduction）：特殊增强，向现有的类添加新的方法或者属性。
- 织入（Weaving）: 将增强添加到目标对象中，并创建一个被增强的对象。织入分为如下三个时期：

1. 编译期：AspectJ
2. 类加载
3. 运行期：spring的动态代理



</br>

# Spring如何选择是用JDK还是cglib？

1、当bean实现接口时，会用JDK代理模式

2、当bean没有实现接口，会用cglib实现

</br>





</br>

参考资料

https://www.cnblogs.com/joy99/p/10941543.html



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


