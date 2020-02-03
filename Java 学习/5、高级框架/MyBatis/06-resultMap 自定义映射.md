[TOC]

## 一、简述

自定义 result，实现高级结果集映射。

- id：用于完成主键值的映射。
- result：用于完成普通列的映射。
- association：一个复杂的类型关联，许多结果将包成这种类型。
- collection：复杂类型的集。

## 二、查询为 null 的问题

### 1、问题的来源

1. Java 中的实体类中的某一个属性和数据库中的列名不一致；

```java
public class User {

    private int id;  //id
    private String name;   //姓名
    private String password;   //密码和数据库不一样！
    
    //构造
    //set/get
    //toString()
}
```

2. 测试的时候发现 password 这一块为 null；

### 2、分析问题

- `select * from user where id = #{id}` 可以看做 select id,name,pwd from user where id = #{id} ；
- mybatis 会根据这些查询的列名(会将列名转化为小写,数据库不区分大小写) , 去对应的实体类中查找相应列名的 set 方法设值 , 由于找不到setPwd() , 所以 password 返回null ; 【自动映射】

### 3、解决方案一：使用别名

```xml
<select id="selectUserById" resultType="User">
    select id , name , pwd as password from user where id = #{id}
</select>
```

### 4、解决方案二：使用 resultMap

```xml
<resultMap id="UserMap" type="User">
    <!-- id为主键 -->
    <id column="id" property="id"/>
    <!-- column是数据库表的列名 , property是对应实体类的属性名 -->
    <result column="name" property="name"/>
    <result column="pwd" property="password"/>
</resultMap>

<select id="selectUserById" resultMap="UserMap">
    select id , name , pwd from user where id = #{id}
</select>
```

## 三、自动映射

ResultMap 的设计思想是，对于简单的语句根本不需要配置显式的结果映射，而对于复杂一点的语句只需要描述它们的关系就行了。

你已经见过简单映射语句的示例了，但并没有显式指定 `resultMap`。比如：

```xml
<select id="selectUserById" resultType="map">
    select id , name , pwd 
    from user 
    where id = #{id}
</select>
```

上述语句只是简单地将所有的列映射到 `HashMap` 的键上，这由 `resultType` 属性指定。虽然在大部分情况下都够用，但是 HashMap 不是一个很好的模型。你的程序更可能会使用 JavaBean 或 POJO（Plain Old Java Objects，普通老式 Java 对象）作为模型。

## 四、手动映射

1. 返回值类型为 resultMap

```xml
<select id="selectUserById" resultMap="UserMap">
	select id , name , pwd from user where id = #{id}
</select>
```

2. 编写 resultMap，实现手动映射

```xml
<resultMap id="UserMap" type="User">
  <!-- id为主键 -->
  <id column="id" property="id"/>
  <!-- column是数据库表的列名 , property是对应实体类的属性名 -->
  <result column="name" property="name"/>
  <result column="pwd" property="password"/>
</resultMap>
```

