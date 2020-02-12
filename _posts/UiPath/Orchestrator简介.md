---
title: Orchestrator简介
category: UiPath
date: 2020-02-12 11:26:59
updated: 2020-02-12 11:26:59
enname: orchestrator
categories: UiPath
tags: UiPath
keywords: UiPath,orchestrator
permalink:
thumbnail:
---

# 简介

Orchestrator首先是一个web应用程序，它的作用是以面板方式集中管理机器人集群。能够对机器人的创建、监控、部署、运行等全流程管控<!--more-->，并且能够同任何第三方应用程序做集成。

</br>

# 项目架构

![](../../../../image/orchestrator_logical.jpg)

单个Orchestrator支持1000无人值守机器人或者10000有人值守机器人的监管，并且Orchestrator支持多节点部署，相同集群下可共同使用相关ES或SQL数据库。

- 表现层：web应用、Orchestrator Data Rest API、提示API
- 服务层：REST API 实现
- 持久层：SQL server、Elasticsearch



</br>

# 功能简介

### 适配用例

Orchestrator能够管理的用例类型如下：

- 无人值守机器人-不限线程数量不需真实用户环境可在虚拟环境运行
- 有人值守机器人-单线程且需真实用户环境
- studio/studioX-拥有有人值守机器人功能并可链接Orchestrator以实现部署功能
- 非产品-既在测试环境下的RPA也可以与Orchestrator链接以实现部署和测试



### 主要功能

- 指配（provisioning）：创建和维护机器人与web之间的连接
- 部署（deployment）：能够将不同版本的Robot发布包交付指定机器人来执行
- 配置（configuration）：维护和传递Robot环境和流程配置
- 队列（queues）：确保分布式自动化机器人负载均衡
- 监控（monitoring）：对机器人运行期间的数据和行为进行监控
- 日志（logging）：能够将日志数据存储至SQL数据库或ES库
- 互联互通（Inter-connectivity）:能够同第三方应用实现互联互通



### 操作界面

![](../../../../image/orchestrator_interface.png)

- 1、仪表盘Dashboard
- 2、用户菜单UserMenu：包括我的资料、系统设置、声明、webhooks（为第三方应用提供robot执行事件的监听接口）、Credential（证书管理）、审计追踪（不同用户的操作行为记录）
- 3、系统提示Alerts：重要历史事件的提示
- 4、系统帮助Help
- 5、Folder（文件夹）选择：Folder用于容纳Robots、监视器、环境、流程、jobs、tasks、queues、assets、Triggers等，可针对不同用户和角色进行分配可操作权限，这里提供选择功能。
- 6、机器人Robot：全局和folder范围内的机器人列表
- 7、Jobs调度任务：机器人所对应的可执行流程processes
- 8、队列Queues：总览队列和事务状态，队列是一种容器用于无限制容纳对象，队列对象可以存储多种数据类型，例如发票信息和用户信息。这些信息需要被process消费，既队列对象的消费会产生process
- 9、logs：平台监控日志信息
- 10、processes：Folder视图内Robot所对应的不同版本package信息
- 11、触发器（Triggers）：针对job执行的时间或事件触发器
- 12、队列和事务：队列信息以及队列所对应的事务信息
- 13、资产Assets：资产用于存储和分享变量以及证书，这些数据可以应用在不同的自动化项目中。
- 14、任务（Tasks）：为长链路流程人工制定干预作业任务task
- 15、Folder（文件夹）：Folder用于容纳Robots、监视器、环境、流程、jobs、tasks、queues、assets、Triggers等，可针对不同用户和角色进行分配可操作权限。
- 16、用户和角色user and roles：用于维护平台内的用户和角色
- 17、机器人和环境：机器人以及环境列表
- 18、机器Machines：容纳Robot的服务器列表
- 19、包/包库：包的上传、发布、部署和分组组织，可分享和重用
- 20、任务类型 Task Catalogs：可定义不同任务类型在创建任务时供选择
- 21、语言切换Language Selector:支持包括中英文在内的12种语言

</br>

# 关于安装

想要使用Orchestrator必须购买，社区版只提供studio编辑工具。安装类型如下：

- 在windows服务器独立安装-包含service、client、SQL、ES等
- 通过Azure服务器安装脚本安装
- 通过UiPath平台安装





</br>

参考链接

[1].https://docs.uipath.com/orchestrator/v2020.4/docs

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


