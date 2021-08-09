---
title: janusGraph图数据库入门
category: 数据库
date: 2021-04-29 14:14:25
updated: 2021-04-29 14:14:25
enname: janusGraph-brief
categories: 数据库
tags:
	- janusGraph
	- 面试
keywords: 图数据库，janusGraph，graph database，数据库
permalink:
thumbnail: ../../image/janusgraph-layer.png
---

# 简介

JanusGraph是一款分布式、开源、可伸缩的图数据库。其伸缩性适用于千亿级图存储和查询，并能够将顶点、边分布式存储在多个集群中。<!--more-->

JanusGraph隶属于Linux基金，由其负责开发和维护，参与者包括谷歌、IBM、亚马逊等。

JanusGraph重点关注紧凑的图形序列化、丰富的图形数据建模和高效的查询执行。在批量图处理和图分析方面借助Hadoop实现并对客户端连接、数据索引查询、数据持久化等提供健壮性接口。JanusGraph的模块化架构允许它与广泛的存储、索引和客户端技术进行互操作。

进一步的详细介绍可参考官网[1]



</br>

# 架构

![](../../../../image/janusgraph-layer.png)

- 图分析支持（OLAP）：基于Tinkerpop开发，支持以Gremlin图语言为基础的图数据分析
- 图事务支持（OLTP）：基于Tinkerpop开发，支持以Gremlin图语言为基础的图事务管理
- API管理：对外提供完备的API支持和管理
- 存储后端：用于图数据存储
- 索引后端：用于图数据搜索



**存储后端支持：**

- Apache Cassandra
- Apache HBase
- Oracle Berkeley DB Java Edition



**索引后端支持：**

- Elasticsearch
- Apache Solr
- Apache Lucene



**JanusGraph目前支持两种接入方式：**

嵌入式：JanusGraph服务可以与应用程序在相同的JVM中，所有的缓存、事务等均在此JVM中，存储后端和索引后端可以连接本地或远程。

命令试：JanusGraph服务可以单独部署，部署成功后会长期运行JanusGraph服务进行，外部应用可以借助Gremlin命令进行图数据库实例获取与使用。



</br>

# 基础使用

本事例仅仅以单机的形式本地部署JanusGraph服务，使用内存作为存储后端，暂无索引后端，采用gremlin-visualizer/graphexp进行可视化展示。以上配置可以方便初入者更快的了解。



- 服务端开启

首先从官网gitpub下载release程序包https://github.com/JanusGraph/janusgraph/releases

然后执行bin文件夹中的gremlin-server.sh文件（也可以执行bat类型文件，不同操作系统如何执行sh/bat自行百度）

```groovy
#启动默认配置-hostname：127.0.0.1，链接：http://127.0.0.1，端口：8182，不进行图实例加载
bin/gremlin-server.sh 
```



```groovy
#根据自定义配置文件yaml启动
#可以自由配置hostname、链接、端口
#yaml中可以配置多种图实例graphs（properties文件），启动后将自动进行图实例加载
bin/gremlin-server.sh ./conf/gremlin-server/socket-gremlin-server.yaml
```



- Gremlin-console开启

console作为Gremlin命令的执行客户端需要启动，在启动后可以单独执行Gremlin命令也可以将执行命令提交到已经链接的server服务端生效。

```groovy
#启动console服务
bin/gremlin.sh

#连接远程服务
#:remote为远程服务连接命令
#remote.yaml文件内进行配置远程服务地址等信息
#(服务重启后需要重新连接)
:remote connect tinkerpop.server conf/remote.yaml

#脚本远程执行命令
#执行该命令后代表今后在gremlin-console执行的命令脚本都将直接在server生效，否则无法生效
:remote console

```



- 测试数据加载

加载内置的测试图数据

```groovy
#通过命令生成图实例，依照指定路径配置文件进行生成实例，实例名称可自由设定
graph = JanusGraphFactory.open('conf/janusgraph-local-inmemory.properties')

#加载数据：graph为图实例，图实例可以在server启动时通过指定的yaml文件中的graphs配置进行生成，也可以通过命令进行生成
GraphOfTheGodsFactory.loadWithoutMixedIndex(graph, true)
```



- 可视化客户端启动

以gremlin-visualizer为例（其他可视化工具大同小异）具体操作如下：

首先官网下载gremlin-visualizerhttps://github.com/prabushitha/gremlin-visualizer

```groovy
#克隆项目
git clone https://github.com/prabushitha/gremlin-visualizer.git

#可视化插件依赖nodejs因此需要先自行安装nodejs和npm
#进入项目文件夹执行npm命令
npm install

#进入项目文件夹执行npm启动命令
npm start

#启动成功后可访问如下地址查看
http://localhost:3000
```



- 图查询命令

在可视化工具中输入查询命令即可，或者在console中输入查询命令可输出结构化原始数据

```groovy
#输出所有顶点
graph.traversal().V()

```



</br>

# 基本概念讲解

- OLTP：on-line transaction processing联机事务处理，用在图数据库
- OLAP：On-Line Analytical Processing联机分析处理，用在图数据分析
- 图：相当于mqsl中的数据库
- 图数据库中没有表的概念，同一个图中无关联的子图可以理解为表



</br>

参考资料：

[1]https://janusgraph.org

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


