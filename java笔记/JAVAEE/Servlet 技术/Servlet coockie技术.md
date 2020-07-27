# Servlet Cookie技术

## 作用

解决了不同请求之间数据共享的问题

## 解决的问题

不同的请求使用相同的请求数据, 但是, 请求只要结束, 数据就被销毁了, 其他请求需要将公共数据重新书写发送. 使用cookie技术之后, 会将多次请求的重复数据, 存储在浏览器中.

## 特点

1. 浏览器端的数据存储技术
2. 用cookie存储的数据需要在服务器端进行声明, 以响应的方式告诉浏览器进行存储,
3. 不适合大量数据的存储.
4. 如果不设置存储有效期, 默认存储在浏览器的内存中, 浏览器关闭就会失效
5. 如果设置了有效期, 那么就会存储到客户端的硬盘里, 到期之后自动销毁
6. 如果不设置有效存储路径, 则任意项目路径的请求都会附带存储的cookie信息
7. 如果设置了有效路径, 则只有在该路径下的请求才会附带设置的cookie信息

## 使用

### 创建一个cookie

```java
String a = req.getParameter("a");
System.out.println("a = " + a);
resp.addCookie(new Cookie("a", a));
```

### 取得cookie

```java
System.out.println("获取cookie数据");
Cookie[] cookies = req.getCookies();
for (Cookie cookie : cookies) {
    String name = cookie.getName();
    String value = cookie.getValue();
    System.out.println("name + value = " + name + ", " + value);
}
```

### 设置cookie有效时限

```java
// 设置有效期为3天, 到期自动销毁
a1.setMaxAge(3600 * 24 * 3);
```

### 设置有效路径

```java
// 设置有效路径. 只有在有效路径发送请求得时候, 会附带cookie信息. (注意: 路径从项目路径开始)
a1.setPath("/lg/CookieServlet");
resp.addCookie(a1);
```

