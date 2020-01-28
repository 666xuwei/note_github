---
typora-root-url: images
---

[TOC]

## 一、MyBatis 历史

MyBatis 是 Apache 的一个开源项目 iBatis，2010 年 6 月这个项目由 Apache Software Foundation 迁移到了 Google Code，接着 iBatis3.x 正式更名为 MyBatis。

iBatis 一词来源于“internet”和“abatis”的组合，是一个基于Java的持久层框架。 iBatis	提供的持久层框架包括SQL Maps和Data Access Objects（DAO）





## 二、MyBatis 简介

MyBatis 是**支持定制化 SQL、存储过程以及高级映射**的优秀的**持久层**框架，它**避免了几乎所有的 JDBC 代码和手动设置参数以及获取结果集**。可以使用**简单的 XML 或注解用于配置和原始映射**，**将接口和 Java 的 POJO（Plain Old Java Objects，普通的 Java 对象）映射成数据库中的记录**。

**持久层** 

- 完成持久化工作的代码块 . ----> dao层 【DAO (Data Access Object) 数据访问对象】
- 数据持久化往往也就意味着将内存中的数据保存到磁盘上加以固化，而持久化的实现过程则大多通过各种**关系数据库**来完成。





## 三、为什么要使用 MyBatis

1、JDBC

① SQL 写在 Java 代码块中，耦合度高导致硬编码内伤。

② 维护不易，且实际开发需求中 sql 有变化，频繁修改的情况多见。



2、 MyBatis

**① 对开发人员而言，核心 sql 还是需要自己优化。**

**② sql 和 java 编码分开，功能边界清晰，一个专注业务、一个专注数据。**





## 四、快速入门

### 1、思路流程

搭建环境 --》导入 MyBatis --》 编写代码 --》测试

### 2、代码演示

#### （1）搭建实验数据库

```sql
CREATE DATABASE `mybatis`;

USE `mybatis`;

DROP TABLE IF EXISTS `user`;

CREATE TABLE `user` (
  `id` int(20) NOT NULL,
  `name` varchar(30) DEFAULT NULL,
  `pwd` varchar(30) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

insert  into `user`(`id`,`name`,`pwd`) values (1,'狂神','123456'),(2,'张三','abcdef'),(3,'李四','987654');
```

#### （2）导入 MyBatis 相关 jar 包

这里需要注意：我电脑使用的是 mysql8.0.18，所以这里 mysql 驱动也必须是这样，否则会报错。

```xml
<!-- 导入依赖 -->
<dependencies>
    <!-- mybatis -->
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.5.2</version>
    </dependency>

    <!-- mysql 驱动 -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.18</version>
    </dependency>

    <!-- junit -->
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
    </dependency>
</dependencies>
```

#### （3）编写 MyBatis 核心配置文件

- 这里使用的是 mysql8，所以 url 中也必须加上 serverTimezone 。
- 此处的 Driver 应该导入 com.mysql.cj.jdbc.Driver。

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">

<configuration>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=true&amp;useUnicode=true&amp;characterEncoding=utf8&amp;serverTimezone=Asia/Shanghai"/>
                <property name="username" value="root"/>
                <property name="password" value="123456"/>
            </dataSource>
        </environment>
    </environments>

    <mappers>
        <mapper resource="com/xuwei/dao/UserMapper.xml"/>
    </mappers>

</configuration>
```

#### （4）创建实体类

```java
public class User {

    private int id;  //id
    private String name;   //姓名
    private String pwd;   //密码

    //构造,有参,无参
    //set/get
    //toString()

}
```

#### （5）编写 Mapper 接口类

```java
public interface UserMapper {
    List<User> selectUser();
}
```

#### （6）编写 userMapper.xml 配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.xuwei.dao.UserMapper">
    <select id="selectUser" resultType="com.xuwei.pojo.User">
        select * from user
    </select>
</mapper>
```

#### （7）编写 MyBatis 工具类

```java
public class MyBatisUtils {
    private static SqlSessionFactory sqlSessionFactory;

    static {
        try {
            String resource = "mybatis-config.xml";
            InputStream is = Resources.getResourceAsStream(resource);
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    //获取SqlSession链接
    public static SqlSession getSession() {
        return sqlSessionFactory.openSession();
    }
}
```



#### （8）编写测试类

```java
public class UserMapperTest {
    @Test
    public void selectUser() {
        //1.获取Session对象
        SqlSession session = MyBatisUtils.getSession();

        //2.getMapper
        UserMapper mapper = session.getMapper(UserMapper.class);
        List<User> users = mapper.selectUser();

        for (User user : users) {
            System.out.println(user);
        }

        //3.关闭session
        session.close();
    }
}
```

#### （9）运行测试

![1](/1.png)

#### （10）出现问题解决

maven 由于它的约定大于配置，然后就是我们写的配置文件无法被导出或者生效等问题，解决方案：



```xml
<build>
    <resources>
        <resource>
            <directory>src/main/java</directory>
            <includes>
                <include>**/*.properties</include>
                <include>**/*.xml</include>
            </includes>
            <filtering>false</filtering>
        </resource>
        <resource>
            <directory>src/main/resources</directory>
            <includes>
                <include>**/*.properties</include>
                <include>**/*.xml</include>
            </includes>
            <filtering>false</filtering>
        </resource>
    </resources>
</build>
```





## 五、Mapper 接口开发注意事项

### 1、编写 Mapper 接口

```
public interface UserMapper {
    List<User> selectUser();
}
```



### 2、完成两个绑定

① Mapper 接口 与 Mapper 映射文件的绑定：在 Mapper 映射文件的 <mapper> 标签中的 namespace 必须指定 Mapper 接口的全类名。

② Mapper 映射文件中的增删改查标签的 id 必须与 Mapper 接口中的方法名。



### 3、获取 Mapper 接口的代理实现类对象

```java
public class UserMapperTest {
    @Test
    public void selectUser() {
        //1.获取Session对象
        SqlSession session = MyBatisUtils.getSession();

        //2.getMapper
        UserMapper mapper = session.getMapper(UserMapper.class);
        List<User> users = mapper.selectUser();

        for (User user : users) {
            System.out.println(user);
        }

        //3.关闭session
        session.close();
    }
}
```