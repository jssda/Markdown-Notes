# Servlet Session技术

Cookie是浏览器端的数据存储技术, 那么session是服务器端的数据存储技术

## session原理

用户使用浏览器第一次向服务器发送请求，服务器在接受到请
求后，调用对应的 Servlet 进行处理。在处理过程中会给用户创建
一个 session 对象，用来存储用户请求处理相关的公共数据，并将
此 session 对象的 JSESSIONID 以 Cookie 的形式存储在浏览器中(临
时存储，浏览器关闭即失效)。用户在发起第二次请求及后续请求
时，请求信息中会附带 JSESSIONID，服务器在接收到请求后，调
用对应的 Servlet 进行请求处理，同时根据 JSESSIONID 返回其对应
的 session 对象。 

## 特点

1. Session 技术是依赖 Cookie 技术的服务器端的数据存储技术。
   由服务器进行创建 
2. 每个用户独立拥有一个session
3. 默认存储事件为30分钟

## 作用

解决了一个用户的不同请求的数据共享问题

## 使用

### 创建session对象

```java
// 获取一个session对象, 如果request存在对应的SessionId,
// 获取已经存在的session对象, 如果没有对应的sessionID, 获取一个新的session对象
HttpSession session = req.getSession();
```

### 存储数据到session对象

```java
// 设置一个新的session数据
session.setAttribute("testName", new Date());
```

### 获取session对象

获取即为创建. 调用的是同一个方法. 假如浏览器端没有存储对应的sessionID, 或者浏览器端存储的SessionId和服务器本身sessionId不同, 则会创建一个新的session对象, 并更新浏览器端的sessionID

### 获取session数据

```java
// 获取session数据, 如果存在则返回Object对象, 如果不存在, 返回null
Date testName = (Date) session.getAttribute("testName");
System.out.println("testName = " + testName);
```

## session作用域

一次会话

## session的作用时间

session默认有效时间为30分钟，可以在tomcat下的web.xml中进行配置( tomcat下配置的是全局默认时间). 也可以在本项目的web.xml中配置(设置的是本项目的默认session时间). 新创建的session对象, 也可以用代码设置

```java
// 设置session的活动间隔时间为5秒. 5秒没有访问此session, 则session会自动销毁
session.setMaxInactiveInterval(5);
```

## 强制销毁一个session

```java
// 手动设置一个session为不可用. 再次调用则会取得一个新的session
session.invalidate();
```

