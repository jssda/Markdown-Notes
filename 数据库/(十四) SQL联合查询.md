[TOC]

# 多表查询

> 当查询的数据并不是来源于一个表时, 需要使用夺标链接操作完成查询, 根据不同表中数据之间的关系查询相关联的数据.

## 多表链接方式

### 内连接: 

连接两个表, 通过相等或不等判断链接列, 成为内连接. 在内连接中典型的连接运算有=或<>之类的比较运算符, 包括等值连接和自然连接

1. 等值连接
2. 非等值连接
3. 自连接
4. SQL99: 交叉连接(CROSS JOIN)
5. SQL99: 内连接(INNER JOIN)
6. SQL99: 自然连接(NATURAL JOIN)

### 外连接

在两个表之间的连接, 返回内连接的结果, 同时返回不匹配行的左或右表的连接, 称为左(或右)连接. 返回内连接的结果, 同时返回左或右连接, 称为全外连接.

1. 左外连接
2. 右外连接
3. 全外连接

### 子查询

当一个查询是另一个查询的条件时, 称为子查询

## 笛卡尔乘积

### 什么是笛卡尔乘积

笛卡尔乘积是指在数学中, 两个集合X和Y的笛卡尔乘积(Cartesian product), 又称直积. 表示为X*Y, 第一个对象时X的成员而第二个对象是Y的所有可能有序对的其中一个成员

### 如何避免出现笛卡尔乘积

当一个连接条件无效或被遗漏时, 其结果是一个笛卡尔乘积, 所有行的组合都会被显示. 第一个表中的所有行连接到第二个表中所有行. 一个笛卡尔乘积会产生大量的行, 其结果没有什么用. 应该在WHERE字句中始终包含一个有效的连接条件. 

## 多表连接语法

### 语法结构

使用一个连接从多个表中查询数据

```sql
select table1.column, table2.column
from table1, table2
where table1.column1 = table2.column2  -- 注意, 如果没有连接条件, 会产生笛卡尔乘积
```

### 使用原则

- 在写一个连接表的select语句时, 在列名前面用表名或者表别名可以使语义更清楚, 并且加快数据库访问. 
- 为了连接n个表在一起, 你最少需要n-1个连接条件. 例如, 为了连接4个表, 最少需要3个连接条件

## 等值连接

### 什么时等值连接

等值连接也被称为简单连接或内连接. 是通过等号来判断连接条件中的数据值是否相匹配

示例一: 查询所有雇员名字以及他们所在的部门名称

```sql
SQL> select last_name, department_name
  2  from employees, departments
  3  where employees.department_id = departments.department_id;
LAST_NAME                 DEPARTMENT_NAME
------------------------- ------------------------------
Whalen                    Administration
Fay                       Marketing
Hartstein                 Marketing
Tobias                    Purchasing
```

示例二: 显示名字为Taylor的雇员的名字和部门号

```sql
SQL> select last_name, employees.department_id
  2  from employees, departments
  3  where employees.department_id = departments.department_id
  4  and employees.last_name='Taylor';
LAST_NAME                 DEPARTMENT_ID
------------------------- -------------
Taylor                               80
Taylor                               50
```

## 使用表的别名

### 表别名

可以使用表的别名代替表名, 就像列别名给列起另一个别名一样. 别名有助于保持代码较小, 因此, 使用的存储器也少. 

### 使用表别名的原则

- 表别名最多可以有30个字符, 但短一些更好
- 如果在FROM字句中别名被用于指定的表, 那么, 在整个select语句中, 都可以使用表别名
- 表别名应该是有意义的
- 表别名只对当前select语句有效

示例: 显示Taylor的名字和部门名

```sql
SELECT
	e.last_name,
	d.department_name
FROM
	employees e,
	departments d 
WHERE
	d.DEPARTMENT_ID = e.DEPARTMENT_ID;
```

## 多于两个表的连接

为了连接n个表, 你最少需要n-1个连接条件. 例如, 为了连接3个表, 你最少需要两个连接条件

示例一: 查询每个雇员的last_name, department_name, city( city 来源于 locations 表);

```sql
SQL> select e.last_name, d.department_name, l.city
  2  from employees e, departments d, locations l
  3  where e.department_id = d.department_id
  4  and d.location_id = l.location_id;
LAST_NAME                 DEPARTMENT_NAME                CITY
------------------------- ------------------------------ ------------------------------
Abel                      Sales                          Oxford
Ande                      Sales                          Oxford
Atkinson                  Shipping                       South San Francisco
Austin                    IT                             Southlake
Baer                      Public Relations               Munich
```

示例二: 查询Taylor的雇员ID, 部门名称, 和工作的城市

```sql
SQL> select e.employee_id, d.department_id, l.city
  2  from employees e, departments d, locations l
  3  where e.department_id = d.department_id
  4  and d.location_id = l.location_id
  5  and e.last_name = 'Taylor';
EMPLOYEE_ID DEPARTMENT_ID CITY
----------- ------------- ------------------------------
        176            80 Oxford
        180            50 South San Francisco
```

## 非等值连接

非等值连接是一种不使用相等作为连接条件的连接查询, 如!=, <, >, >=, <=, BETWEEN AND, 等都是非等值连接的条件判断.

先准备一个工资等级的表

```sql
SQL> create table JOB_GRADES(
  2  	lowest_sal number,
  3  	highest_sal number,
  4  	gra varchar2(10)
  5  );
```

加一些数据

```sql
LOWEST_SAL HIGHEST_SAL GRA
---------- ----------- ----------
      1000        2999 A
      3000        5999 B
      6000        9999 C
     10000       14999 D
     15000       24999 E
     25000       40000 F
```

我们利用此表来学习非等值连接

示例: 查询所有员工的工资等级

```sql
SQL> select e.last_name, e.salary, g.gra from employees e, job_grades g where e.salary between g.lowest_sal and g.highest_sal;
LAST_NAME                     SALARY GRA
------------------------- ---------- ----------
Olson                        2100.00 A
Markle                       2200.00 A
Philtanker                   2200.00 A
Landry                       2400.00 A
Gee                          2400.00 A
Colmenares                   2500.00 A
```

## 自连接

自连接是一个表连接其自身的操作

示例: 查询每个雇员的经理的名字

```sql
SQL> select e1.last_name, e2.last_name from employees e1, employees e2 where e1.manager_id = e2.employee_id;
LAST_NAME                 LAST_NAME
------------------------- -------------------------
Smith                     Cambrault
Ozer                      Cambrault
Kumar                     Cambrault
Fox                       Cambrault
Bloom                     Cambrault
Bates                     Cambrault
```

## 外连接

### 什么是外连接

外连接是查询出符合连接条件的数据同时还包含孤儿数据. 左外连接包含左表的孤儿数据, 右外连接包含右表的孤儿数据

### 什么是孤儿数据

孤儿数据是指被连接的列的值为空的数据

### 外连接类型 (SQL99)

左外连接(LEFT OUTER JOIN): 包含左表的孤儿数据

右外连接(RIGHT OUTER JOIN): 包含右表的孤儿数据

全外连接(FULL OUTER JOIN): 包含两个表中的孤儿数据

### SQL99中的外连接语法格式

用LEFT OUTER JOIN | RIGHT OUTER JOIN | FULL OUTER JOIN定义连接类型, 用ON字句创建连接条件

### 左外连接(LEFT OUTER JOIN)

示例: 用左外连接查询雇员名字以及他们所在的部门名称, 包含那些没有部门的雇员

```sql
SQL> select e.last_name, d.department_name
  2  from employees e
  3  left outer join departments d
  4  on e.department_id = d.department_id;
LAST_NAME                 DEPARTMENT_NAME
------------------------- ------------------------------
Whalen                    Administration
Fay                       Marketing
Hartstein                 Marketing
Colmenares                Purchasing
Gietz                     Accounting
Higgins                   Accounting
Grant                     
```

### 右外连接(RIGHT OUTER JOIN)

示例: 用右外连接查询雇员名以及他们所在部门的名称, 包括没有雇员的部门

```sql
SQL> select e.last_name, d.department_name
  2  from employees e
  3  right outer join departments d
  4  on e.department_id = d.department_id;
LAST_NAME                 DEPARTMENT_NAME
------------------------- ------------------------------
Whalen                    Administration
Fay                       Marketing
Hartstein                 Marketing
Gietz                     Accounting
Higgins                   Accounting
                          Treasury
                          Corporate Tax
                          Control And Credit
```

### 全外连接

示例: 查询所有雇员和部门, 包含那些没有没有部门的雇员以及没有雇员的部门

```sql
SQL> select e.last_name, d.department_name
  2  from employees e
  3  full outer join departments d
  4  on e.department_id = d.department_id;
LAST_NAME                 DEPARTMENT_NAME
------------------------- ------------------------------
Livingston                Sales
Grant                     
Johnson                   Sales
Taylor                    Shipping
                          NOC
                          Manufacturing
                          Government Sales
                          IT Support
                          Benefits
                          Shareholder Services
                          Retail Sales
```

### Oracle扩展的外连接

在Oracle数据库中对外连接中的左外和右外连接做了扩展, 可以简化外连接的语法. 通过在连接条件的后侧使用(+)来表示是否显示孤儿数据, 有(+)表示不显示孤儿数据, 而另一侧则显示孤儿数据. **但是该种写法仅在Oracle数据库中有效**

示例一: 查询雇员名字以及他们所在部门名称, 包含那些没有雇员的部门. 

```sql
SQL> select e.last_name, d.department_name
  2  from employees e, departments d
  3  where e.department_id(+) = d.department_id;
  LAST_NAME                 DEPARTMENT_NAME
------------------------- ------------------------------
Whalen                    Administration
Fay                       Marketing
Hartstein                 Marketing
Gietz                     Accounting
Higgins                   Accounting
                          Treasury
                          Corporate Tax
                          Control And Credit
```

示例二: 用左外连接查询雇员名字以及他们所在的部门名称, 包含那些没有部门的雇员

```sql
LAST_NAME                 DEPARTMENT_NAME
------------------------- ------------------------------
Whalen                    Administration
Fay                       Marketing
Hartstein                 Marketing
Gietz                     Accounting
Higgins                   Accounting
                          Treasury
                          Corporate Tax
                          Control And Credit
```

## SQL99中的交叉连接

- CROSS JOIN 字句导致两个表的交叉乘积
- 该连接和两个表之间的笛卡尔乘积是一样的

示例: 查询Employee表和departments的笛卡尔乘积

```sql
SQL> select employee_id, department_name
  2  from employees
  3  cross join departments;
EMPLOYEE_ID DEPARTMENT_NAME
----------- ------------------------------
        202 Payroll
        203 Payroll
        204 Payroll
        205 Payroll
        206 Payroll
2889 rows selected
```

## SQL99 中的自然连接

- NATURAL JOIN 字句基于两个表之间有相同名字的所有列
- 他从两个表中选择所有的匹配列中有等值的行
- 如果有相同名字的列的数据类型不同, 返回一个错误
- 实际上自然连接就是简化了的等值连接, 性能和等值连接并没有什么差异

### 使用自然连接的时候需要注意

1. 如果做自然连接的两个表的有多个字段都满足有相同的名称个类型, 那么他们会被作为自然连接的条件
2. 如果自然连接的两个表仅是字段名称相同, 但数据类型不同, 那么他会返回一个错误. 
3. 由于Oracle中可以进行这种非常简单的natural join, 我们在设计表时对具有相同含义的字段需要考虑到使用相同的名字和数据类型

示例: 查询部门ID, 部门名以及他们所在的城市

```sql
SQL> select d.department_id, d.department_name, l.city
  2  from departments d
  3  natural join locations l;
```

这个句子等同于等值连接如下

```sql
SQL> select d.department_id, d.department_name, l.city
  2  from departments d, locations l
  3  where d.location_id = l.location_id;
```

## 使用USING字句查询

- 当有多个列匹配时, 用USING字句匹配唯一的列
- 如果某列在USING中使用, 那么在引用该列时不要使用表名或者别名
- NATURAL JOIN 和 USING 字句时相互排斥的
- 实际上USING字句的使用就是把where中的等值连接放在了using字句中

示例: 

查询location_id为1800的部门名称以及他们所在的城市名称, 指定location_id为连接列

```sql
SQL> select d.department_name, l.city
  2  from departments d
  3  join locations l
  4  using(location_id)
  5  where location_id = 1800;
DEPARTMENT_NAME                CITY
------------------------------ ------------------------------
Marketing                      Toronto
```

## SQL99中的内连接

内连接(INNER JOIN): 内连接通过INNER JOIN来建立两个表的连接. 在内连接中使用INNER JOIN作为表的连接, 用ON字句给定绑定条件, INNER  JOIN 语句在性能上和其他语句并没有什么优势

示例: 查询雇员id为202的雇员名字, 部门名称, 以及工作的成时

```sql
SQL> select e.last_name, d.department_name, l.city
  2  from employees e
  3  inner join departments d
  4  on e.department_id = d.department_id
  5  inner join locations l
  6  on l.location_id = d.location_id
  7  where e.employee_id = 202;
LAST_NAME                 DEPARTMENT_NAME                CITY
------------------------- ------------------------------ ------------------------------
Fay                       Marketing                      Toronto
```

可以把on的连接条件换成using字句

```sql
SQL> select e.last_name, d.department_name, l.city
  2  from employees e
  3  inner join departments d
  4  using(department_id)
  5  inner join locations l
  6  using(location_id)
  7  where e.employee_id = 202;
LAST_NAME                 DEPARTMENT_NAME                CITY
------------------------- ------------------------------ ------------------------------
Fay                       Marketing                      Toronto
```

