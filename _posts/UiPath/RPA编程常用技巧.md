---
title: RPA编程常用技巧
category: UiPath
date: 2020-04-08 17:59:13
updated: 2020-04-08 17:59:13
enname: common
categories: UiPath
tags:
	- UiPath
	- RPA
keywords: UiPath,RPA
permalink:
thumbnail:
---

# 1.引入命名空间

作用:UiPath可以基于VB和C#进行编程,同样的我们在处理数据的过程中可以利用VB或C#的命名空间(包)来进行数据的高校处理<!--more-->

常用命名空间如下:(新建的RPA文件并不包含如下文件)

Microsoft.VisualBasic.CompilerServices
Newtonsoft.Json
Newtonsoft.Json.Linq
System.Collections.ObjectModel
System.Net

</br>

# 2.字符串处理

- 分割字符串:如productVesselLincode.Split({"/"},StringSplitOptions.None)
- 移除字符串空格:如productVesselLincode=productVesselLincode.Replace(" ", [String].Empty)
- 移除字符串换行符:如productVesselLincode=System.Text.RegularExpressions.Regex.Replace(productVesselLincode,"[\n]","")

</br>

# 3.Object对象处理

- 将字符串转为Jobject对象:如JObject.Parse(inputJson)
- 将Object对象转为JObject对象:如JObject.FromObject(exsitContainers)("isAllowSub").ToObject(Of Boolean)
- 从Jobject对象获取json对象属性:如JObject.Parse(inputJson)("data")("content")("shipper")   ---获取Object对象      JObject.Parse(inputJson)("data")("content")("fromTerminalEnName").ToString   ---获取string
- 对象格式转换:如JObject.Parse(inputJson)("data")("taskId").ToObject(Of Int32)
- 新建对象:如New With{.logDetail=XXX ,.logType=XXX,.taskId=XXX}

</br>

# 4.数组处理

- 新建数组:如New List(Of String)
- 从指定数据源新建数组:如New List(Of String)() From { "vgm" }
- 复杂数组的空判断:如IsNothing(JObject.FromObject(exsitContainers)("containers")) Or JObject.FromObject(exsitContainers)("containers").Count<=0
- 初始化集合(数组)对象,例如,ICollection<UiElement>初始化格式为New List(Of UiElement)

</br>

# 5.空对象判断

- IsNothing方法:该方法可用于判断对象是否等于Nothing(相当于java重的null),通常用在例如find children控件的输出值验证(如果找不到对应的子元素,则接收对象为Nothing).但是这个方法也有很大的局限性,很多情况下即使Object对象为空但是其并不等于Nothing,通过此方法无法辨别.这是可以借助字符方法进行判断,例如IsNothing(tableHeaderTh) or String.IsNullOrEmpty(shipper.ToString)
- String.IsNullOrEmpty方法:用于判断字符串是否为空,即""
- String.IsNullOrWhiteSpace方法:用于判断字符串是否为空,即""或者" "
- 数组对象空判断:数组比较特殊,空判断也相对复杂,与我们常见的java不同,如果一个数组为空,其通常有如下表现,借助如下表现可进行空判断

1. JObject.FromObject(exsitContainers)("containers") = []
2. JObject.FromObject(exsitContainers)("containers").Count = 0
3. JObject.FromObject(exsitContainers)("containers").toString = ""

</br>

# 6.发送快捷键

有时我们在处理业务的过程中不可避免的需要用到快捷键功能,向浏览器发送快捷键可以借助控件Send Hotkey完成.这其中需要指定我们发送的快捷键是什么,这也是复杂的地方.

例如我们需要发送ctrl+w则快捷键字符应为"[d(ctrl)]w[u(ctrl)]"其中d代表持续按下,u代表松开

</br>

# 7.正则表达式

利用正则表达式进行条件判断,如在判断验证码是否合规上可以用到

New System.Text.RegularExpressions.Regex("^[0-9a-zA-Z]{4}$").IsMatch(captcha)

</br>

# 8.关于UiElement对象

UiElement对象是Uipath中自定义的一种用于代表页面元素的对象数据类型,它将存放某指定元素的完整信息并可以基于该对象对页面元素进行完整的操作.但是其也有一些特殊的特性需要注意,具体如下:

- UiElement对象就如同Sting\Object对象等可以在不同RPA间作为参数传递
- UiElement对象只在本页面有效,页面刷新后即使元素相同也无法选择对应元素.例如在A页面下获取了某元素对象,由于业务因素必须刷新页面,在刷新页面后即使我们知道是相同的页面相同的元素,也不可以使用刷新前获取的UiElement对象进行操作,此时该对象已经失效.
- 如果UiElement对象有失效的风险,我们可以借助UiElement中的selector属性获取selector字符串,将字符串进行传递,此时将不受页面刷新影响

</br>

# 9.依赖包引用

- UiPath v19.10
- Uipath.Excel.Activities  v2.7.2
- Uipath.Mail.Activities   v1.7.2
- Uipath.System.Activities   v19.10.1
- Uipath.UIAutomation.Activities   v19.11.0
- Uipath.Web.Activities   v1.4.4

</br>

# 10.特殊元素获取

- chrome浏览器alert弹出框内容获取

```html
<html app='chrome.exe' title='::: Sinokor New e-Service :::' />
<ctrl role='dialog' />
<ctrl name='*' role='text' />
```

- 

</br>

# 其它问题解答

- 浏览器分辨率问题导致的点击功能无法正确点击

1. 具体问题:如果浏览器窗口太小,且被处理网站没有做页面自适应,那么将会有很大几率出现横向+纵向滚动条,这种情况下将会造成我们需要点击的按钮被遮盖,从而无法正常点击触发
2. 解决方式:首先是尽可能保证浏览器不处于缩放状态,其次可以借助set focus控件使RPA在点击前聚焦一下,这样页面将自动聚焦到被遮盖的区域

- Uipath日志本地路径:例如C:\Users\EDZ\AppData\Local\UiPath\Logs
- 通配符常用在过滤器或选择器中,例如<webctrl class='*inputField hal-input hal-olb-input sizeSmall*' tag='INPUT' />,其中的*则代表通配符






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


