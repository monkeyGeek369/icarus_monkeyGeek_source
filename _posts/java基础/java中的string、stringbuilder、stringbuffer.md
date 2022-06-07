---
title: java中的string、stringbuilder、stringbuffer
category: java基础
date: 2022-06-07 23:19:36
updated: 2022-06-07 23:19:36
enname: java-string
categories: java基础
tags:
	- 面试
keywords: string、stringbuilder、stringbuffer
permalink:
thumbnail:
---

# String

- String str="hello world"和String str=new String("hello world")的区别，前者存放在运行时常量池，后者存放在堆中。<!--more-->
- String类是final类，也即意味着String类不能被继承，并且它的成员方法都默认为final方法。
- String类其实是通过char数组来保存字符串的
- 对String对象的任何改变都不影响到原对象，相关的任何change操作都会生成新的对象

</br>

# StringBuilder

- 为字符串操作提供更加便捷的方法



</br>

# StringBuffer

- 为字符串操作提供更加便捷的方法
- 内部方法由synchronized修饰，即线程安全的

</br>

# 常见问题

1. 下面这段代码的输出结果是什么？

　　String a = "hello2"; 　　String b = "hello" + 2; 　　System.out.println((a == b));

　　输出结果为：true。原因很简单，"hello"+2在编译期间就已经被优化成"hello2"，因此在运行期间，变量a和变量b指向的是同一个对象。

2.下面这段代码的输出结果是什么？

　　String a = "hello2"; 　 String b = "hello";    String c = b + 2;    System.out.println((a == c));

　　输出结果为:false。由于有符号引用的存在，所以 String c = b + 2;不会在编译期间被优化，不会把b+2当做字面常量来处理的，因此这种方式生成的对象事实上是保存在堆上的。

3.下面这段代码的输出结果是什么？

　　String a = "hello2";  　 final String b = "hello";    String c = b + 2;    System.out.println((a == c));

　　输出结果为：true。对于被final修饰的变量，会在class文件常量池中保存一个副本，也就是说不会通过连接而进行访问，对final变量的访问在编译期间都会直接被替代为真实的值。

4.String str = new String("abc")创建了多少个对象？

　　这个问题在很多书籍上都有说到比如《Java程序员面试宝典》，包括很多国内大公司笔试面试题都会遇到，大部分网上流传的以及一些面试书籍上都说是2个对象，这种说法是片面的。

　　很显然，new只调用了一次，也就是说只创建了一个对象。

　　而这道题目让人混淆的地方就是这里，这段代码在运行期间确实只创建了一个对象，即在堆上创建了"abc"对象。而为什么大家都在说是2个对象呢，这里面要澄清一个概念 该段代码执行过程和类的加载过程是有区别的。在类加载的过程中，确实在运行时常量池中创建了一个"abc"对象，而在代码执行过程中确实只创建了一个String对象。

　　因此，这个问题如果换成 String str = new String("abc")涉及到几个String对象？合理的解释是2个。

　　个人觉得在面试的时候如果遇到这个问题，可以向面试官询问清楚”是这段代码执行过程中创建了多少个对象还是涉及到多少个对象“再根据具体的来进行回答。

5.下面这段代码输出结果为：

```java
public class Main {
    public static void main(String[] args) {
        String a = "hello2";
        final String b = getHello();
        String c = b + 2;
        System.out.println((a == c));
    }
     
    public static String getHello() {
        return "hello";
    }
}
```

　　输出结果为false。这里面虽然将b用final修饰了，但是由于其赋值是通过方法调用返回的，那么它的值只能在运行期间确定，因此a和c指向的不是同一个对象。



6.下面这段代码的输出结果是什么？

```java
    public static void main(String[] args) {
        String a = "hello";
        String b =  new String("hello");
        String c =  new String("hello");
        String d = b.intern();
         
        System.out.println(a==b);
        System.out.println(b==c);
        System.out.println(b==d);
        System.out.println(a==d);
    }
}
```

False false false true

这里面涉及到的是String.intern方法的使用。在String类中，intern方法是一个本地方法，在JAVA SE6之前，intern方法会在运行时常量池中查找是否存在内容相同的字符串，如果存在则返回指向该字符串的引用，如果不存在，则会将该字符串入池，并返回一个指向该字符串的引用。因此，a和d指向的是同一个对象。

</br>

7.下面这段代码1）和2）的区别是什么？

```java
public class Main {
    public static void main(String[] args) {
        String str1 = "I";
        //str1 += "love"+"java";        1)
        str1 = str1+"love"+"java";      //2)
         
    }
}
```

　　1）的效率比2）的效率要高，1）中的"love"+"java"在编译期间会被优化成"lovejava"，而2）中的不会被优化。

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


