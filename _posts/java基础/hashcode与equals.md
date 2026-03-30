---
title: hashcode与equals
category: java基础
date: 2022-06-06 23:56:20
updated: 2022-06-06 23:56:20
enname: dashcode-equals
categories: java基础
tags:
	- 面试
keywords: hashcode,equals
permalink:
thumbnail:
---

**equals介绍**

equals是Object类提供的方法之一<!--more-->,java中每一个对象都有equals这个方法。

public boolean equals(Object obj) { 

 return (this == obj); 

}

此时比较的是两个引用是否指向内存中的同一个对象。

</br>

**hashcode介绍**

hashcode()这个方法也是从object类中继承过来的，在object类中定义如下：

public native int hashCode();

hashCode()返回该对象的哈希码值，该值通常是一个由该对象的内部地址转换而来的整数。程序先进行 hashcode 的比较，如果不同，那没就不必在进行 equals 的比较了，这样就大大减少了 equals 比较的次数

</br>

**相互关系**

- 规则1：如果两个对象equals是相等的，那么他们必须拥有一样的hashcode
- 规则2：如果两个对象有一样的hashcode，但仍不一定相等，因为还需要第二个要求，也就是equals方法的判断。

</br>

**注意事项**

重写equals时一定要重写hashcode

例如：只重写了A对象的equals方法，如果创建两个A对象实例A1,A2并使两者满足equals相等hashcode不等。

A1,A2存放入hashmap时会被当作不同的对象重复存储，如果此时创建A3并以A3为key进行查询则永远不可能查到A1,A2。

</br>

**重写equals举例**

```java
@Override
public boolean equals(Object obj) {
if(this == obj){
return true;//地址相等 }

if(obj == null){
return false;//非空性：对于任意非空引用x，x.equals(null)应该返回false。 }

if(obj instanceof User){
User other = (User) obj;
//需要比较的字段相等，则这两个对象相等
if(equalsStr(this.name, other.name)
&& equalsStr(this.age, other.age)){
return true;
}
}

return false;
}
```

</br>

**重写hashCode举例**

```java
@Override
public int hashCode() {
int result = 17;
result = 31 * result + (name == null ? 0 : name.hashCode());
result = 31 * result + (age == null ? 0 : age.hashCode());
return result;
}
```

</br>

**为什么要使用31**

原因一：更少的乘积结果冲突

　　31是质子数中一个“不大不小”的存在，如果你使用的是一个如2的较小质数，那么得出的乘积会在一个很小的范围，很容易造成哈希值的冲突。而如果选择一个100以上的质数，得出的哈希值会超出int的最大范围，这两种都不合适。而如果对超过 50,000 个英文单词（由两个不同版本的 Unix 字典合并而成）进行 hash code 运算，并使用常数 31, 33, 37, 39 和 41 作为乘子，每个常数算出的哈希值冲突数都小于7个（国外大神做的测试），那么这几个数就被作为生成hashCode值得备选乘数了。

　　所以从 31,33,37,39 等中间选择了 31 的原因看原因二。



原因二：可以被jvm优化

　　JVM里最有效的计算方式就是进行位运算了：



　　* 左移 << : 左边的最高位丢弃，右边补全0（把 << 左边的数据*2的移动次幂）。

　　* 右移 >> : 把>>左边的数据/2的移动次幂。

　　* 无符号右移 >>> : 无论最高位是0还是1，左边补齐0。 　　

​    所以 ： 31 * i = (i << 5) - i（左边 31*2=62,右边  2*2^5-2=62） - 两边相等，JVM就可以高效的进行计算



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


