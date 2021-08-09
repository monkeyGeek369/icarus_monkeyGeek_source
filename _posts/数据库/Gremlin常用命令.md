---
title: Gremlin常用命令
category: 数据库
date: 2021-04-29 16:54:45
updated: 2021-04-29 16:54:45
enname: gremlin-line
categories: 数据库
tags:
	- janusGraph
	- 面试
keywords: 图数据库，janusGraph，graph database，数据库,Gremlin
permalink:
thumbnail:
---

本文仅针对Gremlin进行常用命令介绍<!--more-->



</br>

```groovy
#连接远程服务
#:remote为远程服务连接命令
#remote.yaml文件内进行配置远程服务地址等信息
#(服务重启后需要重新连接)
:remote connect tinkerpop.server conf/remote.yaml

#脚本远程执行命令
#执行该命令后代表今后在gremlin-console执行的命令脚本都将直接在server生效，否则无法生效
:remote console

#通过命令生成图实例，依照指定路径配置文件进行生成实例，实例名称可自由设定
graph = JanusGraphFactory.open('conf/janusgraph-local-inmemory.properties')

#加载数据：graph为图实例，图实例可以在server启动时通过指定的yaml文件中的graphs配置进行生成，也可以通过命令进行生成
GraphOfTheGodsFactory.loadWithoutMixedIndex(graph, true)

#输出所有顶点
graph.traversal().V()

#通过:>可以直接将命令submit到server无需预先执行:remote console命令，不过每一个执行的命令斗都需要携带
:>命令

#获取便利对象g，这个g为默认的便利对象无法设定其他的名称
g = graph.traversal()

#添加顶点
g.addV("test1").property("name","name1").property("age",333).property("height",180)
ggraph.traversal().addV("test2").property("name","name2").property("age",333).property("height",180)
gggraph.traversal().addV("test3").property("name","name3").property("age",333).property("height",180)

#添加边
g.addE("testE1").property("length",123).from(g.V(8208)).to(g.V(20656))
ggraph.traversal().addE("testE2").property("length",123).from(g.V(8208)).to(g.V(20656))
gggraph.traversal().addE("testE3").property("length",123).from(g.V(12528)).to(g.V(4152))

```



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


