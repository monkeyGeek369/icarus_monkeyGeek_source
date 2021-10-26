---
title: rabbitMQ常见问题
category: mq
date: 2021-10-20 22:07:52
updated: 2021-10-20 22:07:52
enname: rabbitmq-qa
categories: mq
tags:
	- mq
	- rabbitmq
	- 面试
keywords: rabbitmq、mq、常见问题
permalink:
thumbnail:
---

## RabbitMQ是什么？ 

RabbitMQ是实现了高级消息队列协议（`AMQP`）的开源消息代理软件（亦称面向消息的中间件）。<!--more-->RabbitMQ服务器是用Erlang语言编写的，而群集和故障转移是构建在开放平台框架上的。所有主要的编程语言均有与代理接口通讯的客户端库。



## RabbitMQ特点? 

**可靠性**: RabbitMQ使用一些机制来保证可靠性， 如持久化、传输确认及发布确认等。

**灵活的路由** : 在消息进入队列之前，通过交换器来路由消息。对于典型的路由功能， RabbitMQ 己经提供了一些内置的交换器来实现。针对更复杂的路由功能，可以将多个 交换器绑定在一起， 也可以通过插件机制来实现自己的交换器。

**扩展性**: 多个RabbitMQ节点可以组成一个集群，也可以根据实际业务情况动态地扩展 集群中节点。

**高可用性** : 队列可以在集群中的机器上设置镜像，使得在部分节点出现问题的情况下队 列仍然可用。

**多种协议**: RabbitMQ除了原生支持AMQP协议，还支持STOMP， MQTT等多种消息 中间件协议。

**多语言客户端** :RabbitMQ 几乎支持所有常用语言，比如 Java、 Python、 Ruby、 PHP、 C#、 JavaScript 等。

**管理界面** : RabbitMQ 提供了一个易用的用户界面，使得用户可以监控和管理消息、集 群中的节点等。

**令插件机制** : RabbitMQ 提供了许多插件 ， 以实现从多方面进行扩展，当然也可以编写自 己的插件。



## AMQP是什么? 

RabbitMQ就是 AMQP 协议的 `Erlang` 的实现(当然 RabbitMQ 还支持 `STOMP2`、 `MQTT3` 等协议 ) 

RabbitMQ 中的交换器、交换器类型、队列、绑定、路由键等都是遵循的 AMQP 协议中相 应的概念。



## AMQP协议3层？ 

**Module Layer**:主要定义了一些客户端调用的命令，客户端可以用这些命令实现自己的业务逻辑。

**Session Layer**:主要负责客户端命令发送给服务器，再将服务端应答返回客户端，提供可靠性同步机制和错误处理。

**TransportLayer**:主要传输二进制数据流，提供帧的处理、信道服用、错误检测和数据表示等。



## AMQP模型的几大组件？ 

- 交换器 (Exchange)：消息代理服务器中用于把消息路由到队列的组件。
- 绑定 (Binding)：一套规则，告知交换器消息应该将消息投递给哪个队列。
- 队列 (Queue)：用来存储消息的数据结构，位于硬盘或内存中。



## 说说生产者Producer和消费者Consumer? 

生产者

- 消息生产者，就是投递消息的一方。
- 消息一般包含两个部分：消息体（`payload`)和标签(`Label`)。

消费者

- 消费消息，也就是接收消息的一方。
- 消费者连接到RabbitMQ服务器，并订阅到队列上。消费消息时只消费消息体，丢弃标签。



## 为什么需要消息队列？ 

用来实现：`异步处理`、`服务解耦`、`流量控制（削峰）`。



## 说说Broker服务节点、Queue队列、Exchange交换器？ 

- Broker可以看做RabbitMQ的服务节点。一般情况下一个Broker可以看做一个RabbitMQ服务器。
- Queue:RabbitMQ的内部对象，用于存储消息。多个消费者可以订阅同一队列，这时队列中的消息会被平摊（轮询）给多个消费者进行处理。
- Exchange:生产者将消息发送到交换器，由交换器将消息路由到一个或者多个队列中。当路由不到时，或返回给生产者或直接丢弃。



## 消息队列有什么优缺点 

优点：

解耦、异步、削峰

缺点：

- 系统可用性降低（会存在mq崩溃带来的风险）
- 系统复杂度提高
- 消息传递的顺序性需要额外解决
- 一致性问题需要额外解决（即一项业务涉及多个服务消费不同的mq，其中一个出现问题就会产生一致性问题）



## 如何保证消息的可靠性？ 

`生产者到RabbitMQ`：事务机制和Confirm机制，注意：事务机制和 Confirm 机制是互斥的，两者不能共存，会导致 RabbitMQ 报错。

`RabbitMQ自身`：持久化、集群、普通模式、镜像模式。

`RabbitMQ到消费者`：basicAck机制、死信队列、消息补偿机制。



## 什么是RoutingKey路由键？ 

生产者将消息发送给交换器的时候，会指定一个`RoutingKey`,用来指定这个消息的路由规则，这个`RoutingKey`需要与交换器类型和绑定键(`BindingKey`)联合使用才能最终生效。

RoutingKey` 为一个 点号'.': 分隔的字符串。比如: `java.xiaoka.show



## Binding绑定？ 

通过绑定将交换器和队列关联起来，一般会指定一个`BindingKey`,这样RabbitMq就知道如何正确路由消息到队列了。



## 交换器4种类型？ 

主要有以下4种。

- fanout:（广播模式）把所有发送到该交换器的消息路由到所有与该交换器绑定的队列中。
- direct:把消息路由到和RoutingKey完全匹配的队列中。
- topic:*匹配一个单词，#匹配多个或者0个
- headers：不依赖路由键匹配而是根据发送消息内容中的`headers`属性进行匹配，性能差，基本不用。



## 生产者消息发送过程？ 

1.`Producer`先连接到Broker,建立连接Connection,开启一个信道(Channel)。

2.`Producer`声明一个交换器并设置好相关属性。

3.`Producer`声明一个队列并设置好相关属性。

4.`Producer`通过路由键将交换器和队列绑定起来。

5.`Producer`发送消息到`Broker`,其中包含路由键、交换器等信息。

6.相应的交换器根据接收到的路由键查找匹配的队列。

7.如果找到，将消息存入对应的队列，如果没有找到，会根据生产者的配置丢弃或者退回给生产者。

8.关闭信道。

9.管理连接。



## 消费者接收消息过程？ 

1.`Producer`先连接到`Broker`,建立连接`Connection`,开启一个信道(`Channel`)。

2.向`Broker`请求消费响应的队列中消息，可能会设置响应的回调函数。

3.等待`Broker`回应并投递相应队列中的消息，接收消息。

4.消费者确认收到的消息,`ack`。

5.`RabbitMq`从队列中删除已经确定的消息。

6.关闭信道。

7.关闭连接。



### 交换器无法根据自身类型和路由键找到符合条件队列时，有哪些处理？

- mandatory ：true 返回消息给生产者。
- mandatory: false 直接丢弃。



## 死信队列

DLX，全称为 `Dead-Letter-Exchange`，死信交换器。

当消息在一个队列中变成死信 (`dead message`) 之后，它能被重新被发送到另一个交换器中，这个交换器就是 DLX，绑定 DLX 的队列就称之为死信队列。



## 导致死信的几种原因

- 消息被拒（`Basic.Reject /Basic.Nack`) 且 `requeue = false`。
- 消息TTL过期。
- 队列满了，无法再添加。



## 延迟队列

当消息被发送以后，并不想让消费者立刻拿到消息，而是等待特定时间后，消费者才能拿到这个消息进行消费。rabbitmq中并没有延迟队列但是可以借助死心队列来实现功能。



## 优先级队列？ 

- 优先级高的队列会先被消费。
- 可以通过`x-max-priority`参数来实现。
- 当消费速度大于生产速度且Broker没有堆积的情况下，优先级显得没有意义。



## 事务机制

RabbitMQ 客户端中与事务机制相关的方法有三个:

`channel.txSelect` 用于将当前的信道设置成事务模式。

`channel.txCommit` 用于提交事务 。

`channel.txRollback` 用于事务回滚,如果在事务提交执行之前由于 RabbitMQ 异常崩溃或者其他原因抛出异常,通过txRollback来回滚。



## 发送确认机制

生产者把信道设置为`confirm`确认模式,设置后，所有在该信道发布的消息都会被指定一个唯一的ID，一旦消息被投递到所有匹配的队列之后，RabbitMQ就会发送一个确认（`Basic.Ack`)给生产者（包含消息的唯一ID)，这样生产者就知道消息到达对应的目的地了。



## 消费者获取消息的方式

- 推
- 拉



## 消费者拒绝消息

- channel .basicNack(deliveryTag,false,true)

  不确认deliveryTag对应的消息。第二个参数是否应用于多消息，第三个参数是否requeue。可以nack该消费者先前接收未ack的所有消息。nack后的消息也会被自己消费到。

- channel .basicReject(deliveryTag,true)

  拒绝deliveryTag对应的消息，第二个参数是否requeue，true则重新入队列

- channel. basicRecover(true)

  是否恢复消息到队列.true重新入队并尽可能分给其他消费者，false重新入队并自己消费



## 消息传输保证层级

`At most once`:最多一次。消息可能会丢失，但不会重复传输。

`At least once`：最少一次。消息绝不会丢失，但可能会重复传输。

`Exactly once`:  恰好一次，每条消息肯定仅传输一次。



## Virtual Host

每一个RabbitMQ服务器都能创建虚拟的消息服务器，也叫虚拟主机(virtual host)，简称vhost。

默认为“/”。



## 集群中的节点类型

内存节点：ram,将变更写入内存。

磁盘节点：disc,磁盘写入操作。

RabbitMQ要求最少有一个磁盘节点。



## 队列结构 

通常由以下两部分组成

`rabbit_amqqueue_process`:负责协议相关的消息处理，即接收生产者发布的消息、向消费者交付消息、处理消息的确认(包括生产端的 confirm 和消费端的 ack) 等。

`backing_queue`:是消息存储的具体形式和引擎，并向 rabbit a`mqqueue process`提供相关的接口以供调用。



## RabbitMQ中消息可能有的几种状态

`alpha`: 消息内容(包括消息体、属性和 headers) 和消息索引都存储在内存中 。

`beta`: 消息内容保存在磁盘中，消息索引保存在内存中。

`gamma`: 消息内容保存在磁盘中，消息索引在磁盘和内存中都有 。

`delta`: 消息内容和索引都在磁盘中 。



## 生产者如何将消息可靠投递到MQ

1.Client发送消息给MQ

2.MQ将消息持久化后，发送Ack消息给Client，此处有可能因为网络问题导致Ack消息无法发送到Client，那么Client在等待超时后，会重传消息；

3.Client收到Ack消息后，认为消息已经投递成功。



## MQ如何将消息可靠投递到消费者

1.MQ将消息push给Client（或Client来pull消息）

2.Client得到消息并做完业务逻辑

3.Client发送Ack消息给MQ，通知MQ删除该消息，此处有可能因为网络问题导致Ack失败，那么Client会重复消息，这里就引出消费幂等的问题；

4.MQ将已消费的消息删除



## 保证RabbitMQ消息队列的高可用

RabbitMQ 有三种模式：`单机模式`，`普通集群模式`，`镜像集群模式`。

**单机模式**：就是demo级别的，一般就是你本地启动了玩玩儿的，没人生产用单机模式

**普通集群模式**：意思就是在多台机器上启动多个RabbitMQ实例，每个机器启动一个。

**镜像集群模式**：这种模式，才是所谓的RabbitMQ的高可用模式，跟普通集群模式不一样的是，你创建的queue，无论元数据(元数据指RabbitMQ的配置数据)还是queue里的消息都会存在于多个实例上，然后每次你写消息到queue的时候，都会自动把消息到多个实例的queue里进行消息同步。

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


