---
title: java8-Optional教程
category: java基础
date: 2020-06-16 20:07:17
updated: 2020-06-16 20:07:17
enname: Optional
categories: java基础
tags:
	- 面试
keywords: java,java8新特性,Optional简介,Optional教程
permalink:
thumbnail:
---

# 简介

Optional作为对象容器可以容纳空/非空对象.Optional对象作为值对象,在使用等价比较\hash code获取\同步操作等时可能会出现不可预知的异常,应该避免使用.<!--more-->

Optional的出现主要是起到两点作用:

第一,其可以有效的解决空指针异常问题,通过内置isPresent方法来判断容器内是否有值,通过else等内置方式可针对空/异常情况做特殊处理.

第二,极大的促进了函数式编程的发展,我们在使用lambda表达式上不仅有Stream的选择,也可以借助Optional实现.

</br>

注:本文以java8为例进行讲解,java9或更高版本有功能的扩展,本文知识点弄清后对扩展功能将更容易接受.

</br>

# 方法介绍



| 内置方法                                                     | 方法简介                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| public static <T> Optional<T> of(T value)                    | 设置值为value                                                |
| public static <T> Optional<T> ofNullable(T value)            | 设置值为可空value                                            |
| public T get()                                               | 获取值                                                       |
| public boolean isPresent()                                   | 值是否为空                                                   |
| public void ifPresent(Consumer<? super T> consumer)          | 如果值非空则对值进行处理                                     |
| public Optional<T> filter(Predicate<? super T> predicate)    | 如果值非空且匹配lambda表达式则返回Optional,否则返回空,若不匹配则返回当前Optional |
| public<U> Optional<U> map(Function<? super T, ? extends U> mapper) | 根据lambda重新映射值,返回值自动封装                          |
| public<U> Optional<U> flatMap(Function<? super T, Optional<U>> mapper) | 根据lambda重新映射值,返回值需手动封装                        |
| public T orElse(T other)                                     | 如果值非空则返回值,否则返回指定对象                          |
| public T orElseGet(Supplier<? extends T> other)              | 如果值非空则返回值,否则返回lambda表达式执行结果              |
| public <X extends Throwable> T orElseThrow(Supplier<? extends X> exceptionSupplier) | 如果值非空则返回值,否则抛出由lambda表达式执行结果所指定的异常对象 |



</br>

# 核心方法案例

***ifPresent***

```java
    @Test
    public void ifPresentTest() {
        Student student = new Student(20, 90);

        //如果value(值)不为空则通过lambda处理值,否则不进行任何处理
        Optional<Student> optionalStudent = Optional.of(student);
        optionalStudent.ifPresent(item -> {
            int grade = item.getAge() > 20 ? item.getGrade() + 5 : item.getGrade() - 1;
            item.setGrade(grade);
        });
        System.out.println(optionalStudent);
    }
```

</br>

***filter***

```java
    @Test
    public void filterTest() {
        Student student = new Student(20, 90);

        //如果value(值)不为空则可实现过滤,否则返回原Optional
        Optional<Student> optionalStudent = Optional.of(student).filter(item -> item.getGrade() > 90);
        System.out.println(optionalStudent);
    }
```

</br>

***map***

```java
    @Test
    public void mapTest() {
        Student student = new Student(20, 90);

        //如果value(值)不为空则可借助lambda表达式实现map处理,否则返回Optional.empty
        Optional<Student> optionalStudent = Optional.of(student).map(item -> new Student(item.getGrade(), item.getAge()));
        System.out.println(optionalStudent);
    }
```

</br>

***flatMap***

```java
    @Test
    public void flatMapTest() {
        Student student = new Student(20, 90);

        //flatMap的返回结果需要自行封装成Optional,如果value为空则返回Optional.empty
        //Optional<Student> optionalStudent = Optional.of(student).flatMap(item -> Optional.of(new Student(item.getGrade(), item.getAge())));
        //System.out.println(optionalStudent);

        Student student2 = new Student(22, 92);
        Student student3 = new Student(23, 93);
        List<Student> students = Lists.newArrayList();
        students.add(student);
        students.add(student2);
        students.add(student3);

        Optional<List<Student>> optionalStudent = Optional.of(students).flatMap(item -> Optional.of(this.flatMapTest2(item)));
        System.out.println("结果输出");
        System.out.println(optionalStudent);
    }

    public List<Student> flatMapTest2(List<Student> students) {
        System.out.println("flatMapTest2输出");
        System.out.println(students);
        return students;
    }
```

</br>

***orElseThrow***

```java
    @Test
    public void orElseThrowTest() {
        Student student = null;
        //如果容器值存在则返回容器值,否则将根据lambda表达式抛出异常对象
        try {
            //Student optionalStudent = Optional.ofNullable(student).orElseThrow(IllegalStateException::new);
            //Student optionalStudent = Optional.ofNullable(student).orElseThrow(Exception::new);
            Student optionalStudent = Optional.ofNullable(student).orElseThrow(() -> new Exception("错误"));
        } catch (Exception e) {
            System.out.println(e);
        }
    }
```

</br>

相关资料:

[1]测试代码https://github.com/monkeyGeek369/demoTest

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


