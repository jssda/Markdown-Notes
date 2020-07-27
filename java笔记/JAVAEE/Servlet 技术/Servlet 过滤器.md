[TOC]

# 过滤器

## 什么是过滤器

过滤器可以实现在访问一个项目资源或者项目文件的时候, 先访问过滤器对其进行过滤处理, 或者预处理. 通过过滤器, 也可以设置是否能进行访问.

## 使用过滤器

1. 使用过滤器需要实现Filter接口
2. 使用FIlter需要在web.xml中注册过滤器
3. 访问Filter的顺序是根据web.xml中的`<filter-mapping>`的配置顺序决定的
4. 实现Filter之后, 其中最重要的一个方法就是doFilter方法.

### 示例

```java
public class MyFilter implements Filter {

    public void doFilter(ServletRequest req, ServletResponse resp, FilterChain chain) throws ServletException, IOException {
        chain.doFilter(req, resp);
    }

}
```

chain参数就是过滤链, 当此过滤器通过了之后, 执行chain.doFilter, 就会访问此过滤链中其他的过滤器. 如果没有其他的过滤器了, 就会访问过滤的资源.

web.xml

```xml
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

    <filter>
        <filter-name>MyFilter</filter-name>
        <filter-class>pers.jssd.filter.MyFilter</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>MyFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
</web-app>
```

