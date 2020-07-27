# Servlet Request请求对象详解

浏览器发起请求到服务器，会遵循HTTP协议将请求数据发送给服务器。
那么服务器接受到请求的数据改怎么存储呢?不但要存，而且要保证完成性 . 于是, 服务器解析完请求信息之后, 会将请求信息封装到一个对象中, 那就是request对象.

对象中
存储了此次请求相关的请求数据。服务器在调用 Servlet 时会将创建的
Request 对象作为实参传递给 Servlet 的方法，比如： service 方法 

## Request的使用

### 获取请求行

```java
// 获取请求方式
String method = req.getMethod();
System.out.println("method = " + method);

// 获取请求url
StringBuffer requestURL = req.getRequestURL();
System.out.println("requestURL = " + requestURL);

// 获取请求URI
String requestURI = req.getRequestURI();
System.out.println("requestURI = " + requestURI);

// 获取请求的参数
String queryString = req.getQueryString();
System.out.println("queryString = " + queryString);

// 取得项目根路径
String contextPath = req.getContextPath();
System.out.println("contextPath = " + contextPath);

// 获取使用协议
String scheme = req.getScheme();
System.out.println("scheme = " + scheme);
```

### 获取请求头

```java
// 取得请求头信息. 需要提供请求头键值对的name, 才能返回相对应的信息, 如果没有name, 那么返回一个null
String header = req.getHeader("User-agent");
System.out.println("header = " + header);

// 获取请求头的名字枚举
Enumeration<String> headerNames = req.getHeaderNames();
while (headerNames.hasMoreElements()) {
    String element = headerNames.nextElement();
    System.out.println(element + " : " + req.getHeader(element));
}
```

### 获取用户实体数据

```java
System.out.println("获取用户实体信息, 没有则返回null");
String username = req.getParameter("username");
System.out.println("username = " + username);
// 获取同名不同值得参数
String[] favs = req.getParameterValues("fav");
if (favs != null) {
    for (String fav : favs) {
        System.out.println("fav = " + fav);
    }
}

// 使用枚举获取用户实体数据
Enumeration<String> parameterNames = req.getParameterNames();
while (parameterNames.hasMoreElements()) {
    String element = parameterNames.nextElement();
    if ("fav".equals(element)) {
        String[] parameterValues = req.getParameterValues(element);
        for (String value : parameterValues) {
            System.out.println("element + value = " + element + value);
        }
    } else {
        String parameter = req.getParameter(element);
        System.out.println("parameter = " + parameter);
    }
}
```

### 获取网络相关得数据

```java
// 获取请求地址
String remoteAddr = req.getRemoteAddr();
System.out.println("remoteAddr = " + remoteAddr);
// 获取请求主机得发送端口
int remotePort = req.getRemotePort();
System.out.println("remotePort = " + remotePort);
// 获取请求主机名或者主机地址
String remoteHost = req.getRemoteHost();
System.out.println("remoteHost = " + remoteHost);

// 获取本地服务器地址
String localAddr = req.getLocalAddr();
System.out.println("localAddr = " + localAddr);
// 获取本地服务器端口
int localPort = req.getLocalPort();
System.out.println("localPort = " + localPort);
```

