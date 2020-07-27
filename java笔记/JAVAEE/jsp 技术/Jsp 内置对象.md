# Jsp中的九大内置对象

## 概念

jsp在转译成其对应的java文件(也就是Servlet)时，会默认在_jspService方法中声明创建几个对象。

## 特点

我们可以在jsp页面中声明java局部代码块，在代码块中直接使用内置对象。

## 对象

1. pageContext

   封装了另外八个对象. 

   注意: 每次请求会创建一个新的pageContext对象. 作用域范围只有当前jsp页面.

2. request

   封装了请求的相关数据, 由服务器创建

3. session(可关闭)

4. application

   ServletContext对象, 解决了不同用户之间的数据共享问题

5. out

   数据响应对象. 其实是response.getWriter() 方法返回的PrintStream对象.

6. response

   响应对象

7. page

   实际上就是当前jsp转义的servlet对象. page=this

8. config

   ServletConfit对象. 使用此对象获取配置

9. exception

   jsp中的异常对象