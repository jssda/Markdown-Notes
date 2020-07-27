# SQL 操纵数据(DML)

数据操纵语言: 当添加, 更新或者删除数据库中的数据时, 需要使用DML语句. DML依据的一个集合构成了一个被称为事务的逻辑单元. 

当完成下面操作时, DML语句被执行:

- 添加新行到表中
- 修改表中的行
- 删除表中的行

## 添加一个新行到表中

### INSERT语句语法

```sql
insert into table[(column [,column...])]
values (value[, value...]);
```

该语法一次只能插入一行

### 指定列添加

示例: 向departments表中添加一条数据. 

```sql
Department_id=280
department_name=Teaching
Manager_id=180
location_id=2000

-- 添加语句, 注意, 必须提交事务, 不然数据会添加到临时的表空间中, 关闭数据库再打开, 仍然没有这个数据
SQL> insert into departments(department_id, department_name, manager_id, location_id)
  2  values(280, 'Teaching', 180, 2000);
1 row inserted
-- 提交事务
SQL> commit;
Commit complete
```

### 完全列添加

示例: 向表中添加一条数据

```sql
department_id=290
department_name=development
manager_id=149
location_id=2000

-- 添加语句, 添加时候的添加顺序必须和建表时候的数据顺序一致
SQL> insert into departments values(290, 'development', 149, 2000);
1 row inserted

-- 提交事务
SQL> commit;
Commit complete
```

## 插入带空值的行

### 隐式方式

直接省略插入的列

示例: 插入一条数据

```sql
SQL> insert into departments(department_id, department_name) values(300, 'A');
1 row inserted

SQL> commit;
Commit complete

SQL> select * from departments where department_id = 300;
DEPARTMENT_ID DEPARTMENT_NAME                MANAGER_ID LOCATION_ID
------------- ------------------------------ ---------- -----------
          300 A                                         
```

### 显式方式

手动插入null

示例

```sql
SQL> insert into departments values(310, 'B', null, null);
1 row inserted

SQL> commit;
Commit complete

SQL> select * from departments where department_id = 310;
DEPARTMENT_ID DEPARTMENT_NAME                MANAGER_ID LOCATION_ID
------------- ------------------------------ ---------- -----------
          310 B                                         
```

## 插入日期值

### 使用系统时间插入

示例: 向employees表中插入一条数据

```sql
SQL> insert into employees(employee_id, first_name, last_name, email, phone_number, hire_date, job_id, salary, commission_pct, manager_id, department_id)
  2  values (300, 'JingJing', 'Wang', 'asdf', null, sysdate, 'IT_PROG', 10000, null, 201, 290);
1 row inserted

SQL> commit;
Commit complete

SQL> select * from employees where employee_id = 300;
EMPLOYEE_ID FIRST_NAME           LAST_NAME                 EMAIL                     PHONE_NUMBER         HIRE_DATE   JOB_ID         SALARY COMMISSION_PCT MANAGER_ID DEPARTMENT_ID
----------- -------------------- ------------------------- ------------------------- -------------------- ----------- ---------- ---------- -------------- ---------- -------------
        300 JingJing             Wang                      asdf                                           2019/7/31 2 IT_PROG      10000.00                       201           290
```

### 使用默认时间格式插入

示例

```sql
SQL> insert into employees(employee_id, first_name, last_name, email, phone_number, hire_date, job_id, salary, commission_pct, manager_id, department_id)
  2  values (301, 'JingJing', 'Wang', 'asdff', null, '01/3月/2019', 'IT_PROG', 10000, null, 201, 290);
1 row inserted

SQL> commit;
Commit complete

SQL> select * from employees where employee_id = 301;
EMPLOYEE_ID FIRST_NAME           LAST_NAME                 EMAIL                     PHONE_NUMBER         HIRE_DATE   JOB_ID         SALARY COMMISSION_PCT MANAGER_ID DEPARTMENT_ID
----------- -------------------- ------------------------- ------------------------- -------------------- ----------- ---------- ---------- -------------- ---------- -------------
        301 JingJing             Wang                      asdff                                          2019/3/1    IT_PROG      10000.00                       201           290
```

### 使用指定时间格式插入

示例

```sql
insert into employees(employee_id, first_name, last_name, email, phone_number, hire_date, job_id, salary, commission_pct, manager_id, department_id)
values (302, 'JingJing', 'Wang', 'asdfff', null, to_date('2019-07-31', 'yyyy-mm-dd'), 'IT_PROG', 10000, null, 201, 290);

SQL> commit;
Commit complete

SQL> select * from employees where employee_id = 302;
EMPLOYEE_ID FIRST_NAME           LAST_NAME                 EMAIL                     PHONE_NUMBER         HIRE_DATE   JOB_ID         SALARY COMMISSION_PCT MANAGER_ID DEPARTMENT_ID
----------- -------------------- ------------------------- ------------------------- -------------------- ----------- ---------- ---------- -------------- ---------- -------------
        302 JingJing             Wang                      asdfff                                         2019/7/31   IT_PROG      10000.00                       201           290
```

## 从另一个表中复制行

- 用一个查询写insert语句
- 不用values字句
- 在查询中列的数目要匹配insert子句中列的数目

### 先创建一个表

```sql
create table EMP(
	id number not null,
    name varchar2(50),
    salary number(8,2),
    commission number(2,2)
)
```

### 示例

将employees表中的job_id中含有REP的工作岗位的数据插入到emp表中

```sql
insert into emp select employee_id, last_name, salary, commission_pct from employees where job_id like '%REP%';
```

## 插入使用默认值

### 插入时使用default插入默认值

```sql
insert into emp values(302, 'Old2', default, null);
```

### 默认插入

如果不指定列来插入, 没有默认值的会插入null值, 有默认值的会插入default值

## 改变表中的数据

### KUPDATE语句语法

```sql
update table
set column=value[, column=value,...]
[where condition];
```

示例: 更新emp表中的id为170的数据, 将名字修改为OLDLU;

```sql
SQL> select * from emp where id = 170;
        ID NAME                                                   SALARY COMMISSION
---------- -------------------------------------------------- ---------- ----------
       170 Fox                                                   9600.00       0.20

SQL> update emp e set e.name = 'oldlu' where e.id = 170;
1 row updated

SQL> select * from emp where id = 170;
        ID NAME                                                   SALARY COMMISSION
---------- -------------------------------------------------- ---------- ----------
       170 oldlu                                                 9600.00       0.20
```

### 用查询更新列

示例: 更新emp表中的雇员165的薪水, 使其和雇员156相同

```sql
SQL> update emp e set e.salary = (select salary from emp where id = 156) where id = 165;

SQL> select id, salary from emp where id in (156, 165);
        ID     SALARY
---------- ----------
       156   10000.00
       165   10000.00
```

## 从表中删除行

### DELETE语句语法

```sql
delete [from] table
[where condition];
```

示例: 删除emp表中id为302的雇员

```sql
SQL> select * from emp where id = 302;
        ID NAME                                                   SALARY COMMISSION
---------- -------------------------------------------------- ---------- ----------
       302 Wang                                                 10000.00 
       302 Old2                                                   300.00 

SQL> delete from emp where id = 302;
2 rows deleted

SQL> commit;
Commit complete
```

### 删除基于另一个表的行

示例: 删除emp表中薪水与IT_PROG岗位相同的薪水

```sql
SQL> select distinct salary from employees where job_id = 'IT_PROG';
    SALARY
----------
   9000.00
  10000.00
   4800.00
   4200.00
   6000.00
   
SQL> delete from emp where salary in (select distinct salary from employees where job_id = 'IT_PROG');
8 rows deleted
```

### 删除所有数据

delete emp;

