---
title: 六、通过hexo实现将博客系统部署至github
category: hexo
date: 2019-08-10 21:18:24
updated: 2019-08-10 21:18:24
enname: githubblog
categories: hexo
tags: 
	- hexo
	- github
	- blog
keywords: hexo,github,blog,自建博客
permalink:
thumbnail: ../../image/githubblogtheme.png
---

**github环境准备**

github中有个叫github page 的服务，这项服务支持html等静态站点的访问解析，可以通过访问与github用户名相同的仓库名来实现访问站点，这也是通过github实现自建博客的依据。那么我们的首要条件是建立一个仓库名称为自己github账户名称的仓库，然后通过“你的用户名.github.io”来访问你的博文站点。

<!--more-->

那么问题来了，如何创建仓库？如何实现本地同github的安全通信？这部分内容在文章“[本地版本库创建并提交GitHub](http://www.monkeygeek.cn/2019/8/10/commitgithub/)”已经介绍，这里不再重复叙述。在这里假设我们已经成功建立仓库，名称为：monkeyGeek369（一定要是你自己github的账户名称）



**本地git环境准备**

简单来说git的环境准备就是将git安装到本地，一直下一步即可，如果对git生疏可以查看本博客站点[git相关文章](http://www.monkeygeek.cn/categories/git/)。



**hexo环境配置**

详情可以查看“[一、hexo基础环境准备](http://www.monkeygeek.cn/2019/8/10/environment/)”



**下载主题与配置**

hexo官方提供多种主题下载，官方主提地址 https://hexo.io/themes/，在挑选到自己喜欢的主题后进行如下操作：

1、主题克隆：通过 “git clone 主题地址”命令将主题克隆至hexo安装目录下的“theme”文件夹内

2、主题绑定：通过修改hexo的_config.yml文件绑定主题（_config.yml配置文件的详细介绍可参考“二、hexo结构简介”）

![](../../../../image/githubblogtheme.png)



**文章编辑**

文章编辑就是利用hexo提供的各种命令以及主题配置等实现对文章的编辑操作，详情可参考“三、hexo基础命令”

例如：

- 通过命令hexo new post "文件名" 来创建文档
- 通过命令hexo new page  "分页名称"来创建分页
- 通过命令hexo g 来将source种的文档生成html静态站点到public文件夹
- 通过命令hexo s 来将public种的静态站点发布，默认端口4000
- 通过命令hexo clean来讲public清空、db缓存清空等

md编辑器可以选择使用Typora，相对简介好用



**站点生成**

source文件夹：我们创建的无论是文档还是分页全部存放在source文件夹以.md结尾的文档，这也是生成html静态站点的基础素材。

public文件夹：通过执行hexo g命令实现将source中的md文档生成符合指定模板的html静态站点，静态站点存放在public文件夹下。

通过命令hexo s 来将public种的静态站点发布，默认端口4000



**站点发布**

若站点本地发布成功则可以将public中的静态站点文件提交到github中我们创建的仓库。实现提交github需如下几步：

<span style="color:red">1、hexo配置github地址等信息（在hexo的_config.yml文件内）</span>

```
# 部署配置
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git # 指定仓库类型
  repo: https://github.com/monkeyGeek369/monkeyGeek369.github.io.git # 指定仓库连接
  branch: master # 指定分支
  message: #消息配置
```

<span style="color:red">2、确保本地与github仓库的通信成功，这部分内容在文章“[本地版本库创建并提交GitHub](http://www.monkeygeek.cn/2019/8/10/commitgithub/)”已经介绍</span>

<span style="color:red">3、安装部署插件，执行命令 npm install `--`save hexo-deployer-git</span>

<span style="color:red">4、通过hexo deploy实现部署</span>



**结果验证**

通过<span style="color:red">“你的用户名.github.io”</span>来访问你的博文站点



**域名绑定**

关于自身域名如何绑定至github可以参考我第九篇文章，有专门介绍。

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


