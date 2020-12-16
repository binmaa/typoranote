[TOC]

#### 1. count(1)、count(*)、count(字段)区别

+ count(字段)不统计字段为null（进行全表扫描，判断指定字段的值是否为`NULL`，不为`NULL`则累加）

+ count(1)和count(*)统计全部记录数

+ InnoDB中`COUNT(*)`和`COUNT(1)`实现上没有区别，而且效率一样（依旧建议使用`count(*)`，因为这是SQL92定义的标准统计行数的语法。），但是`COUNT(字段)`需要进行字段的非NULL判断，所以效率会低一些

  ```sql
  InnoDB handles SELECT COUNT(*) and SELECT COUNT(1) operations in the same way. There is no performance difference.
  ```

+ 对count(*)的优化（MyISAM、InnoDB两种存储引擎对比）

  + **MyISAM不支持事务，MyISAM中的锁是表级锁；而InnoDB支持事务，并且支持行级锁。**

  + MyISAM做了一个简单的优化，把表的总行数单独记录下来，如果执行`count(*)`时可以直接返回，<font color='red'>前提是不能有where条件。</font>MyISAM是表级锁，不会有并发的行操作，所以查到的结果是准确的。

  + nnoDB不能使用这种缓存操作，因为支持事务，大部分操作都是行级锁，行可能被并行修改，那么缓存记录不准确。

    但是，InnoDB还是针对COUNT(*)语句做了些优化的。通过低成本的索引进行扫表，而不关注表的具体内容。

    InnoDB中索引分为聚簇索引（主键索引）和非聚簇索引（非主键索引），聚簇索引的叶子节点中保存的是整行记录，而非聚簇索引的叶子节点中保存的是该行记录的主键的值。**MySQL会优先选择最小的非聚簇索引来扫表。**<font color='red'>优化的前提是查询语句中不包含where条件和group by条件。</font>

#### 不推荐使用子查询和JOIN

1. 不推荐原因

   + 对于mysql，不推荐使用子查询和join是因为本身join的效率就是硬伤，一旦数据量很大效率就很难保证，强烈推荐分别根据索引单表取数据，然后在程序里面做join，merge数据。

   + 子查询就更别用了，效率太差，执行子查询时，MYSQL需要创建临时表，查询完毕后再删除这些临时表，所以，子查询的速度会受到一定的影响，这里多了一个创建和销毁临时表的过程。

   + 如果是JOIN的话，它是走嵌套查询的。小表驱动大表，且通过索引字段进行关联。如果表记录比较少的话，还是OK的。大的话业务逻辑中可以控制处理。

   + 数据库是最底层的，瓶颈往往是数据库。建议数据库只是作为数据store的工具，而不要添加业务上去。

2. 应用层关联优势
   - **让缓存的效率更高**。许多应用程序可以方便地缓存单表查询对应的结果对象。如果关联中的某个表发生了变化，那么就无法使用查询缓存了，而拆分后，如果某个表很少改变，那么基于该表的查询就可以重复利用查询缓存结果了。
   - 将查询分解后，执行单个查询可以**减少锁的竞争**。
   - 在应用层做关联，可以更容易对数据库进行拆分，更容易做到**高性能和可扩展**。（并发场景多，DB查询频繁，需要分库分表，可以将数据分布到不同的MySQL服务器上）
   - **查询本身效率也可能会有所提升**。查询id集的时候，使用IN（）代替关联查询，可以让MySQL按照ID顺序进行查询，这可能比随机的关联要更高效。
   - **可以减少冗余记录的查询**。在应用层做关联查询，意味着对于某条记录应用只需要查询一次，而在数据库中做关联查询，则可能需要重复地访问一部分数据。从这点看，这样的重构还可能会减少网络和内存的消艳。
   - 更进一步，这样做相当于在应用中实现了哈希关联，而不是使用MySQL的嵌套循环关联。某些场景哈希关联的效率要高很多。

3. 不推荐使用join原因

   + DB承担的业务压力大，能减少负担就减少。当表处于百万级别后，join导致性能下降； 
   + 分布式的分库分表。这种时候是不建议跨库join的。目前mysql的分布式中间件，跨库join表现不良。 
   + 修改表的schema，单表查询的修改比较容易，join写的sql语句要修改，不容易发现，成本比较大，当系统比较大时，不好维护。

   **解决方式**

   在业务层，单表查询出数据后，作为条件给下一个单表查询。也就是子查询。 会担心子查询出来的结果集太多。mysql对in的数量没有限制，但是mysql限制整条sql语句的大小。通过调整参数max_allowed_packet ，可以修改一条sql的最大值。建议在业务上做好处理，限制一次查询出来的结果集是能接受的。

   **好处**

   做分页查询：

   关联查询的好处时候可以做分页，可以用副表的字段做查询条件，在查询的时候，将副表匹配到的字段作为结果集，用主表去in它，但是问题来了，如果匹配到的数据量太大就不行了，也会导致返回的分页记录跟实际的不一样，解决的方法可以交给前端，一次性查询，让前端分批显示就可以了，这种解决方案的前提是数据量不太，因为sql本身长度有限

   

#### MYSQL子查询和JOIN连接哪个性能更好（待验证）

**JOIN**5.6小数据量没区别 5.7以上没测试

```sql
--EXPLAIN解析以下SQL的时候，MySQL报告了一个警告
EXPLAIN
SELECT t.* FROM test t WHERE t.id IN (SELECT t2.id FROM test t2 )
--显示警告发现子查询被编译为join语句
SHOW WARNINGS

select `test`.`t`.`id` AS `id`,`test`.`t`.`name` AS `name` 
from `test`.`test` `t2` join `test`.`test` `t` 
where (`test`.`t`.`id` = `test`.`t2`.`id`)
```

子查询被编译器优化成了join语句，所以使用子查询凭空增加了编译器的负担，在可以使用join的时候一定不要使用子查询



#### 创建临时表情况

[原文](https://blog.csdn.net/xiaolyuh123/article/details/53286033)

1、UNION查询；
2、用到TEMPTABLE算法或者是UNION查询中的视图；
3、ORDER BY和GROUP BY的子句不一样时；
4、表连接中，ORDER BY的列不是驱动表中的；
5、DISTINCT查询并且加上ORDER BY时；
6、SQL中用到SQL_SMALL_RESULT选项时；
7、FROM中的子查询；
8、子查询或者semi-join时创建的表；

EXPLAIN 查看执行计划结果的 Extra 列中，如果包含 [Using Temporary](http://imysql.com/2015/06/14/mysql-faq-what-important-information-in-explain.shtml) 就表示会用到临时表。

当然了，如果临时表中需要存储的数据量超过了上限（ [tmp-table-size](https://dev.mysql.com/doc/refman/5.6/en/server-system-variables.html#sysvar_tmp_table_size) 或 [max-heap-table-size](https://dev.mysql.com/doc/refman/5.6/en/server-system-variables.html#sysvar_max_heap_table_size) 中取其大者），这时候就需要生成基于磁盘的临时表了。



#### EXPLAIN解析SQL执行计划时

如果有下面几种情况，就需要特别关注下了：

1. 首先看下 type 这列的结果，如果有类型是 ALL 时，表示预计会进行全表扫描（full table scan）。通常全表扫描的代价是比较大的，建议创建适当的索引，通过索引检索避免全表扫描。此外，全索引扫描（full index scan）的代价有时候是比全表扫描还要高的，除非是基于InnoDB表的主键索引扫描。

2. 再来看下 Extra 列的结果，如果有出现 Using temporary 或者 Using filesort 则要多加关注：

   + Using temporary，表示需要创建临时表以满足需求，通常是因为GROUP BY的列没有索引，或者GROUP BY和ORDER BY的列不一样，也需要创建临时表，建议添加适当的索引。

   + Using filesort，表示无法利用索引完成排序，也有可能是因为多表连接时，排序字段不是驱动表中的字段，因此也没办法利用索引完成排序，建议添加适当的索引。

   + Using where，通常是因为全表扫描或全索引扫描时（type 列显示为 ALL 或 index），又加上了WHERE条件，建议添加适当的索引。

     暂时想到上面几个，如果有遗漏，以后再补充。
     其他状态例如：Using index、Using index condition、Using index for group-by 则都还好，不用紧张。





sql执行计划解读

https://blog.csdn.net/poxiaonie/article/details/77757471



批量生成大量数据

https://www.cnblogs.com/bjx2020/p/9727898.html

工具：datafaker