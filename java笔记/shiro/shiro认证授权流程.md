# shiro认证授权流程

## 认证流程

### 图示

![image-20191104100517201](https://raw.githubusercontent.com/jssda/picbed/master/image-20191104100517201.png)

### 文字流程

1. 通过配置文件建立SecurityManager环境
2. 通过SecurityUtil取得subject对象
3. 调用subject.login()对象, 开始执行认证流程
4. subject.login调用SecurityManager.login执行认证
5. SecurityManager最终调用 ModularRealmAuthenticator 进行认证
6. ModulerRealmAuthenticator调用Realm取得认证信息
7. Realm根据传入的Token, 查找数据源中是否有相对应的信息, 找到之后返回用户信息, 没有找到返回null
8.  ModularRealmAuthenticator 接收realm的信息返回值, 如果不是null, 比对token用户名和密码, 比对成功, 则认证成功, 否则抛出异常

## 授权流程

### 图示

![image-20191104102209814](https://raw.githubusercontent.com/jssda/picbed/master/image-20191104102209814.png)

### 文字流程

1. 通过配置文件建立SecurityManager环境
2. 通过SecurityUtil取得subject对象
3. 调用subject.isPermitted查看是否授权
4. subject.isPermitted调用SecurityManager.isPermitted查看是否授权
5. SecurityManager最终调用 ModularRealmAuthorizer查看授权
6. ModularRealmAuthorizer调用Realm取得授权信息
7.  ModularRealmAuthorizer调用PermissionResolver进行权限串比对 
8.   如果比对后，isPermitted中"permission串"在realm查询到权限数据中，说明用户访问permission串有权限，否则 没有权限，抛出异常。 