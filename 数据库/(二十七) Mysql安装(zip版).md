[TOC]

# 准备工作

MySql8.0 Windows zip包下载地址: [https://cdn.mysql.com//Downloads/MySQL-8.0/mysql-8.0.11-winx64.zip](https://cdn.mysql.com//Downloads/MySQL-8.0/mysql-8.0.11-winx64.zip)

环境: Windows 10

# 安装

## 1. 解压zip到安装目录

比如我的安装目录是: D:\Program\MySql

## 2. 配置文件

在Windwos系统中, 配置文件默认是安装目录下的my.ini, 部分配置需要在初始安装时候配置. 大部分也可以在安装完成之后进行更改. 当然, 极端情况下, 所有的都是可以更改的. 

在安装根目录下添加 my.ini，比如我这里是：D:\Program\MySQL\my.ini，写入基本配置:

```ini
[mysqld]
# Remove leading # and set to the amount of RAM for the most important data
# cache in MySQL. Start at 70% of total RAM for dedicated server, else 10%.
# innodb_buffer_pool_size = 128M
 
# Remove leading # to turn on a very important data integrity option: logging
# changes to the binary log between backups.
# log_bin
 
# These are commonly set, remove the # and set as required.
basedir = D:\Program\MySQL
# 本地数据库存储数据文件
datadir = D:\DBs\MySQL
port = 3306
# server_id = .....
 
 
# Remove leading # to set options mainly useful for reporting servers.
# The server defaults are faster for transactions and fast SELECTs.
# Adjust sizes as needed, experiment to find the optimal values.
# join_buffer_size = 128M
# sort_buffer_size = 2M
# read_rnd_buffer_size = 2M 
 
sql_mode=NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES 
 
character-set-server = utf8mb4
 
performance_schema_max_table_instances = 600
table_definition_cache = 400
table_open_cache = 256
[mysql]
default-character-set = utf8mb4
[client]
default-character-set = utf8mb4
```

注意: 里边的basedir是本地安装目录, datadir 是我数据库数据文件要存放的位置，各项配置需要根据自己的环境进行配置。

## 3. 初始化数据库

在MySql安装目录的bin目录下执行命令:

```sql
mysql --initialize --console
```

执行完成后, 会打印root用户的初始默认密码, 比如: 

```shell
2018-04-20T02:35:01.507037Z 0 [Warning] [MY-010915] [Server] 'NO_ZERO_DATE', 'NO_ZERO_IN_DATE' and 'ERROR_FOR_DIVISION_BY_ZERO' sql modes should be used with strict mode. They will be merged with strict mode in a future release.
2018-04-20T02:35:01.507640Z 0 [System] [MY-013169] [Server] D:\Program\MySQL8\bin\mysqld.exe (mysqld 8.0.11) initializing of server in progress as process 11064
2018-04-20T02:35:01.508173Z 0 [ERROR] [MY-010340] [Server] Error message file 'D:\Program\MySQL\share\english\errmsg.sys' had only 1090 error messages, but it should contain at least 4512 error messages. Check that the above file is the right version for this program!
2018-04-20T02:35:05.464644Z 5 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost: APWCY5ws&hjQ
2018-04-20T02:35:07.017280Z 0 [System] [MY-013170] [Server] D:\Program\MySQL8\bin\mysqld.exe (mysqld 8.0.11) initializing of server has completed
```

其中，第4行的“APWCY5ws&hjQ”就是初始密码，在没有更改密码前，需要记住这个密码，后续登录需要用到。

要是你手贱，关快了，或者没记住，那也没事，删掉初始化的 datadir 目录，再执行一遍初始化命令，又会重新生成的。当然，也可以使用安全工具，强制改密码，用什么方法，自己随意。

## 4. 安装服务

在MySql安装目录的Bin目录下执行命令

```sql
mysql --install [服务名]
```

后面的服务名可以不写，默认的名字为 mysql。当然，如果你的电脑上需要安装多个MySQL服务，就可以用不同的名字区分了，比如 mysql5 和 mysql8。

安装完成之后，就可以通过命令**net start mysql**启动MySQL的服务了。

