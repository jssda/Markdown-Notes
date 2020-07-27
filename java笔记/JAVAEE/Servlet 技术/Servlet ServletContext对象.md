# ServletContext对象

## 作用

Request对象解决了一次请求内的数据共享问题, session对象解决了用户相同, 不同请求的数据共享问题. ServletContext解决了不同用户的数据共享问题.

## 原理

ServletContext 对象由服务器进行创建，一个项目只有一个对
象。不管在项目的任意位置进行获取得到的都是同一个对象，那
么不同用户发起的请求获取到的也就是同一个对象了，该对象由
用户共同拥有。 

## 特点

1. 服务器进行创建
2. 用户共享
3. 一个项目只有一个

## 生命周期

服务器启动到服务器关闭

## 作用域

项目内

## 使用

### 获取ServletContext对象

```java
// 获取servletContext对象. 三种获取方式. 获得同一个对象
ServletContext servletContext = this.getServletContext();
ServletContext servletContext1 = this.getServletConfig().getServletContext();
ServletContext servletContext2 = req.getServletContext();
```

### 使用作用域进行共享数据流转

```java
// 存储共享数据
servletContext.setAttribute("flag", false);

// 删除共享数据
servletContext.removeAttribute("flag");

// 取得共享数据
Boolean flag = (Boolean) servletContext.getAttribute("flag");
```

## 使用ServletContext获取web.xml中的全局配置

web.xml中配置全局属性

```xml
<context-param>
    <param-name>Hello</param-name>
    <param-value>你好</param-value>
</context-param>
```

使用ServletContext获取

```java
String hello = servletContext.getInitParameter("Hello");
System.out.println("hello = " + hello);
```

## 获取webroot下资源流对象

```java
// 获取资源流对象. 使用相对路径即可
InputStream resourceAsStream = servletContext.getResourceAsStream("img/1.jpg");
System.out.println("resourceAsStream = " + resourceAsStream);
```

## 获取webRoot下资源的绝对路径

```java
// 获取资源的绝对路径
String realPath = servletContext.getRealPath("img/1.jpg");
System.out.println("realPath = " + realPath);
```

