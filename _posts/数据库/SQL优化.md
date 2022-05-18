---
title: SQL优化
category: 数据库
date: 2022-05-18 22:58:43
updated: 2022-05-18 22:58:43
enname: sql-optimize
categories: 数据库
tags:
	- 面试
keywords: 数据库，sql优化，索引，索引失效
permalink:
thumbnail:
---

# 基本概念

- 基数：某个列唯一键（Distinct_Keys）的数量叫作基数。<!--more-->基数的高低影响列的数据分布。 当某个列基数很低，该列数据分布就会不均衡。数据分布不均衡会导致在查询该列的时候容易走全表扫描。一般查询结果占总数据的5%以上就会走全表扫描。
- 选择性：基数与总行数的比值再乘以100%就是某个列的选择性。某列选择性大于20%的时候证明数据分布相对均衡，适合建立索引。
- 回表：当对一个列创建索引之后，索引会包含该列的键值以及键值对应行所在的rowid。通过索引中记录的rowid访问表中的数据就叫回表。回表一般是单块读，回表次数太多会严重影响SQL性能，如果回表次数太多，就不应该走索引扫描了，应该直接走全表扫描。

</br>



# 优化目标

**1、减少IO次数**

通过分析数据库I/O现状来找到影响的数据库操作，在业务层面做拆分，有些频繁读取的内容不一定需要依赖关系型数据库实现。尽可能的使用优化后的单条sql来得到数据结果。

**2、降低CPU计算**

order by，group by，distinct都需要消耗大量CPU。

</br>



# 优化方法-表设计

- 静态表：所有表字段都有固定长度。优点容易缓存，容易修复，查询速度有优势，缺点需要占用更多的磁盘空间。
- 动态表：表存在varchar、blob、text等不定长字段。优点占用磁盘空间少，缺点容易产生磁盘碎片，影响数据库性能和查询性能。
- 压缩表：只读，使用很少的空间，用myisampack工具创建，每条记录分开压缩，所以不能同时访问，可以压缩静态表和动态表。

</br>



# 优化方法-where优化

- 避免null判断
- ！=，<,>尽量避免使用，否则引擎则会进行全表扫描
- 避免在where中使用in、not in
- 避免使用局部变量
- 避免使用表达式
- 避免对字段进行函数操作

</br>



# 优化方法-索引设计

- 单列索引：例如ALTER TABLE people ADD INDEX lname (lname);仅对表中的一个字段设置索引，如果查询条件where中包含索引字段则优先使用索引字段结果集1，在结果集1的基础上回表查询其他where条件字段。
- 多列索引：在mysql中执行查询时，只能使用一个索引，如果我们在lname,fname,age上分别建索引,执行查询时，只能使用一个索引，mysql会选择一个最严格(获得结果集记录数最少)的索引。
- 组合索引：例如以(lname,fname,age)为组合建立的索引。组合索引的好处是使查询条件或者select查询字段尽可能多的涵盖在索引数据中，通过索引直接快速拿到需要的数据而不需要回表。使用组合索引时一定要注意要符合“最左前缀”原则。

</br>



**<u>最左前缀原则</u>**

组合索引的第一个字段必须出现在查询组句中，这个索引才会被用到。

果有一个组合索引(col_a,col_b,col_c)，下面的情况都会用到这个索引：

```sql
col_a = ``"some value"``;
col_a = ``"some value"` `and col_b = ``"some value"``;
col_a = ``"some value"` `and col_b = ``"some value"` `and col_c = ``"some value"``;
col_b = ``"some value"` `and col_a = ``"some value"` `and col_c = ``"some value"``;
```

对于最后一条语句，mysql会自动优化成第三条的样子

下面的情况就不会用到索引：

```sql
col_b = ``"aaaaaa"``;
col_b = ``"aaaa"` `and col_c = ``"cccccc"``;
```

</br>



**<u>索引缺点</u>**

- 降低更新表的速度
- 占用额外磁盘空间

</br>



**<u>索引使用注意事项</u>**

- 索引不会包含有NULL值的列：只要列中包含有NULL值都将不会被包含在索引中，组合索引中只要有一列含有NULL值，那么这一列对于此复合索引就是无效的
- 使用短索引：对串列进行索引，如果在前一定长度的字符内多数值是唯一的，可以指定一个前缀长度。短索引不仅可以提高查询速度而且可以节省磁盘空间和I/O操作。
- 索引列排序：MySQL查询只使用一个索引，因此如果where子句中已经使用了索引的话，那么order by中的列是不会使用索引的。因此数据库默认排序可以符合要求的情况下不要使用排序操作；尽量不要包含多个列的排序，如果需要最好给这些列创建复合索引。

</br>



**<u>索引失效场景</u>**

- 前导模糊查询不能利用索引(like '%XX'或者like '%XX%')
- 组合索引（a,b,c）在使用时where条件中必须包括a,而无所谓a在where中的顺序
- 特殊关联条件如：or、in、not in
- 避免在索引中判断null值（索引无法存储null值，不利于排序）
- 避免函数运算 substring(name,1,3)='abc'
- 避免表达式操作 如where num/2=100



</br>



# 优化方法-其他方面

- IN与EXIST的区别：区分in和exists主要是造成了驱动顺序的改变（这是性能变化的关键），如果是exists，那么以外层表为驱动表，先被访问，如果是IN，那么先执行子查询。所以IN适合于外表大而内表小的情况；EXISTS适合于外表小而内表大的情况
- 复杂多表尽量少用join
- 尽量用join代替子查询
- 尽量少用排序
- 尽量少用OR
- 避免使用 select *，查询字段要精确
- 尽量用union all代替union：差异主要是前者需要将两个(或者多个)结果集合并后再进行唯一性过滤操作，这就会涉及到排序，增加大量的 CPU 运算，加大资源消耗及延迟
- 避免查询字段类型转换
- 避免频繁操作临时表



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


