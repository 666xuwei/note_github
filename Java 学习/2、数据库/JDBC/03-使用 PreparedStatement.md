# 3. 使用 PreparedStatement 实现 CRUD 操作



## 1. 操作和访问数据库

- 数据库连接被用于向数据库服务器发送命令和 SQL 语句，并接受数据库服务器返回的结果。其实一个数据库连
    接就是一个Socket连接
- 在 java.sql 包中有 3 个接口分别定义了对数据库的调用的不同方式：

- - Statement：用于执行静态 SQL 语句并返回它所生成结果的对象。
    - PrepatedStatement：SQL 语句被预编译并存储在此对象中，可以使用此对象多次高效地执行该语句。
    - CallableStatement：用于执行 SQL 存储过程

![image.png](https://cdn.nlark.com/yuque/0/2019/png/446852/1573024307042-63be7cfe-ad35-4285-ad20-6248bf1ec65b.png)



## 2. 使用 Statement 操作数据表的弊端

- 通过调用 Connection 对象的 createStatement() 方法创建该对象。该对象用于执行静态的 SQL 语句，并且返
    回执行结果。
- Statement 接口中定义了下列方法用于执行 SQL 语句：

> int excuteUpdate(String sql)：执行更新操作INSERT、UPDATE、DELETE
> ResultSet executeQuery(String sql)：执行查询操作SELECT1 2

- 但是使用 Statement 操作数据表存在弊端

- - 问题一：存在拼串操作，繁琐
    - 问题二：存在 SQL 注入问题

- SQL 注入是利用某些系统没有对用户输入的数据进行充分的检查，而在用户输入数据中注入非法的 SQL 语句段
    或命令(如：SELECT user, password FROM user_table WHERE user='a' OR 1 = ' AND password = ' OR '1' =
    '1') ，从而利用系统的 SQL 引擎完成恶意行为的做法。
- 对于 Java 而言，要防范 SQL 注入，只要用 PreparedStatement(从Statement扩展而来) 取代 Statement 就可
    以了。



**就不演示了，麻烦**

![image.png](https://cdn.nlark.com/yuque/0/2019/png/446852/1573024917501-4765e97a-f0c4-4bf1-b711-6dab7750ec87.png)





## 3. PrepareStatement 的使用

### 3.1 PrepareStatement 介绍

- 可以通过调用 Connection 对象的 **preparedStatement(String sql)** 方法获取 PreparedStatement 对象
- **PreparedStatement** **接口是** **Statement** **的子接口，它表示一条预编译过的** **SQL** **语句**
- PreparedStatement 对象所代表的 SQL 语句中的参数用问号(?)来表示，调用 PreparedStatement 对象的setXxx() 方法来设置这些参数. setXxx() 方法有两个参数，第一个参数是要设置的 SQL 语句中的参数的索引(从 1开始)，第二个是设置的 SQL 语句中的参数的值



### 3.2 PrepareStatement vs Statement

- 代码的可读性和可维护性更好
- PreparedStatement 能最大可能提高性能：

- - DBServer会对**预编译**语句提供性能优化。因为预编译语句有可能被重复调用，
    - 

- PreparedStatement 可以防止 SQL 注入

### 3.3 Java 与 SQL 对应数据类型转换表

![image.png](https://cdn.nlark.com/yuque/0/2019/png/446852/1573025115994-d79b6648-5c4d-4df2-907d-94bb7b391f81.png)



### 3.4 使用 PreparedStatement 实现增、删、改操作



```
public class Demo05 {
    public static void main(String[] args) {
        Connection conn = null;
        PreparedStatement ps = null;
        try {
            //1.加载驱动
            Class.forName("com.mysql.jdbc.Driver");
            //2.注册驱动
            conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/test", "root", "123456");
            //3.编写sql语句
            String sql = "insert into student values(?,?,?)";
            //4.获取PreparedStatement的实例
            ps = conn.prepareStatement(sql);
            //5.填充占位符
            ps.setObject(1, 2);
            ps.setObject(2, "威少");
            ps.setObject(3, 23);
            //6.执行sql语句
            ps.executeUpdate();
        } catch (ClassNotFoundException | SQLException e) {
            e.printStackTrace();
        } finally {
            try {
                ps.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
            try {
                conn.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }
}
```



### 3.5 使用PreparedStatement实现查询操作



```
package com.xuwei.prepared;

import java.sql.*;

public class Demo05 {
    public static void main(String[] args) {
        Connection conn = null;
        PreparedStatement ps = null;
        try {
            //1.加载驱动
            Class.forName("com.mysql.jdbc.Driver");
            //2.注册驱动
            conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/test", "root", "123456");
            //3.编写sql语句
            String sql = "select * from student";
            //4.获取PreparedStatement的实例
            ps = conn.prepareStatement(sql);
            //5.执行executeQuery()，得到结果集：ResultSet
            ResultSet rs = ps.executeQuery();
            //6.得到结果集的元数据： ResultSetMetaData
            ResultSetMetaData rsmd = rs.getMetaData();
            System.out.println(rsmd); //获取该表有哪些列，列名是什么

            //6.通过ResultSetMetaData得到columnCount,columnLabel；通过ResultSet得到列值
            int columeCount = rsmd.getColumnCount(); //获取ResultSet中有多少列
            String columnLabel = rsmd.getColumnLabel(3); //获取每一列的别名

            while (rs.next()) {
                int id = rs.getInt("id");
                String name = rs.getString("name");
                int age = rs.getInt("age");
                System.out.print(id + "\t" + name + "\t" + age);
                System.out.println();
            }

        } catch (ClassNotFoundException | SQLException e) {
            e.printStackTrace();
        } finally {
            try {
                ps.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
            try {
                conn.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }
}
```



> 说明：使用PreparedStatement实现的查询操作可以替换Statement实现的查询操作，解决Statement拼串和SQL注入问题。





## 4. ResultSet与ResultSetMetaData

### 4.1 ResultSet

- 查询需要调用PreparedStatement 的 executeQuery() 方法，查询结果是一个ResultSet 对象
- ResultSet 对象以逻辑表格的形式封装了执行数据库操作的结果集，ResultSet 接口由数据库厂商提供实现
- ResultSet 返回的实际上就是一张数据表。有一个指针指向数据表的第一条记录的前面。
- ResultSet 对象维护了一个指向当前数据行的**游标**，初始的时候，游标在第一行之前，可以通过 ResultSet 对象
    的 next() 方法移动到下一行。调用 next()方法检测下一行是否有效。若有效，该方法返回 true，且指针下移。
    相当于Iterator对象的 hasNext() 和 next() 方法的结合体
- 当指针指向一行时, 可以通过调用 getXxx(int index) 或 getXxx(int columnName) 获取每一列的值

- - 例如: getInt(1), getString("name")
    - 注意：Java与数据库交互涉及到的相关Java API中的索引都从1开始。



### 4.2 ResultSetMetaData

- 可用于获取关于 ResultSet 对象中列的类型和属性信息的对象
- ResultSetMetaData meta = rs.getMetaData();

- - getColumnName(int column)：获取指定列的名称
    - getColumnLabel(int column)：获取指定列的别名
    - getColumnCount()：返回当前 ResultSet 对象中的列数。
    - getColumnTypeName(int column)：检索指定列的数据库特定的类型名称。
    - getColumnDisplaySize(int column)：指示指定列的最大标准宽度，以字符为单位。
    - isNullable(int column)：指示指定列中的值是否可以为 null。
    - isAutoIncrement(int column)：指示是否自动为指定列进行编号，这样这些列仍然是只读的

![image.png](https://cdn.nlark.com/yuque/0/2019/png/446852/1573026180768-ee30ef8a-e41f-4853-89bd-a9710f3933da.png)



问题1：得到结果集后, 如何知道该结果集中有哪些列 ？ 列名是什么？

**需要使用一个描述** **ResultSet** **的对象， 即** **ResultSetMetaData**

问题2：关于ResultSetMetaData

1. **如何获取** **ResultSetMetaData****： 调用** **ResultSet** **的** **getMetaData()** **方法即可**
2. **获取** **ResultSet** **中有多少列****：调用** **ResultSetMetaData** **的** **getColumnCount()** **方法**
3. **获取** **ResultSet** **每一列的列的别名是什么****：调用** **ResultSetMetaData** **的****getColumnLabel()** **方法**



## 5. 资源的释放

- 释放ResultSet, Statement,Connection。
- 数据库连接（Connection）是非常稀有的资源，用完后必须马上释放，如果Connection不能及时正确的关闭将
    导致系统宕机。Connection的使用原则是**尽量晚创建，尽量早的释放。**
- 可以在finally中关闭，保证及时其他代码出现异常，资源也一定能被关闭。



## 6. JDBC API 小结

- 两种思想

- - 面向接口编程的思想
    - ORM思想(object relational mapping)

- - - 一个数据表对应一个java类
        - 表中的一条记录对应java类的一个对象
        - 表中的一个字段对应java类的一个属性

sql是需要结合列名和表的属性名来写。注意起别名。

- 两种技术

- - JDBC结果集的元数据：ResultSetMetaData

- - - 获取列数：getColumnCount()
        - 获取列的别名：getColumnLabel()

- - 通过反射，创建指定类的对象，获取指定的属性并赋值