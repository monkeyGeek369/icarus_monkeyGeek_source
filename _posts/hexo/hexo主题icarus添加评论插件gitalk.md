---
title: hexo主题icarus添加评论插件gitalk
enname: icarus
category: hexo
date: 2019-08-10 16:15:14
updated: 2019-08-10 16:15:14
categories: 
	- hexo
tags: 
	- hexo
keywords: 
	- hexo
	- icarus
	- gitalk
permalink:
thumbnail: ../../image/addgitalk.png
---

# 一、gitalk简介

我们在利用github搭建个人博客的时候通常需要添加评论功能，以方便博主同其他志同道合的朋友进行交流。<!--more-->github本身是提供第三方api接入服务的，一些第三方服务提供方可以利用github提供的接口针对具体某github账户下的仓库进行操作，比如我们想要实现通过第三方服务针对自身账户下的某仓库进行远程添加issue问题功能等。

正巧，github内总是有项目能够满足我们的需求。gitalk正是针对github评论功能而开发的第三方服务项目，除此之外还有gitment等，但是gitment很久没有维护了，本片文章针对gitalk进行详细应用介绍。



# 二、需求场景介绍

gitalk如何自定义配置到自己的博客？具体的操作大家可以访问gitalk的[原网址教程](https://github.com/gitalk/gitalk)，介绍的很详细，使用也相对简单。不过要说声抱歉的是，由于本人使用的hexo模板是[icarus](https://github.com/ppoffice/hexo-theme-icarus)，而这个模板其实已经帮助使用者将各种评论插件集成，例如gitalk，gitment，isso，disqus等，使用者只需要在主题的配置文件_config.yml中配置好相应的参数即可。



# 三、具体步骤

### 3.1、创建github授权应用

登录github后（首先需要有github账户），一次点击settings/Developer settings/OAuth Apps，选择New OAuth App，将会看到如下界面：

application name:自行取名，建议英文

Homepage URL:自己博客的地址即可，如https://monkeyGeek369.github.io

description:输入描述

callback URL:服务授权回调地址，也填写博客地址即可，如https://monkeyGeek369.github.io

![](../../../../image/oauthappcreat.png)

创建成功后将看到如下界面：

![](../../../../image/oauthappsuccess.png)

其中Client ID和Client Secret将是我们后面用到的。



### 3.2、在主题中配置id和密钥

进入主题文件夹，打开_config.yml，进行如下设置：

```
comment:
    # Name of the comment plugin
    type: gitalk
    owner: monkeyGeek369         # github用户名
    repo: monkeyGeek369.github.io          # github仓库名称，即博客仓库名称
    client_id:      # (required) OAuth application client id
    client_secret:  # (required) OAuth application client secret
    admin: monkeyGeek369         # 指定本仓库的管理方账户名称，即拥有权限去调用授权接口的github账户
```



### 3.3、博客仓库issue功能开启

点击博客仓库---点击settings，进行如下勾选设置

![](../../../../image/gitalkissue.png)



### 3.4、添加第一条评论

进入博客文章页面，添加第一条博客即可

![](../../../../image/addgitalk.png)



# 四、问题解决

### 4.1、初始化问题

![](../../../../image/gitalkerror.png)

我们在成功配置好评论插件后或许会出现以上问题，这种问题在gitalk中有详细介绍，大家也可以去gitalk查看详细解决方案。这种问题通常是由于博主未给文章评论初始化，博主只需要登陆github账户即可。

### 4.2、面对issues时选择登录github账户后出现404错误

这个问题是因为github的issue问题label不能超过50个字符，label通常由gitalk自动取自当前博文页面的URL的pathname，所以必须确保我们的pathname足够短。需要准寻如下规则;

- 需要在hexo的_config.yml中设置永久链接规则，如permalink: :year/:id/ # 网站的永久链接格式
- 文章的永久链接尽量不要使用:title，如果非要使用那么尽可能使用英文，因为中文解析出来字符会非常多
- 关于这个问题可以去看本站点内的“五、关于写作的那些事”

关于这个问题还有其它解决方案，例如在gitalk配置文件内将lable的获取规则进行修改，进行md5加密等。详情可以看[问题连接](https://github.com/gitalk/gitalk/issues/115#event-1539518527)





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


