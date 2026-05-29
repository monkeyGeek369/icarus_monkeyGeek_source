---
title: Servlet的多线程和线程安全
category: java基础
date: 2022-06-11 22:35:23
updated: 2022-06-11 22:35:23
enname: servlet
categories: java基础
tags:
	- 面试
keywords: servlet
permalink:
thumbnail:
---

# 什么是servlet

狭义的Servlet是指Java语言的一个接口，广义的Servlet是指任何实现了这个Servlet接口的类。<!--more-->

即作为处理web请求与java后端业务逻辑交互的桥梁，如tomcat就是servlet容器，其会将java项目中的servlet类加载至容器成为servlet对象。

</br>

# 工作流程

1. java web项目要实现servlet接口并借助servlet容器发布
2. servlet容器启动时会将servlet加载至容器或者请求来临时需要使用servlet时加载进容器
3. HTTP请求发送至servlet容器
4. servlet容器根据请求信息查询对应的servlet-name并将请求包装为HttpServletRequest类型的request对象转给servlet实例
5. servlet实例将处理结果返回给servlet容器，容器将结果信息封装为HttpServletResponse类型的response对象，容器进一步将response对象转换为http响应文本给到浏览器

</br>

# servlet生命周期

- 加载：通过java类加载器来加载servlet类并实例化一个对象，且只会实例化一次
- 初始化：容器调用初始化方法来初始化servlet对象，即根据ServletConfig来初始化
- 处理请求
- 销毁：容器关闭或重启时

</br>



# 几个重要对象

- ServletConfig：包含了servlet名称、ServletContext，初始化参数等
- ServletContext：tomcat为每个web项目都创建一个ServletContext实例，tomcat在启动时创建，服务器关闭时销毁，在一个web项目中共享数据，管理web项目资源，为整个web配置公共信息等
- request：将请求文本封装而成的对象，所以通过request能获得请求文本中的所有内容，请求头、请求体、请求行
- response：响应体



</br>

# servlet是线程安全的吗？

不是线程安全的

servlet是单实例的且是无状态的，多个请求过来即多线程下访问的是同一个servlet实例，如果servlet实例中有静态变量、静态属性等，那么多线程是可以共享的，因此不安全。



如何实现线程安全？

- 避免使用实例变量、静态变量
- 可以加锁
- 可以利用线程安全的集合等



</br>

# servlet与spring boot的关系

springboot可以自动装配springmvc，springmvc的核心类是DispatcherServlet，这个就是servlet。



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


