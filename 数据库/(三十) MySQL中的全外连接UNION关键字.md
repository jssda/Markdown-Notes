# 全外连接

MySQL中不支持FULL OUTER JOIN连接, 但是可以使用union实现全外连接

## UNION

可以将两个查询结果集 合并, 返回的行都是唯一的, 相当于对整个结果集使用了distinct

## UNION ALL

只是返回简单的两个结果集的合并结果. 如果两个返回的结果集中有重复的数据, 那么返回的结果集就有重复的数据.

## 语法结构

```sql
select 投影列 from 表名 left outer join 表名 on 连接条件
union
select 投影列 from 表名 right outer join 表名 on 连接条件
```

