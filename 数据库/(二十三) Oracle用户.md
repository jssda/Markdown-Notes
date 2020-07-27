# 用户

## 什么是Oracle用户

Oracle用户是用来连接数据库和访问数据库对象的

## 操作用户

### 创建用户

需要具备创建用户的权限

```sql
-- 使用hr用户, 报错如下
SQL> create user temp1 identified by oracle;
create user temp1 identified by oracle
ORA-01031: 权限不足

-- 切换sys用户之后
SQL> create user temp1 identified by oracle;
User created
```

创建的用户没有指定表空间的化, 使用的是默认表空间

### 使用指定表空间创建用户

示例: 创建一个用户名为u_jssd, 使用jssd作为他的表空间

```sql
SQL> create user u_jssd identified by oracle default tablespace jssd;
User created
```

示例. 添加临时表空间

```sql
SQL> create user u_jssd identified by oracle default tablespace jssd temporary tablespace temp;
```

### 删除用户

```sql
SQL> drop user u_jssd;
User dropped
```

删除用户的同时, 删除用户下的其他对象

```sql
drop user u_jssd cascaed;
```

## 数据控制语言DCL

grant... 授予用户权限

revork... 撤销用户权限

### 授予系统权限

```sql
GRANT create session, create table, create sequence, create view, create index
to scott;
```

#### 授予创建其他对象权限

- create session
- create table
- create sequence
- create view
- create procedure
- create tablespace

```sql
-- 授予创建会话权限
SQL> grant create session to u_jssd;
Grant succeeded

-- 授予创建表权限
grant create table to u_jssd;

-- 授予使用表空间的权限(有时候表空间不是用户默认表空间, 是手动指定的, 这时候应该授予使用表空间的权限
grant unlimited tablespace to u_jssd;
```

###  撤销权限

Revoke 权限 from 用户

示例: 撤销u_jssd用户创建表的权限

```sql
revoke create table from u_jssd;
```

### Oracle中的角色

### 什么是角色

角色就是用户权限的集合, 一个角色拥有若干权限, 把角色分配给用户的时候, 就分配了相应的权限

### 创建角色

```sql
SQL> create role manager;
Role created
```

### 给角色分配权限

```sql
-- 向manager用户添加创建会话, 创建表, 创建视图, 创建序列的权限
SQL> grant create table, create session, create view, create sequence to manager;
Grant succeeded
```

### 授予一个角色给用户

```sql
SQL> grant manager to u_jssd;
Grant succeeded
```

### 撤销用户权限

```sql
SQL> revoke manager from u_jssd;
Revoke succeeded
```

