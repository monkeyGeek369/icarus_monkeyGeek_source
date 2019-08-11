---
title: 九、github自建博客实现域名绑定与解析
category: hexo
date: 2019-08-11 23:12:09
updated: 2019-08-11 23:12:09
enname: githubdomain
categories: hexo
tags: 
	- hexo
	- github
keywords: hexo,github,域名绑定
permalink:
thumbnail: ../../image/githubdomain1.png
---

**用户域名绑定**

总所周知我们通过github来实现自建博客利用的其实是github提供的githubpage服务，github page本身是支持自定义域名绑定的，如果我们觉的使用“你的用户名.github.io”来访问博客站点比较费力可以绑定自己购买到的域名。

<!--more-->

<span style="color:red">如何购买域名？</span>

购买域名非常简单，渠道也非常多，例如可以去万网、阿里云、腾讯云、华为域名等等地方去购买一个域名，这里就不详细介绍了。



<span style="color:red">如何获取博客站点的真实ip地址？</span>

这里先说一下如何获取博客站点的真实ip地址，因为我们在购买域名后需要做域名解析，说白了就是告诉DNS服务器，我们的这个域名应该指向哪里，可以指向某ip地址也可以指向其它域名。

windows机器在屏幕左下角搜索框输入cmd，进入命令控制工具，

```
输入“ping   www.你的用户名.github.io”
```

或者

```
输入“ping   你的用户名.github.io”
```

都会看到对应的ip地址，比较神奇的是这两种方式竟然得到不一样的地址，这个本人也是不清楚，不知道github内部的处理和部署逻辑是什么，不过都一样使用。

![](../../../../image/githubdomain1.png)



<span style="color:red">如何解析域名？</span>

在我们得到博客站点的ip后就可以去告诉DNS如何进行指向了，由于本人是在阿里云购买的域名，所以这里以阿里云域名解析为例，我详细这种都大同小异，其它厂商提供的设置方式也是相同。

![](../../../../image/githubdomain2.png)

如上图所示就做好了域名解析，现在做下简单介绍：

- @表示可以直接通过如monkeygeek.cn来访问绑定的ip
- 解析路线如果没有特殊要求选择“默认”即可，其实区别不大



<span style="color:red">如何将域名绑定至github page？</span>

进入博文仓库---settings

![](../../../../image/githubdomain3.png)

拉到页面底部的位置进行github page设置

![](../../../../image/githubdomain4.png)



<span style="color:red">绑定成功验证</span>

浏览器输入你的域名访问即可，需要注意的是域名解析在配饰好之后有一段时间的等待期，大概十分钟，所以请耐心等待。







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


