---
title: ThreadLocal详解
category: java基础
date: 2021-05-21 20:12:34
updated: 2021-05-21 20:12:34
enname: threadlocal
categories: java基础
tags:
	- java
	- 线程与进程
	- 面试
keywords: java、ThreadLocal
permalink:
thumbnail:
---

# ThreadLocal是什么

ThreadLocal是线程本地副本变量工具类。<!--more-->主要用于将私有线程和该线程存放的副本对象做一个映射，各个线程之间的变量互不干扰，在高并发场景下，可以实现无状态的调用，特别适用于各个线程依赖不同的变量值完成操作的场景。

基本使用如下：

```java
//在自定义类中创建ThreadLocal对象，EngineContext为我们自定义的Value对象类
//java是多线程的，父类初始化后，内部定义的ThreadLocal对象即在本线程内起效
public class ThreadLocalTest{
    private static final ThreadLocal<testContext> CONTEXT_HOLDER = new ThreadLocal<>();

    public void initalize(testContext context) {
        CONTEXT_HOLDER.set(context);
    }

    public void clean() {
        CONTEXT_HOLDER.remove();
    }

    public testContext get() {
        return CONTEXT_HOLDER.get();
    }
  ...
}
```

</br>

# ThreadLocal数据结构

先看线程内部变量，线程中本身就有threadLocals与inheritableThreadLocals对象，两者都是ThreadLocalMap类型，主要目的是存放属于本线程的ThreadLocal值对象。这也是线程间做到数据隔离的关键。

```java
public class Thread implements Runnable {
...

    /* ThreadLocal values pertaining to this thread. This map is maintained
     * by the ThreadLocal class. 
     * 与此线程相关的ThreadLocal值。由ThreadLocal类维护*/
    ThreadLocal.ThreadLocalMap threadLocals = null;

    /*
     * InheritableThreadLocal values pertaining to this thread. This map is
     * maintained by the InheritableThreadLocal class.
     * 与此线程相关的可继承父类的ThreadLocal值。由InheritableThreadLocal类维护
     */
    ThreadLocal.ThreadLocalMap inheritableThreadLocals = null;
...
}
```

以threadLocals为例，那么线程中的threadLocals是如何被赋值和获取的呢？

ThreadLocalMap是ThreadLocal工具类的内部类，ThreadLocalMap中有get、set方法用于操作。

```java
...
public T get() {
  	//获取当前线程
    Thread t = Thread.currentThread();
  	//获取当前线程中的threadLocals
    ThreadLocalMap map = getMap(t);
    if (map != null) {
      	//从threadLocals中取值Entry，注意key为ThreadLocal对象本身
        ThreadLocalMap.Entry e = map.getEntry(this);
        if (e != null) {
            @SuppressWarnings("unchecked")
            T result = (T)e.value;
            return result;
        }
    }
    return setInitialValue();
}

...

  public void set(T value) {
  			//获取当前线程
        Thread t = Thread.currentThread();
  			//获取当前线程中的threadLocals
        ThreadLocalMap map = getMap(t);
        if (map != null)
          	//赋值：注意key为ThreadLocal对象本身
            map.set(this, value);
        else
            createMap(t, value);
    }
...
```

总结：

- 每一个Thread内都存在独立的ThreadLocalMap对象
- ThreadLocalMap对象是key为ThreadLocal的map
- Thread内的ThreadLocalMap被ThreadLocal进行统一管理，实现get/set等功能

</br>

# ThreadLocalMap数据结构

```java
static class ThreadLocalMap {
    static class Entry extends WeakReference<ThreadLocal<?>> {
        /** The value associated with this ThreadLocal. */
        Object value;

        Entry(ThreadLocal<?> k, Object v) {
            super(k);
            value = v;
        }
    }
  ...
}
```

总结：

- ThreadLocalMap是由自己定义的Entry组成
- Entry为key-value键值对对象，其中key为ThreadLocal本身
- key是弱引用的，会在下次GC时被回收

</br>

# ThreadLocalMap一致性原理

ThreadLocalMap采用线性探测的方式寻找位置，就是根据初始key的hashcode值确定元素在table数组中的位置，如果发现这个位置上已经有其他key值的元素被占用，则利用固定的算法寻找一定步长的下个位置，依次判断，直至找到能够存放的位置。

ThreadLocalMap解决Hash冲突的方式就是简单的步长加1或减1，寻找下一个相邻的位置。

```java
private void set(ThreadLocal<?> key, Object value) {
    Entry[] tab = table;
    int len = tab.length;
  	//获取数组下标
    int i = key.threadLocalHashCode & (len-1);

    for (Entry e = tab[i];
         e != null;
         e = tab[i = nextIndex(i, len)]) {
        ThreadLocal<?> k = e.get();

      	//如果当前遍历的ThreadLocal与传入的ThreadLocal一致，则覆盖value值
        if (k == key) {
            e.value = value;
            return;
        }

      	//如果遍历到的位置key为null，证明此处没有对象，则创建entry放入该位置
        if (k == null) {
            replaceStaleEntry(key, value, i);
            return;
        }
    }

    tab[i] = new Entry(key, value);
    int sz = ++size;
    if (!cleanSomeSlots(i, sz) && sz >= threshold)
        rehash();
}
```

简单来说，set 方法会先计算该 ThreadLocal 的数据下标，如果该位置上为空，则新建 Entry 键值对并插入；如果该位置有数据且 key 一致，则覆盖原有 value；如果 for 循环一直找不到对应位置，在循环外直接给 tab[i] 赋新得 Entry

```java
private Entry getEntry(ThreadLocal<?> key) {
  	//计算出下标
    int i = key.threadLocalHashCode & (table.length - 1);
    Entry e = table[i];
    if (e != null && e.get() == key)
      	//匹配到相同的ThreadLocal
        return e;
    else
      	//没有匹配到相同的ThreadLocal
        return getEntryAfterMiss(key, i, e);
}

private Entry getEntryAfterMiss(ThreadLocal<?> key, int i, Entry e) {
    Entry[] tab = table;
    int len = tab.length;

    while (e != null) {
        ThreadLocal<?> k = e.get();
      
      	//找到相同的就返回，找不到就继续遍历下一个，都找不到就返回null
        if (k == key)
            return e;
        if (k == null)
            expungeStaleEntry(i);
        else
            i = nextIndex(i, len);
        e = tab[i];
    }
    return null;
}
```

get方法也是很简单找到相同的就返回，找不到就继续遍历下一个，都找不到就返回null

</br>

# 内存泄漏问题

由于ThreadLocalMap的key是弱引用，而Value是强引用。这就导致了一个问题，ThreadLocal在没有外部对象强引用时，发生GC时弱引用Key会被回收，而Value不会回收，如果创建ThreadLocal的线程一直持续运行，那么这个Entry对象中的value就有可能一直得不到回收，发生内存泄露。

ThreadLocalMap 实现中已经考虑了这种情况，在调用 `set()`、`get()`、`remove()` 方法的时候，会清理掉 key 为 null 的记录。使用完 ThreadLocal 方法后 最好手动调用`remove()`方法

</br>

# 为什么使用弱键？

下面我们分两种情况讨论：

**（1）key 使用强引用**：引用的ThreadLocal的对象被回收了，但是ThreadLocalMap还持有ThreadLocal的强引用，如果没有手动删除，ThreadLocal不会被回收，导致Entry内存泄漏。

**（2）key 使用弱引用**：引用的ThreadLocal的对象被回收了，由于ThreadLocalMap持有ThreadLocal的弱引用，即使没有手动删除，ThreadLocal也会被回收。value在下一次ThreadLocalMap调用set、get、remove的时候会被清除。

比较两种情况，我们可以发现：由于ThreadLocalMap的生命周期跟Thread一样长，如果都没有手动删除对应key，都会导致内存泄漏，但是使用弱引用可以多一层保障**：**弱引用ThreadLocal不会内存泄漏，对应的value在下一次ThreadLocalMap调用set、get、remove的时候会被清除。

因此，ThreadLocal内存泄漏的根源是：由于ThreadLocalMap的生命周期跟Thread一样长，如果没有手动删除对应key就会导致内存泄漏，而不是因为弱引用。

</br>

# 如何实现线程资源共享

使用 InheritableThreadLocal 类可以实现多个线程访问 ThreadLocal 的值，我们在主线程中创建一个 InheritableThreadLocal 的实例，然后在子线程中得到这个 InheritableThreadLocal 实例设置的值。
使用ThreadLocal threadLocal = new InheritableThreadLocal();即可

</br>

# 总结：

- 一个线程有一个ThreadLocalMap
- 一个线程可以有多个ThreadLocal即ThreadLocalMap存放多个ThreadLocal
- ThreadLocal内部的ThreadLocalMap键为弱引用，会有内存泄漏的风险。
- ThreadLocal仅仅是工具类，可以将其理解为list、map等集合类（特殊的是其只能存放单个对象），ThreadLocal对象的创建可以是自定义线程类内部，也可也是其它非线程类内部，只要某句代码使用ThreadLocal对象的get、set等方法，其实此时该工具类自动会同当前进程相管理，讲变量副本关联到当前线程Thread内置的ThreadLocalMap内以实现变量隔离效果。



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


