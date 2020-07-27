[TOC]

# 组函数(聚合函数)

##  组函数介绍

### 什么时组函数

组函数操作行集, 给出每组的结果. 组函数不像单行函数, 组函数对行的集合进行操作, 对每组给出一个结果. 这些集合可能是整个表或者表分成的组. 

### 组函数与单行函数的区别

单行函数对查询到每个结果集做处理, 而组函数只对分组数据做处理. 单行函数对每个结果集返回一个结果, 而组函数对每个分组返回一个结果

### 组函数的类型

- avg平均值
- count 计算
- max 最大值
- min 最小值
- sum 合计

### 使用组函数的原则

- 用于函数的参数的数据类型可以是char, varchar2, number, 或 date.
- 所有组函数忽略空值. 为了用一个值代替空值, 用NVL, VNL2或COALESCE函数

## 组函数的使用

###  使用AVG和SUM函数

1. avg(arg)函数: 对分组数据做平均值运算

   arg: 参数类型只能是数字类型

2. SUM(arg)函数: 对分组数据求和

   arg: 参数只能是数字类型

示例: 求雇员表中的平均薪水与薪水总额

```sql
SQL> select avg(salary), sum(salary) from employees;
AVG(SALARY) SUM(SALARY)
----------- -----------
6461.831775      691416
```

### 使用MIN和MAX函数

1. MIN(arg)函数: 求分组中最小的数据

   arg: 参数类型可以是字符, 数字, 日期

2. MAX(arg)函数: 求分组中最大的数据

   arg: 参数类型可以是字符, 数字, 日期

示例: 求雇员表中最高薪水和最低薪水

```sql
SQL> select max(salary), min(salary) from employees;
MAX(SALARY) MIN(SALARY)
----------- -----------
      24000        2100
```

### COUNT函数:

count函数: 返回一个表中的行数

1. count(*)

   返回表中满足select语句标准的行数, 包括重复行, 包括有空值的行, 入伙where字句中包括在select语句中, count(*) 返回满足where字句条件的行数. 

   示例: 返回查询结果的总行数

   ```sql
   SQL> select count(*) from employees;
     COUNT(*)
   ----------
          107
   ```

2. count(expr)函数

   返回在列中的由expr指定的非空值的数. 

   示例: 显示部门在80中有佣金的雇员人数

   ```sql
   -- 此查询会忽略掉空值
   SQL> select count(commission_pct) from employees where department_id = 80;
   COUNT(COMMISSION_PCT)
   ---------------------
                      34
   ```

3. count(distinct expr)

   剔除重复的关键字

   示例: 观察三种使用方式的区别

   ```sql
   SQL> select count(*) from employees;
     COUNT(*)
   ----------
          107
   
   SQL> select count(department_id) from employees;
   COUNT(DEPARTMENT_ID)
   --------------------
                    106
   
   SQL> select count(distinct department_id) from employees;
   COUNT(DISTINCTDEPARTMENT_ID)
   ----------------------------
                             11
   ```

## 组函数和空值

所有组函数忽略列中的空值

在组函数中使用NVL函数来处理空值

示例一: 计算有佣金的员工的佣金平均值

```sql
SQL> select avg(commission_pct) from employees;
AVG(COMMISSION_PCT)
-------------------
  0.222857142857143
```

示例二: 计算所有员工的佣金平均值

```sql
SQL> select avg(nvl(commission_pct, 0)) from employees;
AVG(NVL(COMMISSION_PCT,0))
--------------------------
        0.0728971962616822
```

##  创建数据组

### 什么是创建数据组

可能需要将查询到的结果集划分为较小的组, 用group by字句实现

### Group by字句的语法

```sql
select column, group_function(column)
from table
[where condition]
[group by group_by_expression]
[order by column];
```

### 使用分组原则

- 如果在select 字句中包含了组函数, 就不能选择单独的结果, 除非单独的列出现在Group by子句中, 如果未能在Group by字句中包含一个字段列表, 你会收到一个错误信息
- 使用where字句, 你可以在划分行成组以前过滤行
- 在Group By字句中必须包含列
- 在Group By字句中你不能用列别名
- 默认情况下, 行以包含在Group By列表中的字段升序排序. 可以用Order by字句覆盖这个默认值

### Group By字句的使用

示例一: 求每个部门的平均薪水

```sql
SQL> select department_id, avg(salary) from employees group by department_id;
DEPARTMENT_ID AVG(SALARY)
------------- -----------
          100 8601.333333
           30        4150
                     7000
           90 19333.33333
           20        9500
           70       10000
          110       10154
           50 3475.555555
           80 8955.882352
           40        6500
           60        5760
           10        4400
12 rows selected
```

示例二: 显示每个部门中付给每个工作岗位的合计薪水的报告

```sql
SQL> select department_id, job_id, sum(salary) from employees group by department_id, job_id;
DEPARTMENT_ID JOB_ID     SUM(SALARY)
------------- ---------- -----------
          110 AC_ACCOUNT        8300
           90 AD_VP            34000
           50 ST_CLERK         55700
           80 SA_REP          243500
```

### GROUP BY 字句的执行顺序

先进行数据查询, 在对数据进行分组, 然会执行组函数

## 约束分组结果

### 什么是HAVING字句

having字句通常与group by 语句联合使用, 用来过滤有group by语句返回的记录集

having字句的存在弥补了where关键字不能与聚合函数联合使用的不足

### having语句的使用

示例一: 显示那些最最高薪水大于10000的部门的部门号和最高薪水

```sql
SQL> select department_id, max(salary) from employees group by department_id having max(salary) > 10000;
DEPARTMENT_ID MAX(SALARY)
------------- -----------
          100       12008
           30       11000
           90       24000
           20       13000
          110       12008
           80       14000
```

示例二: 那些最高薪水大于10000的部门的部门号和平均薪水

```sql
SQL> select department_id, avg(salary) from employees group by department_id having max(salary) > 10000;
DEPARTMENT_ID AVG(SALARY)
------------- -----------
          100 8601.333333
           30        4150
           90 19333.33333
           20        9500
          110       10154
           80 8955.882352
```

## 嵌套组函数

在使用组函数时, 我们也可以根据需要来做组函数的嵌套使用

示例: 显示部门中最大的平均薪水

```sql
-- 先求出每个部门的平均工资
SQL> select avg(salary) from employees group by department_id;
AVG(SALARY)
-----------
8601.333333
       4150
       7000
19333.33333
       9500
      10000
      10154
3475.555555
8955.882352
       6500
       5760
       4400
12 rows selected

-- 再从平均工资中找到最大的
SQL> select max(avg(salary)) from employees group by department_id;
MAX(AVG(SALARY))
----------------
19333.3333333333
```

