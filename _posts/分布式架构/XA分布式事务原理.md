---
title: XA分布式事务原理
category: 分布式架构
date: 2019-11-26 20:20:00
updated: 2019-11-26 20:20:00
enname: xaTransaction
categories: 分布式架构
tags:
	- 分布式
	- 数据一致性
	- 事务
	- XA规范
	- 面试
keywords: 分布式,数据一致性,XA规范
permalink:
thumbnail: ../../image/xatransaction.png
---

# XA简介

XA是由X/Open组织提出的分布式事务的规范。 XA规范主要定义了(全局)事务管理器(TM)和(局 部)资源管理器(RM)之间的接口。主流的关系型数据库产品都是实现了XA接口的。XA规范可以说是分布式事务理论的开拓者,现如今的分布式事务框架在实现理论上多少都有XA的身影.

<!--more-->

</br>

# 详细介绍

![](../../../../image/xatransaction.png)

- XA接口是双向的系统接口，在事务管理器 （TM）以及一个或多个资源管理器（RM）之 间形成通信桥梁。
- 由全局事务管理器管理和协调的事务，可以跨 越多个资源（如数据库或JMS队列）和进程。 全局事务管理器一般使用 XA 二阶段提交协议 与数据库进行交互。
- 资源管理器（resource manager）：用来管理系统资源，是通向事务资源的途径。数据库就是一种资源管理器。资源管理还应该具有管理事务提交或回滚的能力。
- 事务管理器（transaction manager）：事务管理器是分布式事务的核心管理者。事务管理器与每个资源管理器（resource manager）进行通信，协调并完成事务的处理。事务的各个分支由唯一命名进行标识

![](../../../../image/XA两段提交.png)

- 第一阶段为 准备（prepare）阶段。即所有的参与者准备执行事务并锁住需要的资源。参与者ready时，向transaction manager报告已准备就绪。
- 第二阶段为提交阶段（commit）。当transaction manager确认所有参与者都ready后，向所有参与者发送commit命令。

</br>

# XA性能局限性

效率低下，准备阶段的成本持久，全局事务状态的成本持久，性能与本地事务相差10倍左右；
提交前，出现故障难以恢复和隔离问题。

</br>

借鉴资源如下:

https://blog.csdn.net/wuzhiwei549/article/details/79925618

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


