---
title: spring中IOC实现原理
category: spring
date: 2022-06-12 15:13:14
updated: 2022-06-12 15:13:14
enname: ioc
categories: spring
tags:
	- 面试
	- spring
keywords: ioc
permalink:
thumbnail:
---

# **简介**

通过spring我们无需手动创建对象，对象的创建与生命周期管理交由spring实现<!--more-->，当我们需要使用对象实例时只需要通过spring获取即可。spring利用IOC（控制反转）实现对Bean的管理。

- 简单来说IOC就是把对象创建和对象之间的调用过程交给Spring框架进行管理，调用方只负责使用。
- 目的：降低耦合度
- IOC是spring中的核心

</br>

# 底层原理

- xml配置/注解：定义bean名称、属性等，为后面的反射获取bean打下基础
- 反射：通过解析xml或者扫描注解来获取bean注册信息，根据反射来获取bean对象
- 工厂模式：bean创建由工厂完成，提供getbean方法，spring容器中借助BeanFactory完成



**<u>IOC底层就是对象工厂</u>**，Spring框架中对IOC容器实现提供了两种方式（即两种接口）

- BeanFactory：IOC容器的基本实现，容器加载时不会创建对象，使用时才会创建。不对开发人员开放
- ApplicationContext：是BeanFactory的子接口，对开发人员开放，此接口在加载配置文件时就会创建配置文件中所配置的对象

</br>

# 加载逻辑

- 第一步-bean配置信息：xml、java类、注解等
- 第二步：读取bean配置信息到spring容器中的注册表
- 第三步：根据注册表利用反射+工厂模式实例化bean
- 第四步：将实例化bean放入缓存池（HashMap实现）
- 第五步：应用程序获取bean

当容器关闭时，销毁（需要配置）

</br>

# 其它内容

- IOC与DI是什么关系？同一概念的不同角度描述。对象的控制交由容器完成这是IOC，调用方需要的依赖由容器实现注入这是DI（依赖注入）
- DI的实现方式：主要有两种，第一是通过类中定义的set方法进行注入，第二是通过有参构造方法进行注入
- Spring中的bean模式是单实例的
- 如何设置bean多实例：在Spring配置文件bean标签中有属性scope用于设置单实例还是多实例，singleton（单实例） prototype（多实例）。单实例在加载配置文件时就会创建，多实例在调用getBean方法时创建。







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


