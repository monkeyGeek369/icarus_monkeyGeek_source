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

# 前言

*Google Chubby的作者Mike Burrows说过这个世界上只有一种一致性算法，那就是Paxos，其它的算法都是残次品。*
*Paxos算法问世已经有将近30年的历史了，是目前公认最有效的解决分布式场景下一致性问题的算法之一，但是缺点是比较难懂，工程化比较难。*

Paxos算法是用来解决分布式系统中，如何就某个值达成一致的算法。

<!--more-->

</br>

# 一致性问题

什么是一致性问题?

假设有一组服务器保存了用户的余额，初始是100块，现在用户提交了两个订单，一个订单是消费10元，一个订单是充值50元。由于网络错误和延迟等原因，导致一部分服务器只收到了第一个订单（余额更新为90元），一部分服务器只收到了第二个订单（余额更新为150元），还有一部分服务器两个订单都接收到了（余额更新为140元），这三者无法就最终余额达成一致。这就是一致性问题。



Paxos是能够基于一大堆完全不可靠的网络条件下却能可靠确定地实现共识一致性的算法。也就是说：它允许一组不一定可靠的处理器（服务器）在某些条件得到满足情况下就能达成确定的安全的共识，如果条件不能满足也确保这组处理器（服务器）保持一致。



需要注意的是paxos的应用场景是假定各消息提供方提供的消息是真实有效的,伪造消息不在paxos研究范围内,Byzantine Paxos 以及区块链技术是paxos的升级和应用,能够有效避免伪造消息等情况发生.

</br>

# paxos的发展历史及演变

![](../../../../image/paxos发展阶段.png)

从上图可以看出paxos发展分为三个阶段:

- 1988年Lislov(利斯洛夫)等人在PODC(分布式计算原理学术讨论会)上发表了一篇文章,关于副本宕机,分布式系统仍能够正常运行的算法,该算法本质上与Paxos一致
- 1989年Leslie Lamport在1989年提出Paxos这个名称，但他的论文因为过于艰涩，未能发表。



- 1996年，Lamport论文的正式发表（与8年前第一次投稿相比，几乎未改）。
- 1999年，Butler Lampson等人重新描述和证明了Paxos算法，并分析了其时间开销及容错性。
- 1999年，Liskov等人提出了PBFT（实用的拜占庭容错算法）被Lampson称为Byzantine Paxos，该算法对基础Paxos进行了改进，使其可以处理拜占庭错误。
- 2000年，Lamport在提出了Disk Paxos，这可以认为是Paxos基于磁盘的版本，以支持持久化。
- 2004年，Lamport提出了Cheap Paxos,其提升了算法的容错性
- 2006年，Lamport提出了Fast Paxos,其降低了消息延迟
- 2007年，google公司研究小组提出了Multi-Paxos,其将paxos中的论证阶段进行简化,提高了效率



- 2007年及其以后google公司在OSDI发表的两篇论文拉开了paxos工业应用的序幕







</br>

参考文献如下:

https://blog.csdn.net/cnh294141800/article/details/53768464

https://www.cnblogs.com/stateis0/p/9062130.html

https://blog.csdn.net/yowasa/article/details/81224936

https://blog.csdn.net/omnispace/article/details/79653932

https://zhuanlan.zhihu.com/p/43800991

https://zhuanlan.zhihu.com/p/69817030

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


