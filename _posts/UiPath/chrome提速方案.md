---
title: chrome提速方案
category: UiPath
date: 2020-08-07 15:02:03
updated: 2020-08-07 15:02:03
enname: chromeSpeed
categories: UiPath
tags:
	- UiPath
	- RPA
keywords: chrome提速，UiPath，RPA
permalink:
thumbnail:
---

# 前言

借助chrome浏览器执行的RPA程序在执行效率上会受到各种因素的影响，网络问题、服务器问题、浏览器问题，其中本文专门针对chrome浏览器的提速做简历介绍。<!--more-->

</br>

# 常规操作

- 更新chrome至最新
- 关闭多余标签
- 关闭多余扩展程序
- 关闭不需要的任务

</br>

# 无图模式

针对某些特定性的网站，例如加载大量图片、登录无验证码的网站可以开启无图模式，提高加载速度。具体设置路径如下：

![](../../../../image/chromeNoImg.png)

***设置>>隐私设置和安全性>>网站设置>>图片***

</br>

# 开启GPU加速

利用GPU硬件对网页加载提速

***浏览器输入chrome://flags并搜索#enable-gpu-rasterization选中开启***

</br>

# 开启QUIC协议

这是 Google 研发的另一项数据传输加速黑科技，2012 年 Google 便研发了 QUIC (快速 UDP Internet 连接) 协议并在公司内部用于减少连接延迟和网络拥塞。虽然目前在 Chrome 中还是实验性功能，但已经在 2015 年 6 月作为标准被提交给了 IETF，相信很快便会得到普及。要启用 QUIC 协议支持，只需将「实验性 QUIC 协议」改为已启用即可。

***浏览器输入chrome://flags并搜索#enable-quic选中开启***

</br>

# 开启Stale-While-Revalidate缓存

关于Stale-While-Revalidate的相关只是可以参考[2]，总的来说利用Stale-While-Revalidate配置可以使浏览器更加智能更加合理的去选择在网站加载时读取缓存数据还是获取最新数据。其通常与max-age匹配使用。

> `stale-while-revalidate` 指令应当与 `max-age` 配合使用，超过 `max-age` 指定的时间的响应就是陈旧的响应。与之相对的，没有超过时间的就是新鲜的 (fresh) 响应。如果一个缓存的响应没有超过 `max-age` 指定的时间（仍是新鲜的），按照上面讲的缓存机制，此时请求这个资源，浏览器会直接返回缓存的结果。如果缓存的结果已经陈旧了呢？按照前面讲的缓存机制，浏览器应该去请求新的响应了，但是如果存在 `stale-while-revalidate` 指令就不一样了，浏览器会检查这个陈旧的响应是否超过了 `stale-while-revalidate` 规定的时间窗口。如果没有超过，那么浏览器仍然会直接返回缓存的结果，同时在后台请求新的结果用来更新缓存。

***浏览器输入chrome://flags并搜索#enable-stale-while-revalidate选中开启***

</br>

# 提速插件

FasterChrome下载及介绍可参考[3]

</br>

</br>

参考资料：

[1]https://www.sysgeek.cn/speed-up-chrome-8-flags/

[2]https://juejin.im/post/6844904136920547341

[3]https://hao.su/3081/

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


