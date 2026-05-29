---
title: springboot项目启动原理
category: spring
date: 2022-06-12 21:53:50
updated: 2022-06-12 21:53:50
enname: sb-start
categories: spring
tags:
	- 面试
	- springboot
keywords: springboot项目启动原理
permalink:
thumbnail:
---

我们开发任何一个Spring Boot项目，都会用到如下的启动类<!--more-->，从上面代码可以看出，Annotation定义（@SpringBootApplication）和类定义（SpringApplication.run）最为耀眼

```java
@SpringBootApplication
public class Application {
public static void main(String[] args) {
SpringApplication.run(Application.class, args);
}
}
```



</br>

# @SpringBootApplication

```java
@Target(ElementType.TYPE) // 注解的适用范围，其中TYPE用于描述类、接口（包括包注解类型）或enum声明
@Retention(RetentionPolicy.RUNTIME) // 注解的生命周期，保留到class文件中（三个生命周期）
@Documented // 表明这个注解应该被javadoc记录
@Inherited // 子类可以继承该注解
@SpringBootConfiguration // 继承了Configuration，表示当前是注解类
@EnableAutoConfiguration // 开启springboot的注解功能，springboot的四大神器之一，其借助@import的帮助
@ComponentScan(excludeFilters = { // 扫描路径设置（具体使用待确认）
@Filter(type = FilterType.CUSTOM, classes = TypeExcludeFilter.class),
@Filter(type = FilterType.CUSTOM, classes = AutoConfigurationExcludeFilter.class) })
public @interface SpringBootApplication {
...
}
```

重要的只有三个Annotation：

@Configuration（@SpringBootConfiguration点开查看发现里面还是应用了@Configuration）

@EnableAutoConfiguration

@ComponentScan



</br>

# @Configuration

指示一个类声明一个或多个@Bean方法，并且可以由Spring容器处理。指示类为ioc容器配置类。

通常使用`AnnotationConfigApplicationContext`或其支持Web变体`AnnotationConfigWebApplicationContext`进行引导。

```java
AnnotationConfigApplicationContext ctx = new AnnotationConfigApplicationContext();
ctx.register(AppConfig.class);//AppConfig为标注了@Configuration的类
ctx.refresh();
MyBean myBean = ctx.getBean(MyBean.class);//AppConfig类中声明的bean
```



</br>

# @ComponentScan

目的是自动扫描并加载符合条件的组件（比如@Component和@Repository等）或者bean定义，最终将这些bean定义加载到IoC容器中。

我们可以通过basePackages等属性来细粒度的定制@ComponentScan自动扫描的范围，如果不指定，则默认Spring框架实现会从声明@ComponentScan所在类的package进行扫描。

注：所以SpringBoot的启动类最好是放在root package下，因为默认不指定basePackages。



</br>

# @EnableAutoConfiguration

帮助SpringBoot应用将所有符合条件的@Configuration配置都加载到当前SpringBoot，并创建对应配置类的Bean，并把该Bean实体交给IoC容器进行管理。

```java
@SuppressWarnings("deprecation")
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@AutoConfigurationPackage //自动配置包，自动配置被标注类的同级及子级包组件
@Import(EnableAutoConfigurationImportSelector.class) //导入自动配置的组件
public @interface EnableAutoConfiguration {
...
}
```

最关键的要属@Import(EnableAutoConfigurationImportSelector.class)，借助EnableAutoConfigurationImportSelector，@EnableAutoConfiguration可以帮助SpringBoot应用将所有符合条件的@Configuration配置都加载到当前SpringBoot创建并使用的IoC容器。就像一只“八爪鱼”一样。



</br>

</br>

# SpringApplication

执行流程大体如下：

- 信息收集：是否创建应用程序上下文（ApplicationContext）、有哪些上下文初始化器（ApplicationContextInitializer），有哪些应用监听器（ApplicationListener）
- 环境创建：包括配置要使用的PropertySource以及Profile，并调用监听器的environmentPrepared()方法进行告知。
- 应用程序上下文创建：创建ApplicationContext并调用初始化方法初始化，如contextPrepared()方法、contextLoaded()方法等
- 刷新应用程序上下文：调用refresh()方法进行刷新，并调用监听器finished()方法进行通知



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


