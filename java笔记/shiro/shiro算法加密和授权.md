# 算法加密和授权

## 算法加密

###   散列算法(算法加密)  

在身份认证的过程中往往都会涉及到加密,如果不加密,这个时
候信息就会非常的不安全

shiro 中提供的算法比较多 如 MD5
SHA 等
使用 MD5 进行 ‘’ 1111 加密
`b59c67bf196a4758191e42f76670ceba  `

一次加密可能还有些不安全, 我们会对它加盐, 进行多次迭代

### 代码

配置文件

```ini
[main]

# 配置realm域
jdbcRealm=pers.jssd.shiro03.DiyRealm

# 配置加密算法
credentialsMatcher=org.apache.shiro.authc.credential.HashedCredentialsMatcher
# 指定加密算法
credentialsMatcher.hashAlgorithmName=md5
# 迭代次数
credentialsMatcher.hashIterations=2

# 指定加密算法
jdbcRealm.credentialsMatcher=$credentialsMatcher

# 指定realm域
securityManager.realms=$jdbcRealm
```

自定义realm域

```java
package pers.jssd.shiro03;

import org.apache.shiro.authc.AuthenticationException;
import org.apache.shiro.authc.AuthenticationInfo;
import org.apache.shiro.authc.AuthenticationToken;
import org.apache.shiro.authc.SimpleAuthenticationInfo;
import org.apache.shiro.realm.AuthenticatingRealm;
import org.apache.shiro.util.ByteSource;

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
        String name = (String) authenticationToken.getPrincipal();

        try {
            Class.forName("com.mysql.cj.jdbc.Driver");
            Connection connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/blog?serverTimezone=GMT",
                    "root", "1624022009.a");
            PreparedStatement statement = connection.prepareStatement("select password from user where name = ?");
            statement.setString(1, name);
            ResultSet resultSet = statement.executeQuery();
            if (resultSet.next()) {
                // 第三个参数是添加的盐值
                return new SimpleAuthenticationInfo(name, resultSet.getString("password"),
                        ByteSource.Util.bytes("jssd"), "userReal");
            }
        } catch (ClassNotFoundException | SQLException e) {
            e.printStackTrace();
        }
        return null;
    }
}

```



## 授权

### 配置文件定义授权

配置文件

```ini
# 定义用户及其所拥有的角色
[users]
zs=123456,role1
jssd=root,role2

# 定义角色及其拥有的某个资源
[roles]
role1=add,delete,update
role2=find
```

测试方法

```java
package pers.jssd.shiro04;

import org.apache.shiro.SecurityUtils;
import org.apache.shiro.authc.AuthenticationException;
import org.apache.shiro.authc.UsernamePasswordToken;
import org.apache.shiro.authz.AuthorizationException;
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
        try {
            // 6. 进行身份验证
            subject.login(token);
            // 7. 判断是否认证成功
            if (subject.isAuthenticated()) {
                System.out.println("登录成功");
            }
        } catch (AuthenticationException e) {
            e.printStackTrace();
        }


        // 判断用户是否拥有某个角色
        boolean role2 = subject.hasRole("role2");
        System.out.println("role2 = " + role2);

        try {
            subject.checkRole("role1");
        } catch (AuthorizationException e) {
            e.printStackTrace();
        }

        // 检测是否拥有某个资源
        boolean add = subject.isPermitted("add");
        System.out.println("add = " + add);

        try {
            subject.checkPermission("add");
        } catch (AuthorizationException e) {
            e.printStackTrace();
        }
    }
}
```

### 注解定义授权(适用于web项目)

```java
@RequiresRoles("管理员")
public void aa(){
    // do something
}
```

### jsp中标签使用授权

```jsp
<!--需要导入一个taglib库-->
<%@ taglib prefix="shiro" uri="http://shiro.apache.org/tags" %>

<shiro:hasPermission name="add">
	<a>添加操作</a>
</shiro:hasPermission>
```

### 自定义realm授权(常用)

自定义realm代码

```java
package pers.jssd.shiro05;

import org.apache.shiro.authc.AuthenticationException;
import org.apache.shiro.authc.AuthenticationInfo;
import org.apache.shiro.authc.AuthenticationToken;
import org.apache.shiro.authc.SimpleAuthenticationInfo;
import org.apache.shiro.authz.AuthorizationInfo;
import org.apache.shiro.authz.SimpleAuthorizationInfo;
import org.apache.shiro.realm.AuthenticatingRealm;
import org.apache.shiro.realm.AuthorizingRealm;
import org.apache.shiro.subject.PrincipalCollection;
import org.apache.shiro.util.ByteSource;

import java.sql.*;
import java.util.ArrayList;
import java.util.List;

/**
 * @author jssdjing@gmail.com
 */
public class DiyRealm extends AuthorizingRealm {

    /**
     * 认证
     */
    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken authenticationToken) throws AuthenticationException {
        String name = (String) authenticationToken.getPrincipal();

        try {
            Class.forName("com.mysql.cj.jdbc.Driver");
            Connection connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/blog?serverTimezone=GMT",
                    "root", "xxxxxx");
            PreparedStatement statement = connection.prepareStatement("select password from user where name = ?");
            statement.setString(1, name);
            ResultSet resultSet = statement.executeQuery();
            if (resultSet.next()) {
                return new SimpleAuthenticationInfo(name, resultSet.getString("password"),
                        ByteSource.Util.bytes("jssd"), "userReal");
            }
        } catch (ClassNotFoundException | SQLException e) {
            e.printStackTrace();
        }
        return null;
    }

    // 授权
    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principalCollection) {
        // 取得用户姓名
        String name = principalCollection.getPrimaryPrincipal().toString();
        System.out.println("name = " + name);

        // 通过姓名查询数据库, 找到用户所对应的角色, 查询角色, 找到所对应的所有功能
        // 假设这是从数据库中查询出来的所有功能
        List<String> list = new ArrayList<>();
        list.add("addUser");
        list.add("deleteUser");
        list.add("find");

        // 讲功能添加到用户中
        SimpleAuthorizationInfo simpleAuthorizationInfo = new SimpleAuthorizationInfo();
        simpleAuthorizationInfo.addStringPermissions(list);

        return simpleAuthorizationInfo;
    }
}

```



配置文件

```ini
[main]

# 配置realm域
jdbcRealm=pers.jssd.shiro05.DiyRealm

# 配置加密算法
credentialsMatcher=org.apache.shiro.authc.credential.HashedCredentialsMatcher
credentialsMatcher.hashAlgorithmName=md5
credentialsMatcher.hashIterations=2

# 指定加密算法
jdbcRealm.credentialsMatcher=$credentialsMatcher

# 指定realm域
securityManager.realms=$jdbcRealm
```



测试代码

```java
package pers.jssd.shiro05;

import org.apache.shiro.SecurityUtils;
import org.apache.shiro.authc.AuthenticationException;
import org.apache.shiro.authc.IncorrectCredentialsException;
import org.apache.shiro.authc.UnknownAccountException;
import org.apache.shiro.authc.UsernamePasswordToken;
import org.apache.shiro.config.IniSecurityManagerFactory;
import org.apache.shiro.mgt.SecurityManager;
import org.apache.shiro.subject.Subject;
import org.apache.shiro.util.Factory;

/**
 * @author jssdjing@gmail.com
 */
public class Test {
    public static void main(String[] args) {
        // 1. 解析shiro文件
        Factory<SecurityManager> factory = new IniSecurityManagerFactory("classpath:shiro-jdbc3.ini");
        // 2. 取得安全管理
        SecurityManager securityManager = factory.getInstance();
        // 3. 把安全管理设置到运行环境中
        SecurityUtils.setSecurityManager(securityManager);
        // 4. 获取主题域对象 subject
        Subject subject = SecurityUtils.getSubject();
        // 5. 取得用户输入令牌
        UsernamePasswordToken token = new UsernamePasswordToken("jssda", "1111");
        try {
            // 6. 进行身份验证
            subject.login(token);
            // 7. 判断是否认证成功
            if (subject.isAuthenticated()) {
                System.out.println("登录成功");

                System.out.println("是否有addUser功能?");
                boolean addUser = subject.isPermitted("addUser");
                System.out.println("addUser = " + addUser);

                System.out.println("是否有deleteUser功能?");
                boolean deleteUser = subject.isPermitted("deleteUser");
                System.out.println("deleteUser = " + addUser);

                System.out.println("是否有find功能?");
                boolean find = subject.isPermitted("find");
                System.out.println("find = " + find);
            }
        } catch (IncorrectCredentialsException e) {
            e.printStackTrace();
            System.out.println("密码错误");
        } catch (UnknownAccountException e) {
            e.printStackTrace();
            System.out.println("用户名不正确");
        } catch (AuthenticationException e) {
            e.printStackTrace();
        }
    }
}

```

