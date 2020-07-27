# shior介绍

>   Apache Shiro 是一个强大而灵活的开源安全框架， 它干净利
> 落地处理身份认证， 授权， 企业会话管理和加密  
>
>   Shiro 的官网： http://shiro.apache.org/  

## 基本使用

### 导包

```
shiro-all-1.3.2.jar
shiro-core-1.3.2.jar
shiro-spring-1.3.2.jar
shiro-web-1.3.2.jar

额外的包
commons-beanutils-1.9.2.jar
commons-logging-1.1.1.jar
log4j-1.2.17.jar
slf4j-api-1.7.5.jar
slf4j-log4j12-1.7.5.jar
```

### 代码

```java
package pers.jssd.shiro01;

import org.apache.shiro.SecurityUtils;
import org.apache.shiro.authc.UsernamePasswordToken;
import org.apache.shiro.config.IniSecurityManagerFactory;
import org.apache.shiro.mgt.SecurityManager;
import org.apache.shiro.subject.Subject;
import org.apache.shiro.util.Factory;

/**
 * @author jssdjing@gmail.com
 */
public class Test01 {
    public static void main(String[] args) {
        // 1. 解析shiro文件
        Factory<SecurityManager> factory = new IniSecurityManagerFactory("classpath:shiro.ini");
        // 2. 取得安全管理
        SecurityManager securityManager = factory.getInstance();
        // 3. 把安全管理设置到运行环境中
        SecurityUtils.setSecurityManager(securityManager);
        // 4. 获取主题域对象 subject
        Subject subject = SecurityUtils.getSubject();
        // 5. 取得用户输入令牌
        UsernamePasswordToken token = new UsernamePasswordToken("jssd", "root");
        // 6. 进行身份验证
        subject.login(token);
        // 7. 判断是否认证成功
        if (subject.isAuthenticated()) {
            System.out.println("登录成功");
        } else {
            System.out.println("登录失败");
        }
    }
}
```

shiro.ini文件

```ini
[users]
zs=123456
jssd=root
```

## 常见异常类型

```
DisabledAccountException
账户失效异常

ConcurrentAccessException
竞争次数过多异常

ExcessiveAttemptsException
尝试次数过多异常

UnknownAccountException
未知账户(用户名不正确)

IncorrectCredentialsException
凭证不正确(密码错误)

ExpiredCredentialsException
凭证过期
```

