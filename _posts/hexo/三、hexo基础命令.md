---
title: 三、hexo基础命令
category: hexo
date: 2019-08-10 20:05:05
updated: 2019-08-10 20:05:05
enname: basecom
categories: hexo
tags: hexo
keywords: hexo,命令
permalink:
thumbnail:
---

| 命令                             | 类型                | 作用                                                         |
| -------------------------------- | ------------------- | ------------------------------------------------------------ |
| hexo init [folder]               | init                | 初始化hexo，如果未指定文件夹则在当前目录生成                 |
| hexo new [layout] <title>        | new                 | 新建文章，可以选择layout布局类型和title文章名称              |
| hexo g                           | generate<!--more--> | 将source文章文件夹内的文章生成html静态站点文件至public文件夹 |
| hexo publish [layout] <filename> | publish             | 发布草稿文件，草稿文件为source文件夹内的draft文件夹文件，后面新建文章时会介绍 |
| hexo s                           | server              | 启动public文件夹内生成的动态站点文件，默认发布端口为4000，可通过http://localhost:4000进行访问。可使用--port重设端口，--static只使用静态文件，--log启动日志记录。使用时如 hexo s --port 4001 修改端口为40001 |
| hexo d                           | deploy              | 部署public文件夹内生成的静态站点文件，部署指的是将其部署至git等远程仓库，需要在hexo的_config.yml文件夹内配置远程仓库连接。详情可查看“二、hexo结构简介”中的部署配置 |
| hexo render <file1> [file2] ...  | render              | 渲染文件，实际没用过，效果未知                               |
| hexo migrate <type>              | migrate             | 从其他博客系统迁移内容，目前还未用过                         |
| hexo clean                       | clean               | 清空public文件夹生成的静态站点文件和db.json缓存文件          |
| hexo list <type>                 | list                | 列出网站资料                                                 |
| hexo version                     | version             | 显示hexo版本                                                 |
| hexo --safe                      | option              | 在安全模式下，不会载入插件和脚本。当您在安装新插件遭遇问题时，可以尝试以安全模式重新执行。 |
| hexo --debug                     | option              | 在终端中显示调试信息并记录到 debug.log。当您碰到问题时，可以尝试用调试模式重新执行一次，并 [提交调试信息到 GitHub](https://github.com/hexojs/hexo/issues/new)。 |
| hexo --silent                    | option              | 隐藏终端信息。                                               |
| hexo --config custom.yml         | option              | 自定义配置文件的路径，执行后将不再使用 _config.yml。         |
| hexo --draft                     | option              | 显示 source/_drafts 文件夹中的草稿文章。                     |
| hexo --cwd /path/to/cwd          | option              | 自定义当前工作目录（Current working directory）的路径。      |





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


