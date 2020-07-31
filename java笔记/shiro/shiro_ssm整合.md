# shiro整合ssm使用

首先需要搭建ssm环境

导入shiro的依赖包

## 添加shiro配置

### web.xml配置

需要添加一个过滤器

```xml

<!--shiro 过滤器-->
<filter>
    <filter-name>shiro</filter-name>
    <!--让spring中的bean, 代理这个filter. 所以需要spring框架中的这个代理类-->
    <filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
    <init-param>
        <!--初始化filter生命周期, 使shiro类符合tomcat中的filter的生命周期(可被init destory操作)-->
        <param-name>targetFilterLifecycle</param-name>
        <param-value>true</param-value>
    </init-param>
    <init-param>
        <!--目标工厂类-->
        <param-name>targetBeanName</param-name>
        <param-value>shiroFactory</param-value>
    </init-param>
</filter>
<filter-mapping>
    <!--先拦截所有请求, 后续可详细配置-->
    <filter-name>shiro</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

### 在spring中配置shiro

applicationContext-shiro.xml配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--凭证匹配器(加密算法配置)-->
    <bean id="credentialsMatcher" class="org.apache.shiro.authc.credential.HashedCredentialsMatcher">
        <!--加密方式-->
        <property name="hashAlgorithmName" value="md5"/>
        <!--加密迭代次数-->
        <property name="hashIterations" value="2"/>
    </bean>

    <!--注册自定义Realm数据源-->
    <bean class="pers.jssd.realm.UsersRealm" id="usersRealm">
        <property name="credentialsMatcher" ref="credentialsMatcher"/>
    </bean>

    <!--注册安全管理-->
    <bean class="org.apache.shiro.web.mgt.DefaultWebSecurityManager" id="securityManager">
        <property name="realms" ref="usersRealm"/>
    </bean>

    <!--注册工厂-->
    <bean class="org.apache.shiro.spring.web.ShiroFilterFactoryBean" id="shiroFactory">
        <property name="loginUrl" value="/login"/>
        <property name="successUrl" value="/index.jsp"/>
        <property name="unauthorizedUrl" value="/error.jsp"/>
        <property name="securityManager" ref="securityManager"/>

        <!--设置过滤器链的属性 authc 拦截指定路径 anon 放行资源 -->
        <property name="filterChainDefinitions">
            <value>
                /login=authc
                /**=anon
            </value>
        </property>
    </bean>
</beans>
```

### 自定义realm

```java
package pers.jssd.realm;

import org.apache.shiro.authc.*;
import org.apache.shiro.realm.AuthenticatingRealm;
import org.apache.shiro.util.ByteSource;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import pers.jssd.pojo.Users;
import pers.jssd.service.UserService;

import java.sql.*;

/**
 * @author jssdjing@gmail.com
 */
public class UsersRealm extends AuthenticatingRealm {

    @Autowired
    private UserService userService;

    /**
     * 认证
     */
    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken authenticationToken) throws AuthenticationException {
        String name = (String) authenticationToken.getPrincipal();

        Users users = new Users();
        users.setUsername(name);
        Users login = userService.login(users);

        if (login != null) {
            // 第三个是盐值, 一般放在数据库中, 需要查询出来, 但此处仅模拟
            return new SimpleAuthenticationInfo(name, login.getPassword(),
                    ByteSource.Util.bytes("jssd"), "userReal");
        } else {
            return null;
        }
    }
}

```

### 在Controller中捕获认证失败的异常

```java
package pers.jssd.controller;

import org.apache.shiro.authc.IncorrectCredentialsException;
import org.apache.shiro.authc.UnknownAccountException;
import org.apache.shiro.web.filter.authc.FormAuthenticationFilter;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import pers.jssd.service.UserService;

import javax.servlet.http.HttpServletRequest;

/**
 * @author jssdjing@gmail.com
 */
@Controller
public class UserController {

    @Autowired
    private UserService userService;

    @RequestMapping("/login")
    public String login(HttpServletRequest req) {
        Object attribute = req.getAttribute(FormAuthenticationFilter.DEFAULT_ERROR_KEY_ATTRIBUTE_NAME);
        if (UnknownAccountException.class.getName().equals(attribute)) {
            req.setAttribute("msg", "用户名不正确");
        } else if (IncorrectCredentialsException.class.getName().equals(attribute)) {
            req.setAttribute("msg", "密码错误");
        } else {
            System.out.println("attribute = " + attribute);
            req.setAttribute("msg", "未知异常");
        }
        return "forward:error.jsp";
    }
}
```

## 自定义过滤器

各个过滤器对应名称

![image-20191118111310815](https://raw.githubusercontent.com/jssda/picbed/master/image-20191118111310815.png)

### 表单过滤器

```xml
<!--表单过滤器-->
<bean class="org.apache.shiro.web.filter.authc.FormAuthenticationFilter" id="authc">
    <property name="usernameParam" value="username"/>
    <property name="passwordParam" value="password"/>
    <property name="rememberMeParam" value="remember"/>
</bean>
```

### 退出过滤器

```xml
<!--退出过滤器-->
<bean class="org.apache.shiro.web.filter.authc.LogoutFilter" id="logout">
    <property name="redirectUrl" value="login.jsp"/>
</bean>
```

## session管理

```xml
<!--session管理-->
<bean class="org.apache.shiro.web.session.mgt.DefaultWebSessionManager" id="sessionManager">
    <!--单位是毫秒-->
    <property name="globalSessionTimeout" value="1800000"/>
    <!--自动删除失效的session-->
    <property name="deleteInvalidSessions" value="true"/>
</bean>
```

同时, 需要在securityManager中添加一个属性, sessionManager

## RememberMe功能

```xml
<!--设置cookie-->
<bean class="org.apache.shiro.web.servlet.SimpleCookie" id="cookie">
    <property name="name" value="remember"/>
    <!--单位是秒 7天免登录-->
    <property name="maxAge" value="604800"/>
</bean>

<!--注册rememberMe管理器-->
<bean class="org.apache.shiro.web.mgt.CookieRememberMeManager" id="rememberMeManager">
    <property name="cookie" ref="cookie"/>
</bean>
```

配置securityManager

```xml
<!--配置shiro安全管理模块-->
<bean class="org.apache.shiro.web.mgt.DefaultWebSecurityManager" id="securityManager">
    <property name="realms" ref="adminRealm"/>
    <property name="sessionManager" ref="sessionManager"/>
    <property name="rememberMeManager" ref="rememberMeManager"/>
</bean>
```

配置访问哪些资源的时候需要被记住

```xml
<!--配置shiro过滤工厂-->
<bean class="org.apache.shiro.spring.web.ShiroFilterFactoryBean" id="shiroFilter">
    <property name="securityManager" ref="securityManager"/>
    <property name="loginUrl" value="/login"/>
    <property name="unauthorizedUrl" value="/error.jsp"/>
    <property name="successUrl" value="/showMenu"/>

    <!--anon没有过滤器, authc表单过滤器, user记住我-->
    <property name="filterChainDefinitions">
        <value>
            /login=authc
            /logout=logout
            /showMenu=user
            /**=anon
        </value>
    </property>
</bean>
```

