---
title: feign原理
category: spring
date: 2022-06-25 16:37:58
updated: 2022-06-25 16:37:58
enname: feign
categories: spring
tags:
	- 面试
	- feign
keywords: feign
permalink:
thumbnail:
---

# 简介

作为HTTP的客户端替代RestTemplate，支持注解的方式<!--more-->

Feign组件中引入了Ribbon和Hystrix组件，这使得Feign也能够为我们提供负载均衡、熔断、降级的功能；

</br></br>

# @EnableFeignClients

feign客户端启用注解，目的是加载feign配置到IOC的bean注册表、加载被@FeignClient注解标注的feign客户端bean到bean注册表。

- 注册feign默认配置：将EnableFeignClients指定的默认配置defaultConfiguration注册到注册表
- 注册feign客户端：根据@FeignClient注解中的可配置参数注册bean。注意这里的bean只是BeanDefinition（简单意义上的bean），包括url、name、fallback、configuration等内容。



EnableFeignClients详情如下：其中最重要的是FeignClientsRegistrar

```java
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.TYPE})
@Documented
@Import({FeignClientsRegistrar.class})
public @interface EnableFeignClients {...}
```



FeignClientsRegistrar详情如下：其中最重要的是实现了spring的ImportBeanDefinitionRegistrar接口，目的是自定义的注入feign客户端bean到容器

```java
class FeignClientsRegistrar implements ImportBeanDefinitionRegistrar, ResourceLoaderAware, EnvironmentAware {
  ...
  
  //实现ImportBeanDefinitionRegistrar的接口
  public void registerBeanDefinitions(AnnotationMetadata metadata, BeanDefinitionRegistry registry) {
        this.registerDefaultConfiguration(metadata, registry);
        this.registerFeignClients(metadata, registry);
    }
  
  ...
}
```

</br></br>



# FeignClientFactoryBean

上面提到注册feignclient客户端实际上是注册了BeanDefinition到容器，这个BeanDefinition是通过FeignClientFactoryBean来构建的bean。其主要有以下作用：

- 实现了FactoryBean接口，为bean提供了装饰器模式，使用时是调用的getObject获取
- getObject方法返回的是feignclient的代理对象
- feignclient代理对象由Builder构造器实现构造
- 默认的FeignClientConfiguration也是在构建代理对象的过程中加载到feignclient的



```java
class FeignClientFactoryBean implements FactoryBean<Object>, InitializingBean, ApplicationContextAware {
...
  
    protected Builder feign(FeignContext context) {
        FeignLoggerFactory loggerFactory = (FeignLoggerFactory)this.get(context, FeignLoggerFactory.class);
        Logger logger = loggerFactory.create(this.type);
  
  			//feign构造器获取
        Builder builder = ((Builder)this.get(context, Builder.class)).logger(logger).encoder((Encoder)this.get(context, Encoder.class)).decoder((Decoder)this.get(context, Decoder.class)).contract((Contract)this.get(context, Contract.class));
  
  			//feignclient配置加载
        this.configureFeign(context, builder);
        return builder;
    }
  
  //实现FactoryBean结构，为feign增加装饰器模式
    public Object getObject() throws Exception {
        return this.getTarget();
    }

    <T> T getTarget() {
        FeignContext context = (FeignContext)this.applicationContext.getBean(FeignContext.class);
      
      //获取feign的构造器
        Builder builder = this.feign(context);
        if (!StringUtils.hasText(this.url)) {
            if (!this.name.startsWith("http")) {
                this.url = "http://" + this.name;
            } else {
                this.url = this.name;
            }
          
          //如果没有ur则走负载均衡
            this.url = this.url + this.cleanPath();
          //生成代理对象
          //1.生成请求客户端，内部用到了ribbon的负载均衡
            return this.loadBalance(builder, context, new HardCodedTarget(this.type, this.name, this.url));
        } else {
            if (StringUtils.hasText(this.url) && !this.url.startsWith("http")) {
                this.url = "http://" + this.url;
            }

            String url = this.url + this.cleanPath();
          //生成请求客户端，内部用到了ribbon的负载均衡
            Client client = (Client)this.getOptional(context, Client.class);
            if (client != null) {
                if (client instanceof LoadBalancerFeignClient) {
                    client = ((LoadBalancerFeignClient)client).getDelegate();
                }

                builder.client(client);
            }
          
          //如果有url则走直接链接
            Targeter targeter = (Targeter)this.get(context, Targeter.class);
          //生成代理对象
            return targeter.target(this, builder, context, new HardCodedTarget(this.type, this.name, url));
        }
    }
  
...
}
```

</br></br>

# feignclient代理对象的生成

借助构造器Builder中的build方法实现代理对象生成

- 入参invocationHandlerFactory可以通过feign配置扩展实现自定义代理实现（可以对请求做一下包装等）

```java
public static class Builder {
  ...
  
public Feign build() {
    Factory synchronousMethodHandlerFactory = new Factory(this.client, this.retryer, this.requestInterceptors, this.logger, this.logLevel, this.decode404, this.closeAfterDecode, this.propagationPolicy);
    ParseHandlersByName handlersByName = new ParseHandlersByName(this.contract, this.options, this.encoder, this.decoder, this.queryMapEncoder, this.errorDecoder, synchronousMethodHandlerFactory);
  //代理生成
    return new ReflectiveFeign(handlersByName, this.invocationHandlerFactory, this.queryMapEncoder);
}
  
  ...
}
```



```java
public class ReflectiveFeign extends Feign {
  ...
public <T> T newInstance(Target<T> target) {
    // 1. Contract 将 target.type 接口类上的方法和注解解析成 MethodMetadata，
    //    并转换成内部的MethodHandler处理方式
    Map<String, MethodHandler> nameToHandler = targetToHandlersByName.apply(target);
    Map<Method, MethodHandler> methodToHandler = new LinkedHashMap<Method, MethodHandler>();
    List<DefaultMethodHandler> defaultMethodHandlers = new LinkedList<DefaultMethodHandler>();

    for (Method method : target.type().getMethods()) {
        if (method.getDeclaringClass() == Object.class) {
            continue;
        } else if (Util.isDefault(method)) {
            DefaultMethodHandler handler = new DefaultMethodHandler(method);
            defaultMethodHandlers.add(handler);
            methodToHandler.put(method, handler);
        } else {
            methodToHandler.put(method, nameToHandler.get(Feign.configKey(target.type(), method)));
        }
    }

    // 2. 生成 target.type 的 jdk 动态代理对象
    InvocationHandler handler = factory.create(target, methodToHandler);
    T proxy = (T) Proxy.newProxyInstance(target.type().getClassLoader(),
                                         new Class<?>[]{target.type()}, handler);

    for (DefaultMethodHandler defaultMethodHandler : defaultMethodHandlers) {
        defaultMethodHandler.bindTo(proxy);
    }
    return proxy;
}

  ...
}
```

</br></br>

# client与负载均衡

构造器Builder中需要指定client，feign有默认的，当然我们也可以自己实现扩展。

- client的作用：负责客户端请求、负责是否走负载均衡、请求的底层工具是什么（如HttpURLConnection/okhttp/Apache httpclient）
- feign默认的请求工具是HttpURLConnection，我们可以自定义
- feign负载均衡依赖的是ribbon



feign中默认的负载均衡实现是LoadBalancerFeignClient，我们也可以自己实现client

```java
public class LoadBalancerFeignClient implements Client {
  ...
  
  public Response execute(Request request, Options options) throws IOException {
        try {
            URI asUri = URI.create(request.url());
            String clientName = asUri.getHost();
            URI uriWithoutHost = cleanUrl(request.url(), clientName);
          //获取ribbon请求
            RibbonRequest ribbonRequest = new RibbonRequest(this.delegate, request, uriWithoutHost);
            IClientConfig requestConfig = this.getClientConfig(options, clientName);
          //执行客户端负载均衡
            return ((RibbonResponse)this.lbClient(clientName).executeWithLoadBalancer(ribbonRequest, requestConfig)).toResponse();
        } catch (ClientException var8) {
            IOException io = this.findIOException(var8);
            if (io != null) {
                throw io;
            } else {
                throw new RuntimeException(var8);
            }
        }
    }
    
  ...
}
```



</br></br>

# 执行流程

### 第一步：代理对象的生成

- 添加EnableFeignClients注解，将feign配置类以及feign客户端注入到容器
- feign客户端增加了装饰器模式和构造器模式，效果是可以自定义httpclient、编码器、解码器等
- 通过Spring IOC 容器实例，装配代理实例，然后进行远程调用



### 第二步：代理方法执行

- 执行 InvokeHandler 的invoke(…)方法
- InvocationHandle，内部保持了一个远程调用方法实例和方法处理器的一个Key-Value键值对Map映射dispatch。调用的时候从dispatch中找到MethodHandler 方法处理器
- MethodHandler方法处理器调用feign对象的client进行远程方法调用







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


