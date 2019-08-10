---
title: 七、hexo中md文档插入图片问题
category: hexo
date: 2019-08-10 21:28:40
updated: 2019-08-10 21:28:40
enname: mdimage
categories: hexo
tags: hexo
keywords: hexo,md文档,插入图片
permalink:
thumbnail:
---

**插入图片的途径**

我们知道md文档仅仅是作为文字信息的载体，如果需要插入类似图片这种附件对象则需要通过其他手段，方法如下：

<!--more-->

1、将图片放入外部服务器，md文档中引用图片链接即可

2、将图片放在博客静态站点内，md文档使用相对路径引用即可

总之，图片的引用一定是引用连接的形式



**如何实现高效插入图片（以相对路径的方式）**

1、手动在source文件夹内创建如image名称的文件夹

2、将需要引用的图片放入该文件夹

3、使用md编辑器Typora打开需要编辑的文章md文件，选择插入图片并选择image中的图片即可

<span style="color:red">注意：</span>（假设source子目录存在image文件夹，用于存放图片）

<span style="color:red">1、使用html标签插入图片路径问题</span>

使用html标签插入图片需满足如下条件：

- hexo中的_config.yml配置永久链接，如permalink: :year/:i_month/:i_day/:enname/ # 网站的永久链接格式

- 由永久链接对应的public生成路径为“年/月/日/文章英文名称”

- md文件中插入图片，设置路径如下

  ```html
  <img style="height:auto; width:auto;flot:left;" src="../../../../image/monkey64.png" />
  ```

  注意有四个跳出，因为source中的image文件夹在执行hexo g后将在public一级子目录下生成对应的image文件夹



<span style="color:red">2、使用md直接插入图片路径问题</span>

与html插入图片不同，如果直接通过md插入图片，在设置相对路径时只需要根据source文件夹下的md文件与image文件夹相对路径设置即可。

例如

```
thumbnail: ../../image/mdmenuset.png
```

如果在内容区插入图片依然要依据永久链接相对路径如

```
![](../../../../image/githubblogtheme.png)
```





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


