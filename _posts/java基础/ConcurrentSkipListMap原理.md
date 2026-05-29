---
title: ConcurrentSkipListMap原理
category: java基础
date: 2022-06-16 22:38:28
updated: 2022-06-16 22:38:28
enname: con-skiplistmap
categories: java基础
tags:
	- 面试
	- 并发容器
keywords: ConcurrentSkipListMap
permalink:
thumbnail:
---

# SkipList

Skip list让已排序的数据分布在多层链表中，通过“空间来换取时间”的一个算法，在插入、删除、查找时可以忽略一些不可能涉及到的结点，从而提高了效率。<!--more-->

其插入和查找的效率O(logn)，其效率不低于红黑树，但是其原理和实现的复杂度要比红黑树简单多了。



</br>

## 特性

- 由很多层结构组成，level是通过一定的概率随机产生的
- 每一层都是一个有序的链表，默认是升序，也可以根据创建映射时所提供的Comparator进行排序，具体取决于使用的构造方法
- 最底层(Level 1)的链表包含所有元素
- 如果一个元素出现在Level i 的链表中，则它在Level i 之下的链表也都会出现（换句话说上层元素是从下层元素中挑选出来的）
- 整体结构类似一个金字塔类型，通过每层的索引从上层向下层跳跃查询
- 每个节点包含两个指针，一个指向同一链表中的下一个元素，一个指向下面一层的元素



</br>

## 插入操作

- 查找合适的位置。这里需要明确一点就是在确认新节点要占据的层次K时，采用丢硬币的方式，完全随机。如果占据的层次K大于链表的层次，则重新申请新的层，否则插入指定层次
- 申请新的节点
- 调整指针



</br>

</br>

# ConcurrentSkipListMap

内部采用SkipLis数据结构实现。为了实现SkipList，ConcurrentSkipListMap提供了三个内部类来构建这样的链表结构：Node、Index、HeadIndex。

通过CAS来实现同步



</br>

## 数据结构

- HeadIndex：用来维护索引层次，通过Index从最上层开始往下层查找，一步一步缩小查询范围。

```java
static final class HeadIndex<K,V> extends Index<K,V> {
    final int level;  //索引层，从1开始，Node单链表层为0
    HeadIndex(Node<K,V> node, Index<K,V> down, Index<K,V> right, int level) {
        super(node, down, right);
        this.level = level;
    }
}
```



- Index：提供了一个基于Node节点的索引Node，一个指向下一个Index的right，一个指向下层的down节点

```java
static class Index<K,V> {
    final ConcurrentSkipListMap.Node<K,V> node;
    final ConcurrentSkipListMap.Index<K,V> down;
    volatile ConcurrentSkipListMap.Index<K,V> right;

    /** 省略些许代码 */
}
```



- Node：数据节点。包含key、value和一个指向下一个节点的next。

```java
static final class Node<K,V> {
    final K key;
    volatile Object value;
    volatile ConcurrentSkipListMap.Node<K, V> next;

    /** 省略些许代码 */
}
```



</br>

参考资料：

https://juejin.cn/post/6844903958499033095



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


