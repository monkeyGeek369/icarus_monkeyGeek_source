---
title: ReentrantLock原理
category: java基础
date: 2022-06-19 12:46:38
updated: 2022-06-19 12:46:38
enname: reent-l
categories: java基础
tags:
	- 面试
	- 锁
keywords: ReentrantLock
permalink:
thumbnail:
---

# 简介

- 实现Lock接口的一个类，支持重入性，表示能够对共享资源能够重复加锁<!--more-->
- 当前线程获取该锁再次获取不会被阻塞
- ReentrantLock还支持公平锁和非公平锁两种方式

</br>

# 公平锁与非公平锁

```java
Lock lock=new ReentrantLock(true);//公平锁
Lock lock=new ReentrantLock(false);//非公平锁
```



- 公平锁指的是线程获取锁的顺序是按照加锁顺序来的，而非公平锁指的是抢锁机制，先lock的线程不一定先获得锁，效率更高。
- 公平锁每次获取到锁为同步队列中的第一个节点，保证请求资源时间上的绝对顺序，而非公平锁有可能刚释放锁的线程下次继续获取该锁，则有可能导致其他线程永远无法获取到锁，造成“饥饿”现象。
- 公平锁为了保证时间上的绝对顺序，需要频繁的上下文切换，而非公平锁会降低一定的上下文切换，降低性能开销。因此，ReentrantLock默认选择的是非公平锁，则是为了减少一部分上下文切换，保证了系统更大的吞吐量。



</br>

# Condition类

Lock类可以创建Condition对象，Condition对象用来是线程等待和唤醒线程

一个condition对象的signal（signalAll）方法和该对象的await方法是一一对应的，也就是一个condition对象的signal（signalAll）方法不能唤醒其他condition对象的await方法。

```java
public class MyService {

private Lock lock = new ReentrantLock();
private Condition condition=lock.newCondition();
public void testMethod() {
try {
lock.lock();
System.out.println("开始wait");
condition.await();
for (int i = 0; i < 5; i++) {
System.out.println("ThreadName=" + Thread.currentThread().getName()
+ (" " + (i + 1)));
}
} catch (InterruptedException e) {
// TODO 自动生成的 catch 块
e.printStackTrace();
}
finally
{
lock.unlock();
}
}

}
```



- Condition类的awiat方法和Object类的wait方法等效
- Condition类的signal方法和Object类的notify方法等效
- Condition类的signalAll方法和Object类的notifyAll方法等效

</br>

</br>

# 底层原理

1. ReentrantLock先通过CAS尝试获取锁，

2. 1. 如果此时锁已经被占用，该线程加入AQS队列并wait()

   2. 当前驱线程的锁被释放，挂在CLH队列为首的线程就会被notify()，然后继续CAS尝试获取锁，此时：

   3. 1. 非公平锁，如果有其他线程尝试lock()，有可能被其他刚好申请锁的线程**抢占**。
      2. 公平锁，只有在CLH**队列头的线程**才可以获取锁，**新来的线程**只能插入到队尾。

</br>

### lock()函数

```java
    final void lock() { //非公平锁
        if (compareAndSetState(0, 1))
            setExclusiveOwnerThread(Thread.currentThread());
        else
            acquire(1);
    }

    final void lock() { //公平锁
            acquire(1);
    }
```

- 如果成功通过CAS修改了state，指定当前线程为该锁的独占线程，标志自己成功获取锁。
- 如果CAS失败的话，调用acquire();
- acquire()中调用tryAcquire()，会尝试再次通过CAS修改state为1
- 如果失败而且发现锁是被当前线程占用的，就执行重入（state++）
- 如果锁是被其他线程占有，那么当前线程执行tryAcquire返回失败，并且执行addWaiter()进入等待队列，并挂起自己interrupt()



</br>

### Unlock()函数

- 释放时候，state--，通过state==0判断锁是否完全被释放。
- 成功释放锁的话，唤起一个被挂起的线程



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


