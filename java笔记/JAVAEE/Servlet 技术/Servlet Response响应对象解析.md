# Response 响应对象解析

服务器在调用指定的 Servlet 进行请求处理的时候，会给 Servlet 的方法
传递两个实参 request 和 response。其中 request 中封存了请求相关的请求
数据，而 response 则是用来进行响应的一个对象。 

## Response对象的使用

### 设置响应行

#### 设置相应行的响应状态码

```java
// 设置响应行的状态码
resp.sendError(404);
// 设置对应的状态码
resp.setStatus(200);
```

### 设置响应头

```java
// 设置响应头部信息, 如果响应头部没有这个信息会添加, 如果有, 会修改
resp.setHeader("mouse", "233");
// 添加响应头部信息, 有相同的name的时候,不会覆盖, 会再添加一个
resp.addHeader("mouse", "244");
```

### 设置响应实体

```java
// 取得向浏览器输出实体信息的字符打印流, 并输出
PrintWriter writer = resp.getWriter();
writer.write("test\n");
writer.println("println test");
writer.flush();
```

#### 打印流中print和write的区别

response.getWriter().print(),不仅可以打印输出文本格式的（包括html标签），还可以将一个对象以默认的编码方式转换为二进制字节输出 

response.getWriter().writer(),只能打印输出文本格式的（包括html标签），不可以打印对象

