---
title: maven基础知识
category: 软件技术
date: 2022-11-03 19:23:56
updated: 2022-11-03 19:23:56
enname: maven-base
categories: 软件技术
tags:
keywords: maven、基础知识
permalink:
thumbnail:
---

本文针对《maven实战》阅读之后的知识点整理<!--more-->，主要是重点的罗列和简单讲解，意在描绘一个比较全面的maven，详细的技术细节不进行设计，读者可以自行查阅资料。

</br>

# maven用途

- 项目构建工具
- 项目依赖管理工具
- 项目信息管理工具

</br>

# 坐标

作用：唯一定位一个maven项目

组成：

```xml
<groupId>com.company.test</groupId> 公司级的软件项目组，采用逗号描述路径
<artifactId>service-api</artifactId> maven子项目名称
<version>1.0.1-SNAPSHOT</version> maven子项目版本
<packaging>jar</packaging> maven子项目打包方式，可以没有，默认为jar
```

</br>

# 依赖

- 依赖范围：<scope>元素用于定义依赖范围。

1. test：测试依赖范围。仅在测试范围有效
2. compile：编译依赖范围。在测试、编译、运行是依赖都有效
3. runtime：运行时依赖范围。在测试、运行有效。
4. provided：已提供依赖范围。在测试、编译有效。
5. system：系统依赖范围。与provided相同，但是必须显式的制定依赖包的路径，不建议使用。



- 传递性依赖与调节：依赖是可传递的，A->B->C依次依赖，则A也依赖C。依赖冲突调解原则：

1. 最短路径原则：例如BC中都有依赖D，但是版本不同，那么A最终采用B中的依赖D版本
2. 第一声明者优先原则：如果路径相同则优先加载在pom中顺序靠前的依赖



- 可选依赖：用<optional>标注的依赖为可选依赖，这样的依赖不会被传递



</br>

# 仓库

自行百度，属于软件安装与配置教程。

- 类型：本地仓库、远程仓库、中央仓库、私有仓库
- 镜像



</br>

# 生命周期与插件

- 什么是生命周期？项目构建的过程就是生命周期
- 生命周期种类：clean\validate（验证）\compile\test\package\verify\install\site（生成站点）\deploy
- 插件目标：帮助快速便捷的构建项目和管理项目
- 插件绑定：maven命令的底层实现就是依赖默认的maven插件，即命令即插件。同理插件可用具体的命令进行绑定，执行命令就相当于执行插件。
- 插件配置：不同的插件支持的功能和命令不同，可以对插件与命令进行绑定，将复杂的插件命令进行简单化
- 从命令行调用插件：插件与命令绑定后执行maven命令即可执行插件



</br>

# 聚合与继承

- 聚合：建立聚合项目，在项目的pom中通过modules元素聚合子maven项目，这样只要在聚合项目执行构建命令就可以操作子项目，无需组个构建。
- 继承：建立父项目，在父项目pom中设定好依赖、属性、插件等信息，在子项目中通过<parent>来依赖父项目的配置。

可继承的pom元素：

1. groupid（重点）
2. version（重点）
3. description
4. organization
5. inceptionyear
6. url
7. developers
8. contributors
9. distributionmanagement
10. issuemanagement
11. cimanagement
12. scm
13. mailinglists
14. properties（重点）
15. dependencies（重点）
16. dependencymanagement（重点）父项目使用此配置依赖后，依赖不会被子项目继承，但是子项目可以自行添加父配置中的依赖，可以省略组id和项目id，减少配置。
17. repositories（重点）
18. build（重点）
19. Reporting

- 聚合与继承的关系：聚合与继承虽然作用不同，但是方便起见一般配置到一个pom中



</br>

# 其他内容

- nexus私服
- maven测试



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


