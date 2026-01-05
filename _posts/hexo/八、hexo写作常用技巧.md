---
title: 八、hexo写作常用技巧
category: hexo
date: 2019-08-10 21:47:18
updated: 2019-08-10 21:47:18
enname: hexoskill
categories: hexo
tags: hexo
keywords: hexo,写作技巧
permalink:
thumbnail:
---

**1、文章折叠问题**

在待编辑的文章md中加入

```
 <!-- more -->
```

这个标记是hexo自带并可识别的，该标记之后的内容将被折叠，如果该标记之前无文字内容，则标记无效

<!--more-->

**2、md支持html标签以及内嵌css样式，因此我们可以自由发挥，如下是“版权声明”的内嵌html案例**

```html
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
```

**3、利用categories文章分类来实现自定义菜单**

以创建"经验分享"菜单分类为例进行讲解:

- 首先新建文章，如命令hexo new post “mymd”
- 打开文章，添加categories分类为“share”
- 执行生成html命令，hexo g
- 这样就会在public文件夹内生成categories/share文件夹
- 在主题的menu添加自定义主题如“经验分享: categories/share”
- 访问页面点击“经验分享”即可查看指定目录下的所有文章

**4、md文档头部参数设置范例**

```
title: #主题名称
category: hexo #new_post_name中指定的自定义参数
date: 2019-08-10 16:15:14
updated: 2019-08-10 16:15:14
enname: hexoskill
categories: #分类名称，可多个
    - hexo
tags: #标签名称，可多个
    - hexo
keywords: #关键字，可多个
    - hexo
    - icarus
    - gitalk
permalink: #永久链接，可为空
thumbnail: #缩略图路径，可为空，可以是相对路径
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


