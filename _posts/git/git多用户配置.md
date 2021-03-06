---
title: git多用户配置
category: git
date: 2019-10-31 13:19:08
updated: 2019-10-31 13:19:08
enname: multuser
categories: git
tags: git
keywords: git,基础教程
permalink:
thumbnail:
---

# 一 前言

众所周知,我们通过本机git来访问服务器git通常使用rsa凭证的方式来确保安全性,下面通过三个场景来具体分析git账户的含义:

**场景一:只存在一个git账户,一个rsa密钥**

例如公司内部存在gitlab服务器,那么通过本地生成rsa公钥与私钥,分别将公钥配置到服务器,私钥配置到本机git,那么可以正常通过rsa访问.

<!--more-->

**场景二:存在多个git账户,一个rsa密钥**

例如本机只生成一对rsa密钥分别为id_rsa\id_rsa.pub,然而我们需要操作公司内部gitlab与自身的github,其实通过一对rsa就可以实现,将生成的公钥分别配置到gitlab与github,私钥配置到本机git,那么可以同时操作两者.



**场景三:存在多个git账户,多个rsa密钥**

例如我们既有公司内部的gitlab也有自身的githubA账户\githubB账户,那么通过一对rsa无法实现控制(github有验证,相同公钥不可重复使用在不同github账户),必须进行多用户配置.

</br>

# 二 密钥的基本概念

如何生成密钥以及如何使用密钥在[文章中](http://www.monkeygeek.cn/2019/8/10/gitsshkey/)已经详细介绍,rsa密钥的作用是使git服务器与client之间建立安全通信通道并完成身份认证.

生成密钥时我们可以输入密码,但无需填写账户名\邮箱等特定信息,<span style="color:red;">即密钥与账户无关仅做身份认证.</span>

</br>

# 三 账户的基本概念

通过密钥我们可以实现本机同服务器间的身份认证,但这还远远不够,想要完整的操作git服务器必须具备如下条件:

以如下环境为例:(查看某项目git配置)

```
#输出项目本地配置信息
$ git config --local -l

core.repositoryformatversion=0
core.filemode=false
core.bare=false
core.logallrefupdates=true
core.symlinks=false
core.ignorecase=true
user.name=111111
user.email=111111@qq.com
remote.origin.url=https://github.com/yqnshare/yqnshare.github.io.git
remote.origin.fetch=+refs/heads/*:refs/remotes/origin/*
branch.master.remote=https://github.com/yqnshare/yqnshare.github.io.git
branch.master.merge=refs/heads/master

```

必须具备条件解读:(在密钥已经配置要的前提下)

| 条件                  | 说明                                                         |
| --------------------- | ------------------------------------------------------------ |
| 远程主机remote.origin | 本地仓库必须要指定远程主机信息,通过clone命令即已经具备,无需再设置,当然也可也通过git remote 命令重新设置(如$ git remote set-url origin https://github.com/yqnshare/yqnshare.github.io.git) |
| 本地分支branch.master | 创建本地分支并提交至远程主机,详情可看[git用法之常用命令](http://www.monkeygeek.cn/2019/8/10/gitcommand/) |
| 用户名user.name       | 可以通过git config --system/global/local来分别设定,优先级为local>global>system,用户名仅仅是作为标记,无任何验证作用.详情可看[git用法之常用命令](http://www.monkeygeek.cn/2019/8/10/gitcommand/) |
| 用户邮箱user.email    | 可以通过git config --system/global/local来分别设定,优先级为local>global>system,用户邮箱仅仅是作为标记,无任何验证作用.详情可看[git用法之常用命令](http://www.monkeygeek.cn/2019/8/10/gitcommand/) |
| 账户名                | 账户名必须填写,对应的是远程主机的账户名,如github账户名       |
| 密钥密码              | 如果使用rsa且设置了密码那么必须要输入                        |

</br>

# 四 配置步骤

**4.1针对不同环境生成不同rsa密钥**

密钥的生成看第二部分"密钥的基本概念"



**4.2配置config文件**

以window系统为例,需要在.ssh中创建config文件并进行配置,内容如下:

```
#公司的git地址
Host yqngitlab
   User mahao
   Hostname git.iyunquna.com
   IdentityFile ~/.ssh/id_rsa

Host yqnsharehost
   User yqnshare
   Hostname github.com
   IdentityFile ~/.ssh/yqnshare_rsa

Host monkeygeekhost
   User monkeygeek369
   Hostname github.com
   IdentityFile ~/.ssh/myblog_rsa
```

- Host:可以自定义,实际上是远程地址的别名,如通过 ssh -T git@yqnsharehost  等效于 ssh -T git@github.com
- User:实际上是远程服务器的账户名称,例如自己的github账户名称等,例如可以这样使用git remote set-url origin https://yqnshare@yqnsharehost/yqnshare/test.git
- IdentityFile:指的是rsa对应的路径



配置好后可通过ssh -T命令测试各个配置是否能够联通,如:

ssh -T git@@yqnsharehost



**4.3配置账户信息**

具体可看第三部分"账户的基本概念",其中需要特别注意的是"用户名"和"密钥密码",一般git会将我们第一次输入的账户名和密码进行缓存,当然我们也可以移除缓存或添加缓存,具体操作如下:

```
#缓存项目本地的rsa对应的凭据信息
#1.缓存的账户密码以明文形式存放在.ssh根目录.git-credentials文件内
#2.同服务器账户只能被缓存一次，如account1和account2同为github账户，即github.com的服务器地址只能被缓存一条数据
#3.若account2被缓存那么在account1使用push等命令时会提示“使用account2账户登录无权限”
git config --local credential.helper store

#清除系统\全局\项目本地的rsa对应的凭据信息
git config --system --unset credential.helper
git config --global --unset credential.helper
git config --local --unset credential.helper
```



这里或许大家会有一个疑问,比如我在config配置了多种rsa那么到具体的项目中我们也没有去设置匹配关系,git是如何找到对应的config配置呢?

正如我前文说的,rsa密钥配置仅仅作为身份认证没有其它作用,而账户信息则负责整个本地项目与远程仓库的对应关系,两者不存在直接匹配关系,当我们本地项目需要pull或push等操作远程仓库时,git会根据项目中对应好的关系(如url地址,账户名等)自动取config中匹配,我们只需输入对应的"账户名"和"rsa密钥密码"即可访问.

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


