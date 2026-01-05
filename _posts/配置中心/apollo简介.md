---
title: apollo简介
category: 配置中心
date: 2021-10-27 19:57:09
updated: 2021-10-27 19:57:09
enname: apollo-base
categories: 配置中心
tags:
	- apollo
	- 面试
keywords: apollo，配置中心，面试
permalink:
thumbnail:
---

# 什么是配置中心

配置中心将配置从应用中剥离出来，统一管理，优雅的解决了配置的动态变更、持久化、运维成本等问题。<!--more-->应用自身既不需要去添加管理配置接口，也不需要自己去实现配置的持久化，更不需要引入“定时任务”以便降低运维成本。总得来说，配置中心就是一种统一管理各种应用配置的基础服务组件。

</br>

# Apollo简介

Apollo（阿波罗）是携程框架部门研发的分布式配置中心，能够集中化管理应用不同环境、不同集群的配置，配置修改后能够实时推送到应用端，并且具备规范的权限、流程治理等特性，适用于微服务配置管理场景。

- 统一管理不同环境、不同集群的配置
- 配置修改实时生效（热发布）
- 版本发布管理
- 灰度发布
- 权限管理、发布审核、操作审计
- 客户端配置信息监控
- 提供 Java和.Net原生客户端
- 提供开放平台 API



![](../../../../image/apollo-base.png)

- Config Service提供配置的读取、推送等功能，服务对象是Apollo客户端
- Admin Service提供配置的修改、发布等功能，服务对象是Apollo Portal（管理界面）
- Config Service和Admin Service都是多实例、无状态部署，所以需要将自己注册到Eureka中并保持心跳
- 在Eureka之上架了一层Meta Server用于封装Eureka的服务发现接口
- Client通过域名访问Meta Server获取Config Service服务列表（IP+Port），而后直接通过IP+Port访问服务，同时在Client侧会做load balance、错误重试
- Portal通过域名访问Meta Server获取Admin Service服务列表（IP+Port），而后直接通过IP+Port访问服务，同时在Portal侧会做load balance、错误重试
- 为了简化部署，我们实际上会把Config Service、Eureka和Meta Server三个逻辑角色部署在同一个JVM进程中

</br>

# Admin Service

- 提供配置管理接口
- 提供配置修改、发布等接口
- 接口服务对象为Portal

</br>

# Config Service

- 提供配置获取接口
- 提供配置更新推送接口（基于Http long polling）
  - 服务端使用[Spring DeferredResult](http://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/web/context/request/async/DeferredResult.html)实现异步化，从而大大增加长连接数量
  - 目前使用的tomcat embed默认配置是最多10000个连接（一个应用实例只会发起一个长连接）。
- 接口服务对象为Apollo客户端

</br>

# Meta Server

- Portal通过域名访问Meta Server获取Admin Service服务列表（IP+Port）
- Client通过域名访问Meta Server获取Config Service服务列表（IP+Port）
- Meta Server从Eureka获取Config Service和Admin Service的服务信息，相当于是一个Eureka Client
- 增设一个Meta Server的角色主要是为了封装服务发现的细节，对Portal和Client而言，永远通过一个Http接口获取Admin Service和Config Service的服务信息，而不需要关心背后实际的服务注册和发现组件
- Meta Server只是一个逻辑角色，在部署时和Config Service是在一个JVM进程中的，所以IP、端口和Config Service一致

</br>

# Eureka

- 基于[Eureka](https://github.com/Netflix/eureka)和[Spring Cloud Netflix](https://cloud.spring.io/spring-cloud-netflix/)提供服务注册和发现

- Config Service和Admin Service会向Eureka注册服务，并保持心跳

- 为了简单起见，目前Eureka在部署时和Config Service是在一个JVM进程中的（通过Spring Cloud Netflix）

  

  为什么我们采用Eureka作为服务注册中心，而不是使用传统的zk、etcd呢？我大致总结了一下，有以下几方面的原因：

  - 它提供了完整的Service Registry和Service Discovery实现
    - 首先是提供了完整的实现，并且也经受住了Netflix自己的生产环境考验，相对使用起来会比较省心。
  - 和Spring Cloud无缝集成
    - 我们的项目本身就使用了Spring Cloud和Spring Boot，同时Spring Cloud还有一套非常完善的开源代码来整合Eureka，所以使用起来非常方便。
    - 另外，Eureka还支持在我们应用自身的容器中启动，也就是说我们的应用启动完之后，既充当了Eureka的角色，同时也是服务的提供者。这样就极大的提高了服务的可用性。
    - 为了提高配置中心的可用性和降低部署复杂度，我们需要尽可能地减少外部依赖。
  - Open Source
    - 最后一点是开源，由于代码是开源的，所以非常便于我们了解它的实现原理和排查问题。

</br>

# Portal

- 提供Web界面供用户管理配置
- 通过Meta Server获取Admin Service服务列表（IP+Port），通过IP+Port访问服务
- 在Portal侧做load balance、错误重试

</br>

# Client

- Apollo提供的客户端程序，为应用提供配置获取、实时更新等功能
- 通过Meta Server获取Config Service服务列表（IP+Port），通过IP+Port访问服务
- 在Client侧做load balance、错误重试

</br>

# 定时拉取配置

- 这是一个fallback机制，为了防止推送机制失效导致配置不更新
- 客户端定时拉取会上报本地版本，所以一般情况下，对于定时拉取的操作，服务端都会返回304 - Not Modified
- 定时频率默认为每5分钟拉取一次，客户端也可以通过在运行时指定System Property: `apollo.refreshInterval`来覆盖，单位为分钟。



</br>

# 长轮训拉取配置

### Config Service配置服务端

- 用户在Portal操作配置发布
- Portal调用Admin Service的接口操作发布
- Admin Service在配置发布后会往ReleaseMessage表插入一条消息记录，消息内容就是配置发布的AppId+Cluster+Namespace。
- Config Service有一个线程会每秒扫描一次ReleaseMessage表，看看是否有新的消息记录，
- Config Service如果发现有新的消息记录，那么就会通知到所有的消息监听器（[ReleaseMessageListener](https://github.com/ctripcorp/apollo/blob/master/apollo-biz/src/main/java/com/ctrip/framework/apollo/biz/message/ReleaseMessageListener.java)），如[NotificationControllerV2](https://github.com/ctripcorp/apollo/blob/master/apollo-configservice/src/main/java/com/ctrip/framework/apollo/configservice/controller/NotificationControllerV2.java)
- NotificationControllerV2得到配置发布的AppId+Cluster+Namespace后，会通知对应的客户端



### client客户端

- 客户端会发起一个Http请求到Config Service的`notifications/v2`接口
- NotificationControllerV2不会立即返回结果，而是通过[Spring DeferredResult](http://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/web/context/request/async/DeferredResult.html)把请求挂起。DeferredResult字面意思就是推迟结果，是在servlet3.0以后引入了异步请求之后，spring封装了一下提供了相应的支持，也是一个很老的特性了。
- DeferredResult技术中会把当前的客户端访问的线程hold住，如果在60秒内没有该客户端关心的配置发布，那么会返回Http状态码304给客户端。客户端继续轮训重复以上步骤。
- 如果有该客户端关心的配置发布，NotificationControllerV2会调用DeferredResult的[setResult](http://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/web/context/request/async/DeferredResult.html#setResult-T-)方法，传入有配置变化的namespace信息，同时该请求会立即返回。如果配置有变更，就会返回变更信息，然后向定时任务线程池提交一个任务，任务内容是执行 sync 方法。
- 客户端从Apollo配置中心服务端获取到应用的最新配置后，会保存在内存中
- 客户端会把从服务端获取到的配置在本地文件系统缓存一份
  - 在遇到服务不可用，或网络不通的时候，依然能从本地恢复配置



### 总结一下：

1. 为什么不使用消息系统？太复杂，杀鸡用牛刀。
2. 为什么不用 TCP 长连接？对网络环境要求高，容易推送失败。且有双写问题。
3. 为什么使用 HTTP 长轮询？性能足够，结合 Servlet3 的异步特性。直接使用 Servlet 的 HTTP 协议，比单独用 TCP 连接方便。HTTP 请求/响应模式，保证了不会出现双写的情况。默认情况下apollo的长轮询是基于http的异步响应的，一个tomcat的默认最大连接数是10000，所以一个configservice进程支持最大10000个连接是没有问题的。

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


