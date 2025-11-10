---
title: UiPath-Apps简介
category: UiPath
date: 2020-02-13 10:46:35
updated: 2020-02-13 10:46:35
enname: UiPathApps
categories: UiPath
tags:
	- UiPath
	- RPA
keywords: UiPath,Apps
permalink:
thumbnail: ../../../image/UiPathAppsNotificationsFlowv.jpg
---

Robot可实现自动化业务处理但是如果想要实现更复杂而且更长的流程实现自动化，这些流程需要人类员工处理异常情况并进行验证，这时就需要Uipath Apps进行处理。<!--more-->Uipath Apps使机器人和人类员工能够轻松地无缝协作，在适当的时间将人类员工带入循环协作机制，当机器人需要人类员工帮助处理异常、升级、验证和批准时，它会自动创建一个任务并将其委派给合适的人，人类员工在处理任务的同时机器人可同时进行其它的工作。

UiPath Apps内置强大的访问管理和任务分配权限控制，因此，您可以自己选择一个任务也可以将其委派给团队其它人员。

</br>

# Apps通知功能

Apps的消息通知功能是一个新功能它是基于Orchestrator的Webhooks来实现的，能够实时监控Robot运行。（Webhooks用于Robot的执行事件、消息等的订阅功能，第三方应用或UiPath Apps可以实现订阅）



### 监控内容

- Job（定时任务）失败消息
- Schedule（计划任务）失败消息
- Queue Item Transaction（队列对象的事务）失败消息
- Queue Item Transaction（队列对象的事务）异常终止消息
- Task创建
- Task完成
- Task分配变更



### 运行逻辑

![](../../../../image/UiPathAppsNotificationsFlowv.jpg)

- 1、Orchestrator产生通知信息，webhooks将被触发
- 2、云推送消息服务（Cloud Push Notification Server）将会收到webhooks推送的Post请求事件（相同事件类型下若具备多个通知消息将会归纳为同一组进行发送）
- 3、利用Firebase云消息通讯服务将归类好的消息发送至安卓移动端应用，利用Apple推送服务将归类好的消息发送至IOS移动端应用
- 4、移动应用端的操作记录以及消息接受记录将体现在Orchestrator中



</br>

# 标星流程（Favorite Processes）

为了更快发现和运行流程我们可以在Apps中对流程标记星星icon。所有的标星标记信息均存储在移动端本地，如果重装应用将失去此类信息。

![](../../../../image/UiPathAppsFavoriteStart.png)

Apps可以提供如下功能操作：

- 标记流程

- 将标记流程移除星标
- 重排序标星流程
- 运行标星流程



</br>

# 标星Jobs（Favorite Jobs）

为了更快发现和运行job我们可以在Apps中对流程标记星星icon。所有的标星标记信息均存储在移动端本地，如果重装应用将失去此类信息。

![](../../../../image/UiPathAppsQuickActionsJobs.png)

Apps可以提供如下功能操作：

- 标记job
- 编辑job
- 移除标记
- 重排序标记job
- 运行标记job



</br>

参考链接

[1].https://www.bilibili.com/video/av88197715

[2].https://docs.uipath.com/orchestrator/docs/about-push-notifications

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


