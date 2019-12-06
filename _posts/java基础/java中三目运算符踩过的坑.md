---
title: java中三目运算符踩过的坑
category: java基础
date: 2019-12-06 15:00:58
updated: 2019-12-06 15:00:58
enname: ternary
categories: java基础
tags:
	- java
	- java语法基础
keywords: java,语法基础,java基础,三目运算符
permalink:
thumbnail:
---

# 这是一个什么样的坑?

直接上代码:

```java
//H付款方式
Integer hPaymentWayId = fullfillDTOShip != null ? fullfillDTOShip.getHPaymentWayId() : 0;
```

看似正常的代码在遇到fullfillDTOShip.getHPaymentWayId()为null时会报NPE错误,为什么会这也呢?

<!--more-->

</br>

# 重新认识java中的三目运算符

以JDK8为例通过[JLS](https://docs.oracle.com/javase/specs/jls/se8/html/jls-15.html#jls-15.25)我们知道三目运算符形如下:(JDK7略有差异详情可看[JLS7](https://docs.oracle.com/javase/specs/jls/se7/html/jls-15.html#jls-15.25))

```
条件表达式：
ConditionalOrExpression
ConditionalOrExpression ? 表达式 : ConditionalExpression
ConditionalOrExpression ? 表达式 : LambdaExpression
```

三目运算符由?:将整体表达式分为三部分,依此称为第一操作数\第二操作数\第三操作数其中第一操作数被硬性要求必须时Boolean类型表达式,而整个三目运算符的运算结果由第二\第三操作数共同决定,就因为这个共同决定造成了我们本文开头的问题.

第二操作数与第三操作数只能是布尔条件表达式\数值条件表达式\参考条件表达式,其它表达式均不支持,而第二和第三操作数所对应的不同表达式类型共同作用完整三目表达式结果,其中涉及到拆装箱问题(即引起NPE问题的元凶)

</br>

# 布尔条件表达式

布尔条件表达式的类型确定如下：

- 如果第二和第三操作数均为type `Boolean`，则条件表达式的类型为`Boolean`。
- 否则，条件表达式的类型为`boolean`。

<span style="color:red;">总结以下:如果第二或第三操作数其中之一是原始类型,那么整个三目表达式结果即为原始boolean类型,否则全为原始类型或全为封装类型则最终表达式一致.</span>

这里就会出现NPE风险,例如

```java
Boolean bo3 = (1 == 1 ? fullfillShipDTO.getIsActive() : false);
System.out.print("bo3\n");
System.out.print(bo3 + "\n");
```

以上代码会报错,经过反编译我们可以找到原因:

```java
Boolean bo3 = (boolean)fullfillShipDTO.getIsActive();
System.out.print("bo3\n");
System.out.print(bo3 + "\n");
```

如果fullfillShipDTO.getIsActive()为null那么此时进行拆箱操作必定会报NPE.

</br>

# 数值条件表达式

数字条件表达式的类型确定如下：

- 如果第二个和第三个操作数具有相同的类型，那么这就是条件表达式的类型。

- 如果第二和第三个操作数中的一个是原始类型的Ť，其它的类型是施加装箱转换（结果[§5.1.7](https://docs.oracle.com/javase/specs/jls/se8/html/jls-5.html#jls-5.1.7)）到Ť，那么条件表达式的类型是Ť。

- 如果其中一个操作数的类型为`byte`或`Byte`，而另一个操作符的类型为`short`或`Short`，则条件表达式的类型为`short`。

- 如果操作数之一的类型为Ť其中Ť是`byte`，`short`，或`char`，另一操作数为常量表达式（[§15.28](https://docs.oracle.com/javase/specs/jls/se8/html/jls-15.html#jls-15.28)型的）`int`，它的值是在式表示的Ť，那么条件表达式的类型是Ť。

- 如果一个操作数的类型为T，其中T为`Byte`，`Short`或`Character`，而另一个操作数为类型`int`的常量表达式，其值可表示为U型，这是对T进行拆箱转换的结果，则类型为条件表达式是ü。

- 否则，将二进制数值提升（第[5.6.2节](https://docs.oracle.com/javase/specs/jls/se8/html/jls-5.html#jls-5.6.2)）应用于操作数类型，条件表达式的类型是第二和第三操作数的提升类型。

  请注意，二进制数值升级会执行值集转换（第[5.1.13节](https://docs.oracle.com/javase/specs/jls/se8/html/jls-5.html#jls-5.1.13)），并且可能会执行拆箱转换（第[5.1.8节](https://docs.oracle.com/javase/specs/jls/se8/html/jls-5.html#jls-5.1.8)）。

<span style="color:red;">总结以下:同布尔条件表达式相同如果第二或第三操作数其中之一是原始类型,那么整个三目表达式结果即为原始类型,否则全为原始类型或全为封装类型则最终表达式一致.</span>

例如:

```java
//2.数值条件表达式
        Integer in1 = 1 == 1 ? 1 : 0;
        System.out.print("in1\n");
        System.out.print(in1 + "\n");

        Integer in2 = 1 == 1 ? Integer.valueOf(1) : Integer.valueOf(0);
        System.out.print("in2\n");
        System.out.print(in2 + "\n");

        Integer in3 = (1 == 1 ? 0 : fullfillShipDTO.getHPaymentWayId());
        System.out.print("in3\n");
        System.out.print(in3 + "\n");

        Integer in4 = 1 == 1 ? fullfillShipDTO.getHPaymentWayId() : 0;
        System.out.print("in4\n");
        System.out.print(in4 + "\n");
```

上例中in4会报错,其它正常执行,通过反编译代码我们发现:

```java
Integer in1 = 1;
    System.out.print("in1\n");
    System.out.print(in1 + "\n");
    Integer in2 = 1;
    System.out.print("in2\n");
    System.out.print(in2 + "\n");
    Integer in3 = 0;
    System.out.print("in3\n");
    System.out.print(in3 + "\n");
    Integer in4 = (int)fullfillShipDTO.getHPaymentWayId();
    System.out.print("in4\n");
    System.out.print(in4 + "\n");
```

如果fullfillShipDTO.getHPaymentWayId()为null在进行拆箱操作时报NPE

</br>

# 参考条件表达式

如果引用条件表达式出现在分配上下文或调用上下文中（第[5.2节](https://docs.oracle.com/javase/specs/jls/se8/html/jls-5.html#jls-5.2)，第[5.3节](https://docs.oracle.com/javase/specs/jls/se8/html/jls-5.html#jls-5.3)），则它是一个多边形表达式。否则，它是一个独立的表达式。

当多引用条件表达式出现在目标类型为T的特定种类的上下文中时，其第二和第三操作数表达式类似地出现在目标类型为T的相同种类的上下文中。

多边形引用条件表达式的类型与其目标类型相同。

独立引用条件表达式的类型确定如下：

- 如果第二个操作数和第三个操作数具有相同的类型（可能是null类型），则这是条件表达式的类型。
- 如果第二个操作数和第三个操作数之一的类型为空类型，而另一个操作数的类型为引用类型，则条件表达式的类型为该引用类型。
- 否则，第二和第三操作数分别为S 1和S 2类型。令T 1为对S 1进行装箱转换所产生的类型，而T 2为对S 2进行装箱转换所产生的类型。条件表达式的类型是将捕获转换（第[5.1.10节](https://docs.oracle.com/javase/specs/jls/se8/html/jls-5.html#jls-5.1.10)）应用于lub（T 1，T的结果2）。

参考条件表达式的含义是其可以为lambda表达式\Map\集合\自定义对象等

<span style="color:red;">总结以下:当二\三操作数相同类型时表达式的值即为该类型,否则若出现不同则根据第二或第三操作数所对应的非原始类型进行装修转换操作,不进行拆箱操作</span>

参考条件表达式相对安全,如下:

```java
Map<String, Boolean> map = new HashMap<>();
        Boolean reference1 = (map != null ? map.get("test") : map.get("test2"));
        System.out.print("reference1\n");
        System.out.print(reference1 + "\n");

        Boolean reference2 = (map != null ? map.get("test") : null);
        System.out.print("reference2\n");
        System.out.print(reference2 + "\n");

        Boolean reference3 = (map != null ? true : map.get("test"));
        System.out.print("reference3\n");
        System.out.print(reference3 + "\n");
```

如上代码分别对应参考条件表达式三种状态,均不会报NPE错误,伪代码如下:

```java
HashMap map = new HashMap();
    Boolean reference1 = map != null ? (Boolean)map.get("test") : (Boolean)map.get("test2");
    System.out.print("reference1\n");
    System.out.print(reference1 + "\n");
    Boolean reference2 = map != null ? (Boolean)map.get("test") : null;
    System.out.print("reference2\n");
    System.out.print(reference2 + "\n");
    Boolean reference3 = map != null ? Boolean.valueOf(true) : (Boolean)map.get("test");
    System.out.print("reference3\n");
    System.out.print(reference3 + "\n");
```

根据reference3可知即使是原始类型true也将会根据S2做装箱操作.

</br>

参考内容:

[1]https://docs.oracle.com/javase/specs/jls/se8/html/jls-15.html#jls-15.25

[2]http://www.hollischuang.com/archives/58

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


