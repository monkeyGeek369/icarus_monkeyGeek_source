---
title: nio简介
category: 软件技术
date: 2022-12-29 23:13:22
updated: 2022-12-29 23:13:22
enname: nio_base
categories: 软件技术
tags:
	- 面试
keywords: nio、NIO
permalink:
thumbnail:
---

# 什么是BIO

特点：面向流、阻塞io<!--more-->

通过服务器模型讲解阻塞比较好懂，当客户端请求到达服务器时会创建对应的单线程进行处理，单线程进行读写会阻塞，如果没有客户端请求则服务器接收端进行阻塞，这就是阻塞。



缺点：单线程单连接的方式容易造成阻塞，请求过多导致线程增多，频繁的上下文切换消耗资源。



</br>

</br>

# 什么是NIO（没有多路复用）

- 服务端改用单线程接受客户端连接，多个客户端连接存储起来由单线程遍历执行
- 接收客户端连接的时候，不需要阻塞，如果没有客户端连接服务端的接收方法就返回-1
- 在读写操作的时候，也不阻塞，有数据就读，没数据就直接返回



缺点：在客户端与服务器建立连接后，后续会进行一系列的读写操作。虽然这些读写操作是非阻塞的，但是每调一次读写操作在操作系统层面都要进行一次**用户态和内核态的切换**，这个也是一项巨大的开销（读写等系统调用都是在内核态完成的）



</br>

</br>

# 什么是多路复用

只有在数据准备好了的情况下读写操作才有效，否则只是进行空读，白白浪费了操作系统用户态与内核态的切换。

通过多路复用技术可以一次性知道哪些文件描述符可读哪些可写，从而避免了用户态与内核态的切换。

多路复用器如下表：

|            | select                                                       | poll                                     | epoll                                                        |
| ---------- | ------------------------------------------------------------ | ---------------------------------------- | ------------------------------------------------------------ |
| 操作方式   | 遍历                                                         | 遍历                                     | 回调                                                         |
| 底层实现   | 数组                                                         | 链表                                     | 红黑树                                                       |
| IO效率     | 每次调用都进行线性遍历，时间复杂度为O(n)                     | 每次调用都进行线性遍历，时间复杂度为O(n) | 事件通知方式，每当fd就绪，系统注册的回调函数就会被调用，将就绪fd放到readyList里面，时间复杂度O(1) |
| 最大连接数 | 1024（x86）或2048（x64）                                     | 无上限                                   | 无上限                                                       |
| fd拷贝     | 每次调用select，都需要把fd集合从用户态拷贝到内核态，再拷贝回用户态 | 同select                                 | 调用epoll_ctl时拷贝进内核并保存，之后每次epoll_wait不拷贝    |

## select

方式是将已连接的 Socket 都放到一个文件描述符集合，然后调用 select 函数将文件描述符集合拷贝到内核里，让内核来检查是否有网络事件产生(通过遍历文件描述符集合的方式)，当检查到有事件产生后，将此 Socket 标记为可读或可写， 接着再把整个文件描述符集合拷贝回用户态里，然后用户态还需要再通过遍历的方法找到可读或可写的 Socket，然后再对其处理。

两次遍历、两次拷贝



</br>

## epoll

第一点：在内核中通过红黑树维护待检测文件描述符fd，每次只需要调用epoll_ctl()来加入待检测的socket，不用每次copy全量。



第二点：epoll 使用事件驱动的机制，内核里**维护了一个链表来记录就绪事件**，当某个 socket 有事件发生时，通过回调函数内核会将其加入到这个就绪事件列表中，当用户调用 `epoll_wait()` 函数时，只会返回有事件发生的文件描述符的个数（从内核态拷贝到用户态）。



</br>

</br>

# Java中的NIO

- 特点：面向缓存区、非阻塞io
- 通道channel：数据传输双方的连接，本身不存储数据，只能与缓存区进行交互
- 缓存区buffer：在java中的数据结构是数组，用于存储数据，可以对其进行读取和写入。
- 选择器Selector：对Linux下的select/poll/epoll的包装，负责监听channel事件发生



在java中缓存区氛围直接缓存区、非直接缓存区（可以通过不同的方法调用实现）

- 直接缓存区：将物理内存作为缓存区，读写双方通过通道共同作用于缓存区
- 非直接缓存区：在jvm中建立缓存区供应用程序操作，另外在内核地址空间建立缓存区供文件系统等操作，双方缓存区的读写由通道完成



java NIO的非阻塞模式，是一个线程维护着一个选择器（Selector），客户端读/写操作而不是直接对接线程，而是先进入缓冲区，通道（channel）从缓冲区获取相应事件，准备好读/写事件后通道会通知选择器（Selector）来执行，只对准备好的通道进行数据的读写操作，当前通道没有读写事件，而不是阻塞等待事件，可能去处理其它已经准备好了的通道，这样从而达到了一个线程可以维护多个客户端读写事件

1. 每一个Channel都会对于一个Buffer
2. Selector对应一个线程，一个线程对应多个通道（Channel）
3. 程序切换到那个Channel是由事件决定的
4. Selector会根据不同的事件，在各通道上切换
5. Channel是双向的，读写不冲突



</br>

</br>

# Selector

selector的作用是针对绑定在其上的channel做事件监听，如果某channel发生某事件就会有对应的处理，从而做到了单线程处理多channel。

Selector维护着一个SelectionKeys是一个Set容器里面存放着所有连接注册进来的客户端，每次Selector监听时其实就是查看SelectionKeys容器所有连接有没有事件。



</br>

</br>

参考资料：

[1]https://blog.csdn.net/weixin_44642403/article/details/108692965

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


