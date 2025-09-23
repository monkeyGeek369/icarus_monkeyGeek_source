---
title: LinkedList原理
category: java基础
date: 2022-06-12 23:40:39
updated: 2022-06-12 23:40:39
enname: linkedlist
categories: java基础
tags:
	- 面试
	- java集合
keywords: LinkedList、原理
permalink:
thumbnail:
---

# 简介

- 继承于AbstractSequentialList的双向链表<!--more-->。它也可以被当作堆栈、队列或双端队列进行操作
- 实现 List 接口，能对它进行队列操作
- 实现 Deque 接口，即能将LinkedList当作双端队列使用
- 实现了Cloneable接口，能克隆
- 实现java.io.Serializable接口，能通过序列化去传输
- 是非同步的



</br>

# 数据结构

- header：双向链表的表头，为Entry类的实例
- size：双向链表中节点的个数



注：Entry中包含成员变量： previous, next, element。其中，previous是该节点的上一个节点，next是该节点的下一个节点，element是该节点所包含的值



</br>

# 遍历方式

- 迭代器遍历
- 随机访问：如list.get(i);效率低
- pollFirst()：while(list.pollFirst() != null)
- pollLast()：while(list.pollLast() != null)
- removeFirst()：while(list.removeFirst() != null)
- removeLast()：while(list.removeLast() != null)



</br>

# 特性说明

- 双向链表顺序访问高效，随机访问效率低
- 如何支持索引（随机）访问（即方法*get(int location)*）：首先会比较“location”和“双向链表长度的1/2”；若前者大，则从链表头开始往后查找，直到location位置；否则，从链表末尾开始先前查找，直到location位置
- 不存在容量不足的问题



</br>





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


