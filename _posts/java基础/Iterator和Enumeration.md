---
title: Iterator和Enumeration
category: java基础
date: 2022-06-10 22:18:05
updated: 2022-06-10 22:18:05
enname: ite-enu
categories: java基础
tags:
keywords: Iterator，Enumeration
permalink:
thumbnail:
---

针对集合我们可以通过 “Iterator(迭代器)” 或 “Enumeration(枚举类)” 去遍历集合，但两者是有区别的。<!--more-->

</br>

# 两者区别

- Enumeration只有2个函数接口，可以读取数据不能修改数据
- Iterator只有3个函数接口，可以读取和修改数据
- Iterator支持fail-fast机制，而Enumeration不支持

</br>

</br>

# fail-fast 

fail-fast 机制是java集合(Collection)中的一种错误机制。当多个线程对同一个集合的内容进行操作时，就可能会产生fail-fast事件。
例如：当某一个线程A通过iterator去遍历某集合的过程中，若该集合的内容被其他线程所改变了；那么线程A访问集合时，就会抛出ConcurrentModificationException异常，产生fail-fast事件。

</br>

### 解决方式

使用“java.util.concurrent包下的类”去取代“java.util包下的类”

</br>

### 原理

fail-fast产生的步骤如下：

(01) *新建了一个ArrayList，名称为arrayList。*

(02) *向arrayList中添加内容。*

(03) *新建一个“***线程a***”，并在“线程a”中***通过Iterator反复的读取arrayList的值***。*

(04) *新建一个“***线程b***”，在“线程b”中***删除arrayList中的一个“节点A***”。*

(05) 这时，就会产生有趣的事件了。

​    在某一时刻，“线程a”创建了arrayList的Iterator。此时“节点A”仍然存在于arrayList中，**创建arrayList时，expectedModCount = modCount**(假设它们此时的值为N)。

​    在“线程a”在遍历arrayList过程中的某一时刻，“线程b”执行了，并且“线程b”删除了arrayList中的“节点A”。“线程b”执行remove()进行删除操作时，在remove()中执行了“modCount++”，此时**modCount变成了N+1**！

“线程a”接着遍历，当它执行到next()函数时，调用checkForComodification()比较“expectedModCount”和“modCount”的大小；而“expectedModCount=N”，“modCount=N+1”,这样，便抛出ConcurrentModificationException异常，产生fail-fast事件。

至此，**我们就完全了解了fail-fast是如何产生的！**

即，当多个线程对同一个集合进行操作的时候，某线程访问集合的过程中，该集合的内容被其他线程所改变(即其它线程通过add、remove、clear等方法，改变了modCount的值)；这时，就会抛出ConcurrentModificationException异常，产生fail-fast事件。



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


