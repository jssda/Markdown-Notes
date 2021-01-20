

# Mysql 新建用户及授权



## 添加用户

以root用户登录数据库，运行以下命令：

```mysql
create user jssd identified by 'jssd';
```

上面的命令创建了用户zhangsan，密码是zhangsan。在mysql.user表里可以查看到新增用户的信息：

![img](https://raw.githubusercontent.com/jssda/picbed/master/595137-20170511101440707-1111545474.png)

## 授权

命令格式：grant privilegesCode on dbName.tableName to username@host identified by "password";

```mysql
grant all privileges on zhangsanDb.* to zhangsan@'%' identified by 'zhangsan';
flush privileges;
```

上面的语句将zhangsanDb数据库的所有操作权限都授权给了用户zhangsan。

在mysql.db表里可以查看到新增数据库权限的信息：

```
select user, Db, Host, Select_priv, Insert_priv, Update_priv, Delete_priv from mysql.db where user='zhangsan';
```

![img](https://images2015.cnblogs.com/blog/595137/201705/595137-20170511101449051-92414488.png)

也可以通过`show grants`命令查看权限授予执行的命令：

```
show grants for 'zhangsan';
```

**privilegesCode**表示授予的权限类型，常用的有以下几种类型[1]：

- all privileges：所有权限。
- select：读取权限。
- delete：删除权限。
- update：更新权限。
- create：创建权限。
- drop：删除数据库、数据表权限。

**dbName.tableName**表示授予权限的具体库或表，常用的有以下几种选项：

- *.*：授予该数据库服务器所有数据库的权限。
- dbName.*：授予dbName数据库所有表的权限。
- dbName.dbTable：授予数据库dbName中dbTable表的权限。

`username@host`表示授予的用户以及允许该用户登录的IP地址。其中Host有以下几种类型：

- **localhost：**只允许该用户在本地登录，不能远程登录。
- **%：**允许在除本机之外的任何一台机器远程登录。
- **192.168.52.32：**具体的IP表示只允许该用户从特定IP登录。

**password**指定该用户登录时的面。

**flush privileges**表示刷新权限变更。

## 修改密码

运行以下命令可以修改用户密码

```
update mysql.user set password = password('zhangsannew') where user = 'zhangsan' and host = '%';
flush privileges;
```

## 删除用户

运行以下命令可以删除用户：

```
drop user zhangsan@'%';
```

drop user命令会删除用户以及对应的权限，执行命令后你会发现mysql.user表和mysql.db表的相应记录都消失了。

## 常用命令组

**创建用户并授予指定数据库全部权限：适用于Web应用创建MySQL用户**

```
create user zhangsan identified by 'zhangsan';
grant all privileges on zhangsanDb.* to zhangsan@'%' identified by 'zhangsan';
flush  privileges;
```

创建了用户zhangsan，并将数据库zhangsanDB的所有权限授予zhangsan。如果要使zhangsan可以从本机登录，那么可以多赋予localhost权限：

```
grant all privileges on zhangsanDb.* to zhangsan@'localhost' identified by 'zhangsan';
```







#  参考文章

转载于: https://www.cnblogs.com/chanshuyi/p/mysql_user_mng.html