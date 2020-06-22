---
title: activiti简介
category: activiti
date: 2020-06-22 19:55:51
updated: 2020-06-22 19:55:51
enname: activitiIntroduction
categories: activiti
tags:
	- activiti
keywords: activiti,activiti简介
permalink: ../../image/activitistructure.jpg
thumbnail:
---

# 七大接口

- RepositoryService：提供一系列管理流程部署和流程定义的API。
- RuntimeService：在流程运行时对流程实例进行管理与控制。
- TaskService：对流程任务进行管理，例如任务提醒、任务完成和创建任务等。<!--more-->
- IdentityService：提供对流程角色数据进行管理的API，这些角色数据包括用户组、用户及它们之间的关系。
- ManagementService：提供对流程引擎进行管理和维护的服务。
- HistoryService：对流程的历史数据进行操作，包括查询、删除这些历史数据。
- FormService：表单服务。

</br>

# 结构关系

![](../../../../image/activitistructure.jpg)

</br>

# 数据库表结构

![](../../../../image/activititable.png)

1、act_ge_ 通用数据表，ge是general的缩写

2、act_hi_ 历史数据表，hi是history的缩写，对应HistoryService接口

3、act_id_ 身份数据表，id是identity的缩写，对应IdentityService接口

4、act_re_ 流程存储表，re是repository的缩写，对应RepositoryService接口，存储流程部署和流程定义等静态数据

5、act_ru_ 运行时数据表，ru是runtime的缩写，对应RuntimeService接口和TaskService接口，存储流程实例和用户任务等动态数据

</br>

# 流程处理

![](../../../../image/activitiprocess.png)

说明：关于人员指派我们再做流程图时可以指定具体某节点对应的用户或用户组，这种情况下无需指派人员操作，流程自动识别。若流程图中未指定人员，我们可以通过代码的方式手动指定人员id

</br>

# 流程发布更新问题

场景如下：流程图制作完毕并交付项目使用，在使用一段时间后发现流程需要调整，需要将最新流程文件xml进行替换

原理：xml流程文件即为流程图的规则，在执行流程的过程中activiti流程引擎回去解析，如果流程执行到某节点，那么该节点的xml对应信息将会被解析存储进数据库成为可持久化数据。

- 流程结构不变，需要针对参数等微调：直接将xml流程文件进行替换即可
- 流程结构变化：如果该流程某业务已经执行了一部分则不建议直接替换需要等流程走通后替换。

影响:

- 如果流程没被使用则随时可以替换，无影响
- 如果流程正在被使用，那么已经执行的流程节点已经被持久化至数据库，即使替换流程图也无法更改，未执行节点将按照新版流程图进行

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


