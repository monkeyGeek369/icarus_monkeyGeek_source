---
title: activiti实现流程回退
category: activiti
date: 2020-06-30 19:45:12
updated: 2020-06-30 19:45:12
enname: activitiback
categories: activiti
tags:
	- activiti
	- 面试
keywords: activiti,activiti流程回退
permalink:
thumbnail:
---

activiti流程回退功能在原始api中并没有提供，需要我们自行实现，现有如下几种思路<!--more-->

</br>

# 提供思路

- 复制流程

关闭当前流程并获取当前流程已执行节点信息（截止到当前节点的上一节点），创建新流程并将获取的已执行节点信息赋值系统控制走流程



- 修改数据库

获取上一节点任务信息，修改当前节点信息为上一节点任务信息，需要修改数据库表，如下：

ACT_RU_TASK 正在运行的任务节点表

ACT_RU_EXECUTION 运行时流程执行实例表

ACT_HI_TASKINST 历史任务节点表(包括正在运行的)

ACT_RU_IDENTITYLINK 任务与人员关联表



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


