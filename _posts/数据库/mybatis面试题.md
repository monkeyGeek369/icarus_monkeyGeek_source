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

（2）可以使用 XML 或注解来配置模型与数据库表字段的映射

（3）借助statement映射生成最终执行的sql语句并执行

(Statement是指执行SQL语句的对象.MyBatis会自动将SQL语句编译成对应的Statement类型，在执行时MyBatis会将参数绑定到Statement对象中)



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

#{}实现预编译,用?进行替换,执行时再取值,防止sql注入

```
1、数据类型检查:如数值类型不加引号?,字符串类型加上引号‘?’
2、安全检查:若传入的值带有引号则会进行转义,防止sql注入
```



${}是字符串替换,存在sql注入风险



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

**<u>一级缓存</u>**

sqlsession级别的缓存.在操作数据库时需要构建sqlsession对象,对象中有HashMap结构用于存储缓存数据,各个sqlsession中的缓存相互独立.默认开启一级缓存,针对完全相同的两次查询会使用缓存.

```
一级缓存生命周期?
1、正常情况下与sqlsession相同生命周期
2、调用close、clearcache方法时清除缓存
3、sqlsession中执行任何一个update操作时失效(如增、修、删)


如何判断两次查询完全相同?
1、statementid相同
2、sql相同
3、参数相同
```



**<u>二级缓存</u>**

mapper级别的缓存.多个sqlsession操作同一个mapper得到的数据会存在二级缓存,并共享二级缓存.默认不开启二级缓存.

```
1、开启二级缓存后对应的pojo对象必须是可序列化的
2、通过在mapper文件中增加</cache>开启二级缓存
3、文件中的所有select语句将会被缓存
4、任何更新操作将会刷新缓存,如增、修、删
5、缓存会默认使用最近最少使用算法来回收
6、缓存是可读写的,并且能够做到线程隔离
```



</br>

</br>

## **执行结果与目标对象映射**

> 借助mapper中定义的ResultMap实现映射
>
> 包括了结果集中的列与对象属性之间的对应关系，以及将列值映射为Java对象的类型转换器等信息。



</br>

</br>

## **动态sql**

> 根据表达式的值 完成逻辑判断与sql拼接
>
> 1. trim语句
>
> 对sql对开头/结尾进行处理
>
> - prefix: 表示在SQL语句开头添加的前缀；
> - suffix: 表示在SQL语句结尾添加的后缀；
> - suffixOverrides/prefixOverrides: 表示从SQL语句结尾/开头移除指定的字符。
>
> 2. where语句:用于条件追加
>
> 3. set语句:用于赋值,例如在update中
>
> 4. foreach语句
>
> 遍历集合或数组，并动态生成SQL语句
>
> 5. if语句:条件判断
>
> 6. Choose语句:Choose语句相当于SQL中的case语句
>
> 7. when语句:用于选择分支
>
> 8. otherwise语句
>
> Otherwise语句主要用于if语句或choose语句中的默认分支，类似于SQL语句中的else分支
>
> 9. bind语句
>
> 用于将参数值动态地绑定到一个变量上，从而避免在同一个SQL语句中重复使用相同的参数值

```java
<select id="getUserByNameAndAge" resultType="User">  
  <bind name="minAge" value="18" />   // 将参数值动态绑定到minAge变量 
  SELECT * FROM users WHERE name LIKE CONCAT('%', #{name}, '%') AND age &gt;= #{minAge} </select>
```



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


