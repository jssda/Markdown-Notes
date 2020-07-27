# 子查询

## 子查询介绍

### 什么是子查询

子查询是一个select语句, 他是在另一个select语句中的select语句

### 子查询语法

```sql
select select_list
from table
where expr oprator
```

### 注意事项

- 子查询(内查询) 在主查询之前执行一次

- 子查询的结果被用于主查询(外查询)

- 可以将子查询放在许多的sql字句当中, 包括: 

  where字句

  having字句

  from字句

## 使用子查询

### 使用子查询的原则

- 子查询放在圆括号中
- 将子查询放在比较条件的右边
- 在单行子查询中用单行运算符, 在多行子查询中使用多行运算符

示例: 谁的薪水比Abel高

```sql
-- 用子查询实现
SQL> select last_name from employees
  2  where salary > (select salary from employees where last_name='Abel');
  
-- 用内连接实现
select e1.last_name from employees e1, employees e2 
where e1.salary > e2.salary and e2.last_name='Abel';
  
LAST_NAME
-------------------------
King
Kochhar
De Haan
Greenberg
Russell
Partners
Errazuriz
Ozer
Hartstein
Higgins
10 rows selected
```

## 子查询的类型

### 单行子查询

- 仅返回一行
- 使用单行比较符

示例: 显示那些jobID与雇员141相同的雇员名字与jobID

```sql
-- 先找出雇员141的job_id
SQL> select job_id from employees where employee_id = 141;
JOB_ID
----------
ST_CLERK

-- 通过雇员141的job_id 找到与雇员141相同的雇员名字与job_id
SQL> select last_name, job_id from employees
  2  where job_id = (select job_id from employees where employee_id = 141);
LAST_NAME                 JOB_ID
------------------------- ----------
Nayer                     ST_CLERK
Mikkilineni               ST_CLERK
Landry                    ST_CLERK
Markle                    ST_CLERK
```

示例二: 显示job_id与雇员141相同, 并且薪水高于143的那些雇员

```sql
select employee_id, last_name, salary, job_id
from employees
where salary > (select salary from employees where employee_id = 143)
and job_id = (select job_id from employees where employee_id = 141);

EMPLOYEE_ID LAST_NAME                     SALARY JOB_ID
----------- ------------------------- ---------- ----------
        125 Nayer                        3200.00 ST_CLERK
        126 Mikkilineni                  2700.00 ST_CLERK
```

### 在子查询中使用组函数

示例: 显示所有其薪水等于最低薪水的雇员的last_name, job_id 和 salary.

```sql
SQL> select last_name, job_id, salary
  2  from employees
  3  where salary = (
  4  select min(salary) from employees);
LAST_NAME                 JOB_ID         SALARY
------------------------- ---------- ----------
Olson                     ST_CLERK      2100.00
```

### 带子查询的HAVING字句

可以在WHERE子句中使用子查询, 也可以在HAVING子句中使用子查询

示例: 显示所有其最低薪水大于部门50的最低薪水的部门号, 和最低薪水

```sql
DEPARTMENT_ID MIN(SALARY)
------------- -----------
          100        6900
           30        2500
                     7000
           90       17000
           20        6000
           70       10000
          110        8300
```

### 多行子查询

- 返回多于一行
- 使用多行比较符 (IN , ANY, ALL)

### 在多行子查询中使用IN运算符

示例: 查找各部门收入为部门最低的那些雇员, 显示他们的名字, 薪水, 以及部门id

```sql
SQL> select last_name, salary, department_id
  2  from employees
  3  where salary in (
  4  select min(salary) from employees group by department_id);
LAST_NAME                     SALARY DEPARTMENT_ID
------------------------- ---------- -------------
Kochhar                     17000.00            90
De Haan                     17000.00            90
Ernst                        6000.00            60
Lorentz                      4200.00            60
Popp                         6900.00           100
```

### 在多行子查询中使用ANY运算符

< any 意思是小于最大值, >ang的意思是大于最大值

示例: 显示工作岗位不是IT_PROG的雇员, 并且这些雇员的薪水少于IT_PROG工作岗位的雇员的ID, 名字, 工作岗位和薪水

```sql
select employee_id, last_name, job_id, salary
from employees
where job_id <> 'IT_PROG'
and salary < any(
select salary from employees where job_id = 'IT_PROG')

EMPLOYEE_ID LAST_NAME                 JOB_ID         SALARY
----------- ------------------------- ---------- ----------
        154 Cambrault                 SA_REP        7500.00
        160 Doran                     SA_REP        7500.00
        111 Sciarra                   FI_ACCOUNT    7700.00
        112 Urman                     FI_ACCOUNT    7800.00
        122 Kaufling                  ST_MAN        7900.00
        153 Olsen                     SA_REP        8000.00
        159 Smith                     SA_REP        8000.00
```

### 在多行子查询中使用ALL运算

\>ALL 的意思是大于最大值  <ALL的意思是小于最小值

示例: 显示那些薪水低于工作岗位IT_PROG的最低薪水, 并且工作岗位不是IT_PROG的所有雇员

```sql
select employee_id, last_name, job_id, salary
from employees
where job_id <> 'IT_PROG'
and salary < all(
select salary from employees where job_id = 'IT_PROG')

EMPLOYEE_ID LAST_NAME                 JOB_ID         SALARY
----------- ------------------------- ---------- ----------
        128 Markle                    ST_CLERK      2200.00
        136 Philtanker                ST_CLERK      2200.00
        132 Olson                     ST_CLERK      2100.00
```



> 