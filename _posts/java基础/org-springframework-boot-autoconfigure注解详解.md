---
title: org.springframework.boot.autoconfigure注解详解
category: java基础
date: 2020-06-30 21:25:26
updated: 2020-06-30 21:25:26
enname: autoconfigureboot
categories: java基础
tags:
	- annotation注解
	- Spring
keywords: java,annotation,annotation注解,java注解,autoconfigure注解
permalink:
thumbnail:
---

# @SpringBootApplication

详情可查看“Springboot体系结构及实现原理”

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(
excludeFilters = {@Filter(
type = FilterType.CUSTOM,
classes = {TypeExcludeFilter.class}
), @Filter(
type = FilterType.CUSTOM,
classes = {AutoConfigurationExcludeFilter.class}
)}
)
public @interface SpringBootApplication {
@AliasFor(
annotation = EnableAutoConfiguration.class
)
Class<?>[] exclude() default {};

@AliasFor(
annotation = EnableAutoConfiguration.class
)
String[] excludeName() default {};

@AliasFor(
annotation = ComponentScan.class,
attribute = "basePackages"
)
String[] scanBasePackages() default {};

@AliasFor(
annotation = ComponentScan.class,
attribute = "basePackageClasses"
)
Class<?>[] scanBasePackageClasses() default {};
}
```

- exclude:可以设置根据类路径排除自动配置
- excludeName:可以设置根据类名称排除自动配置
- scanBasePackages：针对ComponentScan注解类型进行扫描和注入，添加别名basePackages
- scanBasePackageClasses：针对ComponentScan注解类型进行扫描和注入，添加别名basePackageClasses

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


