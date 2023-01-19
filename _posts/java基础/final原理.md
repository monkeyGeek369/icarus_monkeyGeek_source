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

确保内存可见性



</br>

# 保证不可变

- 修饰普通变量：只能进行一次赋值操作，且生命周期内不可变
- 修饰成员变量：1、如果是基本类型则不可变，2、如果是引用类型则引用地址不可变，但引用对象内的值可以改变
- 修饰方法：不能被重写
- 修饰类：不能被继承



</br>

# 禁止重排序

与volatitle不同final的禁止重排序规则是针对构造函数而言的

构造函数中的final变量只有先给final赋值或者取值后，才能完成构造函数的初始化。但是普通变量，可能是先初始化构造函数，然后在给变量赋值。

同理，当获取某个对象时，也是只能先成功获取该对象的引用，再去获取final成员变量的值，但是普通变量不能保证，可能会导致普通变量的读取，排在了获取对象引用的前面

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

以上面代码为例，A线程负责写入，B线程负责读取。那么a的值可能为空，因为获取对象引用与构造函数中属性赋值做了指令重排序，赋值操作在构造函数返回之后完成。b一定为2，因为final变量禁止了构造函数内的指令重排序，确保在构造函数return之前完成赋值。



底层原理是：编译器会在final域的写之后，构造函数return之前，插入一个Store屏障



</br>

# 区别

- static作用于成员变量用来表示只保存一份副本，而final的作用是用来保证变量不可变
- final主要是保证某个值不变，或者不类不被继承。volatitle主要是保证多个线程对该变量的内存可见性，常用于多个线程开始、结束的标记符等



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


