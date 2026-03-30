---
title: java对象的深浅复制
category: java基础
date: 2022-06-08 21:20:01
updated: 2022-06-08 21:20:01
enname: java-copy
categories: java基础
tags:
	- 面试
keywords: java深浅复制
permalink:
thumbnail:
---

# 基本概念

浅拷贝：假设对象内部除基本数据类型外还引用有外部对象实例，则浅拷贝复制对象本身及外部应用对象的引用（并非对象）<!--more-->

深拷贝：假设对象内部除基本数据类型外还引用有外部对象实例，则深拷贝复制对象本身及外部引用对象

</br>

</br>

# 如何实现深浅拷贝

### 实现浅拷贝

clone()方法是object对象中提供的方法，java中任何引用类型对象均具备clone()方法。在不重写clone的情况下使用clone方法就可以实现浅拷贝。



</br>

### 实现深拷贝

- 如果需要实现深拷贝则不仅初始对象需要实现cloneable接口并重写clone方法，内部所引用的对象也需要一并实现cloneable接口并重写clone方法。
- 通过java中的反序列化也可以实现深拷贝



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


