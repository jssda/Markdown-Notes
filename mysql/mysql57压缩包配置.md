# mysql57压缩版安装

## 安装环境

1. win10
2. mysql5.7.29

## 安装过程

>  从MySQL 5.7.18开始，my-default.ini不再包含在分发包中或由分发包安装。
> As of MySQL 5.7.18, my-default.ini is no longer included in or installed by distribution packages.
> If you install on Windows from a Zip archive, you can copy the my-default.ini template file in the base installation directory to my.ini and use the latter as the default option file.

解压

我这里解压到了

```
F:\Program Files\mysql-5.7.29-winx64\
```

在安装目录下手动创建my.ini

```ini
[mysqld]
port = 3306
basedir=F:\Program Files\mysql-5.7.29-winx64\

# MySQL程序安装目录
datadir=D:\MySqlData\
# 数据库文件存放地址

max_connections=200
# 最大连接数
character-set-server=utf8
# 数据库默认编码
default-storage-engine=INNODB
# 数据库默认使用引擎
[mysql]
default-character-set=utf8
# mysql客户端默认的字符集，5.7才有的，5.6以及之前的版本没有default-character-set属性
```

配一下环境变量吧, 不多赘述

初始化数据库

```
mysqld --initialize
```

安装服务

```
mysqld --install mysql57
```

 注意：出现**Service successfully installed** 说明服务已安装成功。

启动服务

```
net start mysql57
```

在data目录下找到.err结尾的文件, 找到这一行, 可以看到我的密码

```
A temporary password is generated for root@localhost: Cd=>EcdBl8mw
```

更改密码

```
先登陆  mysql -uroot -pCd=>EcdBl8mw
更改密码: set password = password('root');
```

至此 安装完成.



