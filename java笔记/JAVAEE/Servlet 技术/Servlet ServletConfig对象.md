# ServletConfit对象

使用 ServletContext 对象可以获取 web.xml 中的全局配置文件，
在 web.xml 中
每个 Servlet 也可以进行单独的配置，可以使用ServletConfig获取单独配置的servlet初始化属性

## 作用

ServletConfig对象是Servlet的专属配置对象, 每个servlet都单独拥有一个servletConfig对象, 用来获取web.xml中的配置信息.

## 使用

### 获取ServletConfig对象

```java
ServletConfig servletConfig = this.getServletConfig();
```

### 获取web.xml中的servlet配置信息

web.xml配置

```xml
<init-param>
    <param-name>code</param-name>
    <param-value>utf-8</param-value>
</init-param>
```

```java
String code = servletConfig.getInitParameter("code");
System.out.println("code = " + code);
```

