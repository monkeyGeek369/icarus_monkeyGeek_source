---
title: activiti事件监听
category: activiti
date: 2020-06-30 19:51:08
updated: 2020-06-30 19:51:08
enname: activitilisten
categories: activiti
tags:
	- activiti
keywords: activiti,事件监听
permalink:
thumbnail:
---

工作流程事件监听可用于任务提醒、超时提醒等的模块的设计。以下是相关事件的介绍<!--more-->

</br>

# 事件监听类型

| ENGINE_CREATED            | 监听器监听的流程引擎已经创建完毕，并准备好接受API调用。      |
| ------------------------- | ------------------------------------------------------------ |
| ENGINE_CLOSED             | 监听器监听的流程引擎已经关闭，不再接受API调用。              |
| ENTITY_CREATED            | 创建了一个新实体。实体包含在事件中。                         |
| ENTITY_INITIALIZED        | 创建了一个新实体，初始化也完成了。如果这个实体的创建会包含子实体的创建，这个事件会在子实体都创建/初始化完成后被触发，这是与ENTITY_CREATED的区别。 |
| ENTITY_UPDATED            | 更新了已存在的实体。实体包含在事件中。                       |
| ENTITY_DELETED            | 删除了已存在的实体。实体包含在事件中。                       |
| ENTITY_SUSPENDED          | 暂停了已存在的实体。实体包含在事件中。会被ProcessDefinitions, ProcessInstances 和 Tasks抛出。 |
| ENTITY_ACTIVATED          | 激活了已存在的实体，实体包含在事件中。会被ProcessDefinitions, ProcessInstances 和 Tasks抛出。 |
| JOB_EXECUTION_SUCCESS     | 作业执行成功。job包含在事件中。                              |
| JOB_EXECUTION_FAILURE     | 作业执行失败。作业和异常信息包含在事件中。                   |
| JOB_RETRIES_DECREMENTED   | 因为作业执行失败，导致重试次数减少。作业包含在事件中。       |
| TIMER_FIRED               | 触发了定时器。job包含在事件中。                              |
| JOB_CANCELED              | 取消了一个作业。事件包含取消的作业。作业可以通过API调用取消，   任务完成后对应的边界定时器也会取消，在新流程定义发布时也会取消。 |
| ACTIVITY_STARTED          | 一个节点开始执行                                             |
| ACTIVITY_COMPLETED        | 一个节点成功结束                                             |
| ACTIVITY_SIGNALED         | 一个节点收到了一个信号                                       |
| ACTIVITY_MESSAGE_RECEIVED | 一个节点收到了一个消息。在节点收到消息之前触发。收到后，会触发ACTIVITY_SIGNAL或ACTIVITY_STARTED，这会根据节点的类型（边界事件，事件子流程开始事件） |
| ACTIVITY_ERROR_RECEIVED   | 一个节点收到了一个错误事件。在节点实际处理错误之前触发。   事件的activityId对应着处理错误的节点。 这个事件后续会是ACTIVITY_SIGNALLED或ACTIVITY_COMPLETE， 如果错误发送成功的话。 |
| UNCAUGHT_BPMN_ERROR       | 抛出了未捕获的BPMN错误。流程没有提供针对这个错误的处理器。   事件的activityId为空。 |
| ACTIVITY_COMPENSATE       | 一个节点将要被补偿。事件包含了将要执行补偿的节点id。         |
| VARIABLE_CREATED          | 创建了一个变量。事件包含变量名，变量值和对应的分支或任务（如果存在）。 |
| VARIABLE_UPDATED          | 更新了一个变量。事件包含变量名，变量值和对应的分支或任务（如果存在）。 |
| VARIABLE_DELETED          | 删除了一个变量。事件包含变量名，变量值和对应的分支或任务（如果存在）。 |
| TASK_ASSIGNED             | 任务被分配给了一个人员。事件包含任务。                       |
| TASK_CREATED              | 创建了新任务。它位于ENTITY_CREATE事件之后。当任务是由流程创建时，     这个事件会在TaskListener执行之前被执行。 |
| TASK_COMPLETED            | 任务被完成了。它会在ENTITY_DELETE事件之前触发。当任务是流程一部分时，事件会在流程继续运行之前，   后续事件将是ACTIVITY_COMPLETE，对应着完成任务的节点。 |
| TASK_TIMEOUT              | 任务已超时，在TIMER_FIRED事件之后，会触发用户任务的超时事件，     当这个任务分配了一个定时器的时候。 |
| PROCESS_COMPLETED         | 流程已结束。在最后一个节点的ACTIVITY_COMPLETED事件之后触发。 当流程到达的状态，没有任何后续连线时， 流程就会结束。 |
| MEMBERSHIP_CREATED        | 用户被添加到一个组里。事件包含了用户和组的id。               |
| MEMBERSHIP_DELETED        | 用户被从一个组中删除。事件包含了用户和组的id。               |
| MEMBERSHIPS_DELETED       | 所有成员被从一个组中删除。在成员删除之前触发这个事件，所以他们都是可以访问的。   因为性能方面的考虑，不会为每个成员触发单独的MEMBERSHIP_DELETED事件。 |

</br>

# 示例

创建监听器

```java
package com.jntech.config;

import org.activiti.engine.delegate.event.ActivitiEntityEvent;
import org.activiti.engine.delegate.event.ActivitiEvent;
import org.activiti.engine.delegate.event.ActivitiEventListener;
import org.activiti.engine.impl.persistence.entity.TaskEntity;
import org.springframework.stereotype.Component;

/**
* packageName：com.jntech.config
* class：EntityCreatedListener
* describe：activiti流程对象创建监听器（每一个流程节点创建时均会出发及一个流程初始化是如果其有六个流程节点则会触发六次）
* author：mahao
* time：19:31-2018/12/16
* modify by:请输入修改人
* modify time：请输入修改时间
* version：V1.0
*/
@Component
public class EntityCreatedListener implements ActivitiEventListener {
public void onEvent(ActivitiEvent event){
Object entity = ((ActivitiEntityEvent)event).getEntity();
if(entity instanceof TaskEntity){
TaskEntity taskEntity = (TaskEntity)entity;
//自身业务代码
}
}
public boolean isFailOnException(){
return true;
}
}
```



监听器注册（修改springboot中的activiti配置文件）

```java
@Autowired
private EntityCreatedListener entityCreatedListener;
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
typedListeners.put("ENTITY_CREATED", Collections.singletonList(entityCreatedListener));
/*typedListeners.put("TASK_CREATED", Collections.singletonList(taskCreatedListener));
typedListeners.put("TASK_COMPLETED", Collections.singletonList(taskCompletedListener));*/
configuration.setTypedEventListeners(typedListeners);

return configuration;
}
```



其它监听器

```java
// 关于监听器的注册看上面配置类中typedListeners部分已有
@Component
public class TaskCompletedListener implements ActivitiEventListener {
public void onEvent(ActivitiEvent event){
TaskEntity taskEntity = (TaskEntity)((ActivitiEntityEvent)event).getEntity();
taskEntity.setVariable("fromTaskIdVarKey", taskEntity.getId());
}

public boolean isFailOnException(){
return true;
}
}
```

```java
@Component
public class TaskCreatedListener implements ActivitiEventListener {
public void onEvent(ActivitiEvent event){
TaskEntity taskEntity = (TaskEntity)((ActivitiEntityEvent)event).getEntity();
String fromTaskId = (String)taskEntity.getVariable(WfVarKeyConstants.fromTaskId);
if(StringUtils.isEmpty(fromTaskId)) return;
xxxTaskInfo info = new xxxTaskInfo();
info.setId(taskEntity.getId());
info.setFromId(fromTaskId);
//此处进行任务关系持久化，自行实现
xxxTaskInfoRepository.save(info);
}
public boolean isFailOnException(){
return true;
}
}
```



</br>

参考资料：

[1]https://blog.csdn.net/zhangdaiscott/article/details/80944389



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


