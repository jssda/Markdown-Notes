# 一、Spring的五种作用域

## 1.1 Singleton

当scope的值设置为singleton的时候，整个spring容器中只会存在一个bean实例，通过容器多次查找bean的时候（调用BeanFactory的getBean方法或者bean之间注入依赖的bean对象的时候），返回的都是同一个bean对象，singleton是scope的默认值，所以spring容器中默认创建的bean对象是单例的，通常spring容器在启动的时候，会将scope为singleton的bean创建好放在容器中（有个特殊的情况，当bean的lazy被设置为true的时候，表示懒加载，那么使用的时候才会创建），用的时候直接返回。

注意： 

1. 默认情况下，Bean对象是单例的，会在Spring容器启动的时候加载Bean配置文件，并且创建Bean对象。
2. 由于单例Bean是数据共享的，需要考虑到线程安全的问题。SpringMVC中的Controller就是单例的。当在Controller中定义一些变量的时候，很多线程都会访问到这个变量。当多个线程并发访问这个变量时，会出现数据错乱的问题。

## 1.2 Prototype

如果scope被设置为prototype类型的了，表示这个bean是多例的，通过容器每次获取的bean都是不同的实例，每次获取都会重新创建一个bean实例对象。

注意：

1. 当使用这个作用域的时候，并不会在Spring容器加载的时候创建。因为是多例的，所以每一次从SpringBean中获取Bean对象的时候，才会创建新的Bean对象。
2. 每次获取的时候都会创建新的对象，如果这个Bean很复杂的话，会耗费系统的新能。

## 1.3 request

当一个bean的作用域为request，表示在一次http请求中，一个bean对应一个实例；对每个http请求都会创建一个bean实例，request结束的时候，这个bean也就结束了，request作用域用在spring容器的web环境中，这个以后讲springmvc的时候会说，spring中有个web容器接口WebApplicationContext，这个里面对request作用域提供了支持，配置方式：

```go
<bean id="" class="" scope="request" />
```

## 1.4 session

这个和request类似，也是用在web环境中，session级别共享的bean，每个会话会对应一个bean实例，不同的session对应不同的bean实例，springmvc中我们再细说。

```go
<bean id="" class="" scope="session" />
```

## 1.5 application

全局web应用级别的作用于，也是在web环境中使用的，一个web应用程序对应一个bean实例，通常情况下和singleton效果类似的，不过也有不一样的地方，singleton是每个spring容器中只有一个bean实例，一般我们的程序只有一个spring容器，但是，一个应用程序中可以创建多个spring容器，不同的容器中可以存在同名的bean，但是sope=aplication的时候，不管应用中有多少个spring容器，这个应用中同名的bean只有一个。

```go
<bean id="" class="" scope="application" />
```

# 二、自定义作用域

有时候，spring内置的几种sope都无法满足我们的需求的时候，我们可以自定义bean的作用域。

#### 自定义Scope 3步骤

##### 第1步：实现Scope接口

我们来看一下这个接口定义

```java
package org.springframework.beans.factory.config;
import org.springframework.beans.factory.ObjectFactory;
import org.springframework.lang.Nullable;

public interface Scope {
 /**
   * 返回当前作用域中name对应的bean对象
   * name：需要检索的bean的名称
   * objectFactory：如果name对应的bean在当前作用域中没有找到，那么可以调用这个ObjectFactory来创建这个对象
   **/

    
    Object get(String name, ObjectFactory<?> objectFactory);
    /**
     * 将name对应的bean从当前作用域中移除
     **/
    @Nullable
    Object remove(String name);

    /**
     * 用于注册销毁回调，如果想要销毁相应的对象,则由Spring容器注册相应的销毁回调，而由自定义作用域选择是不是要销毁相应的对象
     */
   void registerDestructionCallback(String name, Runnable callback);



    /**
     * 用于解析相应的上下文数据，比如request作用域将返回request中的属性。
     */
   @Nullable
    Object resolveContextualObject(String key);

    /**
     * 作用域的会话标识，比如session作用域将是sessionId
     */
    @Nullable
    String getConversationId();
}
```

##### 第2步：将自定义的scope注册到容器

需要调用org.springframework.beans.factory.config.ConfigurableBeanFactory#registerScope的方法，看一下这个方法的声明

```java
/**
* 向容器中注册自定义的Scope
*scopeName：作用域名称
* scope：作用域对象
**/
void registerScope(String scopeName, Scope scope);
```

##### 第3步：使用自定义的作用域

定义bean的时候，指定bean的scope属性为自定义的作用域名称。

#### 案例

##### 需求

下面我们来实现一个线程级别的bean作用域，同一个线程中同名的bean是同一个实例，不同的线程中的bean是不同的实例。

##### 实现分析

需求中要求bean在线程中是贡献的，所以我们可以通过ThreadLocal来实现，ThreadLocal可以实现线程中数据的共享。

下面我们来上代码。

##### ThreadScope

```java
package com.javacode2018.lesson001.demo4;
import org.springframework.beans.factory.ObjectFactory;
import org.springframework.beans.factory.config.Scope;
import org.springframework.lang.Nullable;
import java.util.HashMap;
import java.util.Map;
import java.util.Objects;

/**
 * 自定义本地线程级别的bean作用域，不同的线程中对应的bean实例是不同的，同一个线程中同名的bean是同一个实例
 */
public class ThreadScope implements Scope {
    public static final String THREAD_SCOPE = "thread";//@1
    private ThreadLocal<Map<String, Object>> beanMap = new ThreadLocal() {
        @Override
        protected Object initialValue() {
            return new HashMap<>();
        }
    };

    @Override
    public Object get(String name, ObjectFactory<?> objectFactory) {
        Object bean = beanMap.get().get(name);
        if (Objects.isNull(bean)) {
            bean = objectFactory.getObject();
            beanMap.get().put(name, bean);
        }
        return bean;
    }


    @Nullable
    @Override
    public Object remove(String name) {
        return this.beanMap.get().remove(name);
    }


    @Override
    public void registerDestructionCallback(String name, Runnable callback) {
        //bean作用域范围结束的时候调用的方法，用于bean清理
        System.out.println(name);
    }




    @Nullable
    @Override
    public Object resolveContextualObject(String key) {
        return null;
    }

    @Nullable
   @Override
   public String getConversationId() {
        return Thread.currentThread().getName();
    }
}
```

> @1：定义了作用域的名称为一个常量thread，可以在定义bean的时候给scope使用

##### BeanScopeModel

```java
package com.javacode2018.lesson001.demo4;


public class BeanScopeModel {
    public BeanScopeModel(String beanScope) {
       System.out.println(String.format("线程:%s,create BeanScopeModel,{sope=%s},{this=%s}", Thread.currentThread(), beanScope, this));
   }
}
```

> 上面的构造方法中会输出当前线程的信息，到时候可以看到创建bean的线程。

##### bean配置文件

beans-thread.xml内容

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-4.3.xsd">
    <!-- 自定义scope的bean -->
    <bean id="threadBean" class="com.javacode2018.lesson001.demo4.BeanScopeModel" scope="thread">
       <constructor-arg index="0" value="thread"/>
    </bean>
</beans>
```

> 注意上面的scope是我们自定义的，值为thread

##### 测试用例

```java
package com.javacode2018.lesson001.demo4;

import org.springframework.context.support.ClassPathXmlApplicationContext;
import java.util.concurrent.TimeUnit;
/**
 * 公众号：路人甲Java，工作10年的前阿里P7分享Java、算法、数据库方面的技术干货！坚信用技术改变命运，让家人过上更体面的生活!
 * <p>
 * 自定义scope
 */
public class ThreadScopeTest {
    public static void main(String[] args) throws InterruptedException {
        String beanXml = "classpath:/com/javacode2018/lesson001/demo4/beans-thread.xml";
        //手动创建容器
        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext();
        //设置配置文件位置
        context.setConfigLocation(beanXml);
        //启动容器
        context.refresh();
        //向容器中注册自定义的scope
        context.getBeanFactory().registerScope(ThreadScope.THREAD_SCOPE, new ThreadScope());//@1

        //使用容器获取bean
        for (int i = 0; i < 2; i++) { //@2
            new Thread(() -> {
                System.out.println(Thread.currentThread() + "," + context.getBean("threadBean"));
                System.out.println(Thread.currentThread() + "," + context.getBean("threadBean"));
            }).start();

            TimeUnit.SECONDS.sleep(1);
        }
    }
}
```

> 注意上面代码，重点在@1，这个地方向容器中注册了自定义的ThreadScope。
>
> @2：创建了2个线程，然后在每个线程中去获取同样的bean 2次，然后输出，我们来看一下效果。

##### 运行输出

```
线程:Thread[Thread-1,5,main],create BeanScopeModel,{sope=thread},{this=com.javacode2018.lesson001.demo4.BeanScopeModel@4049d530}

Thread[Thread-1,5,main],com.javacode2018.lesson001.demo4.BeanScopeModel@4049d530

Thread[Thread-1,5,main],com.javacode2018.lesson001.demo4.BeanScopeModel@4049d530

线程:Thread[Thread-2,5,main],create BeanScopeModel,{sope=thread},{this=com.javacode2018.lesson001.demo4.BeanScopeModel@87a76da}

Thread[Thread-2,5,main],com.javacode2018.lesson001.demo4.BeanScopeModel@87a76da

Thread[Thread-2,5,main],com.javacode2018.lesson001.demo4.BeanScopeModel@87a76da
```

从输出中可以看到，bean在同样的线程中获取到的是同一个bean的实例，不同的线程中bean的实例是不同的。