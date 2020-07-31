[TOC]

# Oracle的连接配置

## 文件位置

Oracle目录/product/11.2.0/dbhome_1/NETWORK/ADMIN

## sqlnet.ora文件

名称解析, 通过这个文件来决定怎么样找一个出现的连接字符串

如: sqlplus jssd/oracle@ORCL, 假如使用此命令登录数据库, 就会使用sqlnet.ora来解析ORCL是什么, 是哪个网址, 端口号是什么

sqlnet.ora中配置有一项: NAMES.DIRECTORY_PATH= (TNSNAMES, EZCONNECT)

此配置映射了解析NAMES是TNSNAMES文件, 也就是tnsnames.ora文件

## tnsnames.ora文件

> 用在oracle client端, 用户配置连接数据库别名参数, 就像系统中的hosts文件一样

通过sqlnet.ora文件, 我们知道, 需要使用tnsnames.ora文件对一个实例名进行映射ip地址和端口号配置.

我们来看看配置文件中的内容

```
ORCL =
  (DESCRIPTION =
    (ADDRESS = (PROTOCOL = TCP)(HOST = localhost)(PORT = 1521))
    (CONNECT_DATA =
      (SERVER = DEDICATED)
      (SERVICE_NAME = orcl.168.239.1)
    )
  )
```

### ORCL

客户端连接服务器使用的服务器别名, 注意一定要预行书写, 否则无法识别服务器名

### PROTOCOL

客户端和服务器通信的协议, 一般为TCP, 改内容一般不用修改

### HOST

Oracle服务器端IP地址或者hostname. 确保服务器的监听启动正常

### PORT

数据库正在监听的端口, 此处port的值一定要与数据库监听正在监听的端口一致

## listener.ora文件

用在oracle server端, 可配置Oracle的监听端口

```
LISTENER =
  (DESCRIPTION_LIST =
    (DESCRIPTION =
      (ADDRESS = (PROTOCOL = IPC)(KEY = EXTPROC1521))
      (ADDRESS = (PROTOCOL = TCP)(HOST = localhost)(PORT = 1521))
    )
  )

ADR_BASE_LISTENER = E:\oracle
```

### LISTENER

监听名称, 可以配置多个监听, 多个监听端口号要区分开来

### PROTOCOL

监听协议, 一般使用TCP

### HOST

本机IP地址或者localhostname

### PORT

监听的端口号

# Oracle的监听配置

对listener.ora文件熟悉的童鞋, 建议使用工具进行配置

## 配置监听程序

![1563851953538](https://raw.githubusercontent.com/jssda/picbed/master/1563851953538.png)

![1563852034496](https://raw.githubusercontent.com/jssda/picbed/master/1563852034496.png)

重新配置好了

![1563852089531](https://raw.githubusercontent.com/jssda/picbed/master/1563852089531.png)

![1563852102340](https://raw.githubusercontent.com/jssda/picbed/master/1563852102340.png)

![1563852120237](https://raw.githubusercontent.com/jssda/picbed/master/1563852120237.png)

配置连接协议

![1563852144951](https://raw.githubusercontent.com/jssda/picbed/master/1563852144951.png)

使用标准端口

![1563852154152](https://raw.githubusercontent.com/jssda/picbed/master/1563852154152.png)

![1563852180025](https://raw.githubusercontent.com/jssda/picbed/master/1563852180025.png)

![1563852197439](https://raw.githubusercontent.com/jssda/picbed/master/1563852197439.png)

这时打开编辑器, 就会发现已经重写加载了

![1563852246252](https://raw.githubusercontent.com/jssda/picbed/master/1563852246252.png)

## 配置本地网络服务

> 本地网络服务,就是在同一局域网, 我能访问你的Oracle数据库服务

![1563852372928](https://raw.githubusercontent.com/jssda/picbed/master/1563852372928.png)

没配置过, 添加一个

![1563852392516](https://raw.githubusercontent.com/jssda/picbed/master/1563852392516.png)

需要访问的服务器中tnsnames.ora中配置的服务器别名

![1563852452614](https://raw.githubusercontent.com/jssda/picbed/master/1563852452614.png)

连接协议

![1563852472623](https://raw.githubusercontent.com/jssda/picbed/master/1563852472623.png)

添加服务器主机的IP地址

![1563852531852](https://raw.githubusercontent.com/jssda/picbed/master/1563852531852.png)

可进行连接测试

![1563852550321](https://raw.githubusercontent.com/jssda/picbed/master/1563852550321.png)

更改登录

![1563852597827](https://raw.githubusercontent.com/jssda/picbed/master/1563852597827.png)

![1563852792143](https://raw.githubusercontent.com/jssda/picbed/master/1563852792143.png)

可以更改一个自己想要的网络服务名

![1563852938955](https://raw.githubusercontent.com/jssda/picbed/master/1563852938955.png)

![1563852962253](https://raw.githubusercontent.com/jssda/picbed/master/1563852962253.png)

