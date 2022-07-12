---
title: Eureka原理
category: spring
date: 2022-06-25 14:14:30
updated: 2022-06-25 14:14:30
enname: eureka
categories: spring
tags:
	- 面试
	- eureka
keywords: eureka
permalink:
thumbnail:
---

# 简介

服务注册中心，包含服务发现、服务治理等。<!--more-->

- 服务端：提供服务注册服务，缓存服务信息
- 客户端：服务提供者、服务消费者。会拉取、更新和缓存 Eureka Server 中的服务信息

Eureka Client 和 Eureka Server 可以使用 JSON/XML 格式进行通讯。在默认情况下 Eureka Client 使用压缩 JSON 格式来获取注册列表的信息。

</br></br>

# 工作流程

1、Eureka Server 启动成功，等待服务端注册。在启动过程中如果配置了集群，集群之间定时通过 Replicate 同步注册表，每个 Eureka Server 都存在独立完整的服务注册表信息

2、Eureka Client 启动时根据配置的 Eureka Server 地址去注册中心注册服务

3、Eureka Client 会每 30s 向 Eureka Server 发送一次心跳请求，证明客户端服务正常

4、当 Eureka Server 90s 内没有收到 Eureka Client 的心跳，注册中心则认为该节点失效，会注销该实例

5、Eureka Server 在运行期间会去统计心跳失败比例在 15 分钟之内是否低于 85%，如果低于 85%，Eureka Server 即会进入自我保护机制。

6、当 Eureka Client 心跳请求恢复正常之后，Eureka Server 自动退出自我保护模式

7、Eureka Client 定时30s全量或者增量从注册中心获取服务注册表，并且将获取到的信息缓存到本地

8、服务调用时，Eureka Client 会先从本地缓存找寻调取的服务。如果获取不到，先从注册中心刷新注册表，再同步到本地缓存

9、Eureka Client 获取到目标服务器信息，发起服务调用

10、Eureka Client 程序关闭时向 Eureka Server 发送取消请求，Eureka Server 将实例从注册表中删除

</br></br>

# 服务剔除机制

- 如果开启了自我保护机制，不做操作，不剔除服务
- 如果未开启，根据 lastUpdateTimestamp 收集已过期的服务，加入到List集合中
- 服务剔除机制开启，则Eureka服务端并不会直接剔除所有已过期的服务，而是通过随机数的方式进行剔除，避免自我保护开启之前将所有的服务（包括正常的服务）给剔除。

</br></br>

# 自我保护机制

统计心跳失败比例在 15 分钟之内是否低于 85%，如果低于 85%，Eureka Server 即会进入自我保护机制。

(1 不再从注册列表中移除过期服务
(2 仍然能够接受新服务的注册和查询请求，但是不会被同步到其它节点上(即保证当前节点依然可用)
(3 当网络稳定时，当前实例新的注册信息会被同步到其它节点中

</br></br>

# 集群原理

- 集群相互之间通过 Replicate 来同步数据
- 不区分主从节点
- 节点之间互相注册来提高可用性
- 节点之间状态采用异步方式同步，保持最终状态一致（AP原理）
- 提供了 Region（地理位置） 和 Zone（机房） 两个概念来进行分区。zone内的服务优先内部同步，否则可以跨zone信息同步。



注册中心收到注册信息后会判断是否是其他注册中心同步的信息还是客户端注册的信息，如果是客户端注册的信息，那么他将会将该客户端信息同步到其他注册中心去；否则收到信息后不作任何操作。通过此机制避免集群中信息同步的死循环。

</br></br>

# 服务注册（心跳/续约）

- 客户端启动后创建一些定时任务，其中就有心跳定时任务，通过线程池维护，默认每30s一次
- renew方法会发送一个心跳数据到服务端，包括应用名、instanceInfo、instanceInfo的id等
- 200表示成功，如果返回404则证明服务未注册，需要注册服务
- register方法向服务端的注册信息instanceInfo。包括服务名、ip、端口、唯一实例ID等信息。
- 返回204表示注册成功

</br></br>

# 服务端保存服务

- 双层map保存客户端服务。ConcurrentHashMap<String, Map<String, Lease<InstanceInfo>>>
- 外层map：key是appName,value是服务实例map
- 内层map：key是服务实例编号，value是服务实例

</br></br>

# 服务拉取

- 客户端启动后创建一些定时任务，其中就有服务拉取定时任务
- 默认每60s拉取一次
- 每次拉取后刷新本地已保存的信息，需要使用时直接从本地直接获取
- 获取的方式有两种，全量获取和增量获取。第一次全量获取，后续增量获取；获取到服务器注册实例信息后，保存、或跟新到本地



参考资料

[1]https://blog.csdn.net/u012105931/article/details/104659073



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


