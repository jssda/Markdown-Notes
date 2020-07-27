[TOC]

# SQL语句的使用

## select语句的基本使用

```sql
select * from Employees;

select * from employees where EMPLOYEE_ID='100';
```

### 1. select语句中的算数表达式

> +, -, *, /:  可用算数表达对数字和日期进行运算
>
> 注意, 计算是在查询出结果之后再对结果进行计算, 并不会对表中的数据进行该表

对日期进行操作的时候, 我们只能对DATE和TIMESTAMP数据类型进行操作

实例一: 计算目前距离employee_id为100的员工入职时间有多少天

```sql
SQL> select (sysdate - hire_date) || '天' from employees where employee_id=100;
(SYSDATE-HIRE_DATE)||'天'
-------------------------------------------
5885.44689814814814814814814814814814815天
```

实例二: 计算所有员工全年薪水加100 之后的结果

```sql
SQL> select employee_id, first_name, (salary * 12) + 100 from employees;
EMPLOYEE_ID FIRST_NAME           (SALARY*12)+100
----------- -------------------- ---------------
        100 Steven                        288100
        101 Neena                         204100
        102 Lex                           204100
        103 Alexander                     108100
```

示例三: 计算所有员工薪水加100之后的全年薪水

```sql
SQL> select employee_id, first_name, (salary + 100) * 12 from employees;
EMPLOYEE_ID FIRST_NAME           (SALARY+100)*12
----------- -------------------- ---------------
        100 Steven                        289200
        101 Neena                         205200
        102 Lex                           205200
        103 Alexander                     109200
```

### 2. 定义空值

> 空值是一个未分配的, 未知的, 或不适用的值, 空值既不是0, 也不是空格

1. 如果一行中缺少某个数据, 我们可以把它列为空值
2. 如果算数表达式中的算子有空值, 那么结果一定为空值

实例一: 查看employee_id为100的员工commission_pct是多少

```sql
SQL> select employee_id,last_name, commission_pct from employees where employee_id = 100;
EMPLOYEE_ID LAST_NAME                 COMMISSION_PCT
----------- ------------------------- --------------
        100 King                      
```

实例二: 对employee_id为100的员工佣金进行计算

```sql
SQL> select employee_id,last_name, commission_pct * 12 from employees where employee_id = 100;
EMPLOYEE_ID LAST_NAME                 COMMISSION_PCT*12
----------- ------------------------- -----------------
        100 King                      
```

可以看到, 并没有数值

### 3. 定义别名

> 别名就是标题列的别名, 使用AS关键字, 如果别名中包含特殊字符, 则需要使用("")将别名括起来
>
> AS关键字可以不写, 默认使用空格就行

实例:

```sql
SQL> select last_name, salary as 工资 from employees where employee_id=100;
LAST_NAME                         工资
------------------------- ----------
King                        24000.00
```

```sql
SQL> select last_name, salary 工资 from employees where employee_id=100;
LAST_NAME                         工资
------------------------- ----------
King                        24000.00
```

### 4. 连字运算符

> 连字运算就是两个字符串连接, 也可以和算数运算结果或表达式进行连接
>
> 关键字为||

实例一: first_name和last_name进行连接

```sql
SQL> select first_name || last_name from employees where employee_id=100;
FIRST_NAME||LAST_NAME
---------------------------------------------
StevenKing
```

实例二: 连接字符串常量

```sql
SQL> select first_name || ', '|| last_name from employees where employee_id=100;
FIRST_NAME||','||LAST_NAME
-----------------------------------------------
Steven, King
```

实例三: 连接表达式

```sql
SQL> select first_name || ', '|| last_name || '年薪为:' || salary * 12 from employees where employee_id=100;

FIRST_NAME||','||LAST_NAME||'?
--------------------------------------------------------------------------------
Steven, King年薪为:288000
```

### 5. 文字字符串

> 文字字符串是一个用单引号引起来的字符串, 常用于表示一个文字字符串. 与双引号不同的是, 双引号常用于表示别名

实例: 显示所有雇员的名字和工作代码, 使用is a 连接查询结果。列标题用Employee Details。

```sql
SQL> select last_name || ' is a ' || job_id as "Employee Details" from employees;
Employee Details
-----------------------------------------
Abel is a SA_REP
Ande is a SA_REP
Atkinson is a ST_CLERK
Austin is a IT_PROG
Baer is a PR_REP
Baida is a PU_CLERK
Banda is a SA_REP
Bates is a SA_REP
```

### 6. 去除重复数据

> 去除重复数据关键字是distinct, 他会去除查询中重复的数据

实例: 查询员工表中所有的deparment_id

```sql
SQL> select department_id from employees;
DEPARTMENT_ID
-------------
		...
		...
           70
          110
          110
107 rows selected
```

去除关键字之后的数据

```sql
SQL> select distinct department_id from employees;
DEPARTMENT_ID
-------------
          100
           30
           90
           20
           70
          110
           50
           80
           40
           60
           10
12 rows selected
```

## 约束和排序数据

### 1. 用选择限制行

> 使用where字句限制查询到的数据

查询语法

```sql
select *|{[DISTINCT] column|expression [alias],...}
from table
[where condition(S)];
```

condition: 由列名, 表达式, 常数和比较操作组成

实例: 查询雇员的last_name, job_id和department号, 要求这些雇员的department_id是90

```sql
SQL> select last_name, job_id, department_id from employees where department_id=90;
LAST_NAME                 JOB_ID     DEPARTMENT_ID
------------------------- ---------- -------------
King                      AD_PRES               90
Kochhar                   AD_VP                 90
De Haan                   AD_VP                 90
```

### 2. 字符串和日期

- 字符串和日期的值放在单引号中
- 字符值区分大小写, 日期值是格式敏感的
- 日期的默认格式是DD-MON-RR
- 中文版Oracle与英文版Oracle对于日期的月份格式有区别, 中文版的用1月, 英文版的为月份的简写：如January Jan
- 在where字句中，字符串和日期必须包含在单引号（‘’）中。但是，数字常熟不应该包含在单引号中。所有的字符搜索是大小写敏感的。
- Oracle数据库内部以内部数字格式存储日期，表示为：世纪、年、日、小时、分、秒。默认的日期格式是DD-MON-RR。

实例一：查询员工表中名字为King的员工的工作编号。

```sql
SQL> select job_id from employees where last_name='King';
JOB_ID
----------
SA_REP
AD_PRES

SQL> select job_id from employees where last_name='KING';
JOB_ID
----------
```

可以看到，在字符搜索的过程中，是大小写敏感的。

实例二：查询2006年1月24日入职的员工的姓名、部门编号、以及工作ID。

```sql
SQL> select last_name, department_id, job_id
  2  from employees
  3  where hire_date='24-1月-06';
  
LAST_NAME                 DEPARTMENT_ID JOB_ID
------------------------- ------------- ----------
Fox                                  80 SA_REP
Taylor                               50 SH_CLERK
```

注意查询日期的时候日期格式

### 3. 比较条件

| 运算               | 含义     |
| ------------------ | -------- |
| =                  | 等于     |
| >                  | 大于     |
| \>=                | 小于等于 |
| <                  | 小于     |
| <=                 | 小于等于 |
| <> \|\| != \|\| ^= | 不等于   |

实例：查询员工薪水小于等于3000的员工的姓名与薪水

```sql
SQL> select last_name,salary
  2  from employees
  3  where salary <= 3000;
LAST_NAME                     SALARY
------------------------- ----------
Baida                        2900.00
Tobias                       2800.00
Himuro                       2600.00
Colmenares                   2500.00
```

### 4. 其他比较条件

| 操作         | 含义                   |
| ------------ | ---------------------- |
| BETWEEN…AND… | 在两个值之间           |
| IN(set)      | 匹配一个任意值列表     |
| LIKE         | 模糊匹配一个字符值模板 |
| IS NULL      | 是一个空值             |

#### 使用BETWEEN…AND…条件

> 使用BETWEEN…AND…条件的时候，BETWEEN后面跟随的是下限，AND后面跟随的是上限

实例: 查询薪水在\$2500和\$3500(包含2500和3500)那些雇员的姓名和薪水

```sql
SQL> select last_name, salary
  2  from employees
  3  where salary between 2500 and 3500;
LAST_NAME                     SALARY
------------------------- ----------
Khoo                         3100.00
Baida                        2900.00
Tobias                       2800.00
Himuro                       2600.00
```

注意, 使用BETWEEN….AND...条件, 实际上等同于(a>=下限ANDa<=上限), 所以使用BETWEEN...AND...并没有性能上的提高, 只是逻辑上简单了

#### 使用IN条件

> IN条件: 用IN条件在指定的一组值中进行选择. IN(...) 实际上是由Oracle服务器转变为一组OR条件: a=value1 or a=value2 or a=value3, 所以使用IN(...)并没有得到性能的提高, 只是逻辑上简单了

实例: 查询所有经理号为100、101或201的雇员的employee numbers，last_names,salary.

```sql
SQL> select employee_id, last_name, salary
  2  from employees
  3  where manager_id in (100, 101, 201);
EMPLOYEE_ID LAST_NAME                     SALARY
----------- ------------------------- ----------
        101 Kochhar                     17000.00
        102 De Haan                     17000.00
        114 Raphaely                    11000.00
        120 Weiss                        8000.00
        121 Fripp                        8200.00
        122 Kaufling                     7900.00
```

#### 使用LIKE条件

> 使用LIKE条件可以进行模糊匹配
>
> 1. %匹配任意个字符
> 2. _匹配一个字符

实例一: 查询employees表中名字以一个大写字母S开始的雇员的名字

```sql
SQL> select last_name
  2  from employees
  3  where last_name like 'S%';
LAST_NAME
-------------------------
Sarchand
Sciarra
Seo
Sewall
Smith
Smith
Stiles
```

实例二: 查询所有在2005年进入本公司的雇员的名字和受雇日期

```plsql
SQL> select last_name, hire_date
  2  from employees
  3  where hire_date like '%05';
LAST_NAME                 HIRE_DATE
------------------------- -----------
Kochhar                   2005/9/21
Austin                    2005/6/25
Chen                      2005/9/28
Sciarra                   2005/9/30
Baida                     2005/12/24
Tobias                    2005/7/24
```

> ESCAPE关键字: 指定一个符号为转义符号

比如, 要模糊匹配一个带有下划线的字符串, 可以这样写

```plsql
LIKE '%SA\_%' ESCAPE '\';
```

此句将\声明为了转义字符

实例: 查询员工表中工作ID中包含SA_的员工姓名和工作ID

```sql
SQL> select last_name, job_id
  2  from employees
  3  where job_id like '%SA\_%' escape '\';
LAST_NAME                 JOB_ID
------------------------- ----------
Abel                      SA_REP
Ande                      SA_REP
Banda                     SA_REP
Bates                     SA_REP
Bernstein                 SA_REP
Bloom                     SA_REP
```

#### IS NULL条件

> 判断是否为空
>
> IS NOT NULL 判断是否不为空

实例: 查询所有没有佣金的雇员的last_name, job_id, commission

```plsql
SQL> select last_name, job_id, commission_pct
  2  from employees
  3  where commission_pct is null;
LAST_NAME                 JOB_ID     COMMISSION_PCT
------------------------- ---------- --------------
King                      AD_PRES    
Kochhar                   AD_VP      
De Haan                   AD_VP      
Hunold                    IT_PROG    
Ernst                     IT_PROG    
```

实例: 查询所有有佣金的雇员的last_name, job_id, commission

```plsql
SQL> select last_name, job_id, commission_pct
  2  from employees
  3  where commission_pct is not null;
LAST_NAME                 JOB_ID     COMMISSION_PCT
------------------------- ---------- --------------
Russell                   SA_MAN               0.40
Partners                  SA_MAN               0.30
Errazuriz                 SA_MAN               0.30
Cambrault                 SA_MAN               0.30
Zlotkey                   SA_MAN               0.20
Tucker                    SA_REP               0.30
```

###  5. 逻辑条件关系

| 运算 | 含义                                        |
| ---- | ------------------------------------------- |
| AND  | 如果两个组成部分条件都为真, 返回true        |
| OR   | 如果两个组成部分任意一个为真, 返回true      |
| NOT  | 如果跟随的条件为假, 返回true, 否则返回false |

#### AND条件

实例: 查询工作岗位包含字符串MAN并且收入大于$10000的那些雇员的编号, 工作ID, 名字以及薪水

```sql
SQL> select employee_id, job_id, last_name, salary
  2  from employees
  3  where job_id like '%MAN%' and salary > 10000;
EMPLOYEE_ID JOB_ID     LAST_NAME                     SALARY
----------- ---------- ------------------------- ----------
        114 PU_MAN     Raphaely                    11000.00
        145 SA_MAN     Russell                     14000.00
        146 SA_MAN     Partners                    13500.00
```

#### OR条件

实例: 查询任何job_id中包含MAN或者收入大于等于$10000的雇员编号, 工作ID, 名字以及薪水

```sql
SQL> select employee_id, job_id, last_name, salary
  2  from employees
  3  where job_id like '%MAN%' or salary > 10000;
EMPLOYEE_ID JOB_ID     LAST_NAME                     SALARY
----------- ---------- ------------------------- ----------
        100 AD_PRES    King                        24000.00
        101 AD_VP      Kochhar                     17000.00
        102 AD_VP      De Haan                     17000.00
        108 FI_MGR     Greenberg                   12008.00
        114 PU_MAN     Raphaely                    11000.00
```

#### NOT条件

NOT就是取反, 可以适用于任何一种条件之前

实例: 查询那些工作岗位不是IT_PROG, ST_CLERK或者SA_REP的雇员名字和工作岗位.

```sql
SQL> select last_name, job_id
  2  from employees
  3  where job_id not in('IT_PROG', 'ST_CLEARK', 'SA_REP');
LAST_NAME                 JOB_ID
------------------------- ----------
Atkinson                  ST_CLERK
Baer                      PR_REP
Baida                     PU_CLERK
Bell                      SH_CLERK
```

### 6. 优先规则

| 求值顺序 |                               |
| -------- | ----------------------------- |
| 1        | 算数运算                      |
| 2        | 连字操作                      |
| 3        | 比较运算                      |
| 4        | IS [NOT] NULL, LIKE, [NOT] IN |
| 5        | [NOT] BETWEEN                 |
| 6        | NOT 逻辑条件                  |
| 7        | AND 操作                      |
| 8        | OR 操作                       |

优先规则定义表达是求值和计算的顺序, 表中列出了默认的优先顺序, 你可以用圆括号括住你想要先计算的表达式来覆盖默认的优先顺序

实例一: 查询工作岗位是SA_REP或者工作岗位是AD_PRES并且薪水大于15000的员工姓名, 工作ID以及薪水

```sql
SQL> select last_name, job_id, salary
  2  from employees
  3  where job_id='SA_REP' or (job_id='AD_PRES' and salary > 15000);
LAST_NAME                 JOB_ID         SALARY
------------------------- ---------- ----------
King                      AD_PRES      24000.00
Tucker                    SA_REP       10000.00
Bernstein                 SA_REP        9500.00
Hall                      SA_REP        9000.00
```

实例二: 查询工作岗位是SA_REP或者是AD_PRES并且他们的薪水大于15000的员工姓名, 工作ID以及薪水

```sql
SQL> select last_name, job_id, salary
  2  from employees
  3  where (job_id='SA_REP' or job_id='AD_PRES') and salary > 15000;
LAST_NAME                 JOB_ID         SALARY
------------------------- ---------- ----------
King                      AD_PRES      24000.00
```

### 7. 使用ORDER BY字句排序

> 用ORDER BY字句排序
>
> ASC: 升序排序, 默认
>
> DESC: 降序排序

#### order by语法

```sql
ORDER BY 字句在SELECT语句的最后

ORDER BY 后侧指定需要排序的排序列
ASC 以升序排序行
DESC 以降序排序行
```

#### 升序规则

1. 对于数组值, 小的值排在前面显示--例如, 1-99
2. 对于日期, 早的日期在前面显示--例如, 01-01-92在01-01-95前面
3. 对于字符值, 依字母顺序显示--例如, A第一, Z最后
4. 对于空值, 升序排序的时候显示在最后, 降序排序时显示在最前面

实例一: 以日期排序, 日期小的排在最前面

```sql
SQL> select last_name, hire_date, salary, commission_pct
  2  from employees
  3  order by hire_date;
LAST_NAME                 HIRE_DATE       SALARY COMMISSION_PCT
------------------------- ----------- ---------- --------------
De Haan                   2001/1/13     17000.00 
Gietz                     2002/6/7       8300.00 
Baer                      2002/6/7      10000.00 
Mavris                    2002/6/7       6500.00 
Higgins                   2002/6/7      12008.00 
Faviet                    2002/8/16      9000.00 
Greenberg                 2002/8/17     12008.00 
Raphaely                  2002/12/7     11000.00 
```

实例二: 有空值的排序. 有空值的排在最后

```sql
SQL> select last_name, hire_date, salary, commission_pct
  2  from employees
  3  order by commission_pct;
LAST_NAME                 HIRE_DATE       SALARY COMMISSION_PCT
------------------------- ----------- ---------- --------------
Cambrault                 2007/10/15    11000.00           0.30
Doran                     2005/12/15     7500.00           0.30
King                      2004/1/30     10000.00           0.35
Sully                     2004/3/4       9500.00           0.35
McEwen                    2004/8/1       9000.00           0.35
Russell                   2004/10/1     14000.00           0.40
King                      2003/6/17     24000.00 
Kochhar                   2005/9/21     17000.00 
De Haan                   2001/1/13     17000.00 
```

实例三: 按照字母排序

```sql
SQL> select last_name, hire_date, salary, commission_pct
  2  from employees
  3  order by last_name;
LAST_NAME                 HIRE_DATE       SALARY COMMISSION_PCT
------------------------- ----------- ---------- --------------
Abel                      2004/5/11     11000.00           0.30
Ande                      2008/3/24      6400.00           0.10
Atkinson                  2005/10/30     2800.00 
Austin                    2005/6/25      4800.00 
Baer                      2002/6/7      10000.00 
Baida                     2005/12/24     2900.00 
Banda                     2008/4/21      6200.00           0.10
Bates                     2007/3/24      7300.00           0.15
Bell                      2004/2/4       4000.00 
```

#### 使用列号排序

使用查询投影的列的号码排序

```sql
SQL> select last_name, hire_date, salary, commission_pct
  2  from employees
  3  order by last_name;
  
  对于这个语句来说, 使用last_name排序可以这样写
  select last_name, hire_date, salary, commission_pct
  from employees
  order by 1;
  
  使用入职时间排序的话, 可以这样写
  select last_name, hire_date, salary, commission_pct
  from employees
  order by 2;
```

#### 使用列的别名来排序

别名是可以用来指定排序的

```sql
SQL> select last_name, hire_date, salary 工资, commission_pct
  2  from employees
  3  order by 工资;
LAST_NAME                 HIRE_DATE           工资 COMMISSION_PCT
------------------------- ----------- ---------- --------------
Olson                     2007/4/10      2100.00 
Markle                    2008/3/8       2200.00 
Philtanker                2008/2/6       2200.00 
Landry                    2007/1/14      2400.00 
Gee                       2007/12/12     2400.00 
```

#### 多列排序

多列排序使用逗号分隔

实例: 使用工资和奖金倒序排列 (工资倒序, 同样工资的话奖金倒序)

```sql
SQL> select last_name, hire_date, salary, commission_pct
  2  from employees
  3  order by salary desc, commission_pct desc;
LAST_NAME                 HIRE_DATE       SALARY COMMISSION_PCT
------------------------- ----------- ---------- --------------
King                      2003/6/17     24000.00 
Kochhar                   2005/9/21     17000.00 
De Haan                   2001/1/13     17000.00 
Russell                   2004/10/1     14000.00           0.40
Partners                  2005/1/5      13500.00           0.30
Hartstein                 2004/2/17     13000.00 
Greenberg                 2002/8/17     12008.00 
Higgins                   2002/6/7      12008.00 
Errazuriz                 2005/3/10     12000.00           0.30
```

#### select语句执行顺序如下

1. FROM语句
2. WHERE字句
3. SELECT字句
4. ORDER BY字句



