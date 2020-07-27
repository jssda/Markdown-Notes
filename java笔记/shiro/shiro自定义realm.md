# 认证策略和自定义realm

## 认证策略

### 三种认证策略

```
AtLeastOneSuccessfulStrategy
如果一个（或更多） Realm 验证成功， 则整体的尝试被认为是成功的。
如果没有一个验证成功，
则整体尝试失败 类似于 java 中的 &

FirstSuccessfulStrategy
只有第一个成功地验证的 Realm 返回的信息将被使用。 所有进一步的
Realm 将被忽略。 如果没有一个验证成功， 则整体尝试失败。
类似于 java 中的 &&

AllSucessfulStrategy
为了整体的尝试成功， 所有配置的 Realm 必须验证成功。 如果没有一个验
证成功， 则整体尝试失败
```

### 示例代码

配置文件, 配置了两个数据源

```ini
[main]
# 配置数据源
dataSource=com.mchange.v2.c3p0.ComboPooledDataSource
dataSource.driverClass=com.mysql.cj.jdbc.Driver
dataSource.jdbcUrl=jdbc:mysql://localhost:3306/shiro?serverTimezone=GMT
dataSource.user=root
dataSource.password=xxxxxxx

# 配置数据源2
dataSource1=com.mchange.v2.c3p0.ComboPooledDataSource
dataSource1.driverClass=com.mysql.cj.jdbc.Driver
dataSource1.jdbcUrl=jdbc:mysql://localhost:3306/shiro1?serverTimezone=GMT
dataSource1.user=root
dataSource1.password=xxxxxxx

# 配置realm域
jdbcRealm=org.apache.shiro.realm.jdbc.JdbcRealm
jdbcRealm1=org.apache.shiro.realm.jdbc.JdbcRealm

# 配置域中的数据源
jdbcRealm.dataSource=$dataSource
jdbcRealm1.dataSource=$dataSource1


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

测试代码

```java
package pers.jssd.shiro01;

import org.apache.shiro.SecurityUtils;
import org.apache.shiro.authc.AuthenticationException;
import org.apache.shiro.authc.UsernamePasswordToken;
import org.apache.shiro.config.IniSecurityManagerFactory;
import org.apache.shiro.mgt.SecurityManager;
import org.apache.shiro.subject.Subject;
import org.apache.shiro.util.Factory;

/**
 * @author jssdjing@gmail.com
 */
public class Test03 {
    public static void main(String[] args) {

        // 1. 使用shiro第一步, 建立工厂
        Factory<SecurityManager> factory = new IniSecurityManagerFactory("classpath:shiro-jdbc.ini");
        // 2. 取得安全管理核心
        SecurityManager se = factory.getInstance();
        // 3. 设置运行环境
        SecurityUtils.setSecurityManager(se);
        // 4. 取得用户subject对象
        Subject subject = SecurityUtils.getSubject();
        // 5. 取得用户登录令牌
        UsernamePasswordToken token = new UsernamePasswordToken("jssd", "123456");
        try {
            // 6. 登录
            subject.login(token);
            // 验证是否登录成功
            if (subject.isAuthenticated()) {
                System.out.println("登录成功");
            }
        } catch (AuthenticationException e) {
            e.printStackTrace();
            System.out.println("登录失败");
        }
    }
}
```

## 自定义realm

自定义Realm需要实现一个Realm接口, 但是一般情况下, 我们都是继承实现Realm接口的抽象类AuthorizingRealm.

### 继承AuthenticatingRealm抽象类

```java
package pers.jssd.shiro02;

import org.apache.shiro.authc.AuthenticationException;
import org.apache.shiro.authc.AuthenticationInfo;
import org.apache.shiro.authc.AuthenticationToken;
import org.apache.shiro.authc.SimpleAuthenticationInfo;
import org.apache.shiro.authz.AuthorizationInfo;
import org.apache.shiro.realm.AuthorizingRealm;
import org.apache.shiro.subject.PrincipalCollection;

import java.sql.*;

/**
 * @author jssdjing@gmail.com
 */
public class DiyRealm extends AuthenticatingRealm {

    /**
     * 认证
     */
    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken authenticationToken) throws AuthenticationException {
        String  name = (String) authenticationToken.getPrincipal();

        try {
            Class.forName("com.mysql.cj.jdbc.Driver");
            Connection connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/blog?serverTimezone=GMT", "root", "xxxxx");
            PreparedStatement statement = connection.prepareStatement("select password from user where name = ?");
            statement.setString(1, name);
            ResultSet resultSet = statement.executeQuery();
            // 封装成一个认证信息并返回, 认证信息可以使用SimpleAuthenticationInfo进行封装
            if (resultSet.next()) {
                return new SimpleAuthenticationInfo(name, resultSet.getString("password"), "userReal");
            }
        } catch (ClassNotFoundException | SQLException e) {
            e.printStackTrace();
        }
        return null;
    }
}
```

### 配置文件

那么配置shrio.ini的时候, 就需要这样配置了

```ini
[main]

# 配置realm域(自定义realm)
jdbcRealm=pers.jssd.shiro02.DiyRealm

# 配置认证策略
# 三个认证策略:
# org.apache.shiro.authc.pam.FirstSuccessfulStrategy
# org.apache.shiro.authc.pam.AllSuccessfulStrategy
# org.apache.shiro.authc.pam.AtLeastOneSuccessfulStrategy
authenticationStrategy=org.apache.shiro.authc.pam.AllSuccessfulStrategy

# 指定realm域
securityManager.realms=$jdbcRealm
# 指定认证策略
securityManager.authenticator.authenticationStrategy=$authenticationStrategy
```



