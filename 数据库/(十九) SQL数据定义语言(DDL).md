# 数据定义语言(DDL)

> 数据定义语言主要用于操作数据库对象, 比如: 用户对象, 序列对象, 表对象, 索引对象等等

create... 创建数据库对象

drop... 删除数据库对象

alter…. 修改数据库对象

rename... 修改数据库对象名称

## 创建表

```sql
create table dept(
	deptno number(2),
	dname varchar2(14),
	loc varchar2(13)
)
```

### Oracle数据库中的表

- 用户表: 
  - 由用户创建和维护的表的集合
  - 包含用户信息
- 数据字典
  - 由Oracle服务器创建和维护的表的集合
  - 包含数据库信息

#### 常见的数据字典表

- 查看本用户所拥有的表的名称

  ```sql
  select table_name from user_tables;
  ```

- 查看本用户所拥有的不同的对象类型

  ```sql
  select distinct object_type from user_objects;
  ```

- 查看本用户所拥有的表, 视图, 同义词和序列

  ```sql
  select * from user_catalog;
  ```

## 用查询来创建表

示例;  查询创建的表, 相当于把查询到的数据复制到新的表中

```sql
SQL> create table emp2 as select employee_id, last_name, salary, hire_date from employees;
Table created

SQL> desc emp2;
Name        Type         Nullable Default Comments 
----------- ------------ -------- ------- -------- 
EMPLOYEE_ID NUMBER(6)    Y                         
LAST_NAME   VARCHAR2(25)                           
SALARY      NUMBER(8,2)  Y                         
HIRE_DATE   DATE                                   

SQL> select * from emp2;
EMPLOYEE_ID LAST_NAME                     SALARY HIRE_DATE
----------- ------------------------- ---------- -----------
        100 King                        24000.00 2003/6/17
        101 Kochhar                     17000.00 2005/9/21
        102 De Haan                     17000.00 2001/1/13
        103 Hunold                       9000.00 2006/1/3
```

## ALTER TABLE语句

ALTER TABLE语句可以修改表的信息

- 添加一个新列
- 修改一个已经存在的列
- 删除一个列

### 添加一个新的列

#### 语法

```sql
alter table table_name
add (column datatype [default expr]
    [, column datatype]...);
```

#### 使用:

```sql
SQL> alter table emp2 add(first_name varchar2(16));
Table altered

SQL> desc emp2;
Name        Type         Nullable Default Comments 
----------- ------------ -------- ------- -------- 
EMPLOYEE_ID NUMBER(6)    Y                         
LAST_NAME   VARCHAR2(25)                           
SALARY      NUMBER(8,2)  Y                         
HIRE_DATE   DATE                                   
FIRST_NAME  VARCHAR2(16) Y   
```

### 修改列

#### 语法

```sql
alter table table_name
modify (column datatype [default expr]
    [, column datatype]...);
```

#### 使用:

修改数据类型

```sql
SQL> alter table emp2 modify(first_name varchar2(20));
Table altered

SQL> desc emp2;
Name        Type         Nullable Default Comments 
----------- ------------ -------- ------- -------- 
EMPLOYEE_ID NUMBER(6)    Y                         
LAST_NAME   VARCHAR2(25)                           
SALARY      NUMBER(8,2)  Y                         
HIRE_DATE   DATE                                   
FIRST_NAME  VARCHAR2(20) Y 
```

修改默认值

```sql
SQL> alter table emp2 modify(salary number(8,2) default 1000);
Table altered

SQL> desc emp2;
Name        Type         Nullable Default Comments 
----------- ------------ -------- ------- -------- 
EMPLOYEE_ID NUMBER(6)    Y                         
LAST_NAME   VARCHAR2(25)                           
SALARY      NUMBER(8,2)  Y        1000             
HIRE_DATE   DATE                                   
FIRST_NAME  VARCHAR2(20) Y                         

SQL> alter table emp2 modify(salary number(8,2) default null);
Table altered

SQL> desc emp2;
Name        Type         Nullable Default Comments 
----------- ------------ -------- ------- -------- 
EMPLOYEE_ID NUMBER(6)    Y                         
LAST_NAME   VARCHAR2(25)                           
SALARY      NUMBER(8,2)  Y        null             
HIRE_DATE   DATE                                   
FIRST_NAME  VARCHAR2(20) Y  
```

修改列名, 使用rename

```sql
SQL> alter table emp2 rename column employee_id to id;
Table altered

SQL> desc emp2;
Name       Type         Nullable Default Comments 
---------- ------------ -------- ------- -------- 
ID         NUMBER(6)    Y                         
LAST_NAME  VARCHAR2(25)                           
SALARY     NUMBER(8,2)  Y        null             
HIRE_DATE  DATE                                   
FIRST_NAME VARCHAR2(20) Y  
```



### 删除一个列

#### 语法

```sql
alter table table_name
drop column (column datatype [default expr]
    [, column datatype]...);
```

#### 使用:

```sql
SQL> alter table emp2 drop column salary;
Table altered

SQL> desc emp2;
Name       Type         Nullable Default Comments 
---------- ------------ -------- ------- -------- 
ID         NUMBER(6)    Y                         
LAST_NAME  VARCHAR2(25)                           
HIRE_DATE  DATE                                   
FIRST_NAME VARCHAR2(20) Y  
```

## 修改表名

示例: 

```sql
SQL> rename emp2 to emp3;
Table renamed

SQL> select table_name from user_tables;
TABLE_NAME
------------------------------
REGIONS
DEPARTMENTS
JOB_GRADES
EMP
DEPT
EMP3
COUNTRIES
JOB_HISTORY
EMPLOYEES
LOCATIONS
JOBS
11 rows selected
```

## 截断表(TRUNCATE TABLE)

- 将表中所有的数据,再次添加数据的时候, ID从头开始,相当于重置表
- 删除表中的所有数据, 但是保留表的结构
- 在截断表时不能给定条件
- 截断表隐式事务

### 示例

```sql
SQL> truncate table emp3;
Table truncated

SQL> select * from emp3;
     ID LAST_NAME                 HIRE_DATE   FIRST_NAME
------- ------------------------- ----------- --------------------
```

## 删除表

示例: 

```sql
SQL> drop table emp3;
Table dropped

SQL> select table_name from user_tables;
TABLE_NAME
------------------------------
REGIONS
DEPARTMENTS
JOB_GRADES
EMP
DEPT
COUNTRIES
JOB_HISTORY
EMPLOYEES
LOCATIONS
JOBS
10 rows selected
```

