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

# Paxos算法流程



### **Paxos中的角色**

![](../../../../image/paxos角色关系.png)

- Proposer(提议人):负责提出提案(Proposal),Proposal信息包括提案编号 (Proposal ID) 和提议的值 (Value)
- Acceptor(接收人):参与决策，回应Proposers的提案
- Learner(学习者):不参与决策，从Proposers/Acceptors学习最新达成一致的提案（Value）

注:在多副本状态机中，每个副本同时具有Proposer、Acceptor、Learner三种角色



### **Paxos的决议阶段**

- 第一阶段：Prepare阶段。Proposer向Acceptors发出Prepare请求，Acceptors针对收到的Prepare请求进行Promise承诺。
- 第二阶段：Accept阶段。Proposer收到多数Acceptors承诺的Promise后，向Acceptors发出Propose请求，Acceptors针对收到的Propose请求进行Accept处理。

注:在第二阶段结束后如果Proposer收到多数Acceptors的Accept，标志着本次Accept成功，决议形成，将形成的决议发送给所有Learners



### **Acceptor的两个承诺一个应答**

**两个承诺**

- 不再接受Proposal ID小于等于（注意：这里是<= ）当前请求的Prepare请求。
- 不再接受Proposal ID小于（注意：这里是< ）当前请求的Propose请求。

**一个应答**

- 不违背以前作出的承诺下，回复已经Accept过的提案中Proposal ID最大的那个提案的Value和Proposal ID，没有则返回空值。



### **Paxos算法伪代码Base-Paxos**

![](../../../../image/Base-Paxos基础流程.png)

- 步骤1:提议人生成最新提议编号N(N自增且唯一,Paxos不同实现其N的生成算法不同),并将N广播至所有接收人
- 步骤2:接收人响应提议人Proposal,如果接收人maxN(记录最大N)为null则记录N到maxN并返回(null,null),如果N>maxN则记录N到maxN并返回(acceptN,accetpV),如果N<=maxN则不响应
- 步骤3:提议人收集所有来自接收人的响应数据,如果响应数量小于等于半数则重新执行步骤1,否则将从所获取所有响应的最大编号accetpedN所对应的acceptedV作为阶段二value的提交值.
- 步骤4:此步骤提议人将提交(N,value)至所有接收人,其中N将重新生成,value来自步骤3,如果value==null则将有提议人自行决定
- 步骤5:接收人将针对提议人的(N,value)进行响应,如果N小于maxN,接收人不响应,否则,接收人将更新maxN,accetpedN,accetpedV并返回(acceptN,accetpV)
- 步骤6:由提议人根据响应结果确定value的决议结果并将结果分发至各Learner,其中如果响应数过半数但响应结果的编号result大于发送编号N,那么证明value有被更新,需要重新执行步骤1



**总结:**

- 阶段一本质上是收集各接收人现存(acceptN,accetpV)并为阶段二的数据提交做准备
- 阶段二本质上是完成决议确定value值并将决议结果发送至各Learner



注:原始的Paxos算法（Basic Paxos）只能对一个值形成决议，决议的形成至少需要两次网络来回，在高并发情况下可能需要更多的网络来回。如果想连续确定多个值，Basic Paxos搞不定了。因此Basic Paxos几乎只是用来做理论研究，并不直接应用在实际工程中。









</br>

</br>







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


