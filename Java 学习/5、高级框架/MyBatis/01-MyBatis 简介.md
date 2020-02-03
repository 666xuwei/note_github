[TOC]

## 一、历史

MyBatis 是 Apache 的一个开源项目 iBatis，2010 年 6 月这个项目由 Apache Software Foundation 迁移到了 Google Code，接着 iBatis3.x 正式更名为 MyBatis。

iBatis 一词来源于“internet”和“abatis”的组合，是一个基于Java的持久层框架。 iBatis	提供的持久层框架包括SQL Maps和Data Access Objects（DAO）。

## 二、简介

1. MyBatis 是一个持久层的框架，它内部封装了 JDBC，开发时只需要关注 SQL 语句本身，不需要花费精力去处理加载驱动、创建连接、创建 statement  等繁杂的过程。程序员直接编写原生态 sql，可以严格控制 sql 执行性能，灵活度高。

2. MyBatis 可以使用 XML 或注解来配置和映射原生信息，将 POJO(普通的 Java 对象) 映射成数据库中的记录，避免了几乎所有的 JDBC 代码和手动设置参数以及获取结果集。

3. 通过 xml 文件或注解的方式将要执行的各种 statement 配置起来，并通过 java 对象和 statement 中sql 的动态参数进行映射生成最终执行的 sql 语句，最后由 mybatis 框架执行 sql 并将结果映射为 java 对象并返回。（从执行 sql 到返回 result 的过程）。

持久层：

- 完成持久化工作的代码块 . ----> dao层 【DAO (Data Access Object) 数据访问对象】 
- 数据持久化往往也就意味着将内存中的数据保存到磁盘上加以固化，而持久化的实现过程则大多通过各种**关系数据库**来完成。 

## 三、为什么要使用 MyBatis？

1、JDBC

① SQL 写在 Java 代码块中，耦合度高导致硬编码内伤。

② 维护不易，且实际开发需求中 sql 有变化，频繁修改的情况多见。

2、MyBatis

**① 对开发人员而言，核心 sql 还是需要自己优化。**

**② sql 和 java 编码分开，功能边界清晰，一个专注业务、一个专注数据。**