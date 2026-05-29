---
title: hash函数与冲突解决
category: 软件技术
date: 2022-06-07 23:45:08
updated: 2022-06-07 23:45:08
enname: hash
categories: 软件技术
tags:
	- 面试
keywords: hash、冲突解决
permalink:
thumbnail:
---

# 什么是Hash？

一般翻译做“散列”，也有直接音译为“哈希”的，就是把任意长度的输入，通过散列算法，变换成固定长度的输出，该输出就是散列值。<!--more-->

</br>

# 什么是散列表？

它是基于快速存取的角度设计的，也是一种典型的“空间换时间”的做法。顾名思义，该数据结构可以理解为一个线性表，但是其中的元素不是紧密排列的，而是可能存在空隙。散列表（Hash table，也叫哈希表），是根据关键码值(Key value)而直接进行访问的数据结构。也就是说，它通过把关键码值映射到表中一个位置来访问记录，以加快查找的速度。这个映射函数叫做散列函数，存放记录的数组叫做散列表。

</br>

# 碰撞的产生？

我们知道通过hash函数来生成的固定长度散列值，通过散列值找到散列表中对用的数据以实现快速访问。但散列值产生的质量高低由散列函数决定，理论上存在完美散列函数（即针对不同输入产生不同输出）但实际上做不到，因此可能会出现不同输入产生相同散列值的情况，此时碰撞发生。

</br>

# 碰撞的解决方法？

1、开放地址法： 当发生地址冲突的时候，按照某种方法继续探测哈希表中的其他存储单元，直到找到空位置为止

2、链表法： 将所有关键字为同义词的记录存储在同一线性链表中

3、公共溢出区法： 一旦发生冲突，都填入溢出表

4、再hash法：当发生冲突时，使用第二个、第三个、哈希函数计算地址，直到无冲突时。缺点：计算时间增加。

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


