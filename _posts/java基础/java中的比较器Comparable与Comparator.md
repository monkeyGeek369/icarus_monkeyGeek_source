---
title: java中的比较器Comparable与Comparator
category: java基础
date: 2020-04-07 21:48:37
updated: 2020-04-07 21:48:37
enname: compare
categories: java基础
tags:
keywords: java,java比较器,Comparable,Comparator,排序
permalink:
thumbnail:
---

# 简介

我们在java编码的过程中不可避免的会使用java中提供的集合对象,例如List,ArrayList等等,那么在使用集合的过程中有很大的几率会用到排序.回想一下在学习c语言的时候排序是如何实现的?<!--more-->没错,就是通过手写各种基础的排序算法实现排序(当然现在也有各种c语言的基础包可用),例如选择\冒泡\插入\希尔\二分法等等.在Java中我们当然也可以自己写排序算法实现排序,但是JDK已经提供一些基础的基于集合的排序接口供我们使用,因此无需特意自写算法.本文将介绍两种排序的实现方式Comparable与Comparator.

</br>

# Comparable

首先我们举一个简单的例子，如下：

```java
@Test
    public void integerSort(){
        List<Integer> integers = Lists.newArrayList();
        integers.add(5);
        integers.add(6);
        integers.add(1);
        integers.add(3);
        integers.add(4);
        integers.add(9);

        //排序前
        printUtil.printList(integers);

        //排序--通过集合工具类Arrays
        Collections.sort(integers);

        //排序后
        printUtil.printList(integers);
    }
```

执行结果如下：

```java
5,6,1,3,4,9,
1,3,4,5,6,9,
```

看到这里大家或许会有疑问？我们什么都没有做为什么代码自动实现了排序？

那是因为我们的包装类型Integer内部已经继承并重写了comparable

```java
public final class Integer extends Number implements Comparable<Integer> {
    /**
     * Compares two {@code Integer} objects numerically.
     *
     * @param   anotherInteger   the {@code Integer} to be compared.
     * @return  the value {@code 0} if this {@code Integer} is
     *          equal to the argument {@code Integer}; a value less than
     *          {@code 0} if this {@code Integer} is numerically less
     *          than the argument {@code Integer}; and a value greater
     *          than {@code 0} if this {@code Integer} is numerically
     *           greater than the argument {@code Integer} (signed
     *           comparison).
     * @since   1.2
     */
    public int compareTo(Integer anotherInteger) {
        return compare(this.value, anotherInteger.value);
    }

    /**
     * Compares two {@code int} values numerically.
     * The value returned is identical to what would be returned by:
     * <pre>
     *    Integer.valueOf(x).compareTo(Integer.valueOf(y))
     * </pre>
     *
     * @param  x the first {@code int} to compare
     * @param  y the second {@code int} to compare
     * @return the value {@code 0} if {@code x == y};
     *         a value less than {@code 0} if {@code x < y}; and
     *         a value greater than {@code 0} if {@code x > y}
     * @since 1.7
     */
    public static int compare(int x, int y) {
        return (x < y) ? -1 : ((x == y) ? 0 : 1);
    }
}
```

我们再来举例一个稍微复杂的例子：

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

```java
@Test
    public void studentSortByAge(){
        List<student> students = Lists.newArrayList();

        students.add(student.builder().age(10).build());
        students.add(student.builder().age(30).build());
        students.add(student.builder().age(50).build());
        students.add(student.builder().age(20).build());
        students.add(student.builder().age(40).build());
        students.add(student.builder().age(60).build());

        //排序前
        printUtil.printList(students);

        //排序--通过集合工具类Collections(底层是利用二分法实现的排序)
        Collections.sort(students);

        //排序--通过stream
        //students = students.stream().sorted().collect(Collectors.toList());

        //排序后
        printUtil.printList(students);

    }
```

输出结果如下：

```java
排序前
student(age=10, grade=0),student(age=30, grade=0),student(age=50, grade=0),student(age=20, grade=0),student(age=40, grade=0),student(age=60, grade=0)
排序后
student(age=10, grade=0),student(age=20, grade=0),student(age=30, grade=0),student(age=40, grade=0),student(age=50, grade=0),student(age=60, grade=0)
```

针对这种复杂的自定义模型排序我们需要模型自己继承和重写compareTo方法，很多人第一次接触该方法或许会感到疑惑，返回值只有三种（1、-1、0）分别代表什么含义？为什么这种纯数字的返回值就可以实现排序呢？



*<u>首先要解答的是为什么返回1、-1、0就可以实现排序？</u>*

答：comparable接口本质上是为类提供的比较接口，可以为类附加比较功能，比较的结果要么相等要么存在大小关系。借助比较结果，第三方就可以实现排序功能，例如Collections.sort(）就是利用二分法实现的排序，stream().sorted()利用的流处理进行排序，无论是哪一种排序归根揭底必须借助comparable接口提供的比较功能。



*<u>其次我们要理解1、-1、0分别代表什么含义？</u>*

上文代码注释中已经有比较详细的解答，大家可以思考和领悟一下。这里做一下简单的叙述：简单来说无论我们希望倒叙排序还是正序排序只需要调整好o.getAge()与this.getAge()的大小关系即可。想象一下有一列数组，o表示数组中的前一位，this代表数组中的后一位，如果我们希望升序排序，只需要设定o.getAge()<this.getAge()时返回1即可，同样若需要降序排序则只需要设定o.getAge()>this.getAge()时返回1即可。说到这里大家应该明白数字的含义了，本质上就是比较结果的标记。



*<u>总结</u>*

我们现在来总结一下Comparable的作用和用法，其作为接口为类提供了内部比较功能，使继承了该接口的类不用去依赖外部的比较器就可以实现同类对象的比较。由于实现了类的比较那么我们可以轻而易举的实现类的排序。

</br>

# Comparator

老样子,想要讲解透彻用法还是要代码举例,我们先把必要的比较器对象创建好,具体如下:

```java

    //创建测试数组
    private List<student> students = Lists.newArrayList();

    //创建内部比较器对象--降序
    Comparator<student> studentComparator_desc = new Comparator<student>() {
        @Override
        public int compare(student o1, student o2) {
            // 入参o1:可以理解为排序队列中的后一个对象,即比较对象
            // 入参o2:理解为排序队列的前一个对象,即被比较对象
            if (o1.getAge() > o2.getAge()) {
                return -1;
            } else if (o1.getAge() < o2.getAge()) {
                //解读:如果后一个对象小于前一个对象,那么符合我们的目标排序规则,则返回1
                //因此我们可以想象一下,前大后小的序列是降序序列,以此我们可以理解升序序列
                return 1;
            } else {
                return 0;
            }
        }

    };


    //创建内部比较器对象--升序
    Comparator<student> studentComparator_asc = new Comparator<student>() {
        @Override
        public int compare(student o1, student o2) {
            // 入参o1:可以理解为排序队列中的后一个对象,即比较对象
            // 入参o2:理解为排序队列的前一个对象,即被比较对象
            if (o1.getAge() > o2.getAge()) {
                //解读:如果后一个对象大于前一个对象,那么符合我们的目标排序规则,则返回1
                //因此我们可以想象一下,前小后大的序列是升序序列,以此我们可以理解降序序列
                return 1;
            } else if (o1.getAge() < o2.getAge()) {
                return -1;
            } else {
                return 0;
            }
        }

    };


    //创建内部比较器对象--升序
    Comparator<student> studentComparator_grade_asc = new Comparator<student>() {
        @Override
        public int compare(student o1, student o2) {
            // 入参o1:可以理解为排序队列中的后一个对象,即比较对象
            // 入参o2:理解为排序队列的前一个对象,即被比较对象
            if (o1.getGrade() > o2.getGrade()) {
                //解读:如果后一个对象大于前一个对象,那么符合我们的目标排序规则,则返回1
                //因此我们可以想象一下,前小后大的序列是升序序列,以此我们可以理解降序序列
                return 1;
            } else if (o1.getGrade() < o2.getGrade()) {
                return -1;
            } else {
                return 0;
            }
        }

    };
    //创建内部比较器对象--降序
    Comparator<Integer> studentComparator_grade_desc = new Comparator<Integer>() {
        @Override
        public int compare(Integer o1, Integer o2) {
            // 入参o1:可以理解为排序队列中的后一个对象,即比较对象
            // 入参o2:理解为排序队列的前一个对象,即被比较对象
            if (o1 < o2) {
                //解读:如果后一个对象大于前一个对象,那么符合我们的目标排序规则,则返回1
                //因此我们可以想象一下,前小后大的序列是升序序列,以此我们可以理解降序序列
                return 1;
            } else if (o1 > o2) {
                return -1;
            } else {
                return 0;
            }
        }

    };
```

接下来我们编写测试代码一窥究竟,先从简单比较方式开始,具体如下:

```java
@Test
    public void publicSortMethod() {
        //初始化测试数组
        students.add(student.builder().age(10).build());
        students.add(student.builder().age(30).build());
        students.add(student.builder().age(50).build());
        students.add(student.builder().age(10).build());
        students.add(student.builder().age(20).build());
        students.add(student.builder().age(40).build());
        students.add(student.builder().age(60).build());

        //排序前
        printUtil.printList(students);

        //排序--通过集合工具类Collections(既是student类已经默认实现comprartable比较器接口,并实现升序,这里也会按照降序比较器排序)
        //Collections.sort(students,studentComparator);

        //排序--通过集合工具类Arrays
        //Arrays.sort();

        //排序--通过stream(既是student类已经默认实现comprartable比较器接口,并实现升序,这里也会按照降序比较器排序)
        students = students.stream().sorted(studentComparator_desc).collect(Collectors.toList());

        //排序--通过stream(当前比较器的反向顺序)
        //students = students.stream().sorted(studentComparator_desc.reversed()).collect(Collectors.toList());

        //排序和排重--通过stream(因为比较器中已经定义两对象相等的情况即return 0,因此可以直接直用distinct())
        //students = students.stream().sorted(studentComparator_desc).distinct().collect(Collectors.toList());

        //排序多比较器--通过stream(这里只是简单举例升序,降序,实际业务中应该是根据不同属性进行不同排序)
        //students = students.stream().sorted(studentComparator_desc).sorted(studentComparator_asc).collect(Collectors.toList());

        //排序后
        printUtil.printList(students);
    }
```

Comparator的高级用法如下:

```java
@Test
    public void otherSortMethod() {
        //初始化测试数组
        students.add(student.builder().age(10).grade(200).build());
        students.add(student.builder().age(30).build());
        students.add(student.builder().age(50).build());
        students.add(student.builder().age(10).build());
        students.add(student.builder().age(20).grade(500).build());
        students.add(student.builder().age(40).build());
        students.add(student.builder().age(60).build());

        //排序前
        printUtil.printList(students);

        //1.thenComparing用法--针对相等情况下使用另一种比较器(年龄相等按照分数升序排序)
        //students = students.stream().sorted(studentComparator_desc.thenComparing(studentComparator_grade_asc)).collect(Collectors.toList());
        //直接利用grade排序
        //students.sort(studentComparator_desc.thenComparing(e->e.getGrade()));
        //利用grade指定比较器排序,注意指定的比较器不在是student对象
        //students.sort(studentComparator_desc.thenComparing(e -> e.getGrade(), studentComparator_grade_desc));

        //2.thenComparingInt实现对int对象的比较,入参为函数表达式
        //students.sort(studentComparator_desc.thenComparingInt(e -> e.getGrade()));

        //3.reversed用法--反向比较器
        //students.sort(studentComparator_desc.reversed());

        //4.naturalOrder用法-按照自然顺序排序(本例中按照age升序排序)
        //students.sort(Comparator.naturalOrder());

        //5.reverseOrder用法-按照反自然顺序排序(本例中按照age降序排序)
        //students.sort(Comparator.reverseOrder());

        //6.nullsFirst与nullsLast用法-解决如果排序的字段为null的情况下这条记录怎么排序的问题。nullsFirst是说将null记录排在最前面，而nullsLast是说将null记录排序在最后面。
        //students.sort(Comparator.nullsLast(studentComparator_desc));

        //7.comparing用法-直接指定用于定义比较规则的比较器对象或者函数
        //students.sort(Comparator.comparing(e -> e.getAge()));
        //students.sort(Comparator.comparing(e -> e.getGrade(), studentComparator_grade_desc));
        students.sort(Comparator.comparingInt(e -> e.getAge()));

        //排序后
        printUtil.printList(students);
    }
```



*<u>总结</u>*

以上两段测试代码已经将Comparator讲解的比较透彻,总的来说Comparator是用来定义比较器对象的,适合原本不支持比较的类对象实现比较功能.对比较器(1,0,-1)的详细解说可参考[1]



</br>

参考:

[1]https://www.cnblogs.com/maozp/p/11153403.html



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


