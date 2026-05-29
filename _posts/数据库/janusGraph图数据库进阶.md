---
title: janusGraph图数据库进阶
category: 数据库
date: 2021-04-29 16:54:07
updated: 2021-04-29 16:54:07
enname: janusGraph-adv
categories: 数据库
tags:
	- janusGraph
keywords: 图数据库，janusGraph，graph database，数据库
permalink:
thumbnail:
---

本文针对在janusGraph使用过程中遇到的使用问题、操作问题、架构问题等进行讨论。<!--more-->



</br>

# 如何进行多图管理？

janusGraph支持多图管理，依赖properties文件进行配置来实现，如下图conf中预设的众多配置文件。

![](../../../../image/janusgraph-multi.png)



**在启动服务时加载多图**

在“janusGraph图数据库入门”中介绍了如何指定yaml配置文件来启动server服务，部分内容如下：

```groovy
host: 0.0.0.0
port: 8182
scriptEvaluationTimeout: 30000
channelizer: org.apache.tinkerpop.gremlin.server.channel.WebSocketChannelizer
graphs: {
  graph: conf/janusgraph-inmemory.properties,
  ggraph: conf/janusgraph-local-inmemory.properties,
  gggraph: conf/janusgraph-local2-inmemory.properties
}
```

其中graphs集合就是我们预先配置的图实例，每一个图实例对应唯一的一个配置文件。

<u>不同的存储后端可以创建多图，相同的存储后端也可以创建多个图，加载多个properties配置即可，需要注意的是如果同样的存储后端则需要指定不同的hostname，不同的host代表不同的图，如果多个properties配置相同的hostname，即使创建了多图，实际上多图也是指向相同的一个图实例。</u>



**在启动服务后加载多图**

如果在启动服务配置中没有进行图实例配置，我们也可以使用gremlin命令获取实例。

```groovy
#通过命令生成图实例，依照指定路径配置文件进行生成实例，实例名称可自由设定
graph = JanusGraphFactory.open('conf/janusgraph-inmemory.properties')
ggraph = JanusGraphFactory.open('conf/janusgraph-local-inmemory.properties')
gggraph = JanusGraphFactory.open('conf/janusgraph-local2-inmemory.properties')
```



```groovy
#可以在console中输出图实例看一下
gremlin> graph
==>standardjanusgraph[inmemory:[127.0.0.1]]
gremlin> ggraph
==>standardjanusgraph[inmemory:[localhost]]
gremlin> gggraph
==>standardjanusgraph[inmemory:[localhost2]]
```



</br>

# 顶点/边的数据结构

- 预存属性：id，lable。其中id在创建顶点和边时无需指定将自动生成，janusgraph不支持手动指定id。lable相当于顶点/边的名称
- 自定义属性：可以通过property方法进行自定义属性添加，属性类型默认支持字符串/整型，其他特殊类型需要进行配置。



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


