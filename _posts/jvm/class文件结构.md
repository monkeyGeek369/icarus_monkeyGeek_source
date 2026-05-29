---
title: class文件结构
category: jvm
date: 2023-11-20 00:06:57
updated: 2023-11-20 00:06:57
enname: jvmClass
categories: jvm
tags:
	- 面试
	- jvm
keywords: jvm，class文件结构
permalink:
thumbnail:
---

# 基本概念
* 前端编译器：将符合规范的java代码编译为符合jvm规范的字节码文件。
    * 默认如javac前端编译器
    * 只进行全量编译，不进行编译优化
* 即时编译器：如JIT负责编译优化，在代码的执行过程中发现重复执行的代码段时会被编译成效率更高的机器指令
* 字节码文件：经过前端编译器编译后的二进制文件，包含字节码指令即jvm指令
* 字节码指令：由一个字节的操作码+0个/多个操作数的jvm指令

<!--more-->

# 分析工具

用于反编译分析class二进制文件的组成内容
作用：通过分析字节码文件的指令来更好的理解代码执行过程，排查问题

- javap命令：将字节码文件翻译为字节码指令。基本格式javap <操作符> <class路径>
    - -public：显示公共成员
    - -protected：显示受保护成员+公共成员
    - -private：显示所有成员
    - -package：显示非私有成员
    - -sysinfo：显示系统信息
    - -constants：显示静态成员
    - -s：输出类的内部成员签名
    - -l：输出局部变量表
    - javap -c classpath test.txt：翻译为完整的字节码指令到test文件
    - javap -v classpath test.txt:翻译为完整的字节码指令到test文件且包含编译时的jdk信息
    - javap -v -p classpath test.txt:翻译为完整的字节码指令到test文件且包含编译时的jdk信息自己私有成员信息。这是最完整的
- JClassLib插件：idea插件，将字节码文件翻译为字节码指令
- binary viewer：桌面工具，解读字节码文件


# 组成结构
* 魔数：magic number用于标识字节码文件开头，主要作用是安全验证
    * u4：采用无符号4字节表示
    * ca fe ba be
* 字节码文件版本号
    * u4大小，前两个字节存储小版本，后两个字节存储大版本
    * 不同的版本号对应不同的前端编译器，代表当前字节码文件的编译方式
* 常量池：class文件的基石
    * 常量池计数器：u2，用于记录常量池长度，实际长度=计数器数量-1
    * 常量池表：n字节，从1开始，存放各种字面量和符号引用。类加载后这部分内容会放入运行时常量池
        * 字面量：string值或final修饰的值
        * 符号引用：类的全限定名，方法或属性名，属性类型/参数列表/返回值
        * 注意：常量池中的每一个数据(字面量/符号引用)都有不同的大小，分别由类型/大小/内容组成
* 访问标识：大小U2
    * 用于标识类或接口的访问信息
    * 总共八种访问标识，例如public/final/abstruct等
* 类索引/父类索引/接口索引集合
    * 类索引：u2大小，指向在常量池中的类定义
    * 父类索引：u2大小，指向在常量池中的类定义
    * 接口计数器：u2大小，标记实现的接口数量
    * 接口索引集合：u2大小
* 字段表集合
    * 用于描述类或接口中声明的变量，不包括方法中的局部变量
    * 指向常量表，用于记录访问修饰、字段标识、字段类型、字段名称等
    * 不会列出继承而来的字段
    * 组成结构
        * 字段计数器
        * 字段表：由如下部分组成
            * 访问标识：u2大小，记录访问标识
            * 字段名索引：u2大小，指向常量池
            * 描述符索引：u2大小，标识字段类型
            * 属性计数器：u2大小
            * 属性表集合：记录字段的属性，例如初始化值，修饰信息等。记录有属性名/属性长度/在常量表中索引等
* 方法表集合
    * 指向常量池，描述方法的修饰符/名称/返回值类型/参数类型等。注意不包含父类方法
    * 组成结构
        * 方法计数器
        * 方法表：由多个methed_info结构组成，每一个methed_info具备相同的结构
            * 访问标识：u2
            * 方法名索引：u2
            * 方法描述符：u2，用于描述方法的符号，例如()v
            * 属性计数器：u2
            * 属性集合
* 属性表集合
    * class文件携带的辅助信息，例如class文件名，各种注解等。
    * 组成结构：只是大体结构，具体细节非常多
        * 属性名
        * 属性长度
        * 属性表：用于描述属性的详细组成结构


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


