[TOC]

## 一、历史

MyBatis 是 Apache 的一个开源项目 iBatis，2010 年 6 月这个项目由 Apache Software Foundation 迁移到了 Google Code，接着 iBatis3.x 正式更名为 MyBatis。

iBatis 一词来源于“internet”和“abatis”的组合，是一个基于Java的持久层框架。 iBatis	提供的持久层框架包括SQL Maps和Data Access Objects（DAO）。

## 二、简介

 MyBatis 是**支持定制化 SQL、存储过程以及高级映射**的优秀的**持久层**框架，它**避免了几乎所有的 JDBC 代码和手动设置参数以及获取结果集**。可以使用**简单的 XML 或注解用于配置和原始映射**，**将接口和 Java 的 POJO（Plain Old Java Objects，普通的 Java 对象）映射成数据库中的记录**。 

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