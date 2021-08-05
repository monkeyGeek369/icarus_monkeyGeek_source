---
title: rabbitmq消息积压解决思路
category: mq
date: 2021-08-03 16:58:43
updated: 2021-08-03 16:58:43
enname: mq-backlog
categories: mq
tags:
	- mq
	- rabbitmq
keywords: rabbitmq、mq、消息积压
permalink:
thumbnail:
---

# 问题场景

借助mq实现解耦、削峰、异步场景时由于多种内外部因素很有可能遇到消息积压的情况，即短时间内大量消息无法被立即消费，造成消息阻塞。<!--more-->可以参考“rabbitmq中connection与channel”了解两者关系。



</br>

# 产生原因

- 生产者生产速度大于消费者消费速度
- 消费者部分/全部异常无法消费消息
- 生产者-消费者设计原因导致积压



</br>

# 消费者异常

- 未ACK消息：由于消费者代码问题导致未能成功ACK，建议ACK操作不与业务代码关联，单独做ack
- 重复ACK同一个消息：queue中的每一个消息都有一个唯一tag且自增，如果重复消费相同tag会报io异常并引起channel关闭，虽然channel后续会重启但是有可能造成消息重复消费或者丢失问题。



</br>

# 消费者模型设计

详情可以参考“rabbitmq中SimpleMessageListenerContainer与DirectMessageListenerContainer”



简而言之消费者是需要配置监听容器的用于消息监听与消费



SimpleMessageListenerContainer适用于小业务量简单业务场景，一个消费者可以配置一个/多个channel，一个channel对应一个线程，channel可以被相同queue的不同消费者复用，但需要rabbitmq客户端线程与消费者线程之前切换比较耗费资源。



SimpleMessageListenerContainer的升级玩法是消费者中单独使用业务线程池，将不同的queue消息交由不同的业务线程池处理，channel线程仅仅做ack操作。这样做的优点是现有功能维护升级简便同时兼顾线程调度优势，缺点是同样无法避免channel线程间切换的问题。



DirectMessageListenerContainer相对来说适用于大数据量业务场景，一个消费者可以配置一个/多个channel，一个channel调用一个线程池线程，线程池可以被相同消费者的不同queue通用（主要看如何配置），当然如果每一个queue对应独立的消费者那么线程池是独立的。



</br>

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


