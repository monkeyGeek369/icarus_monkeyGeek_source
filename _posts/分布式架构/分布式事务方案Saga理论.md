---
title: 分布式事务方案Saga理论
category: 分布式架构
date: 2019-12-05 13:23:10
updated: 2019-12-05 13:23:10
enname: sagabase
categories: 分布式架构
tags:
	- 分布式
	- 数据一致性
	- 事务
	- Saga
keywords: 分布式,Saga,数据一致性,事务
permalink:
thumbnail:
---

# 一 简介

谈到分布式事务不得不提Saga理论,具体可查看论文[1].Saga是基于BASE理论的一种分布式事务实现模式以最终实现"最终一致性".<!--more-->

1987年普林斯顿大学的Hector Garcia-Molina和Kenneth Salem发表了一篇Paper Sagas，讲述的是如何处理long lived transaction（长活事务）,Saga经过多年发展衍生出多种实现方式,如集中式\非集中式等。

![](../../../../image/sagatravel.png)

在我们的业务场景下，一个行程规划的事务就是一个Saga，其中包含四个子事务：机票预订、租车、酒店预订、和支付。

</br>

# 二 Saga的运行原理

> Saga中的事务相互关联，应作为（非原子）单位执行。任何未完全执行的Saga是不满足要求的，如果发生，必须得到补偿。要修正未完全执行的部分， 每个saga子交易T1应提供对应补偿事务C1

我们根据上述规则定义以下事务及其相应的事务补偿：

![](../../../../image/sagatravel2.png)

> 当每个saga子事务 T1, T2, …, Tn 都有对应的补偿定义 C1, C2, …, Cn-1, 那么saga系统可以保证
>
> - 子事务序列 T1, T2, …, Tn得以完成 (最佳情况)
> - 或者序列 T1, T2, …, Tj, Cj, …, C2, C1, 0 < j < n, 得以完成

换句话说,通过事务/补偿机制saga保证满足如下业务规则:如果有一个子事务失败则全部取消并执行对应的C补偿事务.



### 2.1Saga的组成

- 每个Saga由一系列sub-transaction Ti 组成
- 每个Ti 都有对应的补偿动作Ci，补偿动作用于撤销Ti造成的结果

可以看到，和[TCC](https://github.com/chanjarster/transactions/blob/master/tcc.md)相比，Saga没有“预留”动作，它的Ti就是直接提交到库。



### 2.2Saga的两种执行顺序

- T1, T2, T3, ..., Tn
- T1, T2, ..., Tj, Cj,..., C2, C1，其中0 < j < n

前者是正常执行Saga长链事务,后者是在出现错误时的补偿机制



### 2.3Saga的两种恢复策略

- backward recovery，向后恢复，即上面提到的第二种执行顺序，其中j是发生错误的sub-transaction，这种做法的效果是撤销掉之前所有成功的sub-transation，使得整个Saga的执行结果撤销。
- forward recovery，向前恢复，适用于必须要成功的场景，执行顺序是类似于这样的：T1, T2, ..., Tj(失败), Tj(重试),..., Tn，其中j是发生错误的sub-transaction。该情况下不需要Ci。

</br>

# 三 Saga的使用条件

- Saga只允许两个层次的嵌套，顶级的Saga和简单子事务
- 每个子事务应该是独立的原子行为
- Ti和Ci是幂等的
- Ci必须是能够成功的，如果无法成功则需要人工介入
- Ti - Ci和Ci - Ti的执行结果必须是一样的：sub-transaction被撤销了

要求Ti和Ci是幂等的，举个例子，假设在执行Ti的时候超时了，此时我们是不知道执行结果的，如果采用forward recovery策略就会再次发送Ti，那么就有可能出现Ti被执行了两次，所以要求Ti幂等。如果采用backward recovery策略就会发送Ci，而如果Ci也超时了，就会尝试再次发送Ci，那么就有可能出现Ci被执行两次，所以要求Ci幂等。



要求Ci必须能够成功，这个很好理解，因为，如果Ci不能执行成功就意味着整个Saga无法完全撤销，这个是不允许的。但总会出现一些特殊情况比如Ci的代码有bug、服务长时间崩溃等，这个时候就需要人工介入了。

</br>

# 四 Saga Log

Saga保证所有的子事务都得以完成或补偿，但Saga系统本身也可能会崩溃。Saga崩溃时可能处于以下几个状态：

- Saga收到事务请求，但尚未开始。因子事务对应的微服务状态未被Saga修改，我们什么也不需要做。
- 一些子事务已经完成。重启后，Saga必须接着上次完成的事务恢复。
- 子事务已开始，但尚未完成。由于远程服务可能已完成事务，也可能事务失败，甚至服务请求超时，saga只能重新发起之前未确认完成的子事务。这意味着子事务必须幂等。
- 子事务失败，其补偿事务尚未开始。Saga必须在重启后执行对应补偿事务。
- 补偿事务已开始但尚未完成。解决方案与上一个相同。这意味着补偿事务也必须是幂等的。
- 所有子事务或补偿事务均已完成，与第一种情况相同。



![](../../../../image/sagalog.png)



为了恢复到上述状态，我们必须追踪子事务及补偿事务的每一步。我们决定通过事件的方式达到以上要求，并将以下事件保存在名为saga log的持久存储中：

- Saga started event 保存整个saga请求，其中包括多个事务/补偿请求
- Transaction started event 保存对应事务请求
- Transaction ended event 保存对应事务请求及其回复
- Transaction aborted event 保存对应事务请求和失败的原因
- Transaction compensated event 保存对应补偿请求及其回复
- Saga ended event 标志着saga事务请求的结束，不需要保存任何内容

通过将这些事件持久化在saga log中，我们可以将saga恢复到上述任何状态。

</br>

# 五 与其它分布式理论的对比

### 5.1与TCC对比

<span style="color:red;">**缺点:**</span>

- Saga相比TCC的缺点是缺少预留动作，导致补偿动作的实现比较麻烦

<span style="color:red;">**优点:**</span>

- 有些业务很简单，套用TCC需要修改原来的业务逻辑，而Saga只需要添加一个补偿动作就行了。
- TCC最少通信次数为2n，而Saga为n（n=sub-transaction的数量）,TCC的缺点是其两阶段协议需要设计额外的服务待处理状态，以及额外的接口来处理尝试请求。
- 有些第三方服务没有Try接口，TCC模式实现起来就比较tricky了，而Saga则很简单。
- 没有预留动作就意味着不必担心资源释放的问题，异常处理起来也更简单（请对比Saga的恢复策略和TCC的异常处理）。



### 5.2与本地消息表对比

<span style="color:red;">**优点:**</span>

- 能够服务的动态治理,新服务加入更加顺畅,本地消息表在新增服务的情况下将影响上下游服务,需要配合改动。



</br>

# 六 Saga的实现

### 6.1Apache ServiceComb Pack

![](../../../../image/apachesaga.png)

- Saga Execution Component解析请求JSON并构建请求图
- TaskRunner 用任务队列确保请求的执行顺序
- TaskConsumer 处理Saga任务，将事件写入saga log，并将请求发送到远程服务

ServiceComb Pack实现的Saga是集中式的实现方式,内部具备协调者的角色并支持日志记录.



### 6.2集中式与非集中式实现

非集中式saga没有专职的协调器。启动下一个服务调用的服务就是当前的协调器。例如，

- 服务A收到要求服务A，B和C之间的数据一致性的事务请求。
- A完成其子事务，并将请求传递给事务中的下一个服务，服务B.
- B完成其子事务，并将请求传递给C，依此类推。
- 如果C处理请求失败，B有责任启动补偿事务，并要求A回滚。 

![](../../../../image/sagauncenter.png)

与集中式相比，非集中式的实现具有服务自治的优势。但每个服务都需要包含数据一致性协议，并提供其所需的额外持久化设施。

集中式还有一个比较大的好处是能够更好的避免事务之间的循环依赖关系



### 6.3其它saga实现框架

**Apache Camel Saga**

https://camel.apache.org/



**Eventuate Tram Saga**

http://eventuate.io/abouteventuatetram.html



**Seata Saga**

Seata是阿里开源的分布式事务框架其中支持Saga方式,详情可参考Seata相关资料

https://github.com/seata/seata



</br>

[1]http://www.cs.cornell.edu/andru/cs711/2002fa/reading/sagas.pdf

[2]https://github.com/chanjarster/transactions/blob/master/saga.md

[3]https://www.upyun.com/opentalk/310.html

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


