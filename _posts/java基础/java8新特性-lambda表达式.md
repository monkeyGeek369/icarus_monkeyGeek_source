---
title: java8新特性-lambda表达式
category: java基础
date: 2020-04-15 20:36:17
updated: 2020-04-15 20:36:17
enname: lambda
categories: java基础
tags:
	- java
	- java8新特性
	- lambda
keywords: java,java8新特性,lambda
permalink:
thumbnail:
---

# 简介

lambda表达式作为java8的新特性极大的提高了编程效率以及可读性,本质上来讲lambda表达式是对传统面向对象编程到函数式编程的转变.<!--more-->举个简单的小栗子来感受下lambda的魅力.

```java
Map<String, String> cargo = specialCargo.getChildren().stream()
                .filter(item -> item.getValue() != null)
                .collect(Collectors.toMap(x -> x.getId(), x -> x.getValue().toString()));
```

lambda与stream API的结合会使得java中的集合操作更加便利和高效.Collectors.toMap(x -> x.getId(), x -> x.getValue().toString())实现将list转换为指定的map集合



</br>

# 语法

***一般语法***

```java
(Type1 param1, Type2 param2, ..., TypeN paramN) -> {
statment1;
statment2;
//.............
return statmentM;
}
```



***单参数语法***

```java
param1 -> {
statment1;
statment2;
//.............
return statmentM;
}
```

当lambda表达式的参数个数只有一个，可以省略小括号

例如：将列表中的字符串转换为全小写

```java
List<String> proNames = Arrays.asList(new String[]{"Ni","Hao","Lambda"});
List<String> lowercaseNames1 = proNames.stream().map(name -> {return name.toLowerCase();}).collect(Collectors.toList());
```



***单语句写法***

```java
param1 -> statment
```

当lambda表达式只包含一条语句时，可以省略**大括号、return和语句结尾的分号**

例如：将列表中的字符串转换为全小写

```java
List<String> proNames = Arrays.asList(new String[]{"Ni","Hao","Lambda"});
List<String> lowercaseNames2 = proNames.stream().map(name -> name.toLowerCase()).collect(Collectors.toList());
```



***方法引用写法***

具体可参考本站"java8新特性-方法引用"

例如：将列表中的字符串转换为全小写

```java
List<String> proNames = Arrays.asList(new String[]{"Ni","Hao","Lambda"});
List<String> lowercaseNames3 = proNames.stream().map(String::toLowerCase).collect(Collectors.toList());
```



</br>

# lambda表达式变量

```java
//将为列表中的字符串添加前缀字符串
String waibu = "lambda :";
List<String> proStrs = Arrays.asList(new String[]{"Ni","Hao","Lambda"});
List<String>execStrs = proStrs.stream().map(chuandi -> {
Long zidingyi = System.currentTimeMillis();
return waibu + chuandi + " -----:" + zidingyi;
}).collect(Collectors.toList());
execStrs.forEach(System.out::println);
```

输出如下:

lambda :Ni -----:1474622341604

lambda :Hao -----:1474622341604

lambda :Lambda -----:1474622341604



变量解释:

变量waibu ：外部变量

变量chuandi ：传递变量

变量zidingyi ：内部自定义变量



解读如下:

lambda表达式访问外部变量有一个非常重要的限制：

变量不可变（只是引用不可变，而不是真正的不可变）。

当在表达式内部修改waibu = waibu + " ";时，IDE就会提示你：

Local variable waibu defined in an enclosing scope must be final or effectively final

编译时会报错。因为变量waibu被lambda表达式引用，所以编译器会隐式的把其当成final来处理。

以前Java的匿名内部类在访问外部变量的时候，外部变量必须用final修饰。现在java8对这个限制做了优化，可以不用显示使用final修饰，但是编译器隐式当成final来处理。

Java8中的lambda表达式,并不是完全闭包，lambda表达式对值封闭，不对变量封闭。简单点来说就是局部变量在lambda表达式中如果要使用，必须是声明final类型或者是隐式的final。其内部使用静态变量或者实例变量则不会出现问题，这是因为final、static、实例变量等存放在堆区，线程间可共享，而局部在栈区线程间不共享，将导致被回收的风险，如果是多线程使用lambda表达式存在不安全因素。

</br>

# lambda表达式中的this

在lambda中，this不是指向lambda表达式产生的那个SAM对象，而是声明它的外部类对象。



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


