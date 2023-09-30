---
title: 一、hexo基础环境准备
category: hexo
date: 2019-08-10 19:38:04
updated: 2019-08-10 19:38:04
enname: environment
categories: hexo
tags: hexo
keywords: 
	- hexo
	- 环境准备
permalink:
thumbnail:
---

**简介**

根据hexo官网简介hexo是一个快速、简介且高效的博客框架。hexo使用markdown解析文章，可以在几秒内利用靓丽主题生成静态网页。<!--more-->本系列教程是以官网简介为基础再加上实际操作经验总结所写，看到这里证明大家希望搭建自己的技术了乐园，那么请细心往下看，避免后期踩坑。



**nodeJS安装**

nodejs是一款优秀的js运行时环境（类似java中的jdk），用于独立于浏览器运行js代码。

安装时建议先安装nvm然后再通过nvm进行不同版本的nodejs安装和切换使用，因为hexo版本的不同对nodejs的版本有要求。

安装教程自行搜索，本文不进行介绍。



**git安装**

关于git的系列教程小伙伴可以去看我写的git相关博文，在这里简而言之，git的安装类似于nodejs，均是傻瓜安装模式，安装成功后鼠标右键若出现git bash here即证明安装成功

注意：hexo生成的网站可以通过github进行发布成可浏览站点，这就要求自身要建立起本地同github是相关密钥配置，本文不再详述。



**hexo安装**

hexo的安装可以按照官方文档推荐的步骤进行，以下介绍一些基本知识点和注意事项：

1、hexo是基于nodejs的插件，插件的安装可以自定义安装为局部或者全局，一般安装局部就够用

2、hexo命令在安装hexo插件后或许不生效，原因是path路径未指定真实的插件路径（将 Hexo 所在的目录下的 node_modules 添加到环境变量之中即可直接使用 hexo命令）



<span style="color:red">注意：</span>本教程基于windows系统，mac或linux等系统的环境安装请参考官网 https://hexo.io/zh-cn/docs/



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


