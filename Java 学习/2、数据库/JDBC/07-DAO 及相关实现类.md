# 7. DAO 及相关实现类

- DAO：Data Access Object访问数据信息的类和接口，包括了对数据的CRUD（Create、Retrival、Update、Delete），而不包含任何业务相关的信息。有时也称作：BaseDAO
- 作用：为了实现功能的模块化，更有利于代码的维护和升级

## 1. 表和 JavaBean

![image.png](https://cdn.nlark.com/yuque/0/2019/png/446852/1573107029433-f5887d70-7c5e-4bb3-9fff-72c8aee284c7.png)





## 2. 实现对 t_department 和 t_employee 的增删查改



**JDBCUtiles：**

```
package com.xuwei2.druid;

import com.alibaba.druid.pool.DruidDataSourceFactory;

import javax.sql.DataSource;
import java.io.IOException;
import java.sql.Connection;
import java.sql.SQLException;
import java.util.Properties;

public class JDBCUtils {
    private static Connection conn;
    private static DataSource ds;
    private static ThreadLocal<Connection> local; //使用ThreadLocal来保存同一个对象,共享局部变量
    private static Properties pro = new Properties();

    static {
        try {
            //把src下建一个druid.properties文件的数据，加载到一个properties对象里
            //加载，读取 jdbc.properties 配置的信息
            pro.load(JDBCUtils.class.getClassLoader().getResourceAsStream("druid.properties"));

            //创建池子
            ds = DruidDataSourceFactory.createDataSource(pro);
            local = new ThreadLocal<Connection>();

        } catch (IOException e) {
            e.printStackTrace();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    ////使用ThreadLocal来保存同一个对象
    public static Connection getConnection() throws SQLException {
        conn = local.get(); //如果从local中能够得到一个连接对象，说明当前线程已经拿过了
        if (conn == null) {
            conn = ds.getConnection();
            local.set(conn);
        }
        return conn;
    }

    //提供关闭连接的方式
    public static void free() {
        conn = local.get();
        try {
            if (conn != null) {
                local.remove();
                conn.setAutoCommit(false); //还原我们的连接为自动提交，等别人再次拿到，默认自动提交.
                conn.close();
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```



 **DAO接口：**

```
public interface DepartmentDao {
    void add(Department department);
    void update(Department department);
    void deleteByDid(int did);
    ArrayList<Department> getAll();
}

public interface EmployeeDao {
    void addEmployee(Employee emp);
    void update(Employee emp);
    void delete(int eid);
    ArrayList<Employee> getAllEmplyee();
}
```



**Bean类：**

```
public class Department {
    private int did;
    private String dname;
    private String description;
    
    //get，set，toString，无参，满参构造省略
}


/**
 * 以后的JavaBean中使用包装类
 * 因为数据库中所有的类型，包括整数等类型可能都是null，而java中只有引用数据类型才会产生null
 */
public class Employee {
    private Integer eid;
    private String ename;
    private String tel;
    private String gender;
    private Double salary;
    private Double commissionPct; //驼峰法
    private Date birthday;
    private Date hiredate;
    private Integer jobId;
    private String email;
    private Integer mid;
    private String address;
    private String nativePlace;
    private Integer did;
}
```



**BasicDAO:**

```
/**
 * 把各个DAO的实现类的相同的代码提取出来
 */
public class BasicDAO {

    //适用于update，insert，delete方法
    public int update(String sql, Object... args) throws Exception {
        //1.获取连接
        Connection conn = JDBCUtils.getConnection();

        //2.创建PrepareStatement
        PreparedStatement pst = conn.prepareStatement(sql);

        //3.设置？的值
        if (args.length > 0 && args != null) {
            for (int i = 0; i < args.length; i++) {
                pst.setObject(i + 1, args[i]);
            }
        }

        //4.执行语句
        int len = pst.executeUpdate();

        //5.关闭
        pst.close();
        JDBCUtils.free();

        //6.返回结果
        return len;

    }

    //查询所有 ,T可能代表Department，Employee等各种Javabean的对象
    public <T> ArrayList<T> getAll(Class<T> clazz, String sql, Object... args) throws Exception {
        //1.获取连接
        Connection conn = JDBCUtils.getConnection();

        //2.创建PrepareStatement
        PreparedStatement pst = conn.prepareStatement(sql);

        //3.设置？的值
        if (args.length > 0 && args != null) {
            for (int i = 0; i < args.length; i++) {
                pst.setObject(i + 1, args[i]);
            }
        }

        //4.执行查询
        ResultSet rs = pst.executeQuery();
        ArrayList<T> list = new ArrayList<>();

        //5.获取结果集的元数据对象，该对象有对结果集的数据进行描述的相关信息
        ResultSetMetaData rsmd = rs.getMetaData();
        //(1)获取结果集的列数
        int count = rsmd.getColumnCount();

        //6.把ResultSet结果集中的数据封装到一个一个JavaBean中，然后把JavaBean存储到集合中
        while (rs.next()) {
            //(2)获取 JavaBean 对象
            T obj = clazz.newInstance();
            //为 obj 的每一个属性赋值，有几列就代表着有几个属性
            for (int i = 0; i < count; i++) {
                //通过反射获取属性的值
                //(3)从结果集的元数据对象中获取第几列的字段名
                String columnName = rsmd.getColumnLabel(i + 1);
                //(4)通过字段名，可以获取属性对象
                Field field = clazz.getDeclaredField(columnName);
                //(5)设置属性可以被访问
                field.setAccessible(true);
                //(6)设置属性的值
                field.set(obj, rs.getObject(i+1)); //从结果集获取i+1的值赋值给属性
            }
            //7.将当前对象添加到集合中
            list.add(obj);
        }
        //8.关闭
        pst.close();
        JDBCUtils.free();

        //9.返回结果
        return list;
    }
}
```



**DAO接口实现类：**



```
public class DepartmentDaoImpl extends BasicDAO implements DepartmentDao{
    @Override
    public void add(Department department) {
        try {
            String sql = "insert into t_department values(null,?,?)";
            update(sql, department.getDname(), department.getDescription());
        } catch (Exception e) {
            throw new RuntimeException(e.getMessage());
        }
    }

    @Override
    public void update(Department department) {
        try {
            String sql = "update t_department set dname=?,description=? where did = ?";
            update(sql, department.getDname(), department.getDescription(), department.getDid());
        } catch (Exception e) {
            throw new RuntimeException(e.getMessage());
        }
    }

    @Override
    public void deleteByDid(int did) {
        try {
            String sql = "delete from t_department where did = ?";
            update(sql, did);
        } catch (Exception e) {
            throw new RuntimeException(e.getMessage());
        }
    }

    @Override
    public ArrayList<Department> getAll() {
        String sql = "select * from t_department";
        try {
            ArrayList<Department> all = getAll(Department.class, sql);
            return all;
        } catch (Exception e) {
            throw new RuntimeException(e.getMessage());
        }

    }
}


public class EmployeeDaoImpl extends BasicDAO implements EmployeeDao{
    @Override
    public void addEmployee(Employee emp) {
        String sql = "insert into t_employee values(null,?,?,?,?,?,?,?,?,?,?,?,?,?)";
        try {
            update(sql,emp.getEname(),
                    emp.getTel(),
                    emp.getGender(),
                    emp.getSalary(),
                    emp.getCommissionPct(),
                    emp.getBirthday(),
                    emp.getHiredate(),
                    emp.getJobId(),
                    emp.getEmail(),
                    emp.getMid(),
                    emp.getAddress(),
                    emp.getNativePlace(),
                    emp.getDid());
        } catch (Exception e) {
            throw new RuntimeException(e.getMessage());
        }
    }

    @Override
    public void update(Employee emp) {

    }

    @Override
    public void delete(int eid) {

    }

    @Override
    public ArrayList<Employee> getAllEmplyee() {
        //如果字段名和属性名对不上，可以取别名
        String sql = "select eid,ename,tel,gender,salary,commission_pct as commissionPct,birthday,hiredate,job_id as jobId,email,mid,address,native_place as nativePlace,did from t_employee";
        try {
            ArrayList<Employee> all = getAll(Employee.class, sql);
            return all;
        } catch (Exception e) {
            throw new RuntimeException(e.getMessage());
        }
    }
}
```



**测试** :



```
public class TestDepartmentDaoImpl {
    public static void main(String[] args) {
        //1.键盘输入
        Scanner input = new Scanner(System.in);

//        System.out.println("部门名称：");
//        String name = input.nextLine();
//
//        System.out.println("部门简介：");
//        String desc = input.nextLine();

        System.out.println("输入需要删除的部门id：");
        int did = input.nextInt();


        //通过DepartmentDAOImpl操作数据库
        DepartmentDaoImpl dao = new DepartmentDaoImpl();

        //1.修改      成功
//        Department dept = new Department(0, name, desc);
//        dao.add(dept);

        //2.删除  通过did
//        dao.deleteByDid(did);

        //3.查询所有 成功
        ArrayList<Department> all = dao.getAll();
        for (Department department:all) {
            System.out.println(department);
        }
        System.out.println("添加成功");
    }
}



public class TestEmployeeDaoImpl {
    public static void main(String[] args) {
        EmployeeDaoImpl dao = new EmployeeDaoImpl();
        ArrayList<Employee> all = dao.getAllEmplyee();
        for (Employee employee:all) {
            System.out.println(employee);
        }
    }
}
```