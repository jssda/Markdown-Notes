[TOC]

# SQL 定义约束

## 数据库中的约束类型

按照约束列数来分, 可以分为列级约束和表级约束, 一般来说, 表级约束用来约束多个列为一个约束. 比如约束id和name共同组成一个主键, 可使用表级约束

- 非空约束(NOT NULL): 数据不能为空
- 唯一约束(UNIQUE): 数据只能唯一
- 主键约束(PRIMARY KEY): 非空唯一, 表中的主键
- 外键约束(FOREIGN KEY): 强制和一个外键进行联系
- 用户自定义约束(CHECK): 指定一个为真的条件

### 定义约束原则

创建一个约束: 在创建表的同时, 或者在创建表之后都可以定义约束. 可以给约束起名字, 但是约束名不能相同, 必须是唯一的. 如果没有为约束起名字, Oracle服务器将会用默认格式SYS_Cn产生一个名字, 这里n是一个唯一的整数,所以约束名时唯一的.

## 定义非空约束

### 语法

```sql
create table employees(
	employeee_id number(6),
    last_name varchar2(25) not null, --此处定义了一个约束, 由系统起名字
    salary number(8,2),
    commission_pct number(2,2)
    hire_date date constraint emp_hire_date_nn not null, -- 此处定义了一个约束, 自己起名字
    ...
)
```

### 创建表示定义NOT NULL约束

```sql
SQL> create table dept80(id number, name varchar(20) not null, salary number constraint dept80_notn not null);
Table created

SQL> desc dept80;
Name   Type         Nullable Default Comments 
------ ------------ -------- ------- -------- 
ID     NUMBER       Y                         
NAME   VARCHAR2(20)                           
SALARY NUMBER           
```

### 修改表的方式定义not null

```sql
SQL> alter table dept80 add (location_id number);
Table altered

SQL> alter table dept80 modify (location_id number not null);
Table altered

Name        Type         Nullable Default Comments 
----------- ------------ -------- ------- -------- 
ID          NUMBER       Y                         
NAME        VARCHAR2(20)                           
SALARY      NUMBER                                 
LOCATION_ID NUMBER 
```

## 定义唯一约束UNIQUE

### 语法

```sql
create table employees(
	employee_id number(6),
    last_name varchar2(25) not null,
    email varchar2(25),
    salary number(8,2),
    hire_date date 	note null,
    ...
    constraint emp_email_uk unique(emial)
)
```

### 创建表时定义unique约束

```sql
create table dept90(
       id number unique,
       name varchar2(20)
)

-- 或者也取上名字
create table dept90(
	id number constraint dept90_uk_id unique,
    name varchar2(20)
)

-- 或者添加表级约束
create table dept10(
       id number not null,
       name varchar2(20),
       loc_id number,
       constraint dept_uq_number unique(name)
)
```

### 修改表时定义unique约束

```sql
SQL> alter table dept90 modify (name varchar(20) unique);
Table altered
```

## 主键约束Primary key

### 创建表时添加约束

```sql
-- 添加列级约束
create table departments(
	department_id number(4) primary key,
    department_name varchar2(30) constraint dept_name_nn not NULL,
    manager_id number(6),
    location_id
)

-- 或者添加表级约束
create table departments(
	department_id number(4),
    department_name varchar2(30) constraint dept_name_nn not NULL,
    manager_id number(6),
    location_id numbner(6),
    constraint dept_id_pk primary key(department_id)
)
```

### 修改表时添加主键约束

```sql
SQL> alter table dept10 modify(loc_id number primary key);
Table altered
```

### 创建联合主键

联合主键就是多个键共同组成一个主键. 使用表级约束定义

```sql
create table departments(
	department_id number(4),
    department_name varchar2(30) constraint dept_name_nn not NULL,
    manager_id number(6),
    location_id numbner(6),
    constraint dept_id_pk primary key(department_id, department_name)
)
```

## 添加外键约束

**添加外键约束必须使用表级约束**

### 创建表时添加约束

```sql
create table dept20(
    id number primary key,
    d90 number,
    constraint d20_fk foreign key(d90) references dept90(id)
)
```

### 修改表时添加约束

```sql
-- 删除
SQL> alter table dept20 drop constraint d20_fk;
Table altered

-- 重新添加
SQL> alter table dept20 add constraint d20_fk foreign key(id) references dept90(id);
Table altered
```

## 定义CHECK约束

### 创建表时定义check约束

```sql
create table emp3(
       id number primary key,
       name varchar2(20) unique,
       sex varchar2(4) check(sex = '男' or sex = '女')
)

SQL> insert into emp3(id, name, sex) values(1, '美美', '人');
insert into emp3(id, name, sex) values(1, '美美', '人')
ORA-02290: 违反检查约束条件 (HR.SYS_C0010954)

SQL> insert into emp3(id, name, sex) values(1, '美美', '女');
1 row inserted
```

### 修改表添加约束

与其他约束添加相同, 不再赘述

## 启用与禁用约束

### 查看约束

```sql
select constraint_name, constraint_type, search_condition
from user_constraints
where table_name='employees';
```

示例: 

```sql
SQL> select constraint_name, constraint_type, search_condition from user_constraints where table_name='EMP3';
CONSTRAINT_NAME                CONSTRAINT_TYPE SEARCH_CONDITION
------------------------------ --------------- --------------------------------------------------------------------------------
SYS_C0010954                   C               sex = '男' or sex = '女'
SYS_C0010955                   P               
SYS_C0010956                   U               
```

### 禁用约束

```sql
alter table employees disable constraint emp_emp_id_pk cascade;
-- cascade是级联禁用约束, 比如一个B表的外键参照A表的主键, 那么禁用A表的主键是不能禁用的, 需要级联禁用B表的外键
```

示例: 可以看到, 虽然差点存在这个约束,但是不起作用

```sql
SQL> alter table emp3 disable constraint SYS_C0010954;
Table altered

SQL> select constraint_name, constraint_type, search_condition from user_constraints where table_name='EMP3';
CONSTRAINT_NAME                CONSTRAINT_TYPE SEARCH_CONDITION
------------------------------ --------------- --------------------------------------------------------------------------------
SYS_C0010954                   C               sex = '男' or sex = '女'
SYS_C0010955                   P               
SYS_C0010956                   U               

SQL> insert into emp3(id, name, sex) values(2, '大美', '人');
1 row inserted

SQL> select * from emp3;
        ID NAME                 SEX
---------- -------------------- ----
         1 美美                 女
         2 大美                 人
```

## 启用约束

```sql
SQL> delete from emp3 where id = 2;
1 row deleted

SQL> select * from emp3;
        ID NAME                 SEX
---------- -------------------- ----
         1 美美                 女

SQL> alter table emp3 enable constraint SYS_C0010954;
Table altered

SQL> insert into emp3(id, name, sex) values(2, '大美', '人');
insert into emp3(id, name, sex) values(2, '大美', '人')
ORA-02290: 违反检查约束条件 (HR.SYS_C0010954)
```

