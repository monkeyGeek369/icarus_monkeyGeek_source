---
title: java常见面试问题
category: java基础
date: 2023-11-11 21:29:48
updated: 2023-11-11 21:29:48
enname: java_question
categories: java基础
tags:
	- 面试
keywords: java,面试，问题
permalink:
thumbnail:
---

java常见且经典面试问题收集

<!--more-->

1、object o = new object();的o占中多少字节

如何查看对象头内存分配：通过idea插件JOL来查看

对象头组成：markword（8字节）、类型指针（默认不开启压缩4字节，开启后8字节）、实例数据（n字节）、对齐（保证对象头可以被8字节整除的补齐字节数）



注：可以通过idea中的插件jol来查看对象的存储信息

答案：开启压缩16字节（8+4+0+4），开启压缩16字节（8+8+0+0）

</br>

</br>

2、对象的创建过程

检查类是否已加载-》分配内存空间--》属性设置默认值--〉设置对象头--》执行构造方法



</br>

</br>

3、对象在内存中的存储分布

普通对象：对象头（mark work/class pointer）、对象实际数据(instance data)、对齐(padding)

数组：对象头（mark work/class pointer/length）、对象实际数据(instance data)、对齐(padding)





</br>

</br>

4、对象头具体包括什么

- Mark Word：8字节，存储对象hashcode、锁信息、分代年龄、GC信息等
- Class Pointer：4字节（默认开启压缩）/8字节类数据指针，用来确定这个对象属于哪个类
- Length：4字节，如果对象是数组则存在length用来确定数组长度



</br>

</br>

5、单例模式下DCL(double check locking双重校验锁)是否需要加volatile

懒汉式：获取的时候才创建

饿汉式：类加载的时候就创建

双重校验锁模式：

```java
public class Singleton7 {
    private static volatile Singleton7 instance = null;

    private Singleton7() {}

    public static Singleton7 getInstance() {
        if (instance == null) {//不能去掉，否则会出现多线程竞争加锁
            synchronized (Singleton7.class) {
                if (instance == null) {//不能去掉，否则会创建多个对象
                    instance = new Singleton7();//此步骤借助volatile设置内存屏障避免读取到默认值
                }
            }
        }
        return instance;
    }
}
```





参考资料：https://zhuanlan.zhihu.com/p/150004430

</br>

</br>

6、对象怎么定位即怎么找到对象

直接指针：指针指向堆对象，堆对象中的类型数据指针指向方法区的c++的class对象，在通过c++对象找到堆中的java class对象

句柄方式：指针指向堆中的二元组，二元组记录实例数据指针（指向堆中的类实例）和类型数据指针（指向方法区的c++的class对象，在通过c++对象找到堆中的java class对象）



</br>

</br>

7、对象怎么分配

1. 是否可以创建在栈上（根据内存逃逸分析来判断），否则继续
2. 是否足够大，足够大则存放在老年代
3. 进行TLAB判断（线程本地分配缓存区），即是否可以分配到本线程在e区独享的区域
4. GC回收后能够存活则进入survive区
5. 多次GC回收后依然存活则进入老年代



</br>

</br>

8、为什么hotspot不实用c++对象来代表java对象

c++对象有虚表的概念，导致对象占用内存加大，不适合代表java对象

目前是用二元指针来代表java对象，记录实例数据指针（指向堆中的类实例）和类型数据指针（指向方法区的class对象）



</br>

</br>

9、class对象是在堆还是在方法区

放在堆



指针指向堆对象，堆对象中的类型数据指针指向方法区的c++的class对象，在通过c++对象找到堆中的java class对象



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


