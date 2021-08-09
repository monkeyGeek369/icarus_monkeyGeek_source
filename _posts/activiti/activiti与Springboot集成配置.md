---
title: activiti与Springboot集成配置
category: activiti
date: 2020-06-30 20:07:47
updated: 2020-06-30 20:07:47
enname: activitispring
categories: activiti
tags:
	- activiti
	- 面试
keywords: activiti,Springboot配置activiti
permalink:
thumbnail:
---

# 第一步：添加项目依赖

<!--more-->

```xml
<dependency>
<groupId>org.activiti</groupId>
<artifactId>activiti-spring-boot-starter-basic</artifactId>
<version>6.0.0</version>
</dependency>
```

</br>



# 第二步：配置activiti配置文件（springboot的配置文件）

```java
package com.jntech.config;

import com.jntech.common.utils.CustomLog;
import org.activiti.engine.delegate.event.ActivitiEventListener;
import org.activiti.spring.SpringProcessEngineConfiguration;
import org.activiti.spring.boot.AbstractProcessEngineAutoConfiguration;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.boot.autoconfigure.jdbc.DataSourceBuilder;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.core.io.Resource;
import org.springframework.core.io.support.ResourcePatternResolver;
import org.springframework.jdbc.datasource.DataSourceTransactionManager;
import org.springframework.transaction.PlatformTransactionManager;
import org.springframework.util.CollectionUtils;

import javax.sql.DataSource;
import java.io.IOException;
import java.util.*;


@Configuration
//@MapperScan(basePackages = "com.jntech.dao.mysql", sqlSessionTemplateRef = "mysqlSqlSessionTemplate")
//@PropertySource("classpath:application.properties")
public class DataSourceActivitiConfig extends AbstractProcessEngineAutoConfiguration {
@Autowired
private ResourcePatternResolver resourceLoader;

@Bean(name = "activitiDataSource")
@ConfigurationProperties(prefix = "spring.datasource.local")
//@Primary
public DataSource testDataSource() {
return DataSourceBuilder.create().build();
}

@Bean(name = "activitiSqlSessionFactory")
// @Primary
public SpringProcessEngineConfiguration springProcessEngineConfiguration(@Qualifier("activitiDataSource") DataSource dataSource,@Qualifier("activitiTransactionManager") PlatformTransactionManager transactionManager) throws IOException {
SpringProcessEngineConfiguration configuration = new SpringProcessEngineConfiguration();
configuration.setDataSource(dataSource);//配置数据源
configuration.setTransactionManager(transactionManager);//配置transaction
configuration.setDatabaseSchemaUpdate("true");//每次项目启动是否更新数据库表(第一次运行项目需要设为true，activiti将在数据库建表)
configuration.setAsyncExecutorActivate(true);//是否激活异步执行器
configuration.setHistory("audit");//流程历史记录登录
configuration.setCreateDiagramOnDeploy(true);//部署流程定义时是否生成图片（新流程部署时或数据库初始化时）
configuration.setActivityFontName("\\u5b8b\\u4f53");//字体 下面内容为转成unicode的'宋体'
configuration.setLabelFontName("\\u5b8b\\u4f53");//字体 下面内容为转成unicode的'宋体'
//设置指定的流程图文件资源
List<Resource> procDefResources = discoverProcessDefinitionResources("classpath*:/processes/", Arrays.asList("**.bpmn20.xml", "**.bpmn"),true);
configuration.setDeploymentResources(procDefResources.toArray(new Resource[procDefResources.size()]));
//设置流程监听器
Map<String, List<ActivitiEventListener>> typedListeners = new HashMap<>();
/*typedListeners.put("ENTITY_CREATED", Collections.singletonList(entityCreatedListener));
typedListeners.put("TASK_CREATED", Collections.singletonList(taskCreatedListener));
typedListeners.put("TASK_COMPLETED", Collections.singletonList(taskCompletedListener));*/
configuration.setTypedEventListeners(typedListeners);

return configuration;
}

@Bean(name = "activitiTransactionManager")
//@Primary
public DataSourceTransactionManager testTransactionManager(@Qualifier("activitiDataSource") DataSource dataSource) {
return new DataSourceTransactionManager(dataSource);
}

/**
* 获取指定的流程图文件资源
* @param prefix 路径前缀
* @param suffixes 文件后缀
* @param checkPDs 如果checkProcessDefinitions为true，则发布新版流程定义，后续可能根据流程定义文件MD5等判断是否真正变化而进行发布
* @return
* @throws IOException
*/
private List<Resource> discoverProcessDefinitionResources(String prefix, List<String> suffixes, boolean checkPDs) throws IOException {
if (checkPDs) {
List<Resource> result = new ArrayList();
for (String suffix : suffixes) {
String path = prefix + suffix;
Resource[] resources = resourceLoader.getResources(path);
if (resources != null && resources.length > 0) {
CollectionUtils.mergeArrayIntoCollection(resources, result);
}
}
if (result.isEmpty()) {
CustomLog.getInstance().createLogger("No process definitions were found for autodeployment");
}
return result;
}
return new ArrayList<>();
}

}
```

databaseSchemaUpdate配置项可以设置流程引擎启动和关闭时数据库执行的策略。 databaseSchemaUpdate有以下四个值：

- false：false为默认值，设置为该值后，Activiti在启动时，会对比数据库表中保存的版本，如果没有表或者版本不匹配时，将在启动时抛出异常。
- true：设置为该值后，Activiti会对数据库中所有的表进行更新，如果表不存在，则Activiti会自动创建。
- create-drop：Activiti启动时，会执行数据库表的创建操作，在Activiti关闭时，执行数据库表的删除操作。
- drop-create：Activiti启动时，执行数据库表的删除操作在Activiti关闭时，会执行数据库表的创建操作。



Activiti提供了history-level属性对其进行配置。history-level属性有点像log4j的日志输出级别，该属性有以下四个值：

- none：不保存任何的历史数据，因此，在流程执行过程中，这是最高效的。
- activity：级别高于none，保存流程实例与流程行为，其他数据不保存。
- audit：除activity级别会保存的数据外，还会保存全部的流程任务及其属性。audit为history的默认值。
- full：保存历史数据的最高级别，除了会保存audit级别的数据外，还会保存其他全部流程相关的细节数据，包括一些流程参数等。

</br>



# 第三步 多数据源下配置activiti数据源

（非多数据源请跳过）只需要借助springboot在配置activiti配置文件的时候制定数据源即可，详情可见“第二步”

</br>



# 第四步 将流程文件放入项目指定路径内

例如放入resources->processes->xxxx.xml

</br>



# 第五步 调用activiti提供的基础工具类实现流程操作

activiti仅仅提供了基础的针对流程对象的操作，我们可以利用基础功能进行封装，将流程创建，流程终止等功能完整自行封装，以提供对外能力。



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


