---
title: mybatis面试题
category: 数据库
date: 2022-12-30 23:23:13
updated: 2022-12-30 23:23:13
enname: mybatis-interview
categories: 数据库
tags:
	- 面试
keywords: mybatis、面试
permalink:
thumbnail:
---

## **什么是Mybatis**

（1）一个半ORM（对象关系映射）框架<!--more-->，它内部封装了JDBC，加载驱动、创建连接等繁杂的过程。

（2）可以使用 XML 或注解来配置和映射原生信息，将 POJO映射成数据库中的记录

（3）通过xml 文件或注解的方式将要执行的各种 statement 配置起来，并通过java对象和 statement中sql的动态参数进行映射生成最终执行的sql语句，最后由mybatis框架执行sql并将结果映射为java对象并返回。（从执行sql到返回result的过程）



</br>

</br>

## 优缺点

（1）优点：

> ① 使用sql灵活度高，且与代码解耦。
>
> ② 消除了JDBC大量冗余的代码
>
> ③ 很好的与各种数据库兼容
>
> ④ 能够与Spring很好的集成
>
> ⑤ 提供映射标签，支持对象与数据库的ORM字段关系映射

（2）缺点：

> ① SQL语句的编写工作量较大
>
> ② SQL语句依赖于数据库，不便于迁移



</br>

</br>

### **#{}和${}的区别是什么**

${}是字符串替换，#{}是预处理；

Mybatis在处理${}时，就是把${}直接替换成变量的值。而Mybatis在处理#{}时，会对sql语句进行预处理，将sql中的#{}替换为?号，调用PreparedStatement的set方法来赋值；

使用#{}可以有效的防止SQL注入，提高系统安全性。



</br>

</br>

## **不同的Xml映射文件，id是否可以重复**

> 不同的Xml映射文件，如果配置了namespace，那么id可以重复；如果没有配置namespace，那么id不能重复；
>
> 原因就是namespace+id是作为Map的key使用的，如果没有namespace，就剩下id，那么，id重复会导致数据互相覆盖。有了namespace，自然id就可以重复，namespace不同，namespace+id自然也就不同。
>
> 备注：在旧版本的Mybatis中，namespace是可选的，不过新版本的namespace已经是必须的了。



</br>

</br>

## **分页插件的原理是什么**

>  Mybatis使用RowBounds对象进行分页，它是针对ResultSet结果集执行的内存分页，而非物理分页。
>
> 可以在sql内直接书写带有物理分页的参数来完成物理分页功能，也可以使用分页插件来完成物理分页。
>
> ​    分页插件的基本原理是使用Mybatis提供的插件接口，实现自定义插件，在插件的拦截方法内拦截待执行的sql，然后重写sql



</br>

</br>

## **一级、二级缓存**

> （1）一级缓存: 基于 PerpetualCache 的 HashMap 本地缓存，其存储作用域为 Session，当 Session flush 或 close 之后，该 Session 中的所有 Cache 就将清空，默认打开一级缓存。
>
> （2）二级缓存与一级缓存其机制相同，默认也是采用 PerpetualCache，HashMap 存储，不同在于其存储作用域为 Mapper(Namespace)，并且可自定义存储源，如 Ehcache。默认不打开二级缓存，要开启二级缓存，使用二级缓存属性类需要实现Serializable序列化接口(可用来保存对象的状态),可在它的映射文件中配置 ；
>
> （3）对于缓存数据更新机制，当某一个作用域(一级缓存 Session/二级缓存Namespaces)的进行了C/U/D 操作后，默认该作用域下所有 select 中的缓存将被 clear 掉并重新更新，如果开启了二级缓存，则只根据配置判断是否刷新。



</br>

</br>

## **执行结果与目标对象映射**

> 第一种是使用标签，逐一定义数据库列名和对象属性名之间的映射关系。
>
> 第二种是使用sql列的别名功能，将列的别名书写为对象属性名。
>
> 有了列名与属性名的映射关系后，Mybatis通过反射创建对象，同时使用反射给对象的属性逐一赋值并返回，那些找不到映射关系的属性，是无法完成赋值的。



</br>

</br>

## **动态sql**

> 执行原理是根据表达式的值 完成逻辑判断 并动态拼接sql的功能
>
> Mybatis提供了9种动态sql标签：trim | where | set | foreach | if | choose | when | otherwise | bind。



</br>

</br>

## **使用MyBatis的mapper接口调用时有哪些要求**

> Mapper接口方法名和mapper.xml中定义的每个sql的id相同；
> Mapper接口方法的输入参数类型和mapper.xml中定义的每个sql 的parameterType的类型相同；
> Mapper接口方法的输出参数类型和mapper.xml中定义的每个sql的resultType的类型相同；
> Mapper.xml文件中的namespace即是mapper接口的类路径。



## 笔试手写题

### **模糊查询like语句该怎么写?**

第1种：在Java代码中添加sql通配符。

```xml
    string wildcardname = “%smi%”;
    list<name> names = mapper.selectlike(wildcardname);
 
    <select id=”selectlike”>
     select * from foo where bar like #{value}
    </select>
```

第2种：在sql语句中拼接通配符，会引起sql注入

```xml
    string wildcardname = “smi”;
    list<name> names = mapper.selectlike(wildcardname);
 
    <select id=”selectlike”>
         select * from foo where bar like "%"${value}"%"
    </select>
```

</br>

</br>

### **当实体类中的属性名和表中的字段名不一样 ，怎么办 ？**

第一种：通过在查询的sql语句中定义字段名的别名，让字段名的别名和实体类的属性名一致。

```xml
    <select id=”selectorder” parametertype=”int” resultetype=”me.gacl.domain.order”>
       select order_id id, order_no orderno ,order_price price form orders where order_id=#{id};
    </select>
```

第2种： 通过来映射字段名和实体类属性名的一一对应的关系。

```xml
 <select id="getOrder" parameterType="int" resultMap="orderresultmap">
        select * from orders where order_id=#{id}
    </select>
 
   <resultMap type=”me.gacl.domain.order” id=”orderresultmap”>
        <!–用id属性来映射主键字段–>
        <id property=”id” column=”order_id”>
 
        <!–用result属性来映射非主键字段，property为实体类属性名，column为数据表中的属性–>
        <result property = “orderno” column =”order_no”/>
        <result property=”price” column=”order_price” />
    </reslutMap>
```

</br>

</br>

### **在mapper中如何传递多个参数?**

（1）第一种：

```xml
//DAO层的函数
Public UserselectUser(String name,String area);  
//对应的xml,#{0}代表接收的是dao层中的第一个参数，#{1}代表dao层中第二参数，更多参数一致往后加即可。
<select id="selectUser"resultMap="BaseResultMap">  
    select *  fromuser_user_t   whereuser_name = #{0} anduser_area=#{1}  
</select>  
```


（2）第二种： 使用 @param 注解

```xml
public interface usermapper {
   user selectuser(@param(“username”) string username,@param(“hashedpassword”) string hashedpassword);
}
//然后,就可以在xml像下面这样使用(推荐封装为一个map,作为单个参数传递给mapper):
<select id=”selectuser” resulttype=”user”>
         select id, username, hashedpassword
         from some_table
         where username = #{username}
         and hashedpassword = #{hashedpassword}
</select>
```



（3）第三种：多个参数封装成map

```java
try{
//映射文件的命名空间.SQL片段的ID，就可以调用对应的映射文件中的SQL
//由于我们的参数超过了两个，而方法中只有一个Object参数收集，因此我们使用Map集合来装载我们的参数
Map<String, Object> map = new HashMap();
     map.put("start", start);
     map.put("end", end);
     return sqlSession.selectList("StudentID.pagination", map);
 }catch(Exception e){
     e.printStackTrace();
     sqlSession.rollback();
    throw e; }
finally{
 MybatisUtil.closeSqlSession();
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


