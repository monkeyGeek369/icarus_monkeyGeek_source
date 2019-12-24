---
title: php环境准备
category: php
date: 2019-12-24 21:18:44
updated: 2019-12-24 21:18:44
enname: phpenv
categories: php语言
tags:
	- php语言
	- php基础
keywords: php,php语言,php基础,php环境准备
permalink:
thumbnail:
---

# 前言

php语言大家比较耳熟能详,作为强大的服务器端脚本语言,其在许多领域得到应用同时也得到了像facebook等大公司的信赖,本文不对其发展史做详细介绍,主要就php的环境准备做详细描述.

<!--more-->想要运行php项目必须具备基本的三要素:

- 第一是完成php运行环境的安装与配置
- 第二是安装服务器容器如iis\apache等
- 第三是必须安装实下流行的浏览器进行浏览

</br>

# php环境配置

首先需要去官网下载适合的版本[下载地址](https://windows.php.net/downloads/releases/archives/)

```
12/6/2018  8:43 PM     24425705 php-7.0.33-nts-Win32-VC14-x64.zip
 12/6/2018  8:43 PM     22346551 php-7.0.33-nts-Win32-VC14-x86.zip
 12/6/2018  9:14 PM     26103356 php-7.0.33-src.zip
 12/6/2018  8:43 PM     24539294 php-7.0.33-Win32-VC14-x64.zip
 12/6/2018  8:43 PM     22419115 php-7.0.33-Win32-VC14-x86.zip
```

例如7.0.33版本提供五种下载,其中nts表示非线程安全,vc14表示php依赖于vc++14,下载包中已经具备,具体配置如下:

- 解压压缩包至指定目录,如D:\installSoft\php-7.0.33-Win32-VC14-x64
- 在PHP根目录中有`php.ini-production`和`php.ini-development`这两个配置文件（使用哪个取决于你自己，PHP官方建议将前者用于生产环境，后者多用于开发环境），这里我选择使用`php.ini-development`，现在将`php.ini-development`复制一份备份，然后将`php.ini-development`改为`php.ini`即可。
- 将php安装目录添加至pash环境变量D:\installSoft\php-7.0.33-Win32-VC14-x64
- cmd输入php -v 测试是否安装成功,如果显示出版本好则成功
- 在php.ini中搜索`extension_dir = "ext"`，删除前面的`;`，并将`exc`修改为你的PHP根目录下的`php/ext`目录，我这里修改为：`extension_dir = "D:/installSoft/php-7.0.33-Win32-VC14-x64/ext"`(注意是/而不是\\)



### PHP开启Mysql扩展

安装PHP之后必定会用到数据库，现在在 php.ini 中搜索：`;extension=mysqli` 和 `;extension=pdo_mysql`，（建议两者都开启）；删除前面的`;`即可。

</br>

# apache容器配置

- 官网下载apach容器,详情可参考[1]
- 解压到指定目录(不要有中文)如D:\installSoft\httpd-2.4.41-o111c-x64-vc15-r2\Apache24
- 进入D:\installSoft\httpd-2.4.41-o111c-x64-vc15-r2\Apache24\conf\httpd.conf文件修改Define SRVROOT为真实apache目录,修改默认端口号80为88(如果有需要)

```
Define SRVROOT "D:/installSoft/httpd-2.4.41-o111c-x64-vc15-r2/Apache24"

Listen 80

ServerName localhost:80
```

- 将apache加入pash环境变量D:\installSoft\httpd-2.4.41-o111c-x64-vc15-r2\Apache24\bin
- 以管理员方式打开cmd,并进入apache目录的bin目录,并执行命令httpd -k install,如下

![](../../../../image/apacheenv.png)

- 启动apache需打开cmd执行命令httpd -k start,浏览器输入localhost:88,若正常出现apache主页则表示成功
- apache常用命令如httpd -k restart/httpd -k stop



### 若需要将apache与php关联需要进行额外配置,如下:

打开 Apache 的配置文件`httpd.conf`，在`LoadModule`行后添加以下代码

针对php版本

```php
LoadModule php7_module "D:/installSoft/php-7.0.33-Win32-VC14-x64/php7apache2_4.dll"

<IfModule php7_module>
    AddHandler application/x-httpd-php .php
    AddType application/x-httpd-php .php .html
    PHPIniDir "D:/installSoft/php-7.0.33-Win32-VC14-x64"
</IfModule>
```

针对php5版本

```php
LoadModule php5_module C:/PHP/php5apache2_4.dll

<IfModule php5_module>
    DirectoryIndex index.html index.php
    AddHandler application/x-httpd-php .php
    PHPIniDir "C:/PHP"
</IfModule>
```

保存文件后重启apache即可 httpd -k restart



</br>

# 运行测试测试

- 启动apache
- 在apache根目录D:\installSoft\httpd-2.4.41-o111c-x64-vc15-r2\Apache24\htdocs创建phptest.php文件
- 在文件编写简单php交本语句如

```php
<?php
phpinfo();
?>
```

- 浏览器输入http://localhost:88/phptest.php



</br>

参考资料:

[1]https://www.cnblogs.com/lovetree/p/6385957.html

[2]https://www.cnblogs.com/nwgdk/p/8654182.html

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


