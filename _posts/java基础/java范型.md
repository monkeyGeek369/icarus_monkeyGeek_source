---
title: java范型
category: java基础
date: 2022-05-28 21:52:25
updated: 2022-05-28 21:52:25
enname: java-generics
categories: java基础
tags:
	- 面试
keywords: java、范型、面试
permalink:
thumbnail:
---

# 什么是范型

范型的本质是参数化数据类型，也就是说操作的数据类型被指定为一个参数，代码可以应用于多种数据类型。<!--more-->一般如果数据类型不确定可以使用范型来简化代码。

```java
class Point<T>{   
    private T x ;  
}
```

</br>

# 范型的优点

范型的最大优点是提供了编译时类型安全检测机制，该机制允许程序员在编译时监测到非法的类型。

```java
class ObjectPoint{  
    private Object x ;  
    private Object y ;  
    public void setX(Object x){  
        this.x = x ;  
    }  
    public void setY(Object y){  
        this.y = y ;  
    }  
    public Object getX(){  
        return this.x ;  
    }  
    public Object getY(){  
        return this.y ;  
    }  
}  

ObjectPoint floatPoint = new ObjectPoint();  
floatPoint.setX(new Float(100.12f));  
String floatX = (String)floatPoint.getX();  
```

floatPoint.getX()返回的类型是Object，所以编译时将Object强转成String是成立的不会报错，但是运行时会报类型转换错误。

```java
class Point<T>{ 
    private T x ;        
    private T y ;        
    public void setX(T x){
        this.x = x ;  
    }  
    public void setY(T y){  
        this.y = y ;  
    }  
    public T getX(){
        return this.x ;  
    }  
    public T getY(){  
        return this.y ;  
    }  
};  
//IntegerPoint使用  
Point<Integer> p = new Point<Integer>() ;   
p.setX(new Integer(100)) ;   
System.out.println(p.getX());    
```

范型不存在强制转换，如果传入类型有问题会在编辑时报错

</br>

# 范型的种类

- 范型类定义：**class** Point<T>{}，T表示派生自Object类的任何类，T只是一个代号可以用其它大些字母表示。
- 多范型变量：**class** MorePoint<T,U>{ }
- 范型接口：**interface** Info<T>{ }有兴趣可以自行研究用法
- 范型函数：

```java
public static  <T> void StaticMethod(T a){  
        Log.d("harvic","StaticMethod: "+a.toString());  
    } 

StaticFans.StaticMethod("adfdsa");//使用方法一  隐式传递类型
StaticFans.<String>StaticMethod("adfdsa");//使用方法二 显式传递类型(推荐使用)
```

返回值中存在范型如下案例

```java
public static <T> List<T> parseArray(String response,Class<T> object){  
    List<T> modelList = JSON.parseArray(response, object);  
    return modelList;  
} 
```

- class<T>类传递

```java
public static <T> List<T> parseArray(String response,Class<T> object){  
    List<T> modelList = JSON.parseArray(response, object);  
    return modelList;  
} 

//Class<T> object来传递类的class对象，Class<T>也是一泛型，它是传来用来装载类的class对象的，它的定义如下：
public final class Class<T> implements Serializable {  
    …………  
} 
```

- 范型数组

```java
public static <T> T[] fun1(T...arg){  // 接收可变参数    
       return arg ;            // 返回泛型数组    
} 
```

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


