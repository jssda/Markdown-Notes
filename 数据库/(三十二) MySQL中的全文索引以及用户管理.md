# 全文索引

全文索引(FULLTEXT INDEX)主要用来查找文本中的关键字， 而不是直接与索引中的值
相比较。 FULLTEXT 索引跟其它索引大不相同， 它更像是一个搜索引擎， 而不是简单的 where
语句的参数匹配。 FULLTEXT 索引配合 match against 操作使用， 而不是一般的 where 语句
加 like。 

全文索引可以从 CHAR、 VARCHAR 或 TEXT 列中作为 CREATE TABLE 语句的一部分
被创建， 或是随后使用 ALTER TABLE 添加。 不过切记对于大容量的数据表， 生成全文索
引是一个非常消耗时间非常消耗硬盘空间的做法 

## 查看索引

```sql
select index from table_name
```

## 修改添加全文索引

### 示例

修改 emp7 表添加 content 列类型为 TEXT

```sql
alter table emp7 add COLUMN content text; 
```

修改 emp7 表， 为 content 列创建全文索引

```sql
alter table emp7 add fulltext emp7_content_fullindex(content) 
```

## 创建表时添加全文索引

### 示例

创 建 emp8 表 包 含 emp_id 列 ， content 列 该 列 类 型 为 text ， 并 为 该 列 添 加 名 为
emp8_content_fulltext 的全文索引

```sql
create table emp8(emp_id int primary key auto_increment,content text,fulltext
emp8_content_fullindex(content)) 
```

## 删除全文索引

删除 emp8 表中名为 emp8_content_full 的索引。 

```sql
drop index emp8_content_fullindex on emp8
```

## 使用全文索引

全 文 索 引 的 使 用 与 其 他 索 引 不 同 。 在 查 询 语 句 中 需 要 使 用 match(column)
against(‘content’) 来检索数据。 

### 全文解析器

全文索引中基本单位是”词”。 分词， 全文索引是以词为基础的， MySQL 默认的分词是
所有非字母和数字的特殊符号都是分词符。 在检索数据时我们给定的检索条件也是词。
MySQL 中默认的全文解析器不支持中文分词。 如果数据含有中文需要更换全文解析器
NGRAM。 

### 使用全文索引

```sql
SELECT 投影列 FROM 表名 WHERE MATCH(全文索引列名) AGAINST(‘搜索内容’) 
```

示例一: 修改 emp8 表， 为 content 列创建名为 emp8_content_full 的全文索引

```sql
alter table emp8 add fulltext emp8_content_full(content) 
```

示例二: 向 emp8 表中插入一条数据 content 的值为”hello,jssd”

```sql
insert into emp8 values(default ,'hello jssd') 
```

查询 emp8 表中内容包含 jssd的数据。

```sql
select * from emp8 where match(content) AGAINST('jssd') 
```

### 更换全文解析器

在创建全文索引时可以指定 ngram 解析器

```sql
ALTER TABLE table_name ADD FULLTEXT index_content(content) WITH PARSER
NGRAM 
```

示例一:
删除 emp8 表中的 emp8_content_full 全文索引

```sql
drop index emp8_content_full on emp8
```

示例二: 修改 emp8 表， 为 content 列添加名称为 emp8_content_full 的全文索引， 并指定 ngram
全文解析器。

```sql
alter table emp8 add fulltext emp8_content_full(content) with parser ngram;
```

示例三: 向 emp8 表中添加一条数据 content 值为”你好， 小菜鸟”

```sql
insert into emp8 values(default ,'你好， 小菜鸟') 
```

示例四: 查询 emp8 表中内容包含”小菜鸟”的数据

```sql
select * from emp8 where match(content) AGAINST('小菜鸟') 
```

# MySQL中的用户管理

MySQL是一个多用户的数据库系统， 按权限， 用户可以分为两种： root 用户， 超级管
理员， 和由 root 用户创建的普通用户 

## MySQL创建用户

```sql
create user username identified by 'password'
```

## 查看用户

```sql
select use, nost from mysql.user
```

## 示例

创建一个 u_sxt 的用户， 并查看创建是否成功。

```sql
select user,host from mysql.user
create user u_sxt IDENTIFIED by 'sxt' 
```

## 分配权限

新用户创建完后是无法登陆的， 需要分配权限。

```sql
GRANT ON .TO @IDENTIFIED BY ""
GRANT ALL PRIVILEGES ON *.* TO 'username'@'localhost' IDENTIFIED BY 'password'
```

| %         | 匹配所有主机                                              |
| --------- | --------------------------------------------------------- |
| localhost | localhost 不会被解析成 IP 地址， 直接通过 UNIXsocket 连接 |
| 127.0.0.1 | 会通过 TCP/IP 协议连接， 并且只能在本机访问；             |
| ::1       | ::1 就是兼容支持 ipv6 的， 表示同 ipv4 的 127.0.0.1       |

### 权限列表

- ALTER: 修改表和索引。
- CREATE: 创建数据库和表。
- DELETE: 删除表中已有的记录。
- DROP: 删除数据库和表。
- INDEX: 创建或删除索引。
- INSERT: 向表中插入新行。
- SELECT: 检索表中的记录。
- UPDATE: 修改现存表记录。 
- FILE: 读或写服务器上的文件。
- PROCESS: 查看服务器中执行的线程信息或杀死线程。
- RELOAD: 重载授权表或清空日志、 主机缓存或表缓存。
- SHUTDOWN: 关闭服务器。
- ALL: 所有权限， 
- ALL PRIVILEGES 同义词。
- USAGE: 特殊的 "无权限" 权限 

### 示例

为 u_sxt 用户分配只能查询 bjsxt 库中的 employees 表， 并且只能在本机登陆的权限。

```sql
grant select ON bjsxt.employees to 'u_sxt'@'localhost' IDENTIFIED by 'sxt' 
```

### 刷新权限

每当调整权限后， 通常需要执行以下语句刷新权限

```sql
FLUSH PRIVILEGES 
```

### 删除用户

```sql
DROP USER username@localhost 
```

