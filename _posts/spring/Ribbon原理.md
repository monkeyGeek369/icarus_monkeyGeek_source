---
title: Ribbon原理
category: spring
date: 2022-07-10 22:25:09
updated: 2022-07-10 22:25:09
enname: ribbon
categories: spring
tags:
	- 面试
	- ribbon
keywords: ribbon, spring
permalink:
thumbnail:
---

# 简介

客户端负载均衡<!--more-->

客户端会有一个服务器地址列表，在发送请求前通过负载均衡算法选择 一个服务器，然后进行访问

</br></br>



# 如何获取服务列表

1. 通过eurekaClientProvider获取对应EurekaClient（由Spring容器帮忙注入的，并不是由Ribbon主动构造的）
2. 调用EurekaClient获取对应注册表集合信息（EurekaClient是从本地的注册表中获取了指定服务的注册表，其中参数的`vipAddress`就是我们要调用的服务的appId）
3. 将注册信息组装成`DiscoveryEnabledServer`列表
4. 将service加到BaseLoadBalancer中的ServersList中

</br></br>



# 如何更新服务列表

### 定时拉取

开启一个后台定时线程，默认延迟1s执行，每30s执行一次



### 事件通知

Ribbon使用EurekaNotificationServerListUpdater实现类进行更新，首先会创建一个Eureka监听器，当接口接受到通知事件之后，会将更新逻辑提交到线程池中执行。

</br></br>



# 对服务进行心跳检测

默认采用了串行的方式进行检测

我们也可以通过自主实现检测结构实现并行的检测





</br></br>

# 负载均衡策略

| 策略                      | 描述                                                         |
| ------------------------- | ------------------------------------------------------------ |
| RoundRobinRule            | 轮训策略                                                     |
| RandomRule                | 随机                                                         |
| RetryRule                 | 重试：选择一个Server，如果失败，重新选择一个Server重试       |
| BestAvailableRule         | 最小并发：过滤出故障服务器后，选择一个并发量最小的           |
| WeightedResponseTimeRule  | 响应时间加权轮询                                             |
| AvailabilityFilteringRule | 可用过滤策略：过滤出故障的或高并发的服务实例，然后再以线性轮询的方式从过滤后的实例清单中选出一个; |
| ZoneAvoidanceRule         | 区域规避策略：从最佳区域实例集合中选择一个最优性能的服务实例 |

</br></br>



# 其它介绍

- 饥饿加载：默认懒加载，意味着只有在发起调用的时候才会创建客户端。可以开启饥饿加载，在项目启动时就完成初始化工作，解决第一次调用慢的问题



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


