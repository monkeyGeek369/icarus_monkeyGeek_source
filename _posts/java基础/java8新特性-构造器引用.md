---
title: java8新特性-构造器引用
category: java基础
date: 2020-04-15 20:34:53
updated: 2020-04-15 20:34:53
enname: constructor
categories: java基础
tags:
	- 面试
keywords: java,java8新特性,构造器引用
permalink:
thumbnail:
---

# 作用

相较于lambda表达式,通过构造器引用的方式使程序代码更加易读\自然,是进一步简化lambda表达式的声明的一种语法糖.(与方法应用相同)

<!--more-->

</br>

# 用法

lambda表达式如下:

```java
Supplier<Apple> c1 = () -> new Apple();
```

方法引用如下:

```java
Supplier<Apple> c1 = Apple::new;
```



</br>

# 适用条件

- 适用于构造方法
- 方法对应的参数必须是函数式编程接口
- 通过::来分割类名和方法名



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


