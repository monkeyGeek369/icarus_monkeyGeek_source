---
title: activiti案例运行及资源包简介
category: activiti
date: 2020-06-30 20:23:30
updated: 2020-06-30 20:23:30
enname: activitisource
categories: activiti
tags:
	- activiti
keywords: activiti
permalink: ../../image/activiti3.png
thumbnail:
---

本文将针对activiti官网提供的资源包以及案例程序做详细介绍，以方便初入人员快速了解activiti<!--more-->

</br>



# 官网下载activiti资源包

[https://www.activiti.org](https://www.activiti.org/)

资源包解压后得到如下图结构：

![](../../../../image/activiti1.png)

</br>



# tomcat运行activiti-app.war

activiti-app.war是activiti提供的供本地流程图制作、流程测试的工具软件

![](../../../../image/activiti2.png)

</br>



# activiti使用

![](../../../../image/activiti3.png)

![](../../../../image/activiti4.png)

流程的运行依赖于流程图以及人员、角色组，因此我们首先需要新建用户用于执行流程，以简易的请假流程来作为示范。



### 进入idenity management 选择用户选项卡进行用户新建

![](../../../../image/activiti5.png)

![](../../../../image/activiti6.png)



### 进入APP创建流程process

![](../../../../image/activiti7.png)



### 将流程节点与用户进行绑定

![](../../../../image/activiti8.png)

![](../../../../image/activiti9.png)



### 进入APP并创建APP

![](../../../../image/activiti10.png)



### 指定APP所包含的流程

![](../../../../image/activiti11.png)



### 发布APP

![](../../../../image/activiti12.png)



### 员工账户登录发起流程

![](../../../../image/activiti13.png)

![](../../../../image/activiti14.png)

![](../../../../image/activiti15.png)

![](../../../../image/activiti16.png)



### 管理人员登录系统审批流程

![](../../../../image/activiti17.png)



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


