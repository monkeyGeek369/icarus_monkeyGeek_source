---
title: 交互式RPA实现方案
category: UiPath
date: 2020-11-06 10:23:21
updated: 2020-11-06 10:23:21
enname: interactive_rpa
categories: UiPath
tags:
	- UiPath
	- RPA
keywords: UiPath,RPA,交互式RPA
permalink:
thumbnail: ../../../image/交互式RPA交互流程模式.png
---

简介

RPA的执行除了无人值守的方式外在某些长链式的业务场景下需要人员的接入，例如业务人员的确认后方可进行后续操作。<!--more-->介于此，交互式RPA便营运而生，本文仅探讨交互式RPA的实现思路以及一些可行方案的简介，详细的落地方案不在本文讨论范围。

</br>

</br>

交互流程

![](../../../../image/交互式RPA交互流程模式.png)

- 正常流程：触发-校验-适配-执行-提交-反馈
- 交互流程一：触发-校验-适配-确认-执行-提交-反馈
- 交互流程二：触发-校验-适配-执行-确认-提交-反馈
- 交互流程三：触发-校验-适配-执行-确认-执行-确认-提交-反馈

交互式RPA本质上是实现人机交互，通过人的介入来更好的完成长链式任务。

人员介入后可进行外部业务逻辑处理，例如数据填写、数据确认、流程回退等等，业务人员的外部操作所产生的数据可以返回并转化为RPA的执行参数。涉及到数据相关的人机交互相当复杂，这里仅仅讨论简单的“确认”逻辑，不涉及数据交互。

针对RPA执行过程的确认可以通过表格/截图/视频的方式进行，或者借助短信、微信、钉钉、app等平台进行确认，确认的过程仅仅是动作的反馈不涉及业务数据交互。

</br>

</br>

脑洞大开

思路一：前段式确认

采用交互流程一的方式，在执行RPA前将提交数据进行校验和适配并以数据表格方式展示和确认
优点：效率高，节省服务器资源，RPA成功执行只需执行一遍
缺点：确认的成功率存在风险，可视化不友好，改造成本中等

</br>

思路二：一遍式确认-服务器单用户
采用交互流程二的方式，RPA仅被触发一遍，当执行到提交节点前时正在执行的RPA程序将暂时停止并等待人员确认，此时服务器仅支持单用户账户同时操作。

优点：执行一遍节省时间

缺点：服务器资源占用、RPA需要断点改造

</br>

思路三：一遍式确认-服务器多用户
采用交互流程二的方式，RPA仅被触发一遍，当执行到提交节点前时正在执行的RPA程序将暂时停止并等待人员确认，此时服务器可支持多用户账户同时操作。

优点：执行一遍节省时间，多用户提高服务器利用率

缺点：RPA需要断点改造，需要服务器配置

</br>

思路四：一遍式确认-浏览器多开隔离
采用交互流程二的方式，RPA仅被触发一遍，当执行到提交节点前时正在执行的RPA程序将暂时停止并等待人员确认，此时浏览器可支持多开并相互不影响。

优点：执行一遍节省时间，多开浏览器提高服务器利用率

缺点：RPA需要断点改造，浏览器多开和隔离技术难度大

</br>

思路五：两遍式确认-利用真实提交
采用交互流程二的方式

</br>

思路六：两遍式确认-适配数据预留
采用交互流程二的方式

</br>

思路七：长链任务编排
采用交互流程三的方式
一遍式单用户---不可取/资源占用严重
一遍式多用户---可取
多遍式单用户---可取/资源占用可接受
多遍式多用户---可取/资源占用少
扩展功能：适配数据预留/日志还原/动态编排







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


