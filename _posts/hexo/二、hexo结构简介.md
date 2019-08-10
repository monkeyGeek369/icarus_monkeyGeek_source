---
title: 二、hexo结构简介
category: hexo
date: 2019-08-10 19:43:25
updated: 2019-08-10 19:43:25
enname: structure
categories: hexo
tags: hexo
keywords:
	- hexo
	- 结构
permalink:
thumbnail: ../../image/hexostructure.png
---

**hexo结构**

当hexo安装成功后其结构如下图：<!--more-->

![](../../../../image/hexostructure.png)



**结构简介**

**node_modules:**nodejs根据hexo的依赖组件而下载的各种工具包

**public:**我们写作的md文档文件均存放子啊source，public是根据source文件夹内的md文档生成的对应静态html等静态网站数据

**scaffolds:**模板文件夹，我们在写各种md文档时可以根据模板文档来生成对应的md文档，默认有post（文档）、page（自定义页面）、draft（草稿）三种模板

**source:**存放有md文档，其实我们的创作成果体现，后面文章将详细介绍

**themes:**主题文件夹，用于存放我们clone的各种主题。hexo在安装时将自动下载“landscape”主题

**package.json：**hexo的应用程序信息，包含版本信息、依赖插件信息等

**package-lock.json：**hexo详细的依赖工具包信息

**_config.yml:****其为hexo的配置文件，下面将详细介绍。**



<span style="color:red">提前注意：配置文件中参数的设置必须要加入空格，例如title: monkeyGeek # 网站标题    ：之后加入空格，#之后加入空格等</span>



**hexo样例_config.yml**

```
# Hexo Configuration
## Docs: https://hexo.io/docs/configuration.html
## Source: https://github.com/hexojs/hexo/

# Site
title: monkeyGeek # 网站标题
subtitle: monkeyGeek # 网站副标题
description: java,软件,IT,学习,分享,成长,activit,AI,android,协议,线程与进程,系统装机,算法,数据库,软件基础,权限,其它,跨域,康波定律,开发标准,集合框架,分布式与架构,springcloud,svn,UML,安全,annotation,scikit-Lean,spring,springboot,RabbitMQ,python,PD,OSGI,numpy,nosql,nginx,mybatis,maven,linux,js,idea,hexo,github,git,feign,eureka,ELK,chrome,BIM,apollo # 网站描述，主要用于SEO搜索，关键词罗列方便搜索
keywords: java,软件,IT,学习,分享,成长,activit,AI,android,协议,线程与进程,系统装机,算法,数据库,软件基础,权限,其它,跨域,康波定律,开发标准,集合框架,分布式与架构,springcloud,svn,UML,安全,annotation,scikit-Lean,spring,springboot,RabbitMQ,python,PD,OSGI,numpy,nosql,nginx,mybatis,maven,linux,js,idea,hexo,github,git,feign,eureka,ELK,chrome,BIM,apollo# 网站关键词，以半角逗号分隔多个关键词
author: monkeyGeek # 作者名称
language: zh-CN # 网站使用的语言
timezone: # 网站时区，录入可设置America/New_York, Japan, 和 UTC等

# URL
## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
url: https://monkeygeek369.github.io # 网站url
root: / # 网站根目录
permalink: :year/:i_month/:i_day/:enname/ # 网站的永久链接格式
permalink_defaults: # 网站永久链接格式的默认值

# Directory
source_dir: source # 指定资源文件夹
public_dir: public # 指定公共文件夹，即生成的静态站点文件 
tag_dir: tags # 指定标签文件夹
archive_dir: archives # 指定归档文件夹
category_dir: categories # 指定分类文件夹
code_dir: downloads/code # 指定include code文件夹
i18n_dir: :lang # 国际化文件夹
skip_render: # 跳过指定文件的渲染，可使用glob表达式来匹配路径

# Writing
new_post_name: :category/:title.md # 新建md文档时生成的文件路径和文件名称指定
default_layout: post # 新建文档的默认布局方式
titlecase: false # 是否把标题转换为title case
external_link: true # 是否在新标签页中打开链接
filename_case: 0 # 把文件名称转换为1大写或2小写，默认为0
render_drafts: false #是否显示草稿
post_asset_folder: false #是否启动asset文件夹
relative_link: false # 是否把链接改为与根目录的相对地址
future: true # 是否显示问来的文章
highlight: # 代码块设置
  enable: true # 是否启用代码高亮
  line_number: true # 是否启用行号
  auto_detect: false # 是否启用代码自动检测
  tab_replace: # 未知，没使用过
  
# 主页设置
# path: 博文网站index启动页面的位置路径，默认为空
# per_page: 每页显示的文章量 (0 = 关闭分页功能)
# order_by: 排序规则 (默认以文章创建日期倒叙排序)
index_generator:
  path: ''
  per_page: 10
  order_by: -date
  
# Category & Tag
default_category: uncategorized # 默认分类
category_map: # 分类别名
tag_map: # 标签别名

# Date / Time format
## Hexo uses Moment.js to parse and display date
## You can customize the date format as defined in
## http://momentjs.com/docs/#/displaying/format/
date_format: YYYY-MM-DD # 日期格式
time_format: HH:mm:ss # 时间格式

# Pagination
# per_page: 每页显示的文章量 (0 = 关闭分页功能)
# pagination_dir: 分页目录
per_page: 10
pagination_dir: page

# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: 需要指定theme文件夹下的主题
theme: icarus_monkeyGeek

# 部署配置
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git # 指定仓库类型
  repo: https://github.com/monkeyGeek369/monkeyGeek369.github.io.git # 指定仓库连接
  branch: master # 指定分支
  message: #消息配置

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


