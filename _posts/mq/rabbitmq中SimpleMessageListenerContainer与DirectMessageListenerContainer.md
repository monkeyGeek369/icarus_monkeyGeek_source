---
title: rabbitmq中SimpleMessageListenerContainer与DirectMessageListenerContainer
category: mq
date: 2021-08-04 16:14:14
updated: 2021-08-04 16:14:14
enname: rab-sim-dir
categories: mq
tags:
	- mq
	- rabbitmq
keywords: SimpleMessageListenerContainer、DirectMessageListenerContainer、rabbitmq
permalink:
thumbnail:
---

SimpleMessageListenerContainer与DirectMessageListenerContainer详细介绍<!--more-->

</br>

# SimpleMessageListenerContainer

![](../../../../image/RabbitMQ中connection与channel.png)



**<u>消费者消费消息-单线程</u>**

- 通常一个应用创建一个connection连接（通过TCP实现）当然可以创建多个
- 一个connection可被多个channel复用
- 一个channel是单线程的
- channel单线程串行消费消息



**<u>消费者消费消息-多线程</u>**

- 通常一个应用创建一个connection连接（通过TCP实现）当然可以创建多个
- 分布式服务创建各自对应的connection
- 一个connection可被多个channel复用
- 设置参数concurrency来控制线程数，相当于为每一个消费者创建了多个channel
- 消费者对应的多个channel可以被不同的微服务共享，例如针对消费者consumer-a的Channel-a/Channel-b可以被微服务Application-b/Application-b1公用，只是线程不同
- channel单线程串行消费消息



</br>

**<u>基本参数</u>**

```java
public class SimpleMessageListenerContainer extends AbstractMessageListenerContainer {
    private static final long DEFAULT_START_CONSUMER_MIN_INTERVAL = 10000L;
    private static final long DEFAULT_STOP_CONSUMER_MIN_INTERVAL = 60000L;
    private static final long DEFAULT_CONSUMER_START_TIMEOUT = 60000L;
    private static final int DEFAULT_CONSECUTIVE_ACTIVE_TRIGGER = 10;
    private static final int DEFAULT_CONSECUTIVE_IDLE_TRIGGER = 10;
    public static final long DEFAULT_RECEIVE_TIMEOUT = 1000L;
    private final AtomicLong lastNoMessageAlert = new AtomicLong();
    private final AtomicReference<Thread> containerStoppingForAbort = new AtomicReference();
    private final BlockingQueue<ListenerContainerConsumerFailedEvent> abortEvents = new LinkedBlockingQueue();
    private volatile long startConsumerMinInterval = 10000L;
    private volatile long stopConsumerMinInterval = 60000L;
    private volatile int consecutiveActiveTrigger = 10;
    private volatile int consecutiveIdleTrigger = 10;
    private volatile int txSize = 1;
    private volatile int concurrentConsumers = 1;
    private volatile Integer maxConcurrentConsumers;
    private volatile long lastConsumerStarted;
    private volatile long lastConsumerStopped;
    private long receiveTimeout = 1000L;
    private Set<BlockingQueueConsumer> consumers;
    private final ActiveObjectCounter<BlockingQueueConsumer> cancellationLock = new ActiveObjectCounter();
    private Integer declarationRetries;
    private Long retryDeclarationInterval;
    private TransactionTemplate transactionTemplate;
    private long consumerStartTimeout = 60000L;
```

- concurrentConsumers：设置当前消费者数量，默认一个，初始化时自动设置
- maxConcurrentConsumers：设置最大消费者数量，根据消息数量动态浮动
- consumers：消费者集合，每一个consumer对应一个channel对应一个线程

</br>

**<u>Consumers初始化</u>**

```java
protected int initializeConsumers() {
        int count = 0;
        synchronized(this.consumersMonitor) {
            if (this.consumers == null) {
                this.cancellationLock.reset();
                this.consumers = new HashSet(this.concurrentConsumers);

                for(int i = 0; i < this.concurrentConsumers; ++i) {
                    BlockingQueueConsumer consumer = this.createBlockingQueueConsumer();
                    this.consumers.add(consumer);
                    ++count;
                }
            }

            return count;
        }
    }
```

根据设定的concurrentConsumers来初始化数量

</br>

**<u>BlockingQueueConsumer基本参数</u>**

```java
    private static Log logger = LogFactory.getLog(BlockingQueueConsumer.class);
    private final BlockingQueue<Delivery> queue;
    private volatile ShutdownSignalException shutdown;
    private final String[] queues;
    private final int prefetchCount;
    private final boolean transactional;
    private Channel channel;
    private RabbitResourceHolder resourceHolder;
    private BlockingQueueConsumer.InternalConsumer consumer;
    private final AtomicBoolean cancelled;
    private final AcknowledgeMode acknowledgeMode;
    private final ConnectionFactory connectionFactory;
    private final MessagePropertiesConverter messagePropertiesConverter;
    private final ActiveObjectCounter<BlockingQueueConsumer> activeObjectCounter;
    private final Map<String, Object> consumerArgs;
    private final boolean noLocal;
    private final boolean exclusive;
    private final Set<Long> deliveryTags;
    private final boolean defaultRequeueRejected;
    private final Map<String, String> consumerTags;
    private final Set<String> missingQueues;
    private long retryDeclarationInterval;
    private long failedDeclarationRetryInterval;
    private int declarationRetries;
    private long lastRetryDeclaration;
    private ConsumerTagStrategy tagStrategy;
    private BackOffExecution backOffExecution;
    private long shutdownTimeout;
    private boolean locallyTransacted;
    private ApplicationEventPublisher applicationEventPublisher;
    private volatile long abortStarted;
    private volatile boolean normalCancel;
    volatile Thread thread;
    volatile boolean declaring;
```

- 一个消费者可以监听多个队列
- prefetchCount：用于控制每次从队列抓取消息的数量
- Channel：对应的channel信息
- thread：消费者对应的线程对象

</br>

**<u>线程处理核心代码</u>**

```java
public void start() throws AmqpException {
......
        try {
            if (!this.cancelled()) {
                String[] var2 = this.queues;
                int var3 = var2.length;

                for(int var4 = 0; var4 < var3; ++var4) {
                    String queueName = var2[var4];
                    if (!this.missingQueues.contains(queueName)) {
                      //从队列中消费消息
                        this.consumeFromQueue(queueName);
                    }
                }
            }

        } catch (IOException var9) {
            throw RabbitExceptionTranslator.convertRabbitAccessException(var9);
        }
......
}


    private void consumeFromQueue(String queue) throws IOException {
        String consumerTag = this.channel.basicConsume(queue, this.acknowledgeMode.isAutoAck(), this.tagStrategy != null ? this.tagStrategy.createConsumerTag(queue) : "", this.noLocal, this.exclusive, this.consumerArgs, new BlockingQueueConsumer.ConsumerDecorator(queue, this.consumer, this.applicationEventPublisher));
        if (consumerTag != null) {
            this.consumerTags.put(consumerTag, queue);
            if (logger.isDebugEnabled()) {
                logger.debug("Started on queue '" + queue + "' with tag " + consumerTag + ": " + this);
            }
        } else {
            logger.error("Null consumer tag received for queue " + queue);
        }

    }

    }
```

</br>

**<u>消息消费的底层逻辑</u>**

```java
public String basicConsume(String queue, final boolean autoAck, String consumerTag, boolean noLocal, boolean exclusive, Map<String, Object> arguments, final Consumer callback) throws IOException {
    Method m = (new com.rabbitmq.client.AMQP.Basic.Consume.Builder()).queue(queue).consumerTag(consumerTag).noLocal(noLocal).noAck(autoAck).exclusive(exclusive).arguments(arguments).build();
    BlockingRpcContinuation<String> k = new BlockingRpcContinuation<String>(m) {
        public String transformReply(AMQCommand replyCommand) {
            String actualConsumerTag = ((ConsumeOk)replyCommand.getMethod()).getConsumerTag();
            ChannelN.this._consumers.put(actualConsumerTag, callback);
            ChannelN.this.metricsCollector.basicConsume(ChannelN.this, actualConsumerTag, autoAck);
            ChannelN.this.dispatcher.handleConsumeOk(callback, actualConsumerTag);
            return actualConsumerTag;
        }
    };
    this.rpc(m, k);

    try {
        if (this._rpcTimeout == 0) {
            return (String)k.getReply();
        } else {
            try {
                return (String)k.getReply(this._rpcTimeout);
            } catch (TimeoutException var11) {
                throw this.wrapTimeoutException(m, var11);
            }
        }
    } catch (ShutdownSignalException var12) {
        throw wrap(var12);
    }
}
```

- 利用rpc进行消息消费的远程通信
- 底层利用反射机制生产消费者类和处理方法

</br>

**<u>总结：</u>**

- SimpleMessageListenerContainer是最简单的消息监听容器，使用简单，适合并发要求不高的场景
- 单线程串行执行，如果消费者处理时间过长，很有可能造成消息积压
- 如果代码中发生重复ack，channel将会被关闭并报异常，通道关闭后造成消息积压
- 由于没有使用线程池，多线程切换会影响效率

```java
java.lang.IllegalStateException: Channel closed; cannot ack/nack^@#\tat org.springframework.amqp.rabbit.connection.CachingConnectionFactory$CachedChannelInvocationHandler.invoke(CachingConnectionFactory.java:982)^@#\tat com.sun.proxy.$Proxy248.basicAck(Unknown Source)^@#\tat com.yqn.pac.zuul.dispatch.mq.TaskResultConsumerTest.dispatch(TaskResultConsumerTest.java:27)^@#\tat sun.reflect.GeneratedMethodAccessor130.invoke(Unknown Source)^@#\tat sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)^@#\tat java.lang.reflect.Method.invoke(Method.java:498)^@#\tat org.springframework.messaging.handler.invocation.InvocableHandlerMethod.doInvoke(InvocableHandlerMethod.java:181)^@#\tat org.springframework.messaging.handler.invocation.InvocableHandlerMethod.invoke(InvocableHandlerMethod.java:114)^@#\tat org.springframework.amqp.rabbit.listener.adapter.HandlerAdapter.invoke(HandlerAdapter.java:51)^@#\tat org.springframework.amqp.rabbit.listener.adapter.MessagingMessageListenerAdapter.invokeHandler(MessagingMessageListenerAdapter.java:182)^@#\tat org.springframework.amqp.rabbit.listener.adapter.MessagingMessageListenerAdapter.onMessage(MessagingMessageListenerAdapter.java:120)^@#\tat org.springframework.amqp.rabbit.listener.AbstractMessageListenerContainer.doInvokeListener(AbstractMessageListenerContainer.java:1414)^@#\tat org.springframework.amqp.rabbit.listener.AbstractMessageListenerContainer.actualInvokeListener(AbstractMessageListenerContainer.java:1337)^@#\tat org.springframework.amqp.rabbit.listener.AbstractMessageListenerContainer.invokeListener(AbstractMessageListenerContainer.java:1324)^@#\tat org.springframework.amqp.rabbit.listener.AbstractMessageListenerContainer.executeListener(AbstractMessageListenerContainer.java:1303)^@#\tat org.springframework.amqp.rabbit.listener.SimpleMessageListenerContainer.doReceiveAndExecute(SimpleMessageListenerContainer.java:817)^@#\tat org.springframework.amqp.rabbit.listener.SimpleMessageListenerContainer.receiveAndExecute(SimpleMessageListenerContainer.java:801)^@#\tat org.springframework.amqp.rabbit.listener.SimpleMessageListenerContainer.access$700(SimpleMessageListenerContainer.java:77)^@#\tat org.springframework.amqp.rabbit.listener.SimpleMessageListenerContainer$AsyncMessageProcessingConsumer.run(SimpleMessageListenerContainer.java:1042)^@#\tat java.lang.Thread.run(Thread.java:748)^@#
```



</br>

</br>

</br>

# DirectMessageListenerContainer

![](../../../../image/RabbitMQ中DirectMessageListenerContainer.png)



**<u>共用线程池</u>**

- 被@RabbitListener标记的消费者方法对应独立的RabbitListenerContainer
- 应用配置一个RabbitListenerContainerFactory且所有的queue使用相同消费者方法（@RabbitListener标记）时，每一个queue使用相同的线程池
- 消费者queue依然可以利用consumersPerQueue参数指定消费者并发数量，即多个channel
- channel对应的线程交由线程池管理



**<u>独立线程池</u>**

- 被@RabbitListener标记的消费者方法对应独立的RabbitListenerContainer
- 应用配置多个RabbitListenerContainerFactory，每个消费者指定不同的监听容器工厂可以实现独立线程池使用
- 应用配置一个RabbitListenerContainerFactory，每个队列拥有不同的消费者方法可以实现独立线程池使用



</br>

**<u>基本参数</u>**

```java
    private static final int DEFAULT_MONITOR_INTERVAL = 10000;
    private static final int DEFAULT_ACK_TIMEOUT = 20000;
    protected final List<DirectMessageListenerContainer.SimpleConsumer> consumers = new LinkedList();
    private final List<DirectMessageListenerContainer.SimpleConsumer> consumersToRestart = new LinkedList();
    private final MultiValueMap<String, DirectMessageListenerContainer.SimpleConsumer> consumersByQueue = new LinkedMultiValueMap();
    private final ActiveObjectCounter<DirectMessageListenerContainer.SimpleConsumer> cancellationLock = new ActiveObjectCounter();
    private TaskScheduler taskScheduler;
    private boolean taskSchedulerSet;
    private long monitorInterval = 10000L;
    private int messagesPerAck;
    private long ackTimeout = 20000L;
    private volatile boolean started;
    private volatile boolean aborted;
    private volatile boolean hasStopped;
    private volatile CountDownLatch startedLatch = new CountDownLatch(1);
    private volatile int consumersPerQueue = 1;
    private volatile ScheduledFuture<?> consumerMonitorTask;
    private volatile long lastAlertAt;
    private volatile long lastRestartAttempt;
```

- consumers：容器内的消费者集合，每一个消费者对象都有对应的channel、queue
- consumersByQueue：以队列名称为key的消费者map，一个queue可能对应多个消费者
- taskScheduler：线程池ThreadPoolTaskScheduler，支持容器内所有消费者
- consumersPerQueue：每一个队列设定的消费者数量，即channel数量

</br>

**<u>线程池初始化</u>**

```java

    public void setTaskScheduler(TaskScheduler taskScheduler) {
        this.taskScheduler = taskScheduler;
        this.taskSchedulerSet = true;
    }

protected void doInitialize() throws Exception {
    if (this.taskScheduler == null) {
        ThreadPoolTaskScheduler threadPoolTaskScheduler = new ThreadPoolTaskScheduler();
        threadPoolTaskScheduler.setThreadNamePrefix(this.getBeanName() + "-consumerMonitor-");
        threadPoolTaskScheduler.afterPropertiesSet();
        this.taskScheduler = threadPoolTaskScheduler;
    }

    if (this.messagesPerAck > 0) {
        Assert.state(!this.isChannelTransacted(), "'messagesPerAck' is not allowed with transactions");
    }

}
```

可以自由设定TaskScheduler,如果不设定则默认使用ThreadPoolTaskScheduler



</br>

**<u>线程池使用</u>**

```java
protected void actualStart() throws Exception {
    this.aborted = false;
    this.hasStopped = false;
    if (this.getPrefetchCount() < this.messagesPerAck) {
        this.setPrefetchCount(this.messagesPerAck);
    }

    super.doStart();
    String[] queueNames = this.getQueueNames();
    this.checkMissingQueues(queueNames);
    if (this.getTaskExecutor() == null) {
        this.afterPropertiesSet();
    }

    long idleEventInterval = this.getIdleEventInterval();
    if (this.taskScheduler == null) {
        this.afterPropertiesSet();
    }

    if (idleEventInterval > 0L && this.monitorInterval > idleEventInterval) {
        this.monitorInterval = idleEventInterval / 2L;
    }

    if (this.getFailedDeclarationRetryInterval() < this.monitorInterval) {
        this.monitorInterval = this.getFailedDeclarationRetryInterval();
    }

    this.lastRestartAttempt = System.currentTimeMillis();
    this.consumerMonitorTask = this.taskScheduler.scheduleAtFixedRate(() -> {
        long now = System.currentTimeMillis();
        if (idleEventInterval > 0L && now - this.getLastReceive() > idleEventInterval && now - this.lastAlertAt > idleEventInterval) {
            this.publishIdleContainerEvent(now - this.getLastReceive());
            this.lastAlertAt = now;
        }

        List consumersToCancel;
        synchronized(this.consumersMonitor) {
            consumersToCancel = (List)this.consumers.stream().filter((c) -> {
                boolean open = c.getChannel().isOpen();
                if (open && this.messagesPerAck > 1) {
                    try {
                        c.ackIfNecessary(now);
                    } catch (IOException var6) {
                        this.logger.error("Exception while sending delayed ack", var6);
                    }
                }

                return !open;
            }).collect(Collectors.toList());
        }

        consumersToCancel.forEach((c) -> {
            try {
                RabbitUtils.closeMessageConsumer(c.getChannel(), Collections.singletonList(c.getConsumerTag()), this.isChannelTransacted());
            } catch (Exception var3) {
                if (this.logger.isDebugEnabled()) {
                    this.logger.debug("Error closing consumer " + c, var3);
                }
            }

            this.logger.error("Consumer canceled - channel closed " + c);
            c.cancelConsumer("Consumer " + c + " channel closed");
        });
        if (this.lastRestartAttempt + this.getFailedDeclarationRetryInterval() < now) {
            synchronized(this.consumersMonitor) {
                List<DirectMessageListenerContainer.SimpleConsumer> restartableConsumers = new ArrayList(this.consumersToRestart);
                this.consumersToRestart.clear();
                if (this.started) {
                    if (restartableConsumers.size() > 0) {
                        this.doRedeclareElementsIfNecessary();
                    }

                    Iterator var8 = restartableConsumers.iterator();

                    while(var8.hasNext()) {
                        DirectMessageListenerContainer.SimpleConsumer consumer = (DirectMessageListenerContainer.SimpleConsumer)var8.next();
                        if (this.logger.isDebugEnabled() && restartableConsumers.size() > 0) {
                            this.logger.debug("Attempting to restart consumer " + consumer);
                        }

                        try {
                            this.doConsumeFromQueue(consumer.getQueue());
                        } catch (AmqpIOException | AmqpConnectException var13) {
                            this.logger.error("Cannot connect to server", var13);
                            if (var13.getCause() instanceof AmqpApplicationContextClosedException) {
                                this.logger.error("Application context is closed, terminating");
                                this.taskScheduler.schedule(this::stop, new Date());
                            }
                            break;
                        }
                    }

                    this.lastRestartAttempt = now;
                }
            }
        }

        this.processMonitorTask();
    }, this.monitorInterval);
    if (queueNames.length > 0) {
        this.doRedeclareElementsIfNecessary();
      //根据queue名称来执行线程池线程
        this.getTaskExecutor().execute(() -> {
            synchronized(this.consumersMonitor) {
                if (this.hasStopped) {
                    if (this.logger.isDebugEnabled()) {
                        this.logger.debug("Consumer start aborted - container stopping");
                    }
                } else {
                    BackOffExecution backOffExecution = this.getRecoveryBackOff().start();

                    while(true) {
                        label54:
                        while(true) {
                            if (this.started || !this.isRunning()) {
                                return;
                            }

                            this.cancellationLock.reset();

                            try {
                                String[] var4 = queueNames;
                                int var12 = queueNames.length;
                                int var6 = 0;

                                while(true) {
                                    if (var6 >= var12) {
                                        break label54;
                                    }

                                    String queue = var4[var6];
                                  //消费者消费消息的核心方法
                                    this.consumeFromQueue(queue);
                                    ++var6;
                                }
                            } catch (AmqpIOException | AmqpConnectException var10) {
                                long nextBackOff = backOffExecution.nextBackOff();
                                if (nextBackOff < 0L || var10.getCause() instanceof AmqpApplicationContextClosedException) {
                                    this.aborted = true;
                                    this.shutdown();
                                    this.logger.error("Failed to start container - fatal error or backOffs exhausted", var10);
                                    this.taskScheduler.schedule(this::stop, new Date());
                                    return;
                                }

                                this.logger.error("Error creating consumer; retrying in " + nextBackOff, var10);
                                this.doShutdown();

                                try {
                                    Thread.sleep(nextBackOff);
                                } catch (InterruptedException var9) {
                                    Thread.currentThread().interrupt();
                                }
                            }
                        }

                        this.started = true;
                        this.startedLatch.countDown();
                    }
                }

            }
        });
    } else {
        this.started = true;
        this.startedLatch.countDown();
    }

    if (this.logger.isInfoEnabled()) {
        this.logger.info("Container initialized for queues: " + Arrays.asList(queueNames));
    }

}
```



</br>

**<u>消费者并发</u>**

```java
private void consumeFromQueue(String queue) {
  //根据queue名称来获取所有的消费者
    List<DirectMessageListenerContainer.SimpleConsumer> list = (List)this.consumersByQueue.get(queue);
  
    if (CollectionUtils.isEmpty(list)) {
      //根据consumersPerQueue参数来控制每一个queue的数量
        for(int i = 0; i < this.consumersPerQueue; ++i) {
            this.doConsumeFromQueue(queue);
        }
    }

}
```



</br>

**<u>消费者消费</u>**

```java
private void doConsumeFromQueue(String queue) {
    if (!this.isActive()) {
        if (this.logger.isDebugEnabled()) {
            this.logger.debug("Consume from queue " + queue + " ignore, container stopping");
        }

    } else {
        String routingLookupKey = this.getRoutingLookupKey();
        if (routingLookupKey != null) {
            SimpleResourceHolder.bind(this.getRoutingConnectionFactory(), routingLookupKey);
        }

        Connection connection = null;

        try {
            connection = this.getConnectionFactory().createConnection();
        } catch (Exception var14) {
            this.addConsumerToRestart(new DirectMessageListenerContainer.SimpleConsumer((Connection)null, (Channel)null, queue));
            throw var14 instanceof AmqpConnectException ? (AmqpConnectException)var14 : new AmqpConnectException(var14);
        } finally {
            if (routingLookupKey != null) {
                SimpleResourceHolder.unbind(this.getRoutingConnectionFactory());
            }

        }

        Channel channel = null;
        DirectMessageListenerContainer.SimpleConsumer consumer = null;

        try {
            channel = connection.createChannel(this.isChannelTransacted());
            channel.basicQos(this.getPrefetchCount());
            consumer = new DirectMessageListenerContainer.SimpleConsumer(connection, channel, queue);
            channel.queueDeclarePassive(queue);
          //消费者消费消息的核心代码，借助channel来获取消息并利用反射来实例化消费者对象
            consumer.consumerTag = channel.basicConsume(queue, this.getAcknowledgeMode().isAutoAck(), this.getConsumerTagStrategy() != null ? this.getConsumerTagStrategy().createConsumerTag(queue) : "", this.isNoLocal(), this.isExclusive(), this.getConsumerArguments(), consumer);
        } catch (AmqpApplicationContextClosedException var16) {
            throw new AmqpConnectException(var16);
        } catch (AmqpConnectException | IOException var17) {
            RabbitUtils.closeChannel(channel);
            RabbitUtils.closeConnection(connection);
            if (var17.getCause() instanceof ShutdownSignalException && var17.getCause().getMessage().contains("in exclusive use")) {
                this.getExclusiveConsumerExceptionLogger().log(this.logger, "Exclusive consumer failure", var17.getCause());
                this.publishConsumerFailedEvent("Consumer raised exception, attempting restart", false, var17);
            } else if (var17.getCause() instanceof ShutdownSignalException && RabbitUtils.isPassiveDeclarationChannelClose((ShutdownSignalException)var17.getCause())) {
                this.logger.error("Queue not present, scheduling consumer " + consumer + " for restart", var17);
            } else if (this.logger.isWarnEnabled()) {
                this.logger.warn("basicConsume failed, scheduling consumer " + consumer + " for restart", var17);
            }

            if (consumer == null) {
                this.addConsumerToRestart(new DirectMessageListenerContainer.SimpleConsumer((Connection)null, (Channel)null, queue));
            } else {
                this.addConsumerToRestart(consumer);
                consumer = null;
            }
        }

        synchronized(this.consumersMonitor) {
            if (consumer != null) {
                this.cancellationLock.add(consumer);
                this.consumers.add(consumer);
                this.consumersByQueue.add(queue, consumer);
                if (this.logger.isInfoEnabled()) {
                    this.logger.info(consumer + " started");
                }

                if (this.getApplicationEventPublisher() != null) {
                    this.getApplicationEventPublisher().publishEvent(new AsyncConsumerStartedEvent(this, consumer));
                }
            }

        }
    }
}
```



</br>

**<u>总结</u>**

DirectMessageListenerContainer利用线程池来管理channel线程，但每一个线程的底层消息消费逻辑与SimpleMessageListenerContainer相同，都是借助channel利用反射实现消费者实例化。

SimpleMessageListenerContainer提供了以下特性，但DirectMessageListenerContainer不提供:

- txSize—使用SimpleMessageListenerContainer，您可以将其设置为控制事务中传递的消息数量和/或减少ack的数量，但这可能会导致失败后重复传递的数量增加。(与txSize和SimpleMessageListenerContainer一样，DirectMessageListenerContainer也有mesagesPerAck，可以用来减少ack，但不能用于事务—每个消息都在单独的事务中交付和打包)。
- maxconcurrentconsumer和consumer伸缩间隔/触发器—DirectMessageListenerContainer中没有自动伸缩;但是，它允许您以编程方式更改consumersPerQueue属性，并相应地调整使用者。

然而，与SimpleMessageListenerContainer相比，DirectMessageListenerContainer有以下优点:

- 在运行时添加和删除队列更有效;使用SimpleMessageListenerContainer，整个使用者线程重新启动(所有使用者取消并重新创建);对于DirectMessageListenerContainer，不受影响的使用者不会被取消。
- 避免了RabbitMQ客户机线程和使用者线程之间的上下文切换。
- 线程是跨使用者共享的，而不是为SimpleMessageListenerContainer中的每个使用者都有一个专用线程。但是，请参阅“线程和异步使用者”一节中有关连接工厂配置的重要说明。





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


