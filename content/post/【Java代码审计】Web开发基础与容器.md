---
title: 【Java代码审计】Web开发基础与容器
date: 2025-10-28
categories:
  - 代码审计
image: images/110.webp
---
# 0x00 Tomcat下载
下载地址：
```
https://tomcat.apache.org/download-90.cgi
```
选择版本下载即可：
我这里使用的是java1.8版本，就直接使用Tomcat9版本了。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251014131737.png)
# 0x01 Tomcat的使用
第一步：
新建项目，选择Maven Archetype：
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251014132010.png)
第二步：
这里Archetype选择`org.apache.maven.archetypes:maven-archetype-webapp`
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251014132153.png)
第三步：
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251014132936.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251014134634.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251014134744.png)
这里会有个警告：没有为部署标记工作，点击修复。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251014134840.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251014134921.png)
直接启动即可。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251014134941.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251014135031.png)
默认端口：8080
可以看到这里目录是Demo05_war，这里也可以修改。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251014135124.png)
可以在这里进行修改。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251014135225.png)
删了之后，就会发现后面没有`Demo05_war`了。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251014135324.png)
即可！！！
# 0x02 maven介绍
Maven是一款服务于Java平台的自动化构建工具，Maven作为Java项目管理工具。
maven是java的管理工具
配置文件：`pom.xml`
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251014132634.png)
内容：
```java
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">  
  <modelVersion>4.0.0</modelVersion>  
  <groupId>org.example</groupId>  
  <artifactId>Demo05</artifactId>  
  <packaging>war</packaging>  
  <version>1.0-SNAPSHOT</version>  
  <name>Demo05 Maven Webapp</name>  
  <url>http://maven.apache.org</url>  
  <dependencies>    <dependency>      <groupId>junit</groupId>  
      <artifactId>junit</artifactId>  
      <version>3.8.1</version>  
      <scope>test</scope>  
    </dependency>  </dependencies>  <build>    <finalName>Demo05</finalName>  
  </build></project>
```
管理包的地方
```java
<dependencies>  
  <dependency>    
	<groupId>junit</groupId>  
    <artifactId>junit</artifactId>  
    <version>3.8.1</version>  
    <scope>test</scope>  
  </dependency>
  </dependencies>
```
# 0x03 配置源
如果项目创建比较慢的话，可以更换一下源：
win下的路径：
```java
maven路径：D:\soft\Jeboss\IDEA\IntelliJ IDEA 2025.1.2\plugins\maven\lib\maven3\conf
配置文件maven3\con\fsettings.xml
```
源：
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251014141224.png)
可以改成这里的阿里源即可。
```
    <mirror>
      <id>aliyumaven</id>
      <mirrorOf>e*</mirrorOf>
      <name>阿里云公共仓库</name>
      <url>http://maven.aliyun.com/repository/public</url>
    </mirror>
    </mirrors>
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251014141441.png)
这里就可以下载的很快了。
# 0x04 Web的使用
## Web的展示页面
web的展示页面是在/webapp/下
默认展示的是`index.jsp`文件
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251014141745.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251014141755.png)
这里一个简单的web页面就完成了。
# Servlet生命周期和API
这里下载API
```
https://jarcasting.com/artifacts/jakarta.servlet/jakarta.servlet-api/4.0.4/
```
导入API，新建一个libs，然后导入这个文件即可。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251014150551.png)
这里使用
```
jakarta.servlet-api  4.0.4版本
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251014150513.png)
右键main，创建目录，java目录即可。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251014150719.png)
创建核心文件：
java里有一个核心规范：创建的名称是域名倒着写的比如：
```
com.slimer.HelloServlet
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251014150914.png)
## 路由的第一种方式
```
这里使用注解的方式进行访问。
@WebServlet("/hello")
```
这里我创建一个Get输出流，可以直接通过Get请求进行访问：
```
http://localhost:8080/hello
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251014151653.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251014151616.png)
源码；
```java
package com.slimer;

// 导入Servlet核心异常类：处理Servlet运行中可能出现的异常（如请求处理失败）
import javax.servlet.ServletException;
// 导入注解式Servlet配置类：通过@WebServlet注解直接配置Servlet映射路径，无需在web.xml中配置
import javax.servlet.annotation.WebServlet;
// 导入HttpServlet抽象类：所有处理HTTP请求的Servlet都需继承此类，它封装了HTTP请求的通用处理逻辑
import javax.servlet.http.HttpServlet;
// 导入HTTP请求对象类：封装客户端发送的HTTP请求信息（如请求参数、请求头、请求方法等）
import javax.servlet.http.HttpServletRequest;
// 导入HTTP响应对象类：封装服务器要返回给客户端的HTTP响应信息（如响应头、响应体、响应状态码等）
import javax.servlet.http.HttpServletResponse;
// 导入IO异常类：处理输入输出操作（如流读写）可能出现的异常
import java.io.IOException;
// 导入字符输出流类：用于向客户端输出字符型响应内容（如HTML、文本等）
import java.io.PrintWriter;

/**
 * 自定义HelloServlet类：继承HttpServlet，处理客户端发送的HTTP GET和POST请求
 * @WebServlet("/hello")：注解配置Servlet的访问路径，客户端可通过 "项目上下文路径/hello" 访问当前Servlet
 * 例如：若项目部署后上下文路径为/Demo05，则完整访问路径为 http://localhost:8080/Demo05/hello
 */
@WebServlet("/hello")
public class HelloServlet extends HttpServlet {

    /**
     * 重写doGet方法：专门处理客户端发送的HTTP GET请求
     * 当客户端通过GET方式访问@WebServlet指定的路径时，此方法会被自动调用
     * 
     * @param req  HttpServletRequest对象：包含客户端发送的所有请求信息，如请求参数、请求URL等
     * @param resp HttpServletResponse对象：用于构建并返回服务器的响应信息给客户端
     * @throws ServletException  Servlet运行时异常（如请求处理逻辑错误）
     * @throws IOException       IO异常（如流读写失败、响应输出异常等）
     */
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 1. 配置响应信息：设置响应内容的MIME类型为text/html（表示返回HTML页面），字符编码为UTF-8（防止中文乱码）
        // 注意：setContentType需在获取输出流（PrintWriter）之前调用，否则编码配置不生效
        resp.setContentType("text/html;charset=utf-8");

        // 2. 获取字符输出流：通过HttpServletResponse的getWriter()方法获取PrintWriter对象
        // 此流用于将服务器的响应内容（如HTML标签、文本）写入响应体，最终返回给客户端
        PrintWriter out = resp.getWriter();

        // 3. 向客户端输出响应内容：通过out.println()写入HTML标签，客户端浏览器会解析为对应的页面元素
        // 此处输出<h1>标题标签，浏览器渲染后会显示一级标题"Get输出流"
        out.println("<h1>Get输出流</h1>");
        
        // 可选：关闭输出流（虽Servlet容器会自动关闭，但显式关闭可释放资源，避免内存泄漏）
        out.close();
    }

    /**
     * 重写doPost方法：专门处理客户端发送的HTTP POST请求
     * 当客户端通过POST方式（如表单提交method="post"）访问当前Servlet时，此方法会被自动调用
     * POST请求相较于GET请求更安全（参数不暴露在URL中），适合传输敏感数据或大量数据
     * 
     * @param req  HttpServletRequest对象：获取POST请求中的参数、请求体等信息
     * @param resp HttpServletResponse对象：构建POST请求的响应内容
     * @throws ServletException  Servlet运行时异常
     * @throws IOException       IO异常
     */
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 1. 处理POST请求的中文乱码：POST请求参数默认编码为ISO-8859-1，需手动设置为UTF-8
        // 注意：此配置仅对POST请求的参数有效，且需在获取请求参数（如req.getParameter()）之前调用
        req.setCharacterEncoding("utf-8");

        // 2. 配置响应的MIME类型和字符编码（同doGet，防止响应中文乱码）
        resp.setContentType("text/html;charset=utf-8");

        // 3. 获取输出流，向客户端返回POST请求的处理结果
        PrintWriter out = resp.getWriter();
        
        // 示例1：直接输出POST请求的处理提示
        out.println("<h1>Post请求处理成功！</h1>");
        
        // 示例2：获取POST请求中的参数（假设客户端提交了名为"username"的表单字段）
        String username = req.getParameter("username"); // 通过参数名获取请求值
        // 输出参数值到响应页面（若客户端未传该参数，username会为null，可添加非空判断优化）
        out.println("<p>客户端提交的用户名：" + (username == null ? "未提交" : username) + "</p>");
        
        // 关闭输出流，释放资源
        out.close();
    }
}
```
这里改成POST请求，就变成POST的内容了。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251014152218.png)
## 路由的第二种方式
**web.xml配置**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251014152753.png)
配置web的路由需要有两步：
**第一步**:注册Servlet
```java
<servlet>  
  //定义名称，且唯一标识  
  <servlet-name>HelloServlet</servlet-name>  
  <servlet-class>com.slimer.HelloServlet</servlet-class>  
</servlet>
```
**第二步**：使用映射访问路径。
```java
<servlet-mapping>  
  <servlet-name>HelloServlet</servlet-name>  
  <url-pattern>/hello</url-pattern>  
</servlet-mapping>
```
代码：
```xml
<!DOCTYPE web-app PUBLIC
 "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
 "http://java.sun.com/dtd/web-app_2_3.dtd" >

<!-- 
  <web-app>：根标签，所有Web应用的配置都必须包含在该标签内
  是Web应用部署描述符的核心，用于定义Servlet、过滤器、监听器、初始化参数等
-->
<web-app>
  
  <!-- 
    <display-name>：定义Web应用的显示名称
    作用：在服务器管理界面（如Tomcat Manager）中显示的应用名称，方便识别
  -->
  <display-name>Archetype Created Web Application</display-name>

  <!-- 
    <servlet>：Servlet的定义标签，用于注册一个Servlet类到Web容器
    每个Servlet都需要在此处声明，告知容器该Servlet的名称和对应的Java类路径
  -->
  <servlet>
    <!-- 
      <servlet-name>：定义当前Servlet的唯一名称（自定义）
      要求：在整个web.xml中必须唯一，用于和<servlet-mapping>关联映射路径
    -->
    <servlet-name>HelloServlet</servlet-name>
    
    <!-- 
      <servlet-class>：指定Servlet类的全限定名（包名+类名）
      作用：告知Web容器该Servlet对应的Java类位置，容器会通过反射创建该类的实例
      此处对应前面定义的com.slimer.HelloServlet类
    -->
    <servlet-class>com.slimer.HelloServlet</servlet-class>
  </servlet>

  <!-- 
    <servlet-mapping>：Servlet的映射标签，用于将Servlet与访问路径关联
    作用：定义客户端通过哪个URL路径可以访问到对应的Servlet
  -->
  <servlet-mapping>
    <!-- 
      <servlet-name>：关联的Servlet名称，必须与<servlet>标签中定义的<servlet-name>一致
      以此建立Servlet定义和映射路径的对应关系
    -->
    <servlet-name>HelloServlet</servlet-name>
    
    <!-- 
      <url-pattern>：定义访问当前Servlet的URL路径
      作用：客户端通过"项目上下文路径 + 此路径"访问Servlet
      示例：若项目上下文路径为/Demo05，则完整访问路径为http://localhost:8080/hello
      注意：路径以"/"开头，表示相对于Web应用的根目录
    -->
    <url-pattern>/hello</url-pattern>
  </servlet-mapping>

</web-app>
```
这样就可以使用/hello进行访问了。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251014153604.png)
## 请求和响应
**GET请求：**
输出内容：
```
请求信息： 请求方法GET URL/hello 请求头Header:Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/140.0.0.0 Safari/537.36 查询字符null 客户端IP:0:0:0:0:0:0:0:1
```
```java
package com.slimer;  
import javax.servlet.ServletException;  
import javax.servlet.http.HttpServlet;  
import javax.servlet.http.HttpServletRequest;  
import javax.servlet.http.HttpServletResponse;  
import java.io.IOException;  
import java.io.PrintWriter;  
public class HelloServlet extends HttpServlet {  
    @Override  
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
  
        //如果我们要处理一个get id参数时  
        String method = req.getMethod();     //请求方法
        String uri = req.getRequestURI();       //请求URL
        String query = req.getQueryString();  
        String header = req.getHeader("User-Agent");    //请求头
        String ip = req.getRemoteAddr();      //客户端IP
  
        //设置响应类型为HTML  
        resp.setContentType("text/html;charset=utf-8");  
  
        //获取该输出流  
        PrintWriter out = resp.getWriter();  
  
        out.println("请求信息：");  
        out.println("请求方法"+method);  
        out.println("URL"+uri);  
        out.println("请求头Header:"+header);  
        out.println("查询字符"+query);  
        out.println("客户端IP:"+ip);  
    }  
}
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251017152104.png)
请求处理结果：
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251017153434.png)
可以看到这里的（id=1）为kv键值对。
那么怎么获取所有参数呢？
那么我们可以直接使用Map进行遍历即可。
代码：
```java
//获取所有参数  
Map<String,String[]> map =  req.getParameterMap();  
//遍历  
for(Map.Entry<String,String[]> entry:map.entrySet()){  
    String key = entry.getKey();  
    String[] value = entry.getValue();  
    out.println("<br>Key"+":"+ Arrays.toString(value));  
    out.println("<br>Value"+Arrays.toString(value));
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251017153958.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251017154721.png)
## 会话管理（Cookie和Session）
请求都是一次的，http没有记忆的。
通过Session获取name
代码：
```java
package com.slimer;  
import javax.servlet.ServletException;  
import javax.servlet.http.HttpServlet;  
import javax.servlet.http.HttpServletRequest;  
import javax.servlet.http.HttpServletResponse;  
import javax.servlet.http.HttpSession;  
import java.io.IOException;  
  
public class HelloServlet extends HttpServlet {  
    @Override  
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        //身份认证  
        //sesssion 身份信息  
        HttpSession session = req.getSession();  
        //获取name  
        String name = (String)session.getAttribute("name");  
  
        resp.setContentType("text/html;charset=utf-8");  
        resp.getWriter().write("name:" + name);  
  
    }  
}
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251017155433.png)
因为现在没有session,所以获取到的值是null。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251017155456.png)
那么我们怎么设置session呢？
代码如下：
```java
package com.slimer;  
import javax.servlet.ServletException;  
import javax.servlet.http.HttpServlet;  
import javax.servlet.http.HttpServletRequest;  
import javax.servlet.http.HttpServletResponse;  
import javax.servlet.http.HttpSession;  
import java.io.IOException;  
  
public class HelloServlet extends HttpServlet {  
    @Override  
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        //身份认证  
        //sesssion 身份信息  
        HttpSession session = req.getSession();  
        String username = req.getParameter("username");  
        //设置session  
        session.setAttribute("name", username);  
        //获取name  
        String name = (String)session.getAttribute("name");  
  
        resp.setContentType("text/html;charset=utf-8");  
        resp.getWriter().write("name:" + name);  
    }  
}
```
访问链接：
```java
http://localhost:8080/hello?username=1123
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251017161745.png)
看一下请求数据包，发现这里服务器自动给我设置了一个Set-Cookie。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251017162114.png)
那么我们写个案例，用来获取Session。
# 案例
我们先创建一个简单的登录口。
在webapp下的WEB-INF下创建一个`login.jsp`文件
代码：
```jsp
<%--  
  Created by IntelliJ IDEA.  User: 11251  Date: 2025/10/17  Time: 16:35  To change this template use File | Settings | File Templates.--%>  
<%@ page contentType="text/html;charset=UTF-8" language="java" %>  
<html>  
<head>  
    <title>Title</title>  
</head>  
<body>  
<form action="${pageContext.request.contextPath}/login" method="post">  
    username:<input name="username">  
    pwd:<input type="password" name="pwd">  
    <button>登录</button>  
</form>  
</body>  
</html>
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251017163750.png)
在src-main-java-com.slimer下创建一个`LoginServlet.java`文件
```java
package com.slimer;  
  
import javax.servlet.ServletException;  
import javax.servlet.annotation.WebServlet;  
import javax.servlet.http.HttpServlet;  
import javax.servlet.http.HttpServletRequest;  
import javax.servlet.http.HttpServletResponse;  
import java.io.IOException;  
import java.io.PrintWriter;  
import java.util.Objects;  
  
//创建登录接口  
@WebServlet("/login")  
public class LoginServlet extends HttpServlet {  
  
        @Override  
        protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
            //1.获取username 和pwd 进行校验  
            String username = req.getParameter("username");  
            String pwd = req.getParameter("pwd");  
  
            resp.setContentType("text/html;charset=utf-8");  
            PrintWriter out = resp.getWriter();  
  
            if (Objects.equals(username, "admin") && "123456".equals(pwd)) {  
                //登录成功  
                //设置session  
                req.getSession().setAttribute("username", username);  
                out.println("登录成功");  
            }else  {  
                out.println("用户名或者密码错误");  
            }  
        }  
}
```
在src-main-java-com.slimer下创建一个`helloServlet.java`文件
```java
package com.slimer;  
import javax.servlet.ServletException;  
import javax.servlet.http.HttpServlet;  
import javax.servlet.http.HttpServletRequest;  
import javax.servlet.http.HttpServletResponse;  
import javax.servlet.http.HttpSession;  
import java.io.IOException;  
import java.io.PrintWriter;  
  
public class HelloServlet extends HttpServlet {  
    @Override  
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        HttpSession session = req.getSession();  
  
        String username = (String) session.getAttribute("username");  
  
        resp.setContentType("text/html;charset=utf-8");  
        PrintWriter out = resp.getWriter();  
  
        if (username == null) {  
            //表示没有登录  
            out.println("未授权");  
        }else{  
            out.println(username+"登录成功");  
        }  
  
        }  
    }
```
成功创建一个登录口：
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251028140812.png)
输入正常的账号密码：admin/123456，则输出：
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251028140831.png)
输入错误的，则：
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251028140900.png)