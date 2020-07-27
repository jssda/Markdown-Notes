# Servlet 生命周期

## Servlet的单例创建

在服务器中, 一个servlet在服务器启动之后, 只会创建一次. 并不是每次有服务请求到达的时候, 就会创建一个servlet, 这样对服务器的压力太大了. 事实上, servlet是单例创建的.

服务器在接收到浏览器的请求后, 会开辟一个线程来处理此次请求, 在线程中调用对应的Servlet进行处理.

服务器调用Servlet处理请求, 但是一个Servlet服务器只会创建一个实例化对象. 该对象是线程共享的.

## Servlet的生命周期

在服务器启动的时候, 并不会直接创建servlet对象. 

在浏览器第一次访问的时候, Servlet才会被创建, 并且被访问. 

在服务器关闭的时候, 会销毁Servlet对象

注意, 在web.xml中对应的servlet中配置load-on-startup标签, 可以让servlet在服务器启动的时候创建.

```xml
<servlet>
    <servlet-name>MyServlet</servlet-name>
    <servlet-class>pers.jssd.servlet.MyServlet</servlet-class>
    <load-on-startup>1</load-on-startup>
</servlet>
```

