---
title: CLI教程
category: UiPath
date: 2020-05-26 20:24:49
updated: 2020-05-26 20:24:49
enname: CLI
categories: UiPath
tags:
	- UiPath
	- RPA
keywords: UiPath,CLI,RPA
permalink:
thumbnail:
---

# 简介

CLI即Command Line Interface命令行接口是一个控制台应用,它可以被用来去启动job或等待他们的执行结果返回.其作为Robot服务直接的客户端程序,通信是通过WCF通道完成的.

<!--more-->

</br>

# 环境准备

![](../../../../image/robotservice.png)

CLI的成功调用需要Robot Service的支持,请确保该进程服务已经开启.

</br>

# 注意点

- 首先需要将命令行操作导航到UiPath的安装目录,例如导航到默认的安装路径cd C:\Program Files (x86)\UiPath\Studio

- Studio v2019.4.x以及更低版本可以使用所有的命令行指令
- 使用CLI需要Robot在有人值守状态

</br>

# 命令详解

### 执行命令

```js
UiRobot.exe execute [--process <Package_ID> | --file <File_Path>] [--folder <Orchestrator_Folder_ID>] [--input <Input_Parameters>]
```

该命令用于执行一个过程process或一个文件,例如.json/.xaml/.nupkg,需要注意的是--process与--file参数不可同时使用.

- --process/-p:(必填项)用于执行本地或Orchestrator上的一个process.例如

  ```js
  UiRobot.exe execute --process UiPathDemoProcess
  UiRobot.exe execute -p UiPathDemoProcess
  ```

- --file/-f:(必填项)用于执行本地文件,例如.json/.xaml/.nupkg

```js
UiRobot.exe execute --file "C:\UiPath\Automation\Project.json"
UiRobot.exe execute --file "C:\UiPath\Automation\Main.xaml"
UiRobot.exe execute --file "C:\UiPath\Automation\Notepad.1.0.6682.21636.nupkg"
```

- --folder:(可选项)允许用户指定Orchestractor中的folder(文件夹),通常配合--process一同使用.

```js
UiRobot.exe -Execute --process UiPathDemoProcess --folder OrchFolder1
```

- --input:(可选项)允许用户指定运行参数json字符串,在运行process或file时有可能会用到传参,此命令参数即可实现

```js
UiRobot.exe execute --process UiPathDemoProcess --input "{'inArg' : 'value' , 'Integer' : 3}"
UiRobot.exe execute --process UiPathDemoProcess --folder OrchFolder1 --input "{'inArg' : 'value' , 'Integer' : 3}"
UiRobot.exe execute --file "C:\UiPath\Automation\Main.xaml" --input "{'inArg' : 'value' , 'Integer' : 3}"
```

</br>

### 打包命令

```js
UiRobot.exe pack <Project_Path> --output <Destination_Folder> [-v <Project_Version>]
```

该命令用于将rpa项目打包成RPA可运行压缩包.nupkg.众说周知我们在创建RPA项目时,在所建项目的根目录会同步生成Project.json文件,该文件是对本项目名称\配置\环境等运行参数的完整描述,因此本打包命令的核心是对Project.json文件的解析.

- --output/-o:(必填项)指定打包文件的输出路径

```js
UiRobot.exe pack "C:\UiPath\Automation\Project.json" --output "C:\UiPath\Automation\Packages"
```

- -v:(可选项)指定版本

```js
UiRobot.exe pack "C:\UiPath\Automation\Project.json" --output "C:\UiPath\Automation\Packages" -v 1.0.6820.22047
```

</br>

### 连接命令

```js
UiRobot.exe connect [--url <Orchestrator_Server_URL> --key <Machine_Key>] | [--connectionString <Connection_String>]
```

用于连接本地Robot到Orchestrator实例.前提条件是我们的RPA应用依赖于Orchestrator运行与控制,如果是不借助Orchestrator控制rpa,那么此命令用不到.这里不做详细介绍,详情可参考官网.[2]

</br>

### 断开连接命令

```js
UiRobot.exe disconnect
```

使本地Robot机器人与Orchestrator断开连接.这里不做详细介绍,详情可参考官网.[2]

</br>

### 机器跟踪命令

```js
UiRobot.exe trace --enableLowLevel | --disableLowLevel
```

该命令用于控制开启或关闭Robot运行期间的低层级信息调用链trace,信息调用链(verbose trace)文件.etl可在事件窗口打开,帮助开发者发现和定位问题.

- --enableLowLevel:开启调用链,将生成.etl文件,该文件可在事件窗口打开观看.

```js
UiRobot.exe trace --enableLowLevel
```

- --disableLowLevel:关闭调用链,在执行该命令后将在桌面生成.etl文件

```js
UiRobot.exe trace --disableLowLevel
```

</br>

### 其它命令

下面的这些命令主要用于提供信息查看,其本身对RPA程序不造成影响.

- -version:输出有关Robot版本的信息
- -help:以列表的形式输出当前版本支持的命令行命令以及对应的介绍信息和案例



</br>

参考资料:

[1]https://docs.uipath.com/robot/docs/robot-command-line-interface

[2]https://docs.uipath.com/robot/docs/arguments-description

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


