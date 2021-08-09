---
title: Activiti数据库表结构说明
category: activiti
date: 2020-06-22 21:28:00
updated: 2020-06-22 21:28:00
enname: activititable
categories: activiti
tags:
	- activiti
	- 面试
keywords: activiti,表结构
permalink:
thumbnail:
---

# 数据库表名说明

Activiti工作流总共包含23张数据表，所有的表名默认以“**ACT_**”开头.并且表名的第二部分用两个字母表明表的用例，而这个用例也基本上跟Service API匹配<!--more-->

- **ACT_GE_\*** : “GE”代表“General”（通用），用在各种情况下；
- **ACT_HI_\*** : “HI”代表“History”（历史），这些表中保存的都是历史数据，比如执行过的流程实例、变量、任务，等等。Activit默认提供了4种历史级别：
- **none**: 不保存任何历史记录，可以提高系统性能；
- **activity**：保存所有的流程实例、任务、活动信息；
- **audit**：也是Activiti的**默认**级别，保存所有的流程实例、任务、活动、表单属性；
- **full**：最完整的历史记录，除了包含**audit**级别的信息之外还能保存详细，例如：流程变量。对于几种级别根据对功能的要求选择，如果需要日后跟踪详细可以开启**full**。
- **ACT_ID_\*** : “ID”代表“Identity”（身份），这些表中保存的都是身份信息，如用户和组以及两者之间的关系。如果Activiti被集成在某一系统当中的话，这些表可以不用，可以直接使用现有系统中的用户或组信息；
- **ACT_RE_\*** : “RE”代表“Repository”（仓库），这些表中保存一些‘静态’信息，如流程定义和流程资源（如图片、规则等）；
- **ACT_RU_\*** : “RU”代表“Runtime”（运行时），这些表中保存一些流程实例、用户任务、变量等的运行时数据。Activiti只保存流程实例在执行过程中的运行时数据，并且当流程结束后会立即移除这些数据，这是为了保证运行时表尽量的小并运行的足够快；

</br>



# 数据库表结构

| 表分类       | 表名                | 说明                                                         |
| ------------ | ------------------- | ------------------------------------------------------------ |
| 一般数据     | ACT_GE_BYTEARRAY    | 通用的流程定义和流程资源                                     |
| 一般数据     | ACT_GE_PROPERTY     | 系统相关属性                                                 |
| 流程历史记录 | ACT_HI_ACTINST      | 历史的流程实例（只记录UserTask类型）                         |
| 流程历史记录 | ACT_HI_ATTACHMENT   | 历史的流程附件                                               |
| 流程历史记录 | ACT_HI_COMMENT      | 历史的说明性信息                                             |
| 流程历史记录 | ACT_HI_DETAIL       | 历史的流程运行中的细节信息                                   |
| 流程历史记录 | ACT_HI_IDENTITYLINK | 历史的流程运行过程中用户关系                                 |
| 流程历史记录 | ACT_HI_PROCINST     | 历史的流程实例（包括正运行的）                               |
| 流程历史记录 | ACT_HI_TASKINST     | 历史的任务实例（包括正运行的）                               |
| 流程历史记录 | ACT_HI_VARINST      | 历史的流程运行中的变量信息（包括正运行的）                   |
| 用户表       | ACT_ID_GROUP        | 用来存储用户组信息                                           |
| 用户表       | ACT_ID_INFO         | 用户扩展信息表。目前该表未用到。                             |
| 用户表       | ACT_ID_MEMBERSHIP   | 用来保存用户的分组信息                                       |
| 用户表       | ACT_ID_USER         | 身份信息-用户信息                                            |
| 流程定义表   | ACT_RE_DEPLOYMENT   | 用来存储部署时需要持久化保存下来的信息                       |
| 流程定义表   | ACT_RE_MODEL        | 创建流程的设计模型时，保存在该数据表中。                     |
| 流程定义表   | ACT_RE_PROCDEF      | 流程解析表，解析成功了，在该表保存一条记录。业务流程定义数据表 |
| 运行实例表   | ACT_RU_EVENT_SUBSCR | 运行时事件                                                   |
| 运行实例表   | ACT_RU_EXECUTION    | 运行时流程执行实例（记录当前节点信息）                       |
| 运行实例表   | ACT_RU_IDENTITYLINK | 主要存储当前节点参与者的信息,任务参与者数据表。              |
| 运行实例表   | ACT_RU_JOB          | 运行时定时任务数据表                                         |
| 运行实例表   | ACT_RU_TASK         | （执行中实时任务）代办任务查询表                             |
| 运行实例表   | ACT_RU_VARIABLE     | 运行时变量表                                                 |

</br>



# 主要表简要说明

## ACT_GE_BYTEARRAY

通用的流程定义和流程资源，用来保存部署文件的大文本数据。

保存流程定义图片和xml、Serializable(序列化)的变量,即保存所有二进制数据，特别注意类路径部署时候，不要把svn等隐藏文件或者其他与流程无关的文件也一起部署到该表中，会造成一些错误（可能导致流程定义无法删除）。

## ACT_GE_PROPERTY

系统相关属性，属性数据表。存储这个流程引擎级别的数据。

## ACT_HI_ACTINST

历史活动信息。这里记录流程流转过的所有节点，与HI_TASKINST不同的是，taskinst只记录usertask内容。

## ACT_HI_DETAIL

历史详情表：流程中产生的变量详细，包括控制流程流转的变量，业务表单中填写的流程需要用到的变量等。

备注：VAR_TYPE_类型说明: jpa-entity、boolean、bytes、serializable(可序列化)、自定义type(根据你自身配置)、 CustomVariableType、date、double、integer、long、null、short、string

## ACT_HI_IDENTITYLINK

历程流程人员表，任务参与者数据表。主要存储历史节点参与者的信息。

## ACT_RE_PROCDEF

流程解析表，解析成功了，在该表保存一条记录。业务流程定义数据表

注：此表和ACT_RE_DEPLOYMENT是多对一的关系，即，一个部署的bar包里可能包含多个流程定义文件，每个流程定义文件都会有一条记录在ACT_RE_PROCDEF表内，每个流程定义的数据，都会对于ACT_GE_BYTEARRAY表内的一个资源文件和PNG图片文件。和ACT_GE_BYTEARRAY的关联是通过程序用ACT_GE_BYTEARRAY.NAME与ACT_RE_PROCDEF.NAME_完成的，在数据库表结构中没有体现。

</br>



# Activiti中主要对象的关系

本节主要介绍在工作流中出现的几个对象及其之间的关系，以及在Activiti中各个对象是如何关联的。

我们模拟一个请假的流程进行分析介绍，该流程主要包含以下几个步骤：

- 员工申请请假
- 部门领导审批
- 人事审批
- 员工销假



## ProcessInstance对象

员工开始申请请假流程，通过runtimeService.startProcessInstance()方法启动，引擎会创建一个流程实例（ProcessInstance）。

简单来说流程实例就是根据一次（一条）业务数据用流程驱动的入口，两者之间是**一对一**的关系。流程引擎会创建一条数据到**ACT_RU_EXECUTION**表，同时也会根据**history**的级别决定是否查询相同的历史数据到**ACT_HI_PROCINST**表。

启动完流程之后业务和流程已经建立了关联关系，第一步结束。

启动流程和业务关联区别：

- 对于**自定义表单**来说启动的时候会传入**businessKey**作为业务和流程的关联属性
- 对于**动态表单**来说不需要使用**businessKey**关联，因为所有的数据都保存在引擎的表中
- 对于**外部表单**来说**businessKey**是可选的，但是一般不会为空，和自定义表单类似



## Execution对象

对于初学者来说，最难理解的地方就是ProcessInstance与Execution之间的关系，要分两种情况说明。Execution的含义就是一个流程实例（ProcessInstance）具体要执行的过程对象。

不过在说明之前先声明两者的对象映射关系：

ProcessInstance（**1**）→ Execution(**N**)，（其中**N**>=1）。

**1)**  **值相等的情况：**

除了在流程中启动的子流程之外，流程启动之后在表**ACT_RU_EXECUTION**中的字段**ID_**和**PROC_INST_ID**_字段值是相同的。

**2)**  **值不相等的情况：**

不相等的情况目前只会出现在子流程中（包含：嵌套、引入），例如一个购物流程中除了下单、出库节点之外可能还有一个付款子流程，在实际企业应用中付款流程通常是作为公用的，所以使用子流程作为主流程（购物流程）的一部分。



## Task对象

前面说了ProcessInstance和业务是一对一关联的，和业务数据最亲密；而Task则和用户最亲密的（UserTask），用户每天的待办事项就是一个个的Task对象。

Task是在流程定义中看到的最大单位，每当一个Task完成的时候引擎会把当前的任务移动到历史中，然后插入下一个任务插入到表**ACT_RU_TASK**中。结合请假流程来说就是让用户点击“完成”按钮提交当前任务是的动作，引擎自动根据任务的顺序流或者排他分支判断走向。



## HistoryActivity（历史活动）

Activity包含了流程中所有的活动数据，例如开始事件（图5表中的第1条数据）、各种分支（排他分支、并行分支等，图5表中的第2条数据）、以及刚刚提到的Task执行记录

有些人认为Activity和Task是多对一关系，其实不是，从上图中可以看出来根本没有Task相关的字段。

结合请假流程来说，如Task中提到的当完成流程的时候所有下一步要执行的任务（包括各种分支）都会创建一个Activity记录到数据库中。例如领导审核节点点击“同意”按钮就会流转到人事审批节点，如果“驳回”那就流转到调整请假内容节点，每一次操作的Task背后实际记录更详细的活动（Activity）。



</br>

参考资料:

[1]https://blog.csdn.net/hj7jay/article/details/51302829



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


