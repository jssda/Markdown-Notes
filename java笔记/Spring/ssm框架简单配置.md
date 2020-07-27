# ssm框架简单配置

## web.xml

web.xml是程序的入口, 所有配置文件都要通过web.xml. 通过xml中的servlet或者listener配置, 可以找到其他的配置文件.

### 解析Spring+Mybatis的配置文件

```xml
<context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath:applicationContext-*.xml</param-value>
</context-param>
<listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
</listener>
```

其中, `applicationContext-*.xml`是以`applicationContext-`为开头的xml文件, 并且必须在classpath目录下, 在ide中就是在src下

### 解析Springmvc-cfg配置文件

```xml
<servlet>
    <servlet-name>mvc</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>

    <init-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:springmcv-cfg.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
</servlet>
<servlet-mapping>
    <servlet-name>mvc</servlet-name>
    <url-pattern>/</url-pattern>
</servlet-mapping>
```

此处配置springmvc. 其中springmvc-cfg是springmvc的配置文件

### 乱码调整过滤器

```xml
<!--乱码过滤器-->
<filter>
    <filter-name>encode</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
        <param-name>encoding</param-name>
        <param-value>utf-8</param-value>
    </init-param>
</filter>
<filter-mapping>
    <filter-name>encode</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```



## Springmvc-cfg

### 必须配置的标签

扫描控制单元中的注解配置(以注解配置的控制单元中的bean)

```xml
    <!--扫描控制单元中的注解配置-->
    <context:component-scan base-package="pers.jssd.controller"/>
```

扫描注解配置映射(@requestMapping)

```xml
    <!--扫描注解映射配置-->
    <mvc:annotation-driven/>
```

### 自定义视图解析器

```xml
<!--  自定义视图解析器  -->
<bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
    <property name="prefix" value="/WEB-INF/"/>
    <property name="suffix" value=".jsp"/>
</bean>
```

### 上传文件解析器

```xml
<!--上传文件解析器-->
<bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
    <property name="defaultEncoding" value="utf-8"/>
</bean>
```

### 放行静态资源

```xml
<mvc:resources mapping="/data/**" location="/data/"/>
<mvc:resources mapping="/files/**" location="/files/"/>
<mvc:resources mapping="/images/**" location="/images/"/>
<mvc:resources mapping="/plugins/**" location="/plugins/"/>
<mvc:resources mapping="/resources/**" location="/resources/"/>
<mvc:resources mapping="/upload/**" location="/upload/"/>
```

### 配置拦截器

**1.  自定义一个拦截器**

```java
/**
 * 一个Session中是否有当前用户的过滤器
 * 
 * @author jssdjing@gmail.com
 */
public class SessionInterpreter extends HandlerInterceptorAdapter {

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        Object currUser = request.getSession().getAttribute("currUser");
        if (currUser == null) {
            response.sendRedirect(request.getContextPath() + "/login.jsp");
            return false;
        }
        return true;
    }
}
```

**2. xml中配置次过滤器**

```xml
<!--配置一个拦截器-->
<mvc:interceptors>
    <mvc:interceptor>
        <mvc:mapping path="/**"/>
        <bean class="pers.jssd.interpreter.SessionInterpreter"/>
    </mvc:interceptor>
</mvc:interceptors>
```



# Spring相关配置

## applicationContext-mybatis.xml

**配置mybatis相关的设置**

```xml
<!--引入数据库配置外部文件-->
<context:property-placeholder location="classpath:db.properties"/>

<!--配置数据源-->
<bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
    <property name="driverClassName" value="${driver}"/>
    <property name="url" value="${url}"/>
    <property name="password" value="${password}"/>
    <property name="username" value="${name}"/>
</bean>

<!--配置sqlSession工厂-->
<bean id="factory" class="org.mybatis.spring.SqlSessionFactoryBean">
    <property name="dataSource" ref="dataSource"/>
    <property name="typeAliasesPackage" value="pers.jssd.pojo"/>
</bean>

<!--扫描Mapper配置-->
<bean id="mapper" class="org.mybatis.spring.mapper.MapperScannerConfigurer">
    <property name="sqlSessionFactoryBeanName" value="factory"/>
    <property name="basePackage" value="pers.jssd.mapper"/>
</bean>
```

## applicationContext-tx.xml

**事务相关的配置**

```xml
<!--声明式事务-->
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <property name="dataSource" ref="dataSource"/>
</bean>

<!--扫描事务注解-->
<tx:annotation-driven transaction-manager="transactionManager"/>
```

## applicationContext-service.xml

**扫描事务层配置的bean**

```xml
<!--扫描事务层配置的bean-->
<context:component-scan base-package="pers.jssd.service"/>
```

