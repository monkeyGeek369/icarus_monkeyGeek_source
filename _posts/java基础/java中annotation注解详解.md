---
title: java中annotation注解详解
category: java基础
date: 2020-06-30 21:23:52
updated: 2020-06-30 21:23:52
enname: javaannotation
categories: java基础
tags:
	- annotation注解
	- Spring
	- 面试
keywords: java,annotation,annotation注解,java注解
permalink:
thumbnail: ../../image/annotationstruct.png
---

# 什么是注解

对于很多初次接触的开发者来说应该都有这个疑问？***Annontation***是Java5开始引入的新特征，中文名称叫**注解**。<!--more-->它提供了一种安全的类似注释的机制，用来将任何的信息或元数据（metadata）与程序元素（类、方法、成员变量等）进行关联。为程序的元素（类、方法、成员变量）加上更直观更明了的说明，这些说明信息是与程序的业务逻辑无关，并且供指定的工具或框架使用。Annontation像一种修饰符一样，应用于包、类型、构造方法、方法、成员变量、参数及本地变量的声明语句中。

Java注解是附加在代码中的一些元信息，用于一些工具在编译、运行时进行解析和使用，起到说明、配置的功能。注解不会也不能影响代码的实际逻辑，仅仅起到辅助性的作用。包含在 java.lang.annotation 包中。

</br>

</br>



# 注解的作用是什么

- 生成文档。这是最常见的，也是java 最早提供的注解。常用的有@param @return 等
- 跟踪代码依赖性，实现替代配置文件功能。比如Dagger 2 依赖注入，未来java 开发，将大量注解配置，具有很大用处;
- 在编译时进行格式检查。如@override 放在方法前，如果你这个方法并不是覆盖了超类方法，则编译时就能检查出。

</br>

</br>



# 源码/代码详解

## 元注解

元注解是指注解的注解。

- Annotation型定义为@interface, 所有的Annotation会自动继承java.lang.Annotation这一接口,并且不能再去继承别的类或是接口.
- 参数成员只能用public或默认(default)这两个访问权修饰
- 参数成员只能用基本类型byte,short,char,int,long,float,double,boolean八种基本数据类型和String、Enum、Class、annotations等数据类型,以及这一些类型的数组.
- 要获取类方法和字段的注解信息，必须通过Java的反射机制来获取 Annotation对象,因为你除此之外没有别的获取注解对象的方法
- 注解也可以没有定义成员



元注解包括  @Retention @Target @Document @Inherited四种

- @Target：用于标注该注解适应的目标

```java
ElemenetType.CONSTRUCTOR 构造器声明
ElemenetType.FIELD 域声明(包括 enum 实例) 
ElemenetType.LOCAL_VARIABLE 局部变量声明 
ElemenetType.METHOD 方法声明 
ElemenetType.PACKAGE 包声明 
ElemenetType.PARAMETER 参数声明 
ElemenetType.TYPE 类，接口(包括注解类型)或enum声明
```

- @Retention：用于标注该注解的生命周期

```java
RetentionPolicy.SOURCE 注解将被编译器丢弃 
RetentionPolicy.CLASS 注解在class文件中可用，但会被VM丢弃 
RetentionPolicy.RUNTIME VM将在运行期也保留注释，因此可以通过反射机制读取注解的信息
```

- @Document：指示将该注解包含在javadoc中
- @Inherited：指示允许子类继承父类中的注解
- @Repeatable:用于标识该自定义注解可以被重复标注到同一个对象，并指定容器

示例如下：

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
public @interface Persons {
Person[] value();
}

@Repeatable(Persons.class)
public @interface Person{
String role() default "";
}

@Person(role="father")
@Person(role="son")
public class Man {
String name="";
}
```



</br>

## 内置注解

在java.lang中包含三种内置注解

- @Override，表示当前方法定义将覆盖超类中的方法，如果签名出错会收到编译器的提示；
- @Deprecated，表示过期或不推荐的方法；
- @SuppressWarnings，关闭不当的编译器警告信息。

</br>

## 自定义注解

自定义注解即通过元注解来构建自己需要的注解

```java
@Target({METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Inherited
@Documented
public @interface RestMethod {

Class<?> queryClass() default ObjectUtils.Null.class;

boolean login() default false;

long ttl() default 0;

boolean parseOpenId() default false;

boolean useCookie() default false;

boolean headerContext() default true;

}
```

</br>

</br>

# 注解的底层实现机制

![](../../../../image/annotationstruct.png)

annotation的底层实现机制如上图所示，注解本质是一个继承了Annotation 的特殊接口，而该接口的具体实现类是Java 运行时生成的动态代理类。我们可以通过反射获取注解，返回的是Java 运行时生成的动态代理对象$Proxy1。通过代理对象调用自定义注解（接口）的方法，会最终调用AnnotationInvocationHandler 的invoke 方法。该方法会从memberValues 这个Map 中索引出对应的值。而memberValues 的来源是Java 常量池。

</br>

</br>

# 注解的具体使用

如上所属注解的实现机制是通过反射，那么在使用时无论何种业务、何种途径归根结底都是通过JVM提供的反射机制实现获取注解和使用，因此我们要构建自己的“注解处理器”，常见如下途径：

- 注解与拦截器结合：通过拦截器传递的handler参数获取反射对象进而获取注解
- 注解与AOP结合：通过AOP传递的ProceedingJoinPoint参数获取反射对象进而获取注解

</br>

</br>

参考资料：

[1]https://www.cnblogs.com/lbangel/p/3523741.html



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


