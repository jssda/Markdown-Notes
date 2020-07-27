[TOC]

# MySQL中的分页查询

## MySQL分页查询原则

- 在MySQL数据库中使用limit字句进行分页查询
- MySQL分页中开始位置为0
- 分页字句在查询语句的最后侧

## LiMIT字句

### 语法格式:

```sql
select 投影列 from 表名 where 条件 order by limit 开始位置, 查询数量
```

### 示例

查询雇员表中所有数据按ID排序, 实现分页查询, 每次返回两条结果

```sql
select * from emp
ORDER BY employee_id
limit 0, 2;
```

## LiMIT OFFSET 字句

### 语法格式

```sql
SELECT 投影列 FROM 表名 WHERE 条件 ORDER BY LIMIT 查询数量 OFFSET
开始位置。 
```

### 示例:

查询雇员表中所有数据按 id 排序， 使用 LIMIT OFFSET 实现分页查询， 每次返回两条
结果。

```sql
select * from emp
ORDER BY employee_id
limit 2 OFFSET 0;
```

# 执行计划

## MySQL执行计划

在MySQL中可以通过explain关键字来模拟优化器执行SQL语句, 从而知道MySQL是如何处理SQL语句的

## MySQL整个查询执行过程

1. 客户端向MySQL服务器发送一条查询请求
2. 服务器首先检查查询缓存, 如果命中缓存, 则立即返回存储在缓存中的结果. 否则进入下一阶段
3. 服务器进行SQL解析. 预处理, 再由优化器生成相应的执行计划
4. MySQL根据执行计划, 调用存储引擎的API来执行查询
5. 将结果返回给客户端, 同时缓存查询结果

## 启动执行计划

explain select 投影列 from 表名 where 条件

## EXPLAIN列的解释

### ID

查询执行顺序

id值相同时表示从上向下执行

id值相同被视为一组

如果是子查询, id值会递增, id值越高, 优先级越高

### select_type

- simple: 表示查询中不包含子查询或者union
- primary: 当查询中包含任何复杂的子查询, 最外层查询被标记为primary
- derived: 在from的列表中包含的子查询被标记为derived
- subquery: 在select或where列表中包含了子查询, 则子查询被标记为subquery
- union: 两个select查询时前一个标记为primary, 后一个标记为union. union出现在from从句子查询中, 外层select标记为primary, union中第一个查询为derived. 第二个子查询标记为union
- unionresult: 从union表获取结果的select被标记为union result

### table

显示这一行的数据是关于哪张表的

### * type

这是重要的列, 显示连接使用了何种类型. 从最好到最差的连接类型为: system、const、eq_reg、ref、range、index和ALL

- System： 表中只有一行数据。属于const的特例。如果物理表中只有一行数据，那么连接类型为ALL
- const：查询结果最多有一个匹配行，所以可以被视为常量。const的查询速度非常快，因为只读一次。一般情况下把主键或唯一索引作为唯一条件的查询都是const
- eq_ref：查询时查询外键表全部数据。且只能查询主键列或关联列。且外键表中外键列中数据不能有重复数据，且这些数据都必须在主键表中有对应数据。（主键表中数据可以有没有用到的）
- ref：相比eq_ref，不对外键列有强制要求，里边的数据可以重复，只要出现重复的数据取值就是ref。也可能时索引查询
- range：把这个列当作条件只检查其中一个范围。常见where从句中出现between、<、in等。主要应用在具有索引的列中
- index：这个连接类型对前面的表中每一个记录联合进行完全扫描（比ALL更好，因为索引一般小于表数据。）
- All：这个连接类型对于前面的每一个记录联合进行完全扫描，这一般比较糟糕。应该尽量避免。

### Possible_keys

查询条件字段涉及到的索引，可能没有使用。

### Key

实际使用的索引。如果为NULL，则没有使用索引

### Key_len

表示索引中使用的字节数，查询中使用的索引长度（最大可能长度），并非实际使用长度，理论上长度越短越好。key_len是根据表定义计算而得的，不是通过表检索出的。

### ref

显示做因的那一列被使用了，如果可能的化，是一个常量const

### rows

根据表统计信息及索引选用情况，大致估算出找到所需的记录所需要读取的行数。

### Fitered

显示了通过条件过滤出的行数的百分比过滤值。

### extra

MySQL如何解析查询的额外信息

- Distinct：MySQL发现第一个匹配行后，停止为当前的行组合搜索更多的行。
- Not exists：MySQL能够对查询进行Left join优化，发现一个匹配LEFT JOIN标准的行后，不再为前面的行组合在该表内检查更多的行。
- range checked for each record（index map: #)：MySQL没有发现好的可以使用的索引，但发现如果来自前面的表的列值已知，可能部分索引可是使用。
- Using filesort：MySQL需要额外的一次传递，以找出如何按照排序顺序检索行
- Using index：从只使用索引树中的信息而不需要进一步搜索读取实际的行来检索表中的列信息。
- Using temporary:为了解决查询,MySQL 需要创建一个临时表来容纳结果。
- Using where:WHERE 子句用于限制哪一个行匹配下一个表或发送到客户。
- Using sort_union(...), Using union(...), Using intersect(...): 这 些 函 数 说 明 如 何 为
  index_merge 联接类型合并索引扫描。
- Using index for group-by:类似于访问表的 Using index 方式,Using index for group-by 表示
  MySQL 发现了一个索引,可以用来查 询 GROUP BY 或 DISTINCT 查询的所有列,而不要
  额外搜索硬盘访问实际的表 

