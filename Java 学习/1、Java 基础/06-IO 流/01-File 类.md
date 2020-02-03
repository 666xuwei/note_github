# 1. File 类

## 1. File 类概述和构造方法

- File 类概述

- - 它是文件和目录路径名的抽象表示
    - 文件和目录是可以通过File封装成对象的
    - 对于File而言，其封装的并不是一个真正存在的文件，仅仅是一个路径名而已。它可以是存在的，也可以
        是不存在的。将来是要通过具体的操作把这个路径的内容转换为具体存在的

- File 类的构造方法

| 方法名                            | 说明                                                        |
| --------------------------------- | ----------------------------------------------------------- |
| File(String pathname)             | 通过将给定的路径名字符串转换为抽象路径名来创建新的 File实例 |
| File(String parent, String child) | 从父路径名字符串和子路径名字符串创建新的 File实例           |
| File(File parent, String child)   | 从父抽象路径名和子路径名字符串创建新的 File实例             |

```
public class FileDemo01 {
    public static void main(String[] args) {
        //File(String pathname)：通过将给定的路径名字符串转换为抽象路径名来创建新的 File实例。
        File f1 = new File("E:\\itcast\\java.txt");
        System.out.println(f1);
        
        //File(String parent, String child)：从父路径名字符串和子路径名字符串创建新的File实例。
        File f2 = new File("E:\\itcast","java.txt");
        System.out.println(f2);
       
        //File(File parent, String child)：从父抽象路径名和子路径名字符串创建新的 File实例。
        File f3 = new File("E:\\itcast");
        File f4 = new File(f3,"java.txt");
        System.out.println(f4);
    }
}
```



## 2. File 类创建功能

- 方法分类

| public boolean createNewFile() | 当具有该名称的文件不存在时，创建一个由该抽象路径名命名的新空 文件 |
| ------------------------------ | ------------------------------------------------------------ |
| public boolean mkdir()         | 创建由此抽象路径名命名的目录                                 |
| public boolean mkdirs()        | 创建由此抽象路径名命名的目录，包括任何必需但不存在的父目录   |

```
public class Demo01 {
    public static void main(String[] args) throws IOException {
        //在D:\\weishao目录下创建一个文件java.txt
        File f1 = new File("D:\\weishao\\java.txt");
        System.out.println(f1.createNewFile());
        System.out.println("------------");

        //在D:\\weishao目录下创建一个多级目录JavaWEB\\HTML
        File f2 = new File("D:\\weishao\\JAVAWEB\\HTML");
        System.out.println(f2.mkdirs());
    }
}
```





## 3. File 类判断和获取功能

- 判断功能

| 方法名                       | 说明                                 |
| ---------------------------- | ------------------------------------ |
| public boolean isDirectory() | 测试此抽象路径名表示的File是否为目录 |
| public boolean isFile()      | 测试此抽象路径名表示的File是否为文件 |
| public boolean exists()      | 测试此抽象路径名表示的File是否存在   |



- 获取功能

| 方法名                          | 说明                                                     |
| ------------------------------- | -------------------------------------------------------- |
| public String getAbsolutePath() | 返回此抽象路径名的绝对路径名字符串                       |
| public String getPath()         | 将此抽象路径名转换为路径名字符串                         |
| public String getName()         | 返回由此抽象路径名表示的文件或目录的名称                 |
| public String[] list()          | 返回此抽象路径名表示的目录中的文件和目录的名称字符串数组 |
| public File[] listFiles()       | 返回此抽象路径名表示的目录中的文件和目录的File对象数组   |



```
public class Demo02 {
    public static void main(String[] args) {
        // 创建一个 File 对象
        File f = new File("D:\\weishao");

        System.out.println(f.isDirectory()); // 是否目录
        System.out.println(f.isFile()); // 是否文件
        System.out.println(f.exists()); // 是否存在

        System.out.println(f.getAbsolutePath()); //返回此抽象路径的绝对路径名字符串
        System.out.println(f.getPath()); // 将此抽象路径名转换为路径名字符串
        System.out.println(f.getName()); //返回由此抽象路径名表示的文件或目录的名字

        File f2 = new File("D:\\weishao");
        String[] strArray = f2.list(); //返回此抽象路径名表示的目录中的文件和目录的名称字符串数组
        for (String s : strArray) {
            System.out.println(s);
        }
        System.out.println("--------------");
        File[] files = f2.listFiles();
        for (File file : files) {
            System.out.println(file.getName());
        }
    }
}
```





## 4. File 类删除功能

| 方法名                  | 说明                               |
| ----------------------- | ---------------------------------- |
| public boolean delete() | 删除由此抽象路径名表示的文件或目录 |



绝对路径和相对路径的区别

- 绝对路径：完整的路径名，不需要任何其他信息就可以定位它所表示的文件。例如：E:\itcast\java.txt
- 相对路径：必须使用取自其他路径名的信息进行解释。例如：myFile\java.txt