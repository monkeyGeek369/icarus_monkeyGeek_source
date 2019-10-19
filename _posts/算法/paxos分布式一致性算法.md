---
title: paxos分布式一致性算法
category: 算法
date: 2019-10-19 19:09:33
updated: 2019-10-19 19:09:33
enname: paxos
categories: 算法
tags:
	- 算法
	- paxos
	- 分布式一致性
keywords: 
	- 算法
	- paxos
	- 分布式一致性
permalink:
thumbnail:
---

# 情景再现





Paxos是能够基于一大堆完全不可靠的网络条件下却能可靠确定地实现共识一致性的算法。也就是说：它允许一组不一定可靠的处理器（服务器）在某些条件得到满足情况下就能达成确定的安全的共识，如果条件不能满足也确保这组处理器（服务器）保持一致。





<!--more-->



</br>

</br>

参考文献如下:

https://blog.csdn.net/cnh294141800/article/details/53768464

https://www.cnblogs.com/stateis0/p/9062130.html

https://blog.csdn.net/yowasa/article/details/81224936

https://blog.csdn.net/omnispace/article/details/79653932

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


