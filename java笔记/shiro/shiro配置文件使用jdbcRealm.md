# shiro使用JDBCRealm

## ini配置文件

```ini
[main]
# 配置数据源
dataSource=com.mchange.v2.c3p0.ComboPooledDataSource
dataSource.driverClass=com.mysql.cj.jdbc.Driver
dataSource.jdbcUrl=jdbc:mysql://localhost:3306/shiro?serverTimezone=GMT
dataSource.user=root
dataSource.password=1624022009.a

# 配置realm域
jdbcRealm=org.apache.shiro.realm.jdbc.JdbcRealm
# 配置域中的数据源
jdbcRealm.dataSource=$dataSource

# 指定realm域
securityManager.realms=$jdbcRealm
```

## 测试文件

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
public class Test02 {
    public static void main(String[] args) {
        // 1. 取得shiro工厂
        Factory<SecurityManager> factory = new IniSecurityManagerFactory("classpath:shiro.ini");
        // 2. 安全管理
        SecurityManager securityManager = factory.getInstance();
        // 3. 设置运行环境
        SecurityUtils.setSecurityManager(securityManager);
        // 4. 取得域对象
        Subject subject = SecurityUtils.getSubject();
        // 5. 取得用户验证令牌
        UsernamePasswordToken token = new UsernamePasswordToken("zs", "123456");
        // 6. 注册用户登录令牌
        subject.login(token);
        // 7. 验证
        if (subject.isAuthenticated()) {
            System.out.println("用户登录成功");
        }
    }
}
```

## 认证策略

一个realm对应一个数据源. shiro支持多个数据源. 不同数据源直接怎么认证呢. shiro提供了好几种认证策略

  规定了如果有多个数据源的时候应该如何操作

### AtLeastOneSuccessfulStrategy

如果一个（或更多） Realm 验证成功， 则整体的尝试被认为是成功的。
如果没有一个验证成功，
则整体尝试失败 类似于 java 中的 &

### FirstSuccessfulStrategy

只有第一个成功地验证的 Realm 返回的信息将被使用。 所有进一步的
Realm 将被忽略。 如果没有一个验证成功， 则整体尝试失败。
类似于 java 中的 &&

### AllSucessfulStrategy

为了整体的尝试成功， 所有配置的 Realm 必须验证成功。 如果没有一个验
证成功， 则整体尝试失败  

## 在配置文件中配置认证策略

```ini
# 配置认证策略
# 三个认证策略:
# org.apache.shiro.authc.pam.FirstSuccessfulStrategy
# org.apache.shiro.authc.pam.AllSuccessfulStrategy
# org.apache.shiro.authc.pam.AtLeastOneSuccessfulStrategy
authenticationStrategy=org.apache.shiro.authc.pam.AllSuccessfulStrategy

# 指定realm域
securityManager.realms=$jdbcRealm,$jdbcRealm1
# 指定认证策略
securityManager.authenticator.authenticationStrategy=$authenticationStrategy
```



