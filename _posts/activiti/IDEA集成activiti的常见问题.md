---
title: IDEA集成activiti的常见问题
category: activiti
date: 2020-06-30 20:50:41
updated: 2020-06-30 20:50:41
enname: activitierror
categories: activiti
tags:
	- activiti
	- 面试
keywords: activiti,activiti常见问题
permalink:
thumbnail: ../../image/activitierror3.png
---

# BPMN设计图中的乱码问题

<!--more-->

需要在这两个文件下的后面都加上-Dfile.encoding=UTF-8

![](../../../../image/activitierror1.png)

![](../../../../image/activitierror2.png)

然后重启IDEA，把原来的PNG图片删掉，再生成一次就不会出现乱码了

</br>



# 设置自定义参数的节点必须进行参数传递

![](../../../../image/activitierror3.png)

如上流程图，在“请假天数判断”设置了days参数用于路径选择，那么在该节点的上一节点完成时必须设定days参数并传递，否则在流转到“请假天数判断”时会报错，缺少days



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


