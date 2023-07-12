---
title: final原理
category: java基础
date: 2023-01-19 17:21:29
updated: 2023-01-19 17:21:29
enname: final-base
categories: java基础
tags:
	- 面试
	- 多线程
keywords: final原理
permalink:
thumbnail:
---

# 作用

保证不可变<!--more-->

禁止重排序



</br>

# 保证不可变

如果修饰基本类型则不可变，如果是引用类型则引用地址不可变，但引用对象内的值可以改变



- 修饰实例变量：只能在声明时初始化、非静态初始化块初始化或者构造函数中初始化
- 修饰类变量(static修饰的)：声明时初始化或静态初始化块中初始化
- 修饰局部变量:只能声明时初始化
- 修饰方法：不能被重写
- 修饰类：不能被继承



</br>

# 禁止重排序

### final域为基本类型时

1. 写final域的重排序:禁止编译器将写final域重排序到构造函数之外.原理是在写之后构造函数return之前插入一个storestore屏障.
2. 读final域的重排序:初次读对象引用与初次读对应内的final域不能重排序.原理是会在读final域之前插入loadload屏障.



### final域为引用类型时

1. 写final域的重排序:在构造函数内对一个final引用对象的成员写入与构造函数外对这个对象的引用赋值给一个引用变量时禁止重排序的.
2. 读final域的重排序:同上



```java
public class FinalExample {
    int i;                            //普通变量
    final int j;                      //final变量
    static FinalExample obj;

    public void FinalExample () {     //构造函数
        i = 1;                        //写普通域
        j = 2;                        //写final域
    }
    public static void writer () {    //写线程A执行
        obj = new FinalExample ();
    }
    public static void reader () {       //读线程B执行
        FinalExample object = obj;       //读对象引用
        int a = object.i;                //读普通域
        int b = object.j;                //读final域
    }
}
```

普通变量，可能是先初始化构造函数，然后再给变量赋值。

以上面代码为例，A线程负责写入，B线程负责读取。那么a的值可能为空，因为获取对象引用与构造函数中属性赋值做了指令重排序，赋值操作在构造函数返回之后完成。b一定为2，因为final变量禁止了构造函数内的指令重排序，确保在构造函数return之前完成赋值。



</br>

# 区别

- static作用于成员变量用来表示只保存一份副本
- volatitle主要是保证多个线程对该变量的内存可见性，常用于多个线程开始、结束的标记符等



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


