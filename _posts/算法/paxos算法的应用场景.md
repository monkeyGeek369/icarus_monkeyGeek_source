---
title: paxos算法的应用场景
category: 算法
date: 2019-10-22 19:34:34
updated: 2019-10-22 19:34:34
enname: paxosUsed
categories: 算法
tags:
	- 算法
	- 分布式一致性
	- 面试
keywords:
	- 算法
	- paxos
	- 分布式一致性
permalink:
thumbnail:
---

在"paxos分布式一致性算法"一文中详细介绍了paxos算法的前世今生以及paxos算法思想,本文主要介绍paxos算法的典型应用场景.paxos主要解决分布式系统一致性问题,确保多节点的数据统一,其在大数据管理,数据备份等方面有重要作用.

<!--more-->

</br>

# 数据备份

Paxos最常见的应用场景是数据库备份(Database Replication),保证数据在多个节点上的一致性。

工业界在这方面的典型系统包括Chubby（谷歌公司），Zookeeper（Yahoo!的Hadoop项目），Nutanix（Vmware），以及PhxPaxos（腾讯微信）

- Chubby-使用了paxos来保证日志在各个副本上的一致性，Paxos算法可以确保每个副本的本地日志具有相同的内容，在这之上是高容错的分布式数据库层
- Zookeeper-可以看作是一个开源的Chubby,其被应用在Hadoop,帮助提供强一致性的分布式文件系统
- Nutanix-分布式文件系统(NDFS)是Vmware虚拟计算平台的核心.该系统负责管理所有的元数据和数据。NDFS具有极高的容错能力，可确保节点发生故障时数据的可用性和一致性。
- PhxPaxos是腾讯公司微信后台团队自主研发的一个类库,基于paxos算法思想,实现多机的状态拷贝.

</br>

# 域名服务器

网络中每个节点都有一个地址，网络能根据消息的目标地址将消息准确送到对应的节点。域名服务器（Name Server）的作用就是将一个节点或服务的名称转换为对应的位置或地址。

一个中心域名服务器必须位于一个众所周知的地址，且永不改变。但当这个中心域名服务器崩溃时，整个网络都将崩溃。一个中心域名服务器也需要一个极多的存储空间，并且可能导致信息过载。为了解决上述问题，我们可以采用分布式域名服务器。

</br>

# Config配置管理

通常对于小的系统，我们习惯采用手工修改配置文件的方法，这样做有两个问题，其一是容易出错；其二，若系统运行在多个节点上，手工修改难以保证多个节点的状态是一致的。因此，对于大规模的应用系统，特别是分布式的应用，我们必须采用自动化的方式统一修改配置文件。

目前一个流行的做法是采用Zookeeper（核心是Paxos），将配置文件放到Zookeeper的某个目录上，然后各个程序对这个目录节点进行监听。若配置发生改变，各个节点上的应用程序就会收到通知，并自行修改配置。

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


