---
title: 一、git基础教程
category: git
date: 2019-08-10 21:57:56
updated: 2019-08-10 21:57:56
enname: gitbase
categories: git
tags: git
keywords: git,基础教程
permalink:
thumbnail: ../../image/gitbaseframe.png
---

# **简介**

Git是分布式版本控制器，由中央服务器、客户机组成。客户机从中央服务器获取代码后除了代码文件外还有一个与服务器相同的完整的版本库，客户机在断网的情况下依然可以与本地版本库进行代码提交、更新、差异化比较等操作，等有网络了再将本地版本库与服务器同步。中央服务器起到代码备份与版本控制作用，即使挂掉也可以由客户机来恢复，因为客户机的版本库一直是最新的。

<!--more-->

<span style="color:red;">GIT与SVN不同的是“记录快照”，GIT不关心具体代码的差异只关心文件和项目整体性的变化，每次变动都会进行备份快照，所以GIT更像是一个特殊的文件系统。</span>

SVN是集成式版本控制器，中央服务器负责版本控制，客户机对获取的代码进行的任何操作都需要同步到中央服务器。每次更新与提交代码进行“差异化比较”，将服务器代码进行更新，并将此次操作记录在.SVN文件中。



# 组成结构

![](../../../../image/gitbaseframe.png)

git与其它版本控制软件最大的区别是其并非存储差异而是存储完整快照，并且git的每一个命令都会在git仓库有记录留存，数据丢失的情况非常少。git版本控制主要由三功能区组成：

- 工作区：可以理解为我们正在编辑的文件，文件有改动后工作区与缓存区出现不一致，git status将显示modify状态，需要执行commit命令
- 缓存区：这个一个抽象的概念，主要作用是为了解决仓库与工作区的不统一问题，祈祷协调作用，通过git add命令可以将新文件放入缓存区（实际上此时仓库也有记录）
- git仓库：仓库分为本地仓库和远程仓库，远程仓库存在主分支、子分支，本地仓库同样存在主分支和子分支，子分支可以由其它分支产生，各主子分支之间各自迭代互不影响。（分支存在更新传递，如C分支来自B分支，B分支来自A分支，那么C分支的提交会更新至B分支，B分支需要在提交至A分支。）



# 经典交互过程

![](../../../../image/gitbasemutual.png)

- master：从服务器clone后默认会生成一个名为master的主库，其它各分支可以来自master库
- HEAD：可以理解为分支的快照，快照指向哪里（通常指向commit）那么git将认为已经提交的状态为指向内容，通过git rest命令可以实现git的移动
- Index空间：可以理解为缓存区，实际上Index空间是新的commit的一个快照
- commit：指的是每一次的提交，实际上就是Index缓存空间的一次记录



# git搭建流程

- 建立git服务器：可以通过第三方软件自己搭建，如Gitlab，也可以直接利用github提供服务
- 服务器配置：根据git服务器软件的不同有差异，通常是配置用户、权限、库等信息
- 客户端安装：根据本教程安装客户端即可
- 客户端ssh配置：为了安全通常借助ssh进行客户端与服务端的加密通信，详情可看“Git SSH KEY生成与配置”
- 客户端与服务器端实现通信



# **辅助软件安装（基本上不用，可以不用安装）**

- 安装tortoiseGit的GUI安装
- GUI的语言包



# **Git的全局账户配置**

首先我们需要明白的是本地仓库与远程仓库的通信需要建立在两方面设置的基础上，第一，首先需要设置github或其他git仓库的账户名设置，即user.name和user.email，第二，需要创建rsa的公钥和私钥并将私钥添加到本地git将公钥加入github等远程仓库。第二点本文将不详细介绍，详情请看文章“Git SSH Key生成与配置”，通常我们会通过全局命令设置全局的账户名信息，具体如下：

```
$ git config --global user.name "xxx" 例如应该填写如github用户名，如我的用户名是monkeyGeek
$ git config --global user.email "aaa.bbb@xxx.com" 例如应该填写的是github内设置并验证成功的email
```

需要注意的是当我们设定好全局的用户名和邮箱后那么整个本地git仓库的所有账户名都用被设置的全局名称，如果存在本地多个仓库来自多个远程git仓库，由于每个git仓库的账户名和email很大程度上是不同的，因此可以用如下命令，单独针对某本地仓库设置账户名和邮箱。

<span style="color:red;">重新设置</span>

```
git remote set-url origin git+ssh://git@github.com/monkeyGeek369/icarus_monkeyGeek.git
```

<span style="color:red;">新添加</span>

```
git remote add origin git@github.com:nickchou/gocode.git
```

<span style="color:red;">其它命令</span>

```
git remote -v     查看
git remote rm origin   移除
```

注意：添加的github账户需要使用git方式不要使用https方式，git方式更加方面不用每次执行命令都要输入密码











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


