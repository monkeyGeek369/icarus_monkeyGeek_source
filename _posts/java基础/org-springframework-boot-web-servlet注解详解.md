---
title: org.springframework.boot.web.servlet注解详解
category: java基础
date: 2020-06-30 21:25:45
updated: 2020-06-30 21:25:45
enname: annotationServlet
categories: java基础
tags:
	- annotation注解
	- Spring
keywords: java,annotation,annotation注解,java注解,spring注解,ServletComponentScan注解
permalink:
thumbnail:
---

# @ServletComponentScan

通常标注在启动类上，用于设置servlet容器组建的扫描，被设定的范围内 Servlet、Filter、Listener可以直接通过@WebServlet、@WebFilter、@WebListener注解自动注册，无需其他代码<!--more-->

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Import({ServletComponentScanRegistrar.class})
public @interface ServletComponentScan {
@AliasFor("basePackages")
String[] value() default {};

@AliasFor("value")
String[] basePackages() default {};

Class<?>[] basePackageClasses() default {};
}
```

- value与basePackages互设置别名
- basePackageClasses：可以通过其来设置需扫描的包类路径

```java
@SpringBootApplication
@ServletComponentScan("com.test")
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

```java
package com.test

@WebServlet(name="TestServlet",urlPatterns="/test")
public class TestServlet extends HttpServlet {
	
	private static final long serialVersionUID = 1L;

	@Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) 
    		throws ServletException, IOException {
        System.out.println("test");
    }
}
```



</br>

</br>

参考资料：

[1]https://blog.csdn.net/m0_37739193/article/details/85097477

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


