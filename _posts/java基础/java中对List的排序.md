---
title: java中对List的排序
category: java基础
date: 2020-04-07 21:16:06
updated: 2020-04-07 21:16:06
enname: list_sort
categories: java基础
tags:
	- java集合
keywords: java,list,排序
permalink:
thumbnail:
---

# 简介

java中的排序本质上是实现对待排序对象的比较,即借助比较器实现排序,例如Comparator和Comparable等.<!--more-->当对象具备比较功能那么可以借助集合工具的排序算法来实现具体排序,例如Collections.sort以及stream.sort等.详细信息可参考[[java中的比较器Comparable与Comparator]](http://www.monkeygeek.cn/2020/4/7/compare/)

</br>

# 具体方案

测试模型

```java
public class student implements Comparable<student>{

    int age;

    int grade;

    //重写默认比较器---升序
    @Override
    public int compareTo(student o){
        // 入参o:可以理解为排序队列中的前一个对象,即被比较对象
        // this:理解为排序队列的后一个对象,即比较对象
        if(o.getAge() > this.getAge()){
            return -1;
        }else if(o.getAge() < this.getAge()){
            //解读:如果前一个对象小于后一个对象,那么符合我们的目标排序规则,则返回1
            //因此我们可以想象一下,前小后大的序列是升序序列,以此我们可以理解降序序列
            return 1;
        }else {
            return 0;
        }
    }
}
```

先上代码

```java
//创建测试数组
    private List<student> students = Lists.newArrayList();

@Test
    public void sortMethod() {
        //初始化测试数组
        students.add(student.builder().age(10).build());
        students.add(student.builder().age(30).build());
        students.add(student.builder().age(50).build());
        students.add(student.builder().age(10).build());
        students.add(student.builder().age(20).build());
        students.add(student.builder().age(40).build());
        students.add(student.builder().age(60).build());

        //Comparable实践
        //1.借助student类中实现的Comparable接口完成排序
        //Collections.sort(students);
        //students = students.stream().sorted();

        //Comparator实践
        //1.匿名内部类实现排序
        Collections.sort(students, new Comparator<student>() {
            @Override
            public int compare(student o1, student o2) {
                // 入参o:可以理解为排序队列中的前一个对象,即被比较对象
                // this:理解为排序队列的后一个对象,即比较对象
                if (o1.getAge() > o2.getAge()) {
                    return -1;
                } else if (o1.getAge() < o2.getAge()) {
                    //解读:如果前一个对象小于后一个对象,那么符合我们的目标排序规则,则返回1
                    //因此我们可以想象一下,前小后大的序列是升序序列,以此我们可以理解降序序列
                    return 1;
                } else {
                    return 0;
                }
            }
        });
        
        //2.借助stream的sort
        //根据年龄排序
        students = students.stream().sorted(Comparator.comparing(student::getAge)).collect(Collectors.toList());
        //根据年龄排序-反序
        students = students.stream().sorted(Comparator.comparing(student::getAge).reversed()).collect(Collectors.toList());
        //先根据年龄排序-如果遇到相等则再根据分数排序
        students = students.stream().sorted(Comparator.comparing(student::getAge).thenComparing(student::getGrade)).collect(Collectors.toList());

    }
```



</br>

*<u>总结</u>*

排序的基础和前提是比较,若要实现比较则必须借助Comparator或Comparable(具体可参考[[java中的比较器Comparable与Comparator]](http://www.monkeygeek.cn/2020/4/7/compare/)),在比较实现后可借助集合工具内置排序算法进行排序.

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


