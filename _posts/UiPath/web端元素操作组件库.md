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

借助UiPath实现RPA自动化的过程就是操作各种元素的过程,其中针对Web端各种页面元素的操作技巧和方式将在本文详细介绍.<!--more-->在使用UiPath编写自动化程序的过程中你或许会遇到如何判断页面元素是否存在?如何获取元素属性?如何获取浏览器弹出窗口内容?等问题,那么你来对地方了!

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

</br>

- 元素是否存在判断


问题:UiPath编写RPA时能够支持判断元素是否存在,无论是web元素还是桌面端元素均可通过UiElementExsits组件进行判断,但是这样远远不够.例如有些时候页面加载非常缓慢,而此时我的UiElementExsits组件已经执行并超过了超时时间,那么结果将会是组件不存在.这样其实是错误的结果.

分析:网站或应用老旧,网络延迟等导致的元素加载缓慢,从而无法真正判断元素是否存在

解决方案:自行封装组件,加入重试机制.[链接在此](http://monkeygeek369.github.io/file/ElementIsExsit.xaml)(也可在git仓库file文件夹内获取https://github.com/monkeyGeek369/icarus_monkeyGeek_source)

</br>

- 获取组件属性

描述:获取组件属性在UiPath中已经提供了GetAttribute控件去使用,但是在实际使用中非常不便利,例如

```html
<div id ="content" style="display: none;color:red;"></div>
```

在某些业务场景下需要知道id=content元素是否展示/隐藏,这个时候只能通过style属性中的display去判断.但通过GetAttribute无法直接过去display内容,获取的属性将是完整的字符串"display: none;color:red;",因此需要在对字符串进行去除空格\分割匹配等处理.非常繁琐.

解决方案:将获取属性功能封装成可移植控件.[链接在此](http://monkeygeek369.github.io/file/ElementIsHaveAttribute.xaml)(也可在git仓库file文件夹内获取https://github.com/monkeyGeek369/icarus_monkeyGeek_source)

</br>

- 获取浏览器对象

描述:我们在操作网页应用的时候必须借助某款浏览器,如chrome或Firefox,这个时候就需要控制浏览器的启动\页面的打开等初始化操作.

解决方案:利用UiPath中的线程获取工具GetProcesses来判断线程是否启动,即浏览器是否启动,再利用OpenBrower打开浏览器或BrowerScope来附加到已打开浏览器的某个具体页面.[链接在此](http://monkeygeek369.github.io/file/GetBrowser.xaml)(也可在git仓库file文件夹内获取https://github.com/monkeyGeek369/icarus_monkeyGeek_source)

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


