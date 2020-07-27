# web.xml配置

web.xml能够帮助我们在访问一个连接的时候, 能够定位到指定的servlet. 同时, 也起到了保护servlet的作用

## 配置方式一

单独配置, 只为一个文件进行别名映射

```xml
<!-- 配置方式一 -->
<servlet>
    <servlet-name>my3</servlet-name>
    <servletclass>pers.jssd.servlet.MyServlet</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>my3</servlet-name>
    <url-pattern>/my3</url-pattern>
</servlet-mapping>
```

## 配置方式二

为指定后缀的别名映射一个servlet.

为所有后缀为.do的url, 映射一个Servlet

```xml
<servlet>
	<servlet-name>my3</servlet-name>
    <servlet-class>pers.jssd.servlet.MyServlet</servlet-class>
</servlet>
<servlet-mapping>
	<servlet-name>my3</servlet-name>
    <url-pattern>*.do</url-pattern>
</servlet-mapping>
```

## 配置方式三

配置指定目录下所有的资源映射

```xml
<servlet>
	<servlet-name>my3</servlet-name>
    <servlet-class>pers.jssd.servlet.MyServlet</servlet-class>
</servlet>
<servlet-mapping>
	<servlet-name>my3</servlet-name>
    <url-pattern>*.do</url-pattern>
    <url-pattern>/one/*</url-pattern>
</servlet-mapping>
```

## web.xml 加载时机

服务器启动的时候, 会将webapps中部署好的项目统一进行加载, 并完成对每个项目的web.xml文件的加载.

## 注意事项

一个servlet可以拥有多个url-pattern配置, 但是一个url-pattern配置只能对应一个servlet