Beta请打开“钉钉”扫码重要文档！重要文档！重要文档！重要文档！领蛋孵福蛋继续赢取大奖奖品分享任务列表去完成已完成热门去完成水印[WIP] 为什么是语雀aboutNEWNEW

Adblocker



# 4. DML 操作表中的数据

## 1. 插入记录

1. 插入所有的列，向学生表中：

> insert into student (id,name,age,sex) values (1, '孙悟空', 20, '男');
> insert into student (id,name,age,sex) values (2, '孙悟天', 16, '男');

1. 如果只插入部分列，必须写列名：

> insert into student values (3, '孙悟饭', 18, '男');





## 2. DOS 命令窗口操作数据乱码问题的解决

1. 乱码产生的原因：

![image.png](https://cdn.nlark.com/yuque/0/2019/png/446852/1572864306588-2cee8530-bf80-4d60-a995-d11fae707fde.png)

1. 解决方案

修改 client、 connection、 results 的编码为 GBK，保证和 DOS 命令行编码保持一致

![image.png](https://cdn.nlark.com/yuque/0/2019/png/446852/1572864341299-310f5aa8-b61c-4434-8a69-acbe0c304296.png)

- 同时设置三项：set names gbk;

- - 退出 DOS 命令就失效了。





## 3. 蠕虫复制

将一张已经存在的表中的数据复制到另一张表中。

**具体操作** ：

1. 创建 student2 表，结构必须和 student 表一样。
2. 将 student 表中的数据添加到 student2 表中
3. 如果只想复制 student 表中 name,age 字段数据到 student2 表中，两张表都写出相应的列名

> create table student2 like student;
>
> insert into student2 select * from student;
>
> insert into student2 (name,age) select name,age from student;





## 4. 更新表记录

1. 不带条件修改数据，将所有的性别改成女：update student set sex='女';
2. 带条件修改数据，将 id 号为 2 的学生性别改成男: update student set sex='男' where id=2;
3. 一次修改多个列，把 id 为 3 的学生，年龄改成 26 岁， address 改成北京

> update student set age=26, address='北京' where id=3;





## 5. 删除表记录

1. 带条件删除数据，删除 id 为 1 的记录：delete from student where id=1;
2. 不带条件删除数据,删除表中的所有数据：delete from student;
3. truncate 和 delete 的区别？







![威](https://cdn.nlark.com/yuque/0/2019/jpeg/anonymous/1565659847119-9cad566e-1fa5-4f06-825d-441f5fef5dc7.jpeg?x-oss-process=image%2Fresize%2Cm_fill%2Cw_64%2Ch_64%2Fformat%2Cpng)

![img](../../imgs/1f44d-1580746433340.svg)![img](../../imgs/1f44e-1580746433340.svg)![img](https://gw.alipayobjects.com/a/g/lark/twemoji/2.3.6/2764.svg)![img](https://gw.alipayobjects.com/a/g/lark/twemoji/2.3.6/1f600.svg)![img](https://gw.alipayobjects.com/a/g/lark/twemoji/2.3.6/1f602.svg)![img](https://gw.alipayobjects.com/a/g/lark/twemoji/2.3.6/1f62e.svg)![img](https://gw.alipayobjects.com/a/g/lark/twemoji/2.3.6/1f625.svg)![img](https://gw.alipayobjects.com/a/g/lark/twemoji/2.3.6/1f621.svg)![img](https://gw.alipayobjects.com/a/g/lark/twemoji/2.3.6/1f44f.svg)![img](https://gw.alipayobjects.com/a/g/lark/twemoji/2.3.6/1f389.svg)

正文

正文标题 1标题 2标题 3标题 4



回复

十万阿里人都在用的笔记与文档知识库

[关于语雀](https://www.yuque.com/yuque/help/about)[使用帮助](https://www.yuque.com/help)[数据安全](https://www.yuque.com/about/security)[服务协议](https://www.yuque.com/terms)[English](https://www.yuque.com/u277073/javase/cy2ylo?language=en-us)