---
title: java中的时间精度
category: java基础
date: 2019-12-04 19:29:28
updated: 2019-12-04 19:29:28
enname: javatime
categories: java基础
tags:
keywords: java,时间精度,时间操作
permalink:
thumbnail:
---

# 一 前言

今天线上业务发现了一个问题,业务需求是获取最新更新的XX一条记录(XX记录可能会有多条并且由于后面业务的影响XX记录的集合可能新增\减少\更新),因此通过sql语句查询并以updateTime做倒叙排序取第一条即可.<!--more-->然而事情并没有这么简单,实际使用发现获取最新一条数据并不稳定,仔细筛查发现数据库DateTime精度过低只精确到秒,这样有很大机率在updateTime上相同.

基于此需要提高updateTime的精度,如精确到毫秒(3)\微妙(6),通过调整datetime精度即可

```
ALTER TABLE `fulfill_shipping_contract` MODIFY COLUMN `updated_date` DATETIME(6) NOT NULL COMMENT '更新时间' ;
```

仅仅设置数据库时间精度还不够,需要编程语言支持高精度时间,以java为例进行java时间精度介绍.

</br>

# 二 时间基础

| 毫秒 | millisecond | 1ms  | 1毫秒=0.001秒=10-3秒              |
| ---- | ----------- | ---- | --------------------------------- |
| 微秒 | microsecond | 1μs  | 1微秒=0.000001=10-6秒             |
| 纳秒 | nanosecond  | 1ns  | 1纳秒=0.0000000001秒=10-9秒       |
| 皮秒 | picosecond  | 1ps  | 1皮秒=0.0000000000001秒=10-12秒   |
| 飞秒 | femtosecond | 1fs  | 1飞秒=0.000000000000001秒=10-15秒 |

</br>

# 三 java中的时间精度

### 3.1获取时间

java.util.Date作为java日期对象的顶级类提供对时间的基本操作.

```
System.out.print("时间测试\n");
System.out.print(new Date() + "\n");
```

继续观察我们可以发现Date()初始化于当前时间戳

```java
/** 
* Allocates a <code>Date</code> object and initializes it so that 
* it represents the time at which it was allocated, measured to the 
* nearest millisecond. 
* * @see     java.lang.System#currentTimeMillis() 
*/
public Date() {    
this(System.currentTimeMillis());
}
```



### 3.2获取毫秒

System.currentTimeMillis() 起始时间是基于 1970.1.1 0:00:00 这个确定的时间

```java
Long milliseconds = System.currentTimeMillis();
System.out.print("毫秒测试-毫秒\n");
System.out.print(milliseconds + "\n");

System.out.print("毫秒测试-毫秒时间\n");
System.out.print(new Date(milliseconds) + "\n");
```

一般情况下保存到数据库中的时间精确到毫秒即可



### 3.3获取微秒

1ms=1000us(一毫秒等于一千微秒),微秒在java中没有具体的方法提供,但是我们可以借助java提供的纳秒通过差值来计算微秒数值.

```java
Long nanoTimeStart = System.nanoTime();
Long nanoTimeEnd = System.nanoTime();
System.out.print("微妙测试-微妙\n");
System.out.print(nanoTimeEnd - nanoTimeStart + "\n");   //得到六位数字,如166400
System.out.print((nanoTimeEnd - nanoTimeStart)/1000L + "\n");   //得到三位数字,如166
System.out.print("微妙测试-毫秒转换成微秒\n");
System.out.print(milliseconds * 1000 + (nanoTimeEnd - nanoTimeStart) / 1000L + "\n");
```

得到的微秒结果将会是三位长整型数字,如166,但是以上代码可能会有风险,因为我们是通过纳秒差值来计算的微秒,而纳秒本身是继续cpu核心的原子钟周期来计时的,在多核处理器中这种分开取纳秒的方式很可能取自不同的内核,从而造成计算错误.

- 微秒无法通过现有手段直接获得,需要借助纳秒差值进行计算获得

- 微秒本质上代表的是两个时间上的微秒差异,因此并不会很精确

- 可以将毫秒转换成微秒,但是无法将转换后的结果生成日期时间对象

  

数据库支持微秒(即精度达到6,如2019-10-28 17:24:25.123456,其中123代表毫秒,456代表微秒)但是微秒无法转换为java中的日期对象,因此不能通过Date直接赋值的形式完成.针对此有两种解决方案:

- 将Date类型替换为字符类型并进行拼接如"2019-10-28 17:24:25.123456",再更新至数据库
- 将毫秒时间与微秒时间存储在数据库表不同属性中,分别记录



### 3.4获取纳秒

1s=1000000000ns

- System.nanoTime()此方法只能用于测量已过的时间，与系统或钟表时间的其他任何时间概念无关
- 返回值表示从某一固定但任意的时间算起的纳秒数（或许从以后算起，所以该值可能为负）
- System.nanoTime()是基于cpu核心的时钟周期来计时,它的开始时间是不确定的,因此纳秒的取值与操作系统有关
- 纳秒适合计算相对时间段,无法换算成具体时间

```java
Long nanoseconds = System.nanoTime();
System.out.print("纳秒测试-纳秒\n");
System.out.print(nanoseconds + "\n");
```



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


