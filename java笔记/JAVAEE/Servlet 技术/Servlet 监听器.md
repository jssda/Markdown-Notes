[TOC]

# 监听器

## 什么是监听器

Servlet技术中, 我们学到了request, response, application等域对象. 其主要作用是在不同的作用域中让数据灵活的流转. 但是数据的具体流转过程我们不知道. 假如我们想要看到域对象是什么时候创建和销毁的. 数据是什么时候存储的, 改变和删除. 这个时候, 就需要用到了监听器. 故名思意, 监听器, 就是用来监听域对象的改变.

## 概念

Servlet监听器是Servlet规范中定义的一个特殊的类, 用于监听ServletContext, HttpSession和HttpServletRequest等域对象的创建和销毁, 以及监听这些域对象中属性发生修改的事件.

## 监听器种类

### ServletRequestListener

request作用域的监听, 有两个监听方法

1. requestDestroyed
2. requestInitialized

这两个方法都传递一个ServletRequestEvent参数, 类似js传递的监听事件

```java
@Override
public void requestDestroyed(ServletRequestEvent sre) {
    // 取得监听的ServletRequest
    ServletRequest servletRequest = sre.getServletRequest();
}
```

### ServletRequestAttributeListener

监听request中的数据更改

有三个方法

1. attributeAdded
2. attributeRemoved
3. attributeReplaced

三个方法传递一个ServletRequestAttributeEvent参数

示例

```java
public void attributeAdded(ServletRequestAttributeEvent srae) {
    // 取得添加参数的名字
    String name = srae.getName();
    System.out.println("name = " + name);
    // 取得修改参数的值
    Object value = srae.getValue();
    System.out.println("value = " + value);
}
```

### HttpSessionListener

使用和request监听相同

1. sessionCreated
2. sessionDestroyed

### ServletRequestAttributeListener

使用和request相似

1. attributeAdded
2. attributeRemoved
3. attributeReplaced

### ServletContextListener

使用类似

### ServletContextAttributeListener

使用类似