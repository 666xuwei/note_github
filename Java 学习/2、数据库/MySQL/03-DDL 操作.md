# 3. DDL 操作

## 1. DDL 操作数据库

1. 创建数据库：create database 数据库名 character set 字符集；

> CREATE DATABASE db3 CHARACTER SET gbk;

1. 查看数据库：

1. 1. 查看所有数据库：show databases;
    2. 查看某个定义数据库的定义信息：show create database db3;

1. 修改数据库：

1. 1. 修改数据库默认的字符集：ALTER DATABASE db3 CHARACTER SET utf8;

1. 删除数据库：

1. 1. 删除 db2 数据库：drop database db2;

1. 使用数据库：

1. 1. 查看正在使用的数据库：select database(); 
    2. 使用/切换数据库：use db3;

**面试题：**

在 MySQL 数据库软件中，有如下三个数据库：test1，test2，test3，登录数据库后，输入语句：select database test2; 运行结果是什么?

**答案：这是一条错误的语句，如果要选中一个数据库，应当使用：use test2;**





## 2. DDL 操作表结构

### 2.1 常用的数据类型

![image.png](https://cdn.nlark.com/yuque/0/2019/png/446852/1572859091723-1be4852d-f439-4aca-8deb-e1a0ddb4b9bf.png)

![image.png](https://cdn.nlark.com/yuque/0/2019/png/446852/1572859111055-9420e345-5927-4cce-9fa2-0c19ad7cad64.png)





### 2.2 具体操作

1. 创建 student 表包含 id，name，birthday 字段

> create table student (
>   id int, -- 整数
>
> name varchar(20), -- 字符串
>
> birthday date -- 生日，最后没有逗号
>
> );

1. 查看表：

1. 1. 查看某个数据库中的所有表：show tables;
    2. 查看student 表的结构：desc student;
    3. 查看 student 的创建表 SQL 语句：show create table student;

1. 快速创建一个表结构相同的表：

1. 1. 创建 s1 表，s1 表结构和 student 表结构相同：CREATE TABLE s1 LIKE student;

1. 删除表：

1. 1. 直接删除 s1 表：drop table s1;
    2. 判断表是否存在并删除 s1 表：drop table if exists s1;

1. 修改表结构：

1. 1. 为学生表添加一个新的字段 remark，类型为 varchar(20)：alter table student add remark varchar(20);
    2. 将 student 表中的 remark 字段的改成 varchar(100)：alter table student modify remark varchar(100);
    3. 将 student 表中的 remark 字段名改成 intro，类型为 varchar(30)：ALTER TABLE student CHANGE remark intro VARCHAR(30);
    4. 删除 student 表中的字段 intro：ALTER TABLE student DROP intro;
    5. 将学生表 student 改名为 student2：rename table student to student2;
    6. 将 student2 表的编码修改为 gbk：alter table student2 character set gbk;