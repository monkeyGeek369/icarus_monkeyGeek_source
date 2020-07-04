---
title: org.springframework.core.annotation注解详解
category: java基础
date: 2020-06-30 21:24:36
updated: 2020-06-30 21:24:36
enname: annotationcore
categories: java基础
tags:
	- annotation注解
	- Spring
keywords: java,annotation,annotation注解,java注解
permalink:
thumbnail:
---

# @AliasFor注解

在Spring的众多注解中，经常会发现很多注解的不同属性起着相同的作用，比如@RequestMapping的value属性和path属性，这就需要做一些基本的限制，比如value和path的值不能冲突，比如任意设置value或者设置path属性的值，都能够通过另一个属性来获取值等等。为了统一处理这些情况，Spring创建了@AliasFor标签。即别名注解

<!--more-->

</br>

## 方式一：在同一注解内使用

```java
@Target({ElementType.METHOD, ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented@Mappingpublic @interface RequestMapping {

@AliasFor("path")
String[] value() default {};

@AliasFor("value")
String[] path() default {};

//...
}
```

注意：互为别名的属性值类型、默认值都需要相同。必须成对配置，必须设置默认值。

</br>

## 方式二：显式覆盖元注解属性

```java
@Retention(RetentionPolicy.RUNTIME)
@ContextConfigurationpublic @interface STC {

@AliasFor(value = "classes", annotation = ContextConfiguration.class)//针对ContextConfiguration.class类型注解内的classes设置别名
Class<?>[] cs() default {};

}
```

</br>

## 方式三：隐士声明别名

```java
@ContextConfiguration
public @interface MyTestConfig {

@AliasFor(annotation = ContextConfiguration.class, attribute = "locations")
String[] value() default {};

@AliasFor(annotation = ContextConfiguration.class, attribute = "locations")
String[] groovyScripts() default {};

@AliasFor(annotation = ContextConfiguration.class, attribute = "locations")
String[] xmlFiles() default {};
}
```

相当于value、groovyScripts和xmlFiles也互为别名

</br>

## 方式四：别名传递

```java
@MyTestConfig
public @interface GroovyOrXmlTestConfig {

@AliasFor(annotation = MyTestConfig.class, attribute = "groovyScripts")
String[] groovy() default {};

@AliasFor(annotation = ContextConfiguration.class, attribute = "locations")
String[] xml() default {};
}
```

- GroovyOrXmlTestConfig把 @MyTestConfig（参考上一个案例）作为元注解；
- 定义了groovy属性，并作为MyTestConfig中的groovyScripts属性的别名；
- 定义了xml属性，并作为ContextConfiguration中的locations属性的别名；
- 因为MyTestConfig中的groovyScripts属性本身就是ContextConfiguration中的locations属性的别名；所以xml属性和groovy属性也互为别名；

</br>

参考资料：

[1]https://www.jianshu.com/p/869ed7037833

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


