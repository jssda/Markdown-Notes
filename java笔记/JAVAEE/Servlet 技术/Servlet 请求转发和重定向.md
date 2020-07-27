# Servlet 请求转发和重定向

## 请求转发

### 特点

1. 降低servlet之间的代码冗余
2. 一次请求转发内的servlet共享此次请求中的request和response对象
3. 浏览器地址栏信息不变, 一共发出了一次请求

### 作用域

一次请求转发内的servlet. request可以作为数据流传输的载体

### 使用

```java
request.getRequestDispatcher("转发的路径").forward(reqest, response);
```

### 实质

其实就是再servlet中调用另一个servlet, 将request对象和response对象传递过去

## 重定向

### 作用

保护第一次请求, 避免因为重复刷新动作导致频繁触发第一次请求.

### 特点

1. 两次请求
2. 浏览器地址信息改变
3. 避免表单重复提交

### 使用

可使用`/项目目录/urlpattern路径`,

也可以直接使用 `urlpattern`路径

```java
response.sendRedirect("路径");

resp.sendRedirect("/lg/RedirectServlet");
// 等同于
resp.sendRedirect("RedirectServlet");
```

### 本质

servlet向浏览器发送信息, 通知浏览器向另外一个servlet再次发送一个请求.