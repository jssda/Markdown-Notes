[TOC]

# 创建数据库其他对象

## 视图

### 什么是视图

可以通过创建表的视图来表现数据的逻辑子集或数据的集合. 视图是基于表或另一个视图的逻辑表, 一个视图并不包含他自己的数据, 他像一个窗口, 通过该窗口可以查看或改变表中的数据. 视图基于其上的表称为基表. 

### 视图的优越性

- 视图限制数据的访问, 因为视图能够选择性的显示表中的列
- 视图可以用来构成简单的查询以取回复杂查询的结果. 例如, 视图能用于从多表中查询信息, 而用户不必知道怎样写连接语句.
- 视图对特别的用户和应用程序提供数据独立性, 一个视图可以从几个表中取回数据. 

### 视图类型

| 特性                | 简单视图 | 复杂视图   |
| ------------------- | -------- | ---------- |
| 表的数目            | 一个     | 一个或多个 |
| 包含函数            | 无       | 有         |
| 包含数据分组        | 无       | 有         |
| 通过视图进行DML操作 | 是       | 不允许     |

## 创建一个简单视图

示例: 创建一个视图, 视图中包含部门id为80的员工id, 名字以及薪水

```sql
 create view emp_view as select employee_id, last_name, salary from employees;
```

### 用子查询中的列别名创建视图

如果在创建视图的查询语句中含有别名, 那么列别名将作为视图的别名

示例: 创建一个视图, 包含部门id为50的员工id使用ID_NUMBER命名该列, 包含员工名字使用name命名该列, 包含员工年薪使用ANN_salary命名该列

```sql
create view emp_view2 as select employee_id ID_NUMBER, last_name name, salary ANN_SALARY from employees

SQL> select * from emp_view2;
ID_NUMBER NAME                      ANN_SALARY
--------- ------------------------- ----------
      100 King                        24000.00
      101 Kochhar                     17000.00
      102 De Haan                     17000.00
      103 Hunold                       9000.00
```

## 从视图中取回数据

与表查询使用相同

## 创建一个复杂视图

示例: 创建一个视图, 包含每个部门的部门名称, 部门最低薪水, 部门最高薪水, 以及部门的平均薪水

```sql
create view myview as
select d.department_name dname, min(salary) min_salary, max(salary) max_salary, avg(salary) avg_salary
from employees e
inner join departments d
on e.department_id = d.department_id
group by d.department_name;

SQL> select * from myview;
DNAME                          MIN_SALARY MAX_SALARY AVG_SALARY
------------------------------ ---------- ---------- ----------
Administration                       4400       4400       4400
Accounting                           8300      12008      10154
Purchasing                           2500      11000       4150
Human Resources                      6500       6500       6500
IT                                   4200       9000       5760
Public Relations                    10000      10000      10000
Executive                           17000      24000 19333.3333
```

### 指定列别名创建

```sql
create view myview(dname, minsal, maxsal, savsal) as
select d.department_name dname, min(salary) min_salary, max(salary) max_salary, avg(salary) avg_salary
from employees e
inner join departments d
on e.department_id = d.department_id
group by d.department_name;
```

## 视图中DML操作的执行规则

如果视图中包含下面的部门就不能修改数据

- 组函数
- Group by字句
- distinct 关键字
- 用表达式定义的列

执行DML操作与表相同, 不作赘述

### 拒接DML操作

我们可以手动禁止视图的DML操作

```sql
create or replace view myview3 (employee_number, employee_name, job_title)
as select employee_id, last_name, job_id
from employees
where department_id = 10
with read only; -- 只读
```

## 删除视图

**删除视图不会丢失数据**

```sql
drop view myview;
```

## 内建视图

### 什么是内建视图

- 内建视图是一个带有别名(或相关名)的可以在SQL语句中使用的子查询.
- 一个主查询的在FROM子句中指定的子查询就是一个内建视图

内建视图: 内建视图由位于FROM字句中命名了别名的子查询创建, 该子查询定义一个可以在主查询中引用的数据源.

示例: 显示那些雇员低于他们部门最高薪水的雇员名字, 薪水, 部门号, 和他们部门最高的薪水

```sql
select e.last_name, e.salary, e.department_id, t.salary 最高薪水
from employees e, (select department_id, max(salary) salary from employees group by department_id) t
where e.department_id = t.department_id
and e.salary < t.salary;

LAST_NAME                     SALARY DEPARTMENT_ID       最高薪水
------------------------- ---------- ------------- ----------
Higgins                     12008.00           110      12008
Gietz                        8300.00           110      12008
Wang                        10000.00           290      10000
```

## Top-N分析

### 什么是Top-N分析(排名)

Top-N查询在需要一个条件, 从表中显示最前面的N条记录或最后面的N条记录是有用的. 该结果可以用于进一步分析, 例如, 用TOC-N分析你可以执行下面的查询类型

- 在中挣钱最多的三个人
- 公司中最新的四个成员
- 销售产品最多的两个销售代表
- 过去6个月中销售最好的3种产品

### 执行Top-N分析

Top-N查询使用一个带有下面描述的元素的一致嵌套查询结构

- 子查询或者内建视图产生数据的排序列表, 该子查询或者内建视图包含ORDER BY字句来确保排序以想要的顺序排列. 为了取回最大值, 需要使用DESC参数
- 在最后的结果集中用外查询限制行数, 外查询包括下面的组成部分:
  - ROWNUM伪列, 他从子查询返回的每一行指定一个从1开始的连续值
  - 一个where字句, 他指定被返回的N行, 外where字句必须用以<或者一个<=操作

示例: 从employees表中显示挣钱最多的3人

```sql
SQL> select rownum, last_name, salary from (select last_name, salary from employees order by salary desc) where rownum <= 3;
    ROWNUM LAST_NAME                     SALARY
---------- ------------------------- ----------
         1 King                        24000.00
         2 Kochhar                     17000.00
         3 De Haan                     17000.00
```

示例二: 显示公司中, 资格最老的4个雇员

```sql
SQL> select rownum, last_name, hire_date from (select last_name, hire_date from employees order by hire_date) where rownum <= 4;
    ROWNUM LAST_NAME                 HIRE_DATE
---------- ------------------------- -----------
         1 De Haan                   2001/1/13
         2 Mavris                    2002/6/7
         3 Higgins                   2002/6/7
         4 Baer                      2002/6/7
```

## 分页查询

### 什么是分页查询

当查询的结果集数据量过大时, 可能会导致各种各样的情况发生. 例如: 服务器资源被耗尽. 解决这个问题的策略就是"分页查询"

分页查询的原则: 在内建视图中通过rownum伪列的值判断来指定获取数据的数量

### 示例

查询employees表中的数据, 每次返回10条数据

```sql
SQL> select * from (select rownum num, e.* from employees e) where num between 1 and 10;
       NUM EMPLOYEE_ID FIRST_NAME           LAST_NAME                 EMAIL                     PHONE_NUMBER         HIRE_DATE   JOB_ID         SALARY COMMISSION_PCT MANAGER_ID DEPARTMENT_ID
---------- ----------- -------------------- ------------------------- ------------------------- -------------------- ----------- ---------- ---------- -------------- ---------- -------------
         1         100 Steven               King                      SKING                     515.123.4567         2003/6/17   AD_PRES      24000.00                                      90
         2         101 Neena                Kochhar                   NKOCHHAR                  515.123.4568         2005/9/21   AD_VP        17000.00                       100            90
         3         102 Lex                  De Haan                   LDEHAAN                   515.123.4569         2001/1/13   AD_VP        17000.00                       100            90
         4         103 Alexander            Hunold                    AHUNOLD                   590.423.4567         2006/1/3    IT_PROG       9000.00                       102            60
         5         104 Bruce                Ernst                     BERNST                    590.423.4568         2007/5/21   IT_PROG       6000.00                       103            60
         6         105 David                Austin                    DAUSTIN                   590.423.4569         2005/6/25   IT_PROG       4800.00                       103            60
         7         106 Valli                Pataballa                 VPATABAL                  590.423.4560         2006/2/5    IT_PROG       4800.00                       103            60
         8         107 Diana                Lorentz                   DLORENTZ                  590.423.5567         2007/2/7    IT_PROG       4200.00                       103            60
         9         108 Nancy                Greenberg                 NGREENBE                  515.124.4569         2002/8/17   FI_MGR       12008.00                       101           100
        10         109 Daniel               Faviet                    DFAVIET                   515.124.4169     
```

