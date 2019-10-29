---
title: 二、Git SSH Key 生成与配置
category: git
date: 2019-08-10 22:09:34
updated: 2019-08-10 22:09:34
enname: gitsshkey
categories: git
tags: 
	- git
	- ssh
	- key
keywords: git,ssh,key,生成与配置
permalink:
thumbnail:
---

Git 是分布式的代码管理工具，远程的代码管理基于SSH的，因此必须由用户本地生成ssh的私钥和公钥来建立客户端与服务器的安全连接，配置SSH的步骤如下：

<!--more-->

# 全局账户配置

全局账户配置的详细教程说明已经在“一、git基础教程”中讲解，各位看官可取查看。



# 生成SSH密钥

### 1、说明

本地git同github或者gitlab等git仓库进行通信通常需借助rsa密钥进行身份确认，即需要我们在创建好公钥和私钥后将私钥进行本地注册，公钥放入远程git仓库注册。



### 2、查看本地ssh

在windows系统中rsa密钥的默认存放路径是在当前登录账户的用户->.ssh文件夹内。右键选择git bash here，执行如下命令：

```
$ cd ~/.ssh
$ ls
```



### 3、执行生成命令

右键选择git bash here，执行如下命令：

```
ssh-keygen -t rsa -C "你的邮箱地址"  #自动在.ssh生成名称为id_rsa的密钥文件
```

在输入此命令后会提示是否设置密码，可以自行选择是否设置密码。

设置密码后在每次进行git push等命令时会进行密码输入

```
ssh-keygen -t rsa -C "你的邮箱地址" -f "yourFileName_rsa"
```

通过该命令可生成指定名称的rsa密钥文件至.ssh文件夹内



### 4、加入ssh代理

右键选择git bash here，执行如下命令：

```
ssh-add rsa密钥文件名
```

<span style="color:red;">注意：直接ssh-add命令可能报错“ Could not open a connection to your authentication agent”这是因为ssh agent配置有问题，需要先执行ssh-agent bash命令。</span>

```
ssh-add id_rsa
```



### 5、公钥使用

需要添加到git上的是id_rsa.pub，此时客户端即可clone，pull，push代码了。





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


