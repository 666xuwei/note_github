Beta请打开“钉钉”扫码重要文档！重要文档！重要文档！重要文档！领蛋孵福蛋继续赢取大奖奖品分享任务列表去完成已完成热门去完成水印[WIP] 为什么是语雀aboutNEWNEW

Adblocker



# jsp

## 1. 概述

JSP 中文名叫 java 服务器页面，其根本时一个简化的 Servlet 设计。

**特点：**

- 本质上还是 servlet。
- 跨平台，一次编写处处运行。
- 组件跨平台
- 健壮性和安全性



请求了一个jsp，其实先执行的时jsp.servlet，将执行的jsp文件转译为servlet文件，然后访问这个文件。



![图片.png](../imgs/1573988476045-406d5bb7-6a46-4b21-b35b-558da1ba23a3.png)







## 2. JSP 的三种注释

- 前端语言注释： 会被转译，也会被发送，但是不会被浏览器执行
- java语言注释： 会被转译，但是不会被servlet执行
- Jsp注释： 不会被转译。







## 3. Jsp 的 page 指令学习

- <%@page  属性名="属性值" 属性名="属性值"...%>
- language：声明jsp要被转译的语言。
- import:声明转译的java文件要导入的包，不同的包使用逗号隔开。
- pageEncoding:设置jsp文件的数据编码格式。
- contentType="text/html; charset=utf-8"  设置jsp数据响应给浏览器时，浏览器的解析和编码格式。
- session:设置转译的servlet中是否开启session支持,默认开启，true表示开启。false表示关闭。
- errorPage:设置jsp运行错误跳转的页面.
- extends:设置jsp转译的java文件要继承的父类(包名+类名)。	

作用：

配置jsp文件的转译相关的参数。



```
<%@ page language="java" import="java.util.*,java.lang.*" pageEncoding="utf-8" contentType="text/html; charset=utf-8"%>
<%@page session="true" %>
<%@page errorPage="error.jsp"  %>
<%@page isErrorPage="true" %>
```







## 3. Jsp 的局部代码块和全局代码块

### 3.1 局部代码块

**特点**：

局部代码块中声明的java代码会被原样转译到jsp对应的servlet文件的_JspService方法中

代码块中声明的变量都是局部变量。

**使用**：

<% java代码  %>

**缺点**：

使用局部代码块在jsp中进行逻辑判断，书写麻烦，阅读困难。

**开发**：

servlet进行请求逻辑处理，使用jsp进行页面展现。





### 3.2 全局代码块

**特点**：

声明的java代码作为全局代码转译到对应的servlet类中。

**使用**：

<%! 全局代码 %>

**注意**：

全局代码块声明的代码，需要使用局部代码块调用。







## 4. Jsp 的脚本段语句

**特点**：

帮助我们快速的获取变量或者方法的返回值作为数据响应给浏览器。

**使用**：

<%=变量名或者方法%>

**注意**：

不要在变量名或者方法后使用分号。	

**位置**：

除jsp语法要求以外的任意位置。







## 5. Jsp 的静态引入和动态引入

**静态引入**：

<%@include  file="要引入的jsp文件的相对路径" %>

**特点**：

会将引入的jsp文件和当前jsp文件转译成一个java(Servlet)文件使用。

在网页中也就显示了合并后的显示效果。

注意：

静态引入的jsp文化不会单独转译成java(Servlet)文件。

当前文件和静态引入的jsp文件中不能够使用java代码块声明同名变量。

**动态引入**：

<jsp:include page="要引入的jsp文件的相对路径"></jsp:include>

**特点**：

会将引入的jsp文件单独转译，在当前文件转译好的java文件中调用引入的jsp文件的转译文件。

在网页中显示合并后的显示效果。

**注意**：

动态引入允许文件中声明同名变量。







## 6. Jsp 的转发标签 forward

**使用**：			

<jsp:forward page="要转发的jsp文件的相对路径"></jsp:forward>

**特点**：

一次请求

地址栏信息不改变。

**注意**：

在转发标签的两个标签中间除了写<jsp:param name="str" value="aaa" />子标签不会报错，其他任意字符都会报错。

<jsp:param name="str" value="aaa" />

name属性为附带的数据的键名

 value为附带的数据内容

注意：会将数据以？的形式拼接在转发路径的后面。







## 7. Jsp 的九大内置对象

**内置对象**：  

 jsp文件在转译成其对应的Servlet文件的时候自动生成的并声明的对象。我们在jsp页面中直接使用即可。

**注意**：  内置对象在jsp页面中使用，使用局部代码块或者脚本段语句来使用。不能够在全局代码块中使用。

**内容**：九个对象  

- pageContext:页面上下文对象，封存了其他内置对象。封存了当前jsp的运行信息。     

- - 注意：每个Jsp文件单独拥有一个pageContext对象。     
    - 作用域：当前页面。  

- request：封存当前请求数据的对象。由tomcat服务器创建。一次请求  
- session:此对象用来存储用户的不同请求的共享数据的。一次会话  
- application：也就是ServletContext对象，一个项目只有一个。存储用户共享数据的对象，以及完成其他操作。项目内  
- response:响应对象，用来响应请求处理结果给浏览器的对象。设置响应头，重定向。  
- out:响应对象，Jsp内部使用。带有缓冲区的响应对象，效率高于response对象。  
- page:代表当前Jsp的对象。相当于java中的this。  
- exception：异常对象。存储了当前运行的异常信息。    

- - 注意：使用此对象需要在page指定中使用属性isErrorPage="true"开启。  

- config：也就是ServletConfig，主要是用来获取web.xml中的配置数据，完成一些初始化数据的读取。







## 8. 四个作用域对象

- pageContext:当前页面.解决了在当前页面内的数据共享问题。获取其他内置对象。
- request:一次请求。一次请求的servlet的数据共享。通过请求转发，将数据流转给下一个servlet。
- session:一次会话.一个用户的不同请求的数据共享。将数据从一次请求流转给其他请求。
- application:项目内.不同用户的数据共享问题。将数据从一个用户流转给其他用户。

**作用：**  **数据流转**

**
**



**
**

## 9. Jsp 的路径

在jsp中资源路径可以使用相对路径完成跳转，但是：

问题一：资源的位置不可随意更改。

问题二：需要使用../进行文件夹的跳出。使用比较麻烦。

**使用绝对路径：(必须会)**

/虚拟项目名/项目资源路径

例如：

```
<a href="/jsp/jspPro.jsp">jspPro.jsp</a>
<a href="/jsp/a/a.jsp">a.jsp</a><br />
```



注意：在jsp中资源的第一个/表示的是服务器根目录，相当于:localhost:8080



**使用jsp中自带的全局路径声明：**



```
<%
String path = request.getContextPath();
String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
%>
    
<html>
    <head>
        <base href="<%=basePath%>"> <!-- 写在head里 -->
```



作用：

给资源前面添加项目路径：		http://127.0.0.1:8080/虚拟项目名/上		