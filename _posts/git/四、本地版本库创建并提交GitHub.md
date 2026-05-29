---
title: 四、本地版本库创建并提交GitHub
category: git
date: 2019-08-10 22:51:36
updated: 2019-08-10 22:51:36
enname: commitgithub
categories: git
tags: 
	- git
	- github
keywords: git,github,本地版本库创建并提交github
permalink:
thumbnail: ../../image/githubonlinecreat4.png
---

# **前言**

github与git在这里就不去介绍，这些基础知识可以查看其它知识文档。我们通常本地使用git进行版本控制，进行与github、gitlab等git仓库的交互，例如克隆、拉去、提交、推送等<!--more-->，这些基本的git操作通常是建立在git远程仓库存在的情况下，那么如果并非系统管理员或有机会触及此类业务的人员，自己将很难接触仓库的创建与上传。本篇文章将专注介绍如何本地创建git仓库并提交至github等git仓库。大体上划分下来主要由三部分组成：在github新建仓库、在本地创建仓库、建立加密认证链接并提交



# **具体步骤**

### **1、在github创建仓库**

- 登陆github
- 进入仓库

![](../../../../image/githubonlinecreat1.png)

- 选择新建

![](../../../../image/githubonlinecreat2.png)

- 设置与提交--设置仓库名称、描述、公开/私有、是否自动生成README文件

![](../../../../image/githubonlinecreat3.png)

<span style="color:red;">github建议每一个仓库都要具备一个叫做README.md的文件，用于对当前仓库的介绍描述。</span>

### **2、在本地创建仓库**

当我们创建好本地仓库后github将自动跳转到仓库代码管理页面，该页面将提示三种代码传递方式，如下：

![](../../../../image/githubonlinecreat4.png)

第一种是通过本地仓库上传的方式，第二种是从已存在的git仓库克隆的方式，第三种是通过其它版本控制仓库导入。

第一种和第二种方式有些类似，下面将介绍第一种方式：

- 本地确保已经安装git版本控制工具
- 本地进入你要创建版本库文件夹，右键选择 git bash here，进入git命令控制界面（该文件夹名称将成为你本地仓库的名称）
- 执行 git init 命令（该文件夹名称将成为你本地仓库的名称）
- git add . （将所有文件添加进仓库，当然也可也指定单独文件）
- git commit -m "本次提交的注释" （执行提交命令，将工作区内容提交至本地仓库缓存区）
- git remote add origin https://你的地址  （在完成3、建立加密认证链接并提交，后执行，否则认证链接不成功）
- git push -u origin master (将仓库提交并作为master主库)

### **3、建立加密认证链接并提交**

详情请看“Git SSH Key 生成与配置”，该文章将详细介绍如何生成rsa公钥与私钥，以及如何配置和链接。













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


