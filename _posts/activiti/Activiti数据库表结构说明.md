---
title: Activiti数据库表结构说明
category: activiti
date: 2020-06-22 21:28:00
updated: 2020-06-22 21:28:00
enname: activititable
categories: activiti
tags:
	- activiti
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

| 表分类 | 表名 | 说明 |
| ------ | ---- | ---- |
|        |      |      |
|        |      |      |
|        |      |      |
|        |      |      |
|        |      |      |
|        |      |      |
|        |      |      |
|        |      |      |
|        |      |      |
|        |      |      |
|        |      |      |
|        |      |      |
|        |      |      |
|        |      |      |
|        |      |      |
|        |      |      |
|        |      |      |
|        |      |      |
|        |      |      |
|        |      |      |
|        |      |      |
|        |      |      |
|        |      |      |



</br>

</br>

</br>

</br>

</br>

</br>

</br>

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


