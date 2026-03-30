---
title: KMP算法
category: 算法
date: 2022-06-19 16:35:28
updated: 2022-06-19 16:35:28
enname: kmp
categories: 算法
tags:
	- 算法
	- 面试
keywords: kmp、算法
permalink:
thumbnail:
---

字符串查找算法，常用于在一个文本串S内查找一个模式串P 的出现位置<!--more-->

KMP算法的关键是求出模式串对应的Next数组，借助next数组来实现字符串匹配失败时的跳转步数。

</br>

</br>

# 算法步骤

1、假设现在文本串S匹配到 i 位置，模式串P匹配到 j 位置

2、如果j = -1，或者当前字符匹配成功（即S[i] == P[j]），都令i++，j++，继续匹配下一个字符；

3、如果j != -1，且当前字符匹配失败（即S[i] != P[j]），则令 i 不变，j = next[j]。此举意味着失配时，模式串P相对于文本串S向右移动了j - next [j] 位。

</br>

</br>

# 关键字说明

- 文本串：匹配字符串文本，如ababaaabd
- 模式串：待匹配字符串，如aab
- 最长相同前缀后缀：如ababacdab中的最长相同前缀后缀为ab（从字符串头和尾分别找寻两者相待的字符串）
- next数组：next数组研究的是对模式串的“最长相同前缀后缀”的跳转记录。next意味着在某个字符失配时，该字符对应的next 值会告诉你下一步匹配中，模式串应该跳到哪个位置（跳到next [j] 的位置）。如果next [j] 等于0或-1，则跳到模式串的开头字符，若next [j] = k 且 k > 0，代表下次匹配跳到j 之前的某个字符，而不是跳到开头，且具体跳过了k 个字符。

</br>

</br>

# 寻找最长前缀后缀

如果给定的模式串是：“ABCDABD”，从左至右遍历整个模式串，其各个子串的前缀后缀分别如下表格所示：

| 模式串的各个子串 | 前缀                | 后缀                | 最大公共元素长度 |
| ---------------- | ------------------- | ------------------- | ---------------- |
| A                | 无                  | 无                  | 0                |
| AB               | A                   | B                   | 0                |
| ABC              | A,AB                | C,BC                | 0                |
| ...              |                     |                     |                  |
| ABCDA            | A,AB,ABC,ABCD       | A,DA,CDA,BCDA       | 1                |
| ABCDAB           | A,AB,ABC,ABCD,ABCDA | B,AB,DAB,CDAB,BCDAB | 2                |
| ABCDABD          | ...                 | ...                 | 0                |



| 字符         | A    | B    | C    | D    | A    | B    | D    |
| ------------ | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| 最长前缀后缀 | 0    | 0    | 0    | 0    | 1    | 2    | 0    |

</br>

</br>

# 寻找next数组

next 数组相当于“最大长度值” 整体向右移动一位，然后初始值赋为-1

| 模式串       | A    | B    | C    | D    | A    | B    | D    |
| ------------ | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| 最长前缀后缀 | 0    | 0    | 0    | 0    | 1    | 2    | 0    |
| next数组     | -1   | 0    | 0    | 0    | 0    | 1    | 2    |



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


