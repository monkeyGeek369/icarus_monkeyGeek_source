---
title: org.springframework.context.annotation注解详解
category: java基础
date: 2020-06-30 21:25:26
updated: 2020-06-30 21:25:26
enname: annotationSpring
categories: java基础
tags:
	- annotation注解
	- Spring
keywords: java,annotation,annotation注解,java注解,spring注解
permalink:
thumbnail: ../../image/componentScan.png

---

# @ImportResource

用在springboot的启动类，用于将外部xml文件进行解析和注入bean<!--more-->

```java
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.TYPE})
@Documented
public @interface ImportResource {
@AliasFor("locations")
String[] value() default {};

@AliasFor("value")
String[] locations() default {};

Class<? extends BeanDefinitionReader> reader() default BeanDefinitionReader.class;
}
```

- value和locations互设别名
- BeanDefinitionReader：设置需解析xml文件内部bean对象的解析器，默认为BeanDefinitionReader.class

</br>

</br>

# @Bean

用于标注在方法实例、配置类上，标识改对象为Bean对象可以被spring扫描，但是如果spring未扫描该标注对象所处路径则不会注入到spring容器。（通常结合@Component与@Configuration使用）

```java
@Target({ElementType.METHOD, ElementType.ANNOTATION_TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface Bean {
@AliasFor("name")
String[] value() default {};

@AliasFor("value")
String[] name() default {};

Autowire autowire() default Autowire.NO;

String initMethod() default "";

String destroyMethod() default "(inferred)";
}
```

- value与name互设别名
- autowire：设置是否自动织入（即注入），默认为否，如果设置是则无需扫描自动实现注入到spring容器
- initMethod：指定初始化方法，默认为空
- destroyMethod：指定销毁方法，默认为(inferred)

</br>

</br>

# @ComponentScan

通常与@configuration一起使用用于标注在配置类、启动类上，设定扫描范围并实现注入。（ @Component, @Repository, @Service, @Controller,@Bean，@Configuration等可以实现被ComponentScan默认识别但不扫描不会自动注入因此需要扫描）

```java
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.TYPE})
@Documented
@Repeatable(ComponentScans.class)
public @interface ComponentScan {
@AliasFor("basePackages")
String[] value() default {};

@AliasFor("value")
String[] basePackages() default {};

Class<?>[] basePackageClasses() default {};

Class<? extends BeanNameGenerator> nameGenerator() default BeanNameGenerator.class;

Class<? extends ScopeMetadataResolver> scopeResolver() default AnnotationScopeMetadataResolver.class;

ScopedProxyMode scopedProxy() default ScopedProxyMode.DEFAULT;

String resourcePattern() default "**/*.class";

boolean useDefaultFilters() default true;

ComponentScan.Filter[] includeFilters() default {};

ComponentScan.Filter[] excludeFilters() default {};

boolean lazyInit() default false;

@Retention(RetentionPolicy.RUNTIME)
@Target({})
public @interface Filter {
FilterType type() default FilterType.ANNOTATION;

@AliasFor("classes")
Class<?>[] value() default {};

@AliasFor("value")
Class<?>[] classes() default {};

String[] pattern() default {};
}
}
```

- 该标注被标注为@Repeatable即可以实现重复标注
- value与basePackages互设别名用于设定扫描路径
- basePackageClasses用于指定扫描类路径
- nameGenerator：用于设置Bean名称生成器
- scopeResolver：用于设置元数据解析器
- scopeProxy：用于设置范围代理
- resourcePattern：用于设置资源路径，默认为“**/*.class”
- userDefaultFilters：设置是否使用默认扫描路径过滤器
- includeFilters：用于设置包含过滤器对象，类型为ComponentScan.Filter[]数组
- excludeFilters：用于设置排除过滤器对象，类型为ComponentScan.Filter[]数组
- lazyInit：设置是否lazy初始化
- Filter：ComponentScan内部过滤器注解，type：标识过滤器类型，value与classes互设别名，pattern设置路径表达式，案例如下：

```java
@Configuration@ComponentScan(basePackages = "org.example",
includeFilters = @Filter(type = FilterType.REGEX, pattern = ".*Stub.*Repository"),
excludeFilters = @Filter(Repository.class))
public class AppConfig {
...
}
```

filter类型以及适用范围如下：

![](../../../../image/componentScan.png)

</br>

</br>

# @Configuration

通常与@Bean结合使用，用于标注配置类，其具备@Component的特性，必须被扫描才可以实现注入

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Component
public @interface Configuration {
@AliasFor(
annotation = Component.class
)
String value() default "";
}
```

- 简单来看@Configuration为特殊的@Component
- value针对Component设置别名

</br>

</br>

# @PropertySource

通常与@Configuration等结合使用，用于引入外部配置文件实现解析和注入。加载指定的属性文件（*.properties）到 Spring 的 Environment 中.

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Repeatable(PropertySources.class)
public @interface PropertySource {
String name() default "";

String[] value();

boolean ignoreResourceNotFound() default false;

String encoding() default "";

Class<? extends PropertySourceFactory> factory() default PropertySourceFactory.class;
}
```

- name：属性资源名称，默认为空
- value：String数组，表示外部资源路径
- ignoreResourceNotFound：设置为发现指定路径资源时是否忽略，默认忽略
- edcoding：指定编码方式，默认为空
- PropertySourceFactory：指定属性资源工厂类

```java
@Configuration
@PropertySource(value = {"classpath:/com/${my.placeholder:default/path}/app.properties"})
public class AppConfig {

@Autowired
Environment env;

@Bean
public TestBean testBean() {
TestBean testBean = new TestBean();
testBean.setName(env.getProperty("testbean.name"));
return testBean;
}
}
```

或者与@Value配合使用

```java
package com.huang.pims.demo.props;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.PropertySource;
import org.springframework.stereotype.Component;

@Component
@PropertySource(value = {"test/props/test.properties"})
public class ReadByPropertySourceAndValue {

    @Value("${test.name}")
    private String name;

    @Value("${test.sex}")
    private int sex;

    @Value("${test.type}")
    private String type;
}
```

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

