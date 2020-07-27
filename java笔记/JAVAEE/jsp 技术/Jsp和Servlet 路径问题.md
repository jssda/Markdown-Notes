# Jsp和Servlet中的路径问题

## 明确两个概念

### 服务器跟路径

服务器根路径就是tomcat服务器下的webapp目录

也可以看作url中不包含部署项目名的跟路径

比如: `localhost:8080/`

### 项目根路径

项目根路径就是tomcat服务器下的webapp目录下的项目目录

也可以看做url中包含部署项目名的根路径

比如: `localhost:8080/myProject/

## 相对路径

相对路径是基于url来讲的. 并不是硬盘中的文件. 比如一个servlet在url中可以直接使用`http://localhost:8080/myProject/myServlet`来进行访问. 但是在硬盘中, 此servlet放在`pers.jssd.servlet`包下. 当使用相对路径的时候, 要参考url中的路径, 不是硬盘存储中的路径.

在servlet和jsp中, 所有的使用到相对路径的地方. 相对的都是自身文件. 

## 绝对路径

使用绝对路径的时候, 需要在访问一个路径的时候, 前边加上`/`

### 请求转发路径

请求转发的绝对路径, `/`表示的是项目根路径

比如: `/OtherServlet`

重定向的绝对路径中, `/`表示的是服务器根路径

比如: `/MyObject/OtherServlet`

在jsp中, 使用超链接标签的绝对路径, `/`表示的是服务器根路径

比如 `/MyObject/OtherServlet`

## 将jsp中超链接定位到项目根路径

```jsp
<%
    String contextPath = request.getContextPath();
    String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+contextPath+"/";
%>

<%--同时, 需要在head标签中, 加上base标签--%>
<head>
    <base href="<%=basePath%>">
</head>
```

