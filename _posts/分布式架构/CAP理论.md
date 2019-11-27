---
title: CAP理论
category: 分布式架构
date: 2019-11-27 19:52:47
updated: 2019-11-27 19:52:47
enname: cap
categories: 分布式架构
tags:
	- 分布式
	- 数据一致性
	- 事务
	- CAP理论
keywords: 分布式,数据一致性,CAP理论
permalink:
thumbnail: ../../image/cap.png
---

CAP：任何分布式系统在可用性、一致性、分区容错性方面，不能兼得，最多只能得其二，因此，任何分布式系统的设计只是在三者中的不同取舍而已。

<!--more-->

# CAP的历史

- 1985年Lynch证明了异步通信中不存在任何一致性的分布式算法（FLP Impossibility）的同时，人们就开始寻找分布式系统设计的各种因素。一致性算法既然不存在，但若能找到一些设计因素，并进行适当的取舍以最大限度满足实现系统需求成为当时的重要议题。比如，在CAP之前研究者就已经发现低延迟和顺序一致性不可能同时被满足。
- 2000年，Eric Brewer教授在PODC的研讨会上提出了一个猜想：一致性、可用性和分区容错性三者无法在分布式系统中被同时满足，并且最多只能满足其中两个！



这个猜想首次把一致性、可用性和分区容错三个因素提炼出来作为系统设计的重要特征，断言用此三者可以划分所有的分布式系统，并指明这三个特征之间的不可能性关系。

Brewer教授当时想象的分布式场景是webservice，一组websevrice后台运行着众多的server，对service的读写会反应到后台的server集群，并对CAP进行了定义：
C（一致性）：所有的节点上的数据时刻保持同步
A（可用性）：每个请求都能接受到一个响应，无论响应成功或失败
P（分区容错）：系统应该能持续提供服务，即使系统内部有消息丢失（分区）



**<u>*高可用、数据一致是很多系统设计的目标，但是分区又是不可避免的事情：</u>***

![](../../../../image/cap.png)

- CA without P：如果不要求P（不允许分区），则C（强一致性）和A（可用性）是可以保证的。但其实分区不是你想不想的问题，而是始终会存在，因此CA的系统更多的是允许分区后各子系统依然保持CA。
- CP without A：如果不要求A（可用），相当于每个请求都需要在Server之间强一致，而P（分区）会导致同步时间无限延长，如此CP也是可以保证的。很多传统的数据库分布式事务都属于这种模式。
- AP wihtout C：要高可用并允许分区，则需放弃一致性。一旦分区发生，节点之间可能会失去联系，为了高可用，每个节点只能用本地数据提供服务，而这样会导致全局数据的不一致性。现在众多的NoSQL都属于此类。



CAP的出现仿佛是一盏明灯，它揭露了分布式系统的本质，并给出了设计的准则，而这正是1985年以来人们正在寻找的东西！所以CAP在当时的影响力是非常大的！

</br>

# CAP证明

2002年，Lynch与其他人证明了Brewer猜想，从而把CAP上升为一个定理。但是，她只是证明了CAP三者不可能同时满足，并没有证明任意二者都可满足的问题，所以，该证明被认为是一个收窄的结果。
Lynch的证明相对比较简单：采用反正法，如果三者可同时满足，则因为允许P的存在，一定存在Server之间的丢包，如此则不能保证C，证明简洁而严谨。
在该证明中，对CAP的定义进行了更明确的声明：

- C：一致性被称为原子对象，任何的读写都应该看起来是“原子“的，或串行的。写后面的读一定能读到前面写的内容。所有的读写请求都好像被全局排序。
- A：对任何非失败节点都应该在有限时间内给出请求的回应。（请求的可终止性）
- P：允许节点之间丢失任意多的消息，当网络分区发生时，节点之间的消息可能会完全丢失



当国内工程师对CAP痴迷的时候，国外的工程师和研究者对CAP提出了各种质疑，纷纷有用反例证明着CAP在各种场合不适用性，同时挑战着Lynch的证明结果！
纵观这些质疑，基本都是拿着一个非常具体的系统，用CAP的理论去套，最后发现要么CAP不能Cover所有的场景，要么是CAP的定义非常模糊，导致自相矛盾！一句话，把CAP接地气是非常困难的！
你是否看了CAP的概念定义后还是感觉很模糊？如果是，你并不孤独，有很多人都是如此！
CAP没有考虑不同的基础架构、不同的应用场景、不同的网络基础和用户需求，而C、A、P在这些不同场景中的含义可能完全不同，这种无视差异化的定义导致了非常大的概念模糊，同时也变成CAP被质疑的源头！

***<u>面对大量的质疑，Brewer和Lynch终于坐不住了，因此两人纷纷出来澄清Brewer于2012年重申：*</u>**

- 在某些分区极少发生的情况下，三者能顺畅地在一起配合
- CAP不仅仅是发生在整个系统中，可能是发生在某个子系统或系统的某个阶段

**<u>*Lynch也在10年后的2012年重写了论文，该论文主要做了几件事：</u>***

- 把CAP理论的证明局限在原子读写的场景，并申明不支持数据库事务之类的场景
- 一致性场景不会引入用户，只是发生在后台集群之内
- 把分区容错归结为一个对网络环境的陈述，而非之前一个独立条件。这实际上就是更加明确了概念
- 引入了活性(liveness)和安全属性(safety)，在一个更抽象的概念下研究分布式系统，并认为CAP是活性与安全熟悉之间权衡的一个特例。其中的一致性属于liveness，可用性属于safety
- 把CAP的研究推到一个更广阔的空间：网络存在同步、部分同步；一致性性的结果也从仅存在一个到存在N个（部分一致）；引入了通信周期round，并引用了其他论文，给出了为了保证N个一致性结果，至少需要通信的round数。也介绍了其他人的一些成果，这些成果分别都对CAP的某一个方面做出了特殊的贡献！

其实Lynch的论文主要就是两件事：缩小CAP适用的定义，消除质疑的场景；展示了CAP在非单一一致性结果下的广阔的研究结果！并顺便暗示CAP定理依旧正确！


</br>

# 如何看代CAP

首先肯定的是，CAP并不适合再作为一个适应任何场景的定理，它的正确性更加适合基于原子读写的NoSQL场景。质疑虽然很多，但很多质疑者只是偷欢概念，并没有解决各个因素之间的取舍问题。而无论如何C、A、P这个三个概念始终存在任何分布式系统，只是不同的模型会对其有不同的呈现，可能某些场景对三者之间的关系敏感，而另一些不敏感。
就像Lynch所说，现在分布式系统有很多特性，比如扩展性、优雅降级等，虽然时间的发展，或许这些也会被纳入研究范畴，而作为开发者，这都是我们需要考虑的问题，而不仅是CAP三者！

</br>

参考内容如下:

【0】https://blog.csdn.net/chen77716/article/details/30635543

【1】http://en.wikipedia.org/wiki/Cap_theorem
【2】http://lpd.epfl.ch/sgilbert/pubs/BrewersConjecture-SigAct.pdf
【3】http://groups.csail.mit.edu/tds/papers/Gilbert/Brewer2.pdf
【4】http://markburgess.org/blog_cap.html
【5】http://blog.cloudera.com/blog/2010/04/cap-confusion-problems-with-partition-tolerance/
【6】http://cacm.acm.org/blogs/blog-cacm/83396-errors-in-database-systems-eventual-consistency-and-the-cap-theorem/fulltext
【7】http://nathanmarz.com/blog/how-to-beat-the-cap-theorem.html
【8】http://highscalability.com/blog/2011/11/23/paper-dont-settle-for-eventual-scalable-causal-consistency-f.html

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


