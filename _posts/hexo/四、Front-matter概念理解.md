---
title: 四、Front-matter概念理解
category: hexo
date: 2019-08-10 20:39:52
updated: 2019-08-10 20:39:52
enname: frontmatter
categories: hexo
tags: hexo
keywords: hexo,Front-matter
permalink:
thumbnail:
---

**基础概念**

Front-matter 是文件最上方以 --- 分隔的区域，用于指定个别文件的变量，举例来说：

```
---
title: Hello World
date: 2013/7/13 20:46:25
---
```

<!--more-->

**详细参数**

| 参数       | 描述                                                         | 默认值       |
| ---------- | ------------------------------------------------------------ | ------------ |
| layout     | 布局，可指定文章布局                                         |              |
| title      | 标题，可指定文章标题                                         |              |
| date       | 建立日期                                                     | 文件建立日期 |
| updated    | 更新日期                                                     | 文件更新日期 |
| comments   | 开启文章的评论功能                                           | true         |
| tags       | 标签（不适用于分页，即新建立的分页无法识别）tags: - PS3 - Games |              |
| categories | 分类（不适用于分页，即新建立的分页无法识别）categories: - Diary |              |
| permalink  | 覆盖文章网址                                                 |              |
| keywords   | 仅用于 meta 标签和 Open Graph 的关键词（不推荐使用）         |              |

**自定义参数**

Front-matter除了以上官方参数之外我们还可以添加自定义参数，例如

```
title: 软件研发（开发）流程与规范
enname: softDevelop
category: share #source路径自定义参数
date: 2019-08-06 23:37:39
updated: 2019-08-06 23:37:39
categories: experienceShare #分类名称，影响永久路径生成
tags: experienceShare #标签名称，影响永久路径生成
keywords: 软件研发（开发）流程与规范
thumbnail: ../../image/研发流程与规范.png #缩略图
```

其中的enname为标题的英文名称，这样我们可以在永久链接中使用，以确保我们生成的html静态站点连接为村英文且保持尽可能的端。

permalink: :year/:i_month/:i_day/:enname/ # 网站的永久链接格式



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


