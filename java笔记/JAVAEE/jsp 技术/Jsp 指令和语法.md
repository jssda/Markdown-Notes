# Jsp 指令和语法

## Page指令

### 作用

对jsp转义的过程进行的配置指令

### 使用

```jsp
<%--配置jsp存储的编码格式, 同时影响response中contentType的编码格式--%>
<%@page pageEncoding="utf-8" %>

<%--配置response中contentType的编码格式. 如果配置了pageEncoding, 则两个编码需要一致--%>
<%@page contentType="text/html; charset=UTF-8" %>

<%--配置jsp中导入的java包. 转成servlet的时候会变成import导入的包--%>
<%@page import="java.util.Random, java.util.Date" %>

<%--jsp继承的java类, 一般不使用. 默认jsp会继承 org.apache.jasper.runtime.HttpJspBase文件--%>
<%--<%@page extends="" %>--%>

<%--设置jsp需要转义成的语言--%>
<%@page language="java" %>

<%--设置session是否开启. 默认true--%>
<%@page session="false" %>

<%--配置如果jsp页面发生异常之后, 需要跳转的页面--%>
<%@page errorPage="web.xml" %>

<%--配置是否开启内置异常--%>
<%@page isErrorPage="false" %>
```

## 局部代码块

局部代码块在jsp解析之后, 会转义到_jspService方法中. 

```jsp
<%
    // 这是一个局部代码块. 会被转义成_jspService方法中的局部代码
    int a = 0;
    System.out.println("a = " + a);
%>
```

## 全局代码块

全局代码块中的代码, 默认转义成转义之后的servlet的全局代码.

```jsp
<%!
    private int age;

    public int getAge() {
        return this.age;
    }
%>

<%
    // 使用局部代码块调用全局定义的方法
    int age1 = getAge();
    System.out.println("age1 = " + age1);
%>
```

## 输出脚本段

<%= 变量或者函数%>

使用标签代替out.print(). 作为一个响应代码

```jsp
<p>这里是调用年龄的时候的输出<%= age1%></p>

<%--
    这里会转义成
    out.write("\r\n");
    out.write("<p>这里是调用年龄的时候的输出");
    out.print( age1);
    out.write("</p>\r\n");
    out.write("\r\n");
--%>
```

## jsp注释

### html注释

```html
<!--这里是一段html注释-->
在jsp中写这段代码, 会作为out.write响应到客户端作为html的注释
```

### css注释

```css
/*
	这里是css注释
*/
与html注释相同, 会被转义并且响应到客户端作为css注释
```

### js注释

与html和css注释相同

### java注释

```jsp
<%
	// 这里是java的注释
	/*java的段落注释*/
%>
会被转义到Servlet中, 但是不会被执行
```

### jsp注释

```jsp
<%--这里是不会被执行的--%>
不会被转义和执行
```

## forward标签

forward标签是jsp中的页面转发标签

### 使用

```jsp
<jsp:forward page="要转发的jsp的相对路径"></jsp:forward>
```

### 特点

从当前jsp跳转到另外一个jsp页面

### 注意

forward标签中处理<jsp:param value="", name=“”/> 子标签以外, 不能出现其他任何内容. 否则会报错