---
title: web端元素操作组件库
category: UiPath
date: 2020-06-04 20:31:08
updated: 2020-06-04 20:31:08
enname: webElement
categories: UiPath
tags:
	- UiPath
	- RPA
	- RPA组件库
keywords: UiPath,RPA组件库,RPA
permalink:
thumbnail:
---

# 简介

<!--more-->

借助UiPath实现RPA自动化的过程就是操作各种元素的过程,其中针对Web端各种页面元素的操作技巧和方式将在本文详细介绍.在使用UiPath编写自动化程序的过程中你或许会遇到如何判断页面元素是否存在?如何获取元素属性?如何获取浏览器弹出窗口内容?等问题,那么你来对地方了!

</br>

# 组件详情

- 如何获取浏览器弹出窗口内容

问题:有些比较老的网站会借助浏览器弹出窗口进行内容提示,前端同学应该非常熟悉,在web页面利用js脚本使用alert("提示内容");来实现浏览器弹出窗口内容提示.

分析:这种浏览器弹出窗口本身并不属于页面元素,所以通过常规的UiPath获取技巧没有办法解决,使用UI Explorer选择器进行选择根本选不中

解决方案:虽然UI Explorer选择器无法选择元素但UiPath的选择器依然强大,它实现了windows系统整个"界面树"的完整解析,通过仔细查找我们可以发现,弹出框的获取选择器如下:

```html
<html app='chrome.exe' title='::: Sinokor New e-Service :::' />
<ctrl role='dialog' />
<ctrl name='*' role='text' />
```

第一行为网站页面选择

第二行为浏览器弹出窗口选择

第三行为浏览器弹出窗口的内容获取

具体案例我已经封装成为工具组件,大家可自行使用,[链接在此](http://monkeygeek369.github.io/file/BrowserDialogIsExsit.xaml)(也可在git仓库file文件夹内获取https://github.com/monkeyGeek369/icarus_monkeyGeek_source)

- 

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


