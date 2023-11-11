---
title: java常见面试问题
category: java基础
date: 2023-11-11 21:29:48
updated: 2023-11-11 21:29:48
enname: java_question
categories: java基础
tags:
	- 面试
keywords: java,面试，问题
permalink:
thumbnail:
---

java常见且经典面试问题收集

<!--more-->

1、object o = new object();的o占中多少字节

如何查看对象头内存分配：通过idea插件JOL来查看

对象头组成：markword（8字节）、类型指针（默认不开启压缩4字节，开启后8字节）、实例数据（n字节）、对齐（保证对象头可以被8字节整除的补齐字节数）



答案：开启压缩16字节（8+4+0+4），开启压缩16字节（8+8+0+0）





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


