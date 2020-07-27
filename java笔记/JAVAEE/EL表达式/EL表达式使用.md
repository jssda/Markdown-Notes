# EL表达式

## 什么是EL表达式

全称: Expression Language. 一种写法非常简洁的表达式. 语法简单易懂, 便于使用. 表达式语言灵感来自于ECMAScript和Xpath表达式语言

## EL表达式的作用

作用: 让JSP书写起来更加简单方便. 简化在 jsp 中获取作用域或者请
求数据的写法。 也会搭配 Jstl 来进行使用。 

## 使用EL表达式

语法结构： ${expression},提供.和[ ]两种运算符来存取数据 

### 1. 使用 EL 表达式获取请求数据

- 获取用户请求数据

  ```jsp
  <%--获取用户请求数据--%>
  ${param.user}
  <br>
  <%--获取多个参数的数据. 类似于request.getParameterValues()--%>
  ${paramValues.fav[0]}
  ```

- 获取请求头数据

  ```jsp
  <%--获取请求头数据--%>
  <%--返回一个键值对数组--%>
  ${header}<br><br><br><br>
  ${header.Accept}<br>
  ${header["Cache-Control"]}<br><br><br>
  ```

- 获取 Cookie 数据

  ```jsp
  <%--获取cookies数据--%>
  <%--返回一个键=Cookie对象的数组--%>
  ${cookie}<br>
  <%--取得指定键名的Cookie对象--%>
  ${cookie.JSESSIONID}<br>
  <%--取得指定键名的Cookie对象的值--%>
  ${cookie.JSESSIONID.value}
  ```

### 2. 使用 EL 表达式获取作用域数据

- 获取作用域数据

  ```jsp
  <%--获取作用域数据, 可以获得pageContext, request, session, applecation--%>
  <%
      pageContext.setAttribute("Hello", "pageContext数据");
      request.setAttribute("Hello", "request数据");
      session.setAttribute("Hello", "Session数据");
      application.setAttribute("Hello", "Application数据");
  %>
  <%--取得了PageContext的数据--%>
  ${Hello} <br><br>
  ```

- 作用域查找顺序

  ```jsp
  <%--不同的作用域取得数据的顺序
  pageContext>request>session>application
  --%>
  ```

- 获取指定作用域中的数据

  ```jsp
  <%--取得指定作用域中的数据--%>
  ${requestScope.Hello} <br><br>
  ${sessionScope.Hello} <br><br>
  ${applicationScope.Hello} <br><br>
  ```

### 3. 使用 EL 表达式进行运算

- 算术运算

  ```jsp
  <%--算数运算, 注意, +在这里只起到了算数运算的作用, 没有字符串拼接的作用.
  如果遇到了字符串, 它会默认对字符串进行number的转换. 转换失败报异常--%>
  ${1+2} <br><br>
  ${1*2} <br><br>
  ${1/2} <br><br>
  ${5-"2"} <br><br>
  ```

- 关系运算

  ```jsp
  <%--关系运算, 支持大于, 小于, 大于等于, 小于等于, 等于. 同时还支持三目运算符--%>
  ${1 > 2} <br><br>
  ${1 > 2 ? "大于" : "不大于"} <br><br>
  ```

- 逻辑运算

  ```jsp
  <%--逻辑运算--%>
  ${true&&false} <br><br>
  ${1>2 || 2<3} <br><br>
  ```

### 4. EL 表达式空值判断 empty

```jsp
<h2>EL 表达式空值运算</h2>
<%
    String ss = "";
    pageContext.setAttribute("ss", ss);
    User user = null;
    pageContext.setAttribute("user", user);
    List list = new ArrayList();
    pageContext.setAttribute("list", list);
%>
<%--如果一个字符串为空, 或者对象为null, 或者一个空集合, empty都会返回true--%>
${empty ss} <br><br>
${empty user} <br><br>
${empty list} <br><br>
```

