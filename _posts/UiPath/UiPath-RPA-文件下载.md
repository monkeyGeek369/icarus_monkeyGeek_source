---
title: UiPath-RPA-文件下载
category: UiPath
date: 2020-06-05 21:31:02
updated: 2020-06-05 21:31:02
enname: rpadownload
categories: UiPath
tags:
	- UiPath
	- RPA
	- RPA组件库
keywords: RPA,UiPath,文件下载
permalink:
thumbnail:
---

# 简介

UiPath编写的RPA本身是支持文件下载的,不仅仅支持文件下载,其对windows计算机内的文件\文件夹等具备完备的查找\创建\删除等功能.<!--more-->下面将针对文件下载做简略介绍并奉上可移植组件供下载:

</br>

# 功能组件

![](../../../../image/rpadownload.png)

如上图所示下载需要依赖HttpClient控件,具体步骤如下:

1. 拖拽控件进入目标区域
2. 填写EndPoint内容即文件下载的URL
3. 指定ResourcePath即文件存放的本地路径



注意:文件下载并非简单的下载即可,如果想要做好还要考虑文件夹是否存在?历史文件是否存在?是否需要删除历史文件?等问题,这里已经封装成可移植组件供各位使用.[点击这里](http://monkeygeek369.github.io/file/DownLoadFile.xaml)(也可在git仓库file文件夹内获取https://github.com/monkeyGeek369/icarus_monkeyGeek_source)

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


