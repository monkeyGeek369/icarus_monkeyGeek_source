---
title: JDK动态代理的底层实现
category: java基础
date: 2021-08-09 21:38:15
updated: 2021-08-09 21:38:15
enname: jdk-invoke
categories: java基础
tags:
	- 面试
keywords: java、动态代理
permalink:
thumbnail:
---

JavaEE的开发中，许多框架用到了动态代理机制，例如Spring的AOP编程。Java中的动态代理一般就两种：1. JDK自带 ； 2. cglib库，本文主要介绍第一种。<!--more-->



</br>

# 什么是代理

真实对象的访问控制交由代理类实现，代理类成为外部对象与委托类的中间桥梁。

通过代理类这中间一层，能有效控制对委托类对象的直接访问，也可以很好地隐藏和保护委托类对象，同时也为实施不同控制策略预留了空间，从而在设计上获得了更大的灵活性。

基于此，代理类可以做一些不影响真实对象功能的附加功能，例如内容校验、日志记录等。



</br>

# 静态代理

代理类的生成必须人为手动编写，无法自动生成，在程序运行前代理类的.class文件就已经存在。

例如：UserServiceImpl与StaticProxy都实现了接口IUserService

```java
IUserService userService = new UserServiceImpl();
        StaticProxy proxy = new StaticProxy(userService);
        proxy.sayHello("andy");
        proxy.sayBye("andy");
```



</br>

# 动态代理-原理简介

动态代理是动态的生成代理类，已JDK动态代理为例，实现代理需要两个关键对象：代理类、调用处理器InvocationHandler。大致流程如下：

- 编写InvocationHandler调用处理器
- 被代理类实现接口
- 生成代理类
- 实现代理调用

示例代码如下：

```java
public class DynamicProxyHandler implements InvocationHandler {

    private IUserService target;

    public DynamicProxyHandler(IUserService target) {
        this.target = target;
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        Object result = null;
        System.out.println("++++++before " + method.getName() + "++++++");
        result = method.invoke(target, args);
        System.out.println("++++++after " + method.getName() + "++++++");
        return result;
    }

    public Object getProxy(){
        /*
         * Proxy.newProxyInstance(ClassLoader loader, Class<?>[] interfaces, InvocationHandler h)
         * 方法的第一个参数的作用就是 获取当前类的类加载器,作用是用来生成类的
         * 第二个参数是获取真实对象的所有接口    获取所有接口的目的是用来生成代理的,因为代理要实现所有的接口
         * 第三个参数是 调用处理器  这里传入调用处理器，是因为生成代理实例需要 调用处理器    为什么需要调用处理器，因为生成的代理不能直接调用真实对象的方法,
         * 而是通过调用处理器来调用真实对象的方法，具体就是通过上面定义的DynamicProxyHandler重写父类InvocationHandler的invoke方法
         */
        return  Proxy.newProxyInstance(this.getClass().getClassLoader(), target.getClass().getInterfaces(), this );
    }

}
```

```java
public static void testDynamicProxy() {
   DynamicProxyHandler handler = new DynamicProxyHandler(new UserServiceImpl());
   IUserService userService = (IUserService) handler.getProxy();
   userService.sayHello("jeck");
   userService.sayBye("jeck");

}
```

</br>

# 动态代理-底层代码分析

动态代理的核心是代理类如何生成？大体上如下流程：

- 生成代理类字节码
- 类加载器加载字节码
- 生成代理类class
- 实力化代理类

```java
public static Object newProxyInstance(ClassLoader loader,
                                          Class<?>[] interfaces,
                                          InvocationHandler h) throws IllegalArgumentException {
    Objects.requireNonNull(h);

    final Class<?>[] intfs = interfaces.clone();
   //生成代理Class对象
    Class<?> cl = getProxyClass0(loader, intfs);

    try {
       //代理对象的构造方法
        final Constructor<?> cons = cl.getConstructor(constructorParams);
        final InvocationHandler ih = h;
        if (!Modifier.isPublic(cl.getModifiers())) {
            AccessController.doPrivileged(new PrivilegedAction<Void>() {
                public Void run() {
                    cons.setAccessible(true);
                    return null;
                }
            });
        }
        //实例化代理对象
        return cons.newInstance(new Object[]{h});
    } catch (IllegalAccessException|InstantiationException e) {

    }
}
```

getProxyClass0用于生成class对象，其首先去缓存WeakCache<ClassLoader, Class<?>[], Class<?>>取值，如果取不到则生成。核心代码如下：

```java
public Class<?> apply(ClassLoader loader, Class<?>[] interfaces) {
       Map<Class<?>, Boolean> interfaceSet = new IdentityHashMap<>(interfaces.length);
        for (Class<?> intf : interfaces) {
            Class<?> interfaceClass = null;
            try {
                interfaceClass = Class.forName(intf.getName(), false, loader);
            } catch (ClassNotFoundException e) {
            }
            if (interfaceClass != intf) {
                throw new IllegalArgumentException(
                    intf + " is not visible from class loader");
            }
            /*
             * Verify that the Class object actually represents an
             * interface.
             */
            if (!interfaceClass.isInterface()) {
                throw new IllegalArgumentException(
                    interfaceClass.getName() + " is not an interface");
            }
            /*
             * Verify that this interface is not a duplicate.
             */
            if (interfaceSet.put(interfaceClass, Boolean.TRUE) != null) {
                throw new IllegalArgumentException(
                    "repeated interface: " + interfaceClass.getName());
            }
        }

        String proxyPkg = null;     // package to define proxy class in
        int accessFlags = Modifier.PUBLIC | Modifier.FINAL;

        /*
         * Record the package of a non-public proxy interface so that the
         * proxy class will be defined in the same package.  Verify that
         * all non-public proxy interfaces are in the same package.
         */
        for (Class<?> intf : interfaces) {
            int flags = intf.getModifiers();
            if (!Modifier.isPublic(flags)) {
                accessFlags = Modifier.FINAL;
                String name = intf.getName();
                int n = name.lastIndexOf('.');
                String pkg = ((n == -1) ? "" : name.substring(0, n + 1));
                if (proxyPkg == null) {
                    proxyPkg = pkg;
                } else if (!pkg.equals(proxyPkg)) {
                    throw new IllegalArgumentException(
                        "non-public interfaces from different packages");
                }
            }
        }

        if (proxyPkg == null) {
            // if no non-public proxy interfaces, use com.sun.proxy package
            proxyPkg = ReflectUtil.PROXY_PACKAGE + ".";
        }

        /*
         * Choose a name for the proxy class to generate.
         */
        long num = nextUniqueNumber.getAndIncrement();
        String proxyName = proxyPkg + proxyClassNamePrefix + num;

        /*
         * Generate the specified proxy class.
         */
         //生成代理对象字节码
        byte[] proxyClassFile = ProxyGenerator.generateProxyClass(
            proxyName, interfaces, accessFlags);
        try {
            //返回代理Class对象
            return defineClass0(loader, proxyName,
                                proxyClassFile, 0, proxyClassFile.length);
        } catch (ClassFormatError e) {
            /*
             * A ClassFormatError here means that (barring bugs in the
             * proxy class generation code) there was some other
             * invalid aspect of the arguments supplied to the proxy
             * class creation (such as virtual machine limitations
             * exceeded).
             */
            throw new IllegalArgumentException(e.toString());
        }
    }
}
```

InvocationHandler的invoke方法如何被代理类使用？我们把代理类解析如下：

```java
import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;
import java.lang.reflect.UndeclaredThrowableException;

public final class $Proxy11 extends Proxy implements IUserService {
    private static Method m1;
    private static Method m3;
    private static Method m2;
    private static Method m4;
    private static Method m0;

    //构造方法中传入处理对象
    public $Proxy11(InvocationHandler var1) throws  {
        super(var1);
    }

    public final boolean equals(Object var1) throws  {
        try {
            return ((Boolean)super.h.invoke(this, m1, new Object[]{var1})).booleanValue();
        } catch (RuntimeException | Error var3) {
            throw var3;
        } catch (Throwable var4) {
            throw new UndeclaredThrowableException(var4);
        }
    }

    //实现了相同的接口，通过处理对象的invoke方法调用
    public final void sayHello(String var1) throws  {
        try {
            super.h.invoke(this, m3, new Object[]{var1});
        } catch (RuntimeException | Error var3) {
            throw var3;
        } catch (Throwable var4) {
            throw new UndeclaredThrowableException(var4);
        }
    }

    public final String toString() throws  {
        try {
            return (String)super.h.invoke(this, m2, (Object[])null);
        } catch (RuntimeException | Error var2) {
            throw var2;
        } catch (Throwable var3) {
            throw new UndeclaredThrowableException(var3);
        }
    }

    public final void sayBye(String var1) throws  {
        try {
            super.h.invoke(this, m4, new Object[]{var1});
        } catch (RuntimeException | Error var3) {
            throw var3;
        } catch (Throwable var4) {
            throw new UndeclaredThrowableException(var4);
        }
    }

    public final int hashCode() throws  {
        try {
            return ((Integer)super.h.invoke(this, m0, (Object[])null)).intValue();
        } catch (RuntimeException | Error var2) {
            throw var2;
        } catch (Throwable var3) {
            throw new UndeclaredThrowableException(var3);
        }
    }

    static {
        try {
            m1 = Class.forName("java.lang.Object").getMethod("equals", new Class[]{Class.forName("java.lang.Object")});
            m3 = Class.forName("com.github.sources.proxy.service.IUserService").getMethod("sayHello", new Class[]{Class.forName("java.lang.String")});
            m2 = Class.forName("java.lang.Object").getMethod("toString", new Class[0]);
            m4 = Class.forName("com.github.sources.proxy.service.IUserService").getMethod("sayBye", new Class[]{Class.forName("java.lang.String")});
            m0 = Class.forName("java.lang.Object").getMethod("hashCode", new Class[0]);
        } catch (NoSuchMethodException var2) {
            throw new NoSuchMethodError(var2.getMessage());
        } catch (ClassNotFoundException var3) {
            throw new NoClassDefFoundError(var3.getMessage());
        }
    }
}
```

由以上可以得知，在实力化代理类时传递invocationHandler对象cons.newInstance(new Object[]{h});，在代理类构造方法中接收对象。



</br>

# 总结

- JDK代理需要依赖代理类、调用处理器
- 代理类的生成是核心：生成字节码、生成class对象、实力化代理类



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


