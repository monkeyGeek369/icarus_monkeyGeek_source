---
title: TCP/IP协议简介
category: 协议
date: 2020-06-17 21:59:00
updated: 2020-06-17 21:59:00
enname: tcpip
categories: 网络协议
tags:
	- 网络协议
keywords: TCP,网络协议,TCP协议,TCP详解,IP协议,TCP/IP协议
permalink:
thumbnail: ../../image/tcpip.jpg
---

# 简介

TCP/IP不是一个协议，而是一个协议族的统称。里面包括了IP协议，IMCP协议，TCP协议，以及我们更加熟悉的http、ftp、pop3协议等等。<!--more-->电脑有了这些，就好像学会了外语一样，就可以和其他的计算机终端做自由的交流了。

</br>

# 协议分层

![](../../../../image/tcpip.jpg)

TCP/IP协议族按照层次由上到下，层层包装。

**应用层**:

提供应用程序间的服务通信。文件传输访问FTP使用FTP协议来提供网络内机器间的文件拷贝功能。

**传输层**:

提供应用程序间的数据通信。

**网络层** ：

负责相邻计算机之间的通信。其功能包括三方面。

- 一、处理来自传输层的分组发送请求，收到请求后，将分组装入IP数据报，填充报头，选择去往信宿机的路径，然后将数据报发往适当的网络接口。
- 二、处理输入数据报：首先检查其合法性，然后进行寻径--假如该数据报已到达信宿机，则去掉报头，将剩下部分交给适当的传输协议；假如该数据报尚未到达信宿，则转发该数据报。
- 三、处理路径、流控、拥塞等问题。

**网络接口层**：

这是TCP/IP软件的最低层，负责接收IP数据报并通过网络发送之，或者从网络上接收物理帧，抽出IP数据报，交给IP层。

</br>

# IP协议

- IP 用于计算机之间的通信。
- IP 是无连接的通信协议。
- 它不会占用两个正在通信的计算机之间的通信线路。这样，IP 就降低了对网络线路的需求。每条线可以同时满足许多不同的计算机之间的通信需要。通过 IP，消息（或者其他数据）被分割为小的独立的包，并通过因特网在计算机之间传送。
- IP 负责将每个包路由至它的目的地。
- CP/IP 使用 32 个比特来编址。一个计算机字节是 8 比特。所以 TCP/IP 使用了 4 个字节。
- 一个计算机字节可以包含 256 个不同的值：00000000、00000001、00000010、00000011、00000100、00000101、00000110、00000111、00001000 ....... 直到 11111111。



</br>

参考资料:

[1]https://www.jianshu.com/p/ef892323e68f

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


