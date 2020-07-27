# Oracle的表空间

## 永久表空间

表空间是数据库的逻辑划分, 一个表空间只能属于一个数据库. 所有的数据库对象都存放在指定的表空间中, 但主要存放的是表, 所以称作表空间

## 临时表空间

Oracle 临时表空间只要用来做查询和存放一些缓冲区数据. 临时表空间消耗的主要原因是需要对查询的中间结果进行排序, 重启数据库可以释放临时表空间.

## 创建永久表空间命令

```plsql
create tablespace 永久表空间名称 datafile '永久表空间物理文件位置' size 15M autoextend on next 10M permanent online;
```

例如创建一个表空间名称为JSSDST, 大小15M 自动增长大小, 每次增长10M

```plsql
create tablespace 
JSSDTS datafile 'E:\oracle\oradata\orcl\JSSDTS.DBF'
size 15M
autoextend on next 10M
permanent online;
```

运行, PL/SQL工具底部出现Done in xxx seconds 的时候, 执行成功

# Oracle的用户

Oracle创建用户的时候, 必须有一个表空间存在

## 命令创建用户

```plsql
create user JSSD
  default tablespace JSSDTS
  temporary tablespace TEMP
  profile DEFAULT
  password expire;
```

## 通过PL/SQL Developer工具创建用户

![1563797958386]((六) Oracle创建用户和表空间以及权限管理.assets/1563797958386.png)

增加用户信息, 点击Apply

![1563798015279]((六) Oracle创建用户和表空间以及权限管理.assets/1563798015279.png)

这时刷新user对象, 出现了jssd用户

![1563798113895]((六) Oracle创建用户和表空间以及权限管理.assets/1563798113895.png)

# Oracle的用户权限管理

**Oracle权限复杂, 本文只是简单介绍, 不做深入研究**

创建完用户以后, 使用PL/SQL登录jssd用户

![1563798459608]((六) Oracle创建用户和表空间以及权限管理.assets/1563798459608.png)

登录不上, 报了个错?? 不然, Oracle是一个管理权限非常严格的数据库

![1563798494342]((六) Oracle创建用户和表空间以及权限管理.assets/1563798494342.png)

提示说没有创建session的权限, 那我们给他分配权限

找到用户, 点击edit

![1563798627241]((六) Oracle创建用户和表空间以及权限管理.assets/1563798627241.png)

依然是上次创建用户的界面

![1563799034749]((六) Oracle创建用户和表空间以及权限管理.assets/1563799034749.png)

## 对象权限

>  允许用户操纵一些特定的对象，如读取视图，可更新某些列、执行存储过程等

不同的对象具有不同的对象权限, 对象的拥有者拥有所有权限, 对象的拥有者可以向外分配权限

       对象权限         表   视图   序列   过程
    
       修改(alter)      √            √
    
       删除(delete)     √    √
    
       执行(execute)                        √
    
       索引(index)      √
    
       插入(insert)     √    √
    
       关联(references) √    √
    
       选择(select)     √    √      √
    
       更新(update)     √    √
## 系统权限

> 允许用户执行特定的数据库动作，如创建表、创建索引、连接实例等

登录一个用户就是要给用户分配相应的系统权限

## 角色

一个角色是一组权限的集合, 比如一个resource角色, 就有建表, 建视图, 建索引等权限

> 图片取自
>
> [博客]:<https://www.cnblogs.com/cycsa/archive/2013/05/21/3091199.html>

![1563799631990]((六) Oracle创建用户和表空间以及权限管理.assets/1563799631990.png)

## 为用户分配创建session和建表权限

### 登录用户权限(创建Session)

![1563799845977]((六) Oracle创建用户和表空间以及权限管理.assets/1563799845977.png)

点击左下角Apply就分配了登录权限

命令如下

```plsql
grant create session to JSSD;
```

### 建表权限

让用户能够建表, 我们需要给用户分配create any table权限, 在这里, 直接给用户一个resource角色即可

![1563800075247]((六) Oracle创建用户和表空间以及权限管理.assets/1563800075247.png)

左下角应用即可分配完毕

命令如下

```plsql
grant resource to JSSD;
```

