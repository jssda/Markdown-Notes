# 数据库引擎

## 查看数据库引擎

```sql
show engines
```

![1565074289941](https://raw.githubusercontent.com/jssda/picbed/master/1565074289941.png)

## MySQL数据库引擎介绍

### ISAM(Indexed Sequential Access Method)

ISAM是一个定义明确且历历史时间考验的数据表格管理方法, 他在设计之时就考虑到数据库被查询到的次数要远大于更新的次数.因此,ISAM执行读取操作的次数很快, 而且不占用大量的内存和存储资源. ISAM的两个主要不足之处在于, 它不支持事务处理, 也不能够容错. 如果你的硬盘崩溃了, 那么数据文件就无法恢复了. 如果你正在把ISAM用在关键任务应用程序里, 那就必须经常备份你所有的实时数据.通过其符值特性, MySQL能够支持这样的备份应用程序.

注意: 使用ISAM时必须经常备份所有的实时数据.

### MyISAM

MyISAM 是 MySQL 的 ISAM 扩展格式和缺省的数据库引擎。 除了提供 ISAM 里所没有
的索引和字段管理的大量功能， MyISAM 还使用一种表格锁定的机制， 来优化多个并发的
读写操作， 其代价是你需要经常运行 OPTIMIZE TABLE 命令， 来恢复被更新机制所浪费的
空间。 MyISAM 还有一些有用的扩展， 例如用来修复数据库文件的 MyISAMCHK 工具和用
来恢复浪费空间的 MyISAMPACK 工具。 MYISAM 强调了快速读取操作， 这可能就是为什
么 MySQL 受到了 WEB 开发如此青睐的主要原因： 在 WEB 开发中你所进行的大量数据操
作都是读取操作。 所以， 大多数虚拟主机提供商和 INTERNET 平台提供商只允许使用
MYISAM 格式。 MyISAM 格式的一个重要缺陷就是不能在表损坏后恢复数据。

注意： MyISAM 引擎使用时必须经常使用 Optimize Table 命令清理空间； 必须经常备份
所有实时数据。 工具有用来修复数据库文件的 MyISAMCHK 工具和用来恢复浪费空间的
MyISAMPACK 工具。

如果使用该数据库引擎， 会生成三个文件：
.frm:表结构信息
.MYD:数据文件
.MYI:表的索引信息 

### InnoDB

InnoDB 数据库引擎都是造就 MySQL 灵活性的技术的直接产品， 这项技术就是 MYSQL++
API。 在使用 MYSQL 的时候， 你所面对的每一个挑战几乎都源于 ISAM 和 MyISAM 数据库
引擎不支持事务处理（transaction process） 也不支持外键。 尽管要比 ISAM 和 MyISAM 引擎
慢很多， 但是 InnoDB 包括了对事务处理和外来键的支持， 这两点都是前两个引擎所没有的。
如前所述， 如果你的设计需要这些特性中的一者或者两者， 那你就要被迫使用后两个引擎中
的一个了。

MySQL 官方对 InnoDB 是这样解释的： InnoDB 给 MySQL 提供了具有提交、 回滚和崩溃
恢复能力的事务安全（ACID 兼容） 存储引擎。 InnoDB 锁定在行级并且也在 SELECT 语句
提供一个 Oracle 风格一致的非锁定读， 这些特色增加了多用户部署和性能。 没有在 InnoDB
中扩大锁定的需要， 因为在 InnoDB 中行级锁定适合非常小的空间。InnoDB 也支持 FOREIGN KEY 强制。 在 SQL 查询中， 你可以自由地将 InnoDB 类型的表与其它 MySQL 的表的类型
混合起来， 甚至在同一个查询中也可以混合。 

InnoDB 是为处理巨大数据量时的最大性能设计， 它的 CPU 效率可能是任何其它基于磁盘
的关系数据库引擎所不能匹敌的。 

InnoDB 存储引擎被完全与 MySQL 服务器整合， InnoDB 存储引擎为在主内存中缓存数据
和索引而维持它自己的缓冲池。 InnoDB 存储它的表＆索引在一个表空间中， 表空间可以包
含数个文件（或原始磁盘分区） 。 这与 MyISAM 表不同， 比如在 MyISAM 表中每个表被存
在分离的文件中。 InnoDB 表可以是任何尺寸， 即使在文件尺寸被限制为 2GB 的操作系统
上。 

### innodb 与 myisam 区别 

1. InnoDB支持事务, MyISAM不支持, 对于InnoDB每一条SQL语言都默认封装成事务, 自动提交, 这样会影响速度. 所以最好把多条sql语言放在begin和commit之间, 组成一个事务.
2. InnoDB支持外键,而 MyISAM不支持. 对一个包含外键的InnoDB表转为 MyISAM会失败
3. InnoDB时聚集索引,数据文件和索引是绑在一起的, 必须要有主键, 通过主键索引效率很高. 但是辅助索引需要两次查询, 先查询到主键, 然后再通过主键查询到数据.因此, 主键不应该过大, 因为主键太大其他索引也都会很大.而 MyISAM 是非聚集索引， 数据
   文件是分离的， 索引保存的是数据文件的指针。 主键索引和辅助索引是独立的。 
4. InnoDB 不保存表的具体行数， 执行 select count(*) from table 时需要全表扫描。 而
   MyISAM 用一个变量保存了整个表的行数， 执行上述语句时只需要读出该变量即可， 速度
   很快； 
5. Innodb 不支持全文索引， 而 MyISAM 支持全文索引， 查询效率上 MyISAM 要高； (在
   MySQL5.7 版本中已经支持全文索引) 

如何选择

1. 是否要支持事务， 如果要请选择 innodb， 如果不需要可以考虑 MyISAM 
2. 如果表中绝大多数都只是读查询， 可以考虑 MyISAM， 如果既有读写也挺频繁， 请
   使用 InnoDB。 
3. 系统奔溃后， MyISAM 恢复起来更困难， 能否接受； 
4. MySQL5.5 版本开始 Innodb 已经成为 Mysql 的默认引擎(之前是 MyISAM)， 说明其优
   势是有目共睹的， 如果你不知道用什么， 那就用 InnoDB， 至少不会差。 

## 修改数据库引擎

修改 MySQL 的 my.ini 配置文件

```ini
default-storage-engine=数据库引擎名称
重启 MySQL 
```

## 修改表级存储引擎

```
ALTER TABLE tableName engine=InnoDB
查询表的存储引擎
show create table table_name;
```

