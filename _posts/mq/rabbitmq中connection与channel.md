---
title: rabbitmq中connection与channel
category: mq
date: 2021-08-03 17:31:40
updated: 2021-08-03 17:31:40
enname: co_ch
categories: mq
tags:
	- mq
	- rabbitmq
keywords: rabbitmq、connection、channel
permalink:
thumbnail:
---

本文重点讲解RabbitMQ中Connection与Channel的作用及相互关系并顺带理清channel与线程的配置方案。

<!--more-->

</br>

# 生产者生产流程

1. 建立链接（Connection）
2. 在链接（Connection）上开启一个信道（Channel）
3. 声明一个交换机（Exchange）
4. 声明一个队列（Queue）
5. 使用路由键（RoutingKey）将队列（Queue）和交换机（Exchange）绑定起来
6. 根据路由键（RoutingKey）发送消息到交换机（Exchange）
7. [MQ]根据交换机（Exchange）和路由键（RoutingKey），将消息或存放到队列（Queue），或丢弃，或回给生产者
8. 关闭信道（Channel）
9. 关闭链接（Connection）
   

</br>

# 消费者消费流程

1. 建立链接（Connection）
2. 在链接（Connection）上开启一个信道（Channel）
3. 请求消费指定队列（Queue）的消息，并设置回调函数（onMessage）
4. [MQ]将消息推送给消费者
5. 消费者发送消息确定（Ack[acknowledge]）
6. [MQ]删除被确认的消息
7. 关闭信道（Channel）
8. 关闭链接（Connection）



</br>

# Connection与Channel的关系

以SimpleMessageListenerContainer为例来看下生产与消费关系，如下图：

![](../../../../image/RabbitMQ中connection与channel.png)



<u>***生产者发送消息***</u>

- 通常一个应用创建一个connection连接（通过TCP实现）当然可以创建多个
- 一个connection可被多个channel复用
- 一个channel是单线程的
- channel单线程串行发送消息



**<u>消费者消费消息-单线程</u>**

- 通常一个应用创建一个connection连接（通过TCP实现）当然可以创建多个
- 一个connection可被多个channel复用
- 一个channel是单线程的
- channel单线程串行消费消息



**<u>消费者消费消息-多线程</u>**

- 通常一个应用创建一个connection连接（通过TCP实现）当然可以创建多个
- 分布式服务创建各自对应的connection
- 一个connection可被多个channel复用
- 设置参数concurrency来控制线程数，相当于为每一个消费者创建了多个channel
- 消费者对应的多个channel可以被不同的微服务共享，例如针对消费者consumer-a的Channel-a/Channel-b可以被微服务Application-b/Application-b1公用，只是线程不同
- channel单线程串行消费消息

SimpleMessageListenerContainer并发的实现并非依赖线程池而是通过对消费者创建多个channel来实现多线程，每个channel依然是单线程串行执行的。



</br>

# 非阻塞I/O多路复用

connection是建立在TCP之上的虚拟连接，多个channel共用同一个connection，这样做的好处是减少了频繁的channel开启或关闭造成的TCP资源浪费。多个channel复用同一个connection。



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


