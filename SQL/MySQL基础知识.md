[toc]

[Mysql下载](https://dev.mysql.com/downloads/mysql/)

#### 数据类型

*MySQL数据类型和Java数据类型对应关系*

|   类型名称    |   显示长度   |   数据库类型              |   JAVA类型               |   JDBC类型索引(int)   |   描述   |
| ------------- | ------------ | ------------------------- | ------------------------ | --------------------- | -------- |
|               |              |                           |                          |                       |          |
|   VARCHAR     |   L+N        |   VARCHAR                 |   java.lang.String       |   12                  |          |
|   CHAR        |   N          |   CHAR                    |   java.lang.String       |   1                   |          |
|   BLOB        |   L+N        |   BLOB                    |   java.lang.byte[]       |   -4                  |          |
|   TEXT        |   65535      |   VARCHAR                 |   java.lang.String       |   -1                  |          |
|               |              |                           |                          |                       |          |
|   INTEGER     |   4          |   INTEGER UNSIGNED        |   java.lang.Long         |   4                   |          |
|   TINYINT     |   3          |   TINYINT UNSIGNED        |   java.lang.Integer      |   -6                  |          |
|   SMALLINT    |   5          |   SMALLINT UNSIGNED       |   java.lang.Integer      |   5                   |          |
|   MEDIUMINT   |   8          |   MEDIUMINT UNSIGNED      |   java.lang.Integer      |   4                   |          |
|   BIT         |   1          |   BIT                     |   java.lang.Boolean      |   -7                  |          |
|   BIGINT      |   20         |   BIGINT UNSIGNED         |   java.math.BigInteger   |   -5                  |          |
|   FLOAT       |   4+8        |   FLOAT                   |   java.lang.Float        |   7                   |          |
|   DOUBLE      |   22         |   DOUBLE                  |   java.lang.Double       |   8                   |          |
|   DECIMAL     |   11         |   DECIMAL                 |   java.math.BigDecimal   |   3                   |          |
|   BOOLEAN     |   1          |   同TINYINT               |                          |                       |          |
|               |              |                           |                          |                       |          |
|   ID          |   11         |   PK (INTEGER UNSIGNED)   |   java.lang.Long         |   4                   |          |
|               |              |                           |                          |                       |          |
|   DATE        |   10         |   DATE                    |   java.sql.Date          |   91                  |          |
|   TIME        |   8          |   TIME                    |   java.sql.Time          |   92                  |          |
|   DATETIME    |   19         |   DATETIME                |   java.sql.Timestamp     |   93                  |          |
|   TIMESTAMP   |   19         |   TIMESTAMP               |   java.sql.Timestamp     |   93                  |          |
|   YEAR        |   4          |   YEAR                    |                          |                       |          |

1. 整数

* 数据类型

| 类型名称      | 说明           | 存储需求 |
| ------------- | -------------- | -------- |
| TINYINT       | 很小的整数     | 1个字节  |
| SMALLINT      | 小的整数       | 2个宇节  |
| MEDIUMINT     | 中等大小的整数 | 3个字节  |
| INT (INTEGHR) | 普通大小的整数 | 4个字节  |
| BIGINT        | 大整数         | 8个字节  |

* 取值范围

| 类型名称      | 说明                                      | 存储需求                |
| ------------- | ----------------------------------------- | ----------------------- |
| TINYINT       | -128〜127                                 | 0 〜255                 |
| SMALLINTs     | -32768〜32767                             | 0〜65535                |
| MEDIUMINT     | -8388608〜8388607                         | 0〜16777215             |
| INT (INTEGER) | -2147483648〜2147483647                   | 0〜4294967295           |
| BIGINT        | -9223372036854775808〜9223372036854775807 | 0〜18446744073709551615 |

> 提示：<font color='red'>显示宽度和数据类型的取值范围是无关的。int(11) 11为显示宽度</font>显示宽度只是指明 MySQL 最大可能显示的数字个数，数值的位数小于指定的宽度时会由空格填充。如果插入了大于显示宽度的值，只要该值不超过该类型整数的取值范围，数值依然可以插入，而且能够显示出来。例如，year 字段插入 19999，当使用 SELECT 查询该列值的时候，MySQL 显示的将是完整的带有 5 位数字的 19999，而不是 4 位数字的值。

***

2. 小数

   | 类型名称            | 说明               | 存储需求   |
   | ------------------- | ------------------ | ---------- |
   | FLOAT               | 单精度浮点数       | 4 个字节   |
   | DOUBLE              | 双精度浮点数       | 8 个字节   |
   | DECIMAL (M, D)，DEC | 压缩的“严格”定点数 | M+2 个字节 |

> 浮点类型有两种，分别是单精度浮点数（**FLOAT**）和双精度浮点数（**DOUBLE**）；
>
> 定点类型只有一种，就是 **DECIMAL**。
>
> ​		浮点类型和定点类型都可以用`(M, D)`来表示，其中`M`称为精度，表示总共的位数；`D`称为标度，表示小数的位数。
>
> ​		浮点数类型的取值范围为 M（1～255）和 D（1～30，且不能大于 M-2），分别表示显示宽度和小数位数。M 和 D 在 FLOAT 和DOUBLE 中是可选的，FLOAT 和 DOUBLE 类型将被保存为硬件所支持的最大精度。DECIMAL 的默认 D 值为 0、M 值为 10。
>
> ​		DECIMAL 的存储空间并不是固定的，而由精度值 M 决定，占用 M+2 个字节。

***浮点数相对于定点数的优点是在长度一定的情况下，浮点数能够表示更大的范围；缺点是会引起精度问题。***

1. **取值范围：**

FLOAT 类型的取值范围如下：

- 有符号的取值范围：-3.402823466E+38～-1.175494351E-38。
- 无符号的取值范围：0 和 -1.175494351E-38～-3.402823466E+38。

DOUBLE 类型的取值范围如下：

- 有符号的取值范围：-1.7976931348623157E+308～-2.2250738585072014E-308。
- 无符号的取值范围：0 和 -2.2250738585072014E-308～-1.7976931348623157E+308。

2. **取值范围计算方式**

[浮点数据(float)类型存储方式.md](./浮点数据(float)类型存储方式.md)

> 整数部分 10进制转2进制
>
> ​	例：6 模2求余可以得到二进制表示为110。
>
> 小数部分 10进制转2进制
>
> ​	例：0.25  乘法2取整数直至无小数 0.25*2=0.5 取0  0.5\*2=1 取1   >  0.25的二进制写法为0.01
>
> 整数转小数 2进制转10进制
>
> ​	例：0.01 对应位乘1/2<sup>x</sup>  0*1/2<sup>1</sup>+1\*1/2<sup>2</sup>=0.25

3. 日期和时间

| 类型名称  | 日期格式            | 日期范围                                          | 存储需求 |
| --------- | ------------------- | ------------------------------------------------- | -------- |
| YEAR      | YYYY                | 1901 ~ 2155                                       | 1 个字节 |
| TIME      | HH:MM:SS            | -838:59:59 ~ 838:59:59                            | 3 个字节 |
| DATE      | YYYY-MM-DD          | 1000-01-01 ~ 9999-12-3                            | 3 个字节 |
| DATETIME  | YYYY-MM-DD HH:MM:SS | 1000-01-01 00:00:00 ~ 9999-12-31 23:59:59         | 8 个字节 |
| TIMESTAMP | YYYY-MM-DD HH:MM:SS | 1980-01-01 00:00:01 UTC ~ 2040-01-19 03:14:07 UTC | 4 个字节 |

> TIMESTAMP 与 DATETIME 除了存储字节和支持的范围不同外，还有一个最大的区别是：
>
> - DATETIME 在存储日期数据时，按实际输入的格式存储，即输入什么就存储什么，与时区无关；
> - 而 TIMESTAMP 值的存储是以 UTC（世界标准时间）格式保存的，存储时对当前时区进行转换，检索时再转换回当前时区。即查询时，根据当前时区的不同，显示的时间值是不同的。
> - TIMESTAMP 也有一个 DATETIME 不具备的属性。默认情况下，当插入一条记录但并没有指定 TIMESTAMP 这个列值时，MySQL 会把 TIMESTAMP 列设为当前的时间。因此当需要插入记录和当前时间时，使用 TIMESTAMP 是方便的，另外 TIMESTAMP 在空间上比 DATETIME 更有效。

> MySQL 没有可以存储到毫秒的数据类型，可以<font color='red'>时间戳分两字段个来存储，分别存储日期和毫秒</font>
>
> 存储毫秒字段使用(mediumint或smallint)类型，具体看你毫秒保留多少位   四位内用SMALLINTs

4. 字符串

| 类型名称   | 说明                                         | 存储需求                                                   |
| ---------- | -------------------------------------------- | ---------------------------------------------------------- |
| CHAR(M)    | 固定长度非二进制字符串                       | M 字节，1<=M<=255                                          |
| VARCHAR(M) | 变长非二进制字符串                           | L+1字节，在此，L< = M和 1<=M<=255                          |
| TINYTEXT   | 非常小的非二进制字符串                       | L+1字节，在此，L<2^8                                       |
| TEXT       | 小的非二进制字符串                           | L+2字节，在此，L<2^16                                      |
| MEDIUMTEXT | 中等大小的非二进制字符串                     | L+3字节，在此，L<2^24                                      |
| LONGTEXT   | 大的非二进制字符串                           | L+4字节，在此，L<2^32                                      |
| ENUM       | 枚举类型，只能有一个枚举字符串值             | 1或2个字节，取决于枚举值的数目 (最大值为65535)             |
| SET        | 一个设置，字符串对象可以有零个或 多个SET成员 | 1、2、3、4或8个字节，取决于集合 成员的数量（最多64个成员） |

> CHAR 和 VARCHAR 的区别如下:
>
> - CHAR 是固定长度字符，VARCHAR 是可变长度字符。
> - <font color = 'red'>CHAR 会自动删除插入数据的尾部空格</font>，VARCHAR 不会删除尾部空格。
> - CHAR 是固定长度，所以它的处理速度比 VARCHAR 的速度要快，但是它的缺点就是浪费存储空间。所以对存储不大，但在速度上有要求的可以使用 CHAR 类型，反之可以使用 VARCHAR类型来实现。

5. 二进制

| 类型名称       | 说明                 | 存储需求               |
| -------------- | -------------------- | ---------------------- |
| BIT(M)         | 位字段类型           | 大约 (M+7)/8 字节      |
| BINARY(M)      | 固定长度二进制字符串 | M 字节                 |
| VARBINARY (M)  | 可变长度二进制字符串 | M+1 字节               |
| TINYBLOB (M)   | 非常小的BLOB         | L+1 字节，在此，L<2^8  |
| BLOB (M)       | 小 BLOB              | L+2 字节，在此，L<2^16 |
| MEDIUMBLOB (M) | 中等大小的BLOB       | L+3 字节，在此，L<2^24 |
| LONGBLOB (M)   | 非常大的BLOB         | L+4 字节，在此，L<2^32 |

| 数据类型   | 存储范围                              |
| ---------- | ------------------------------------- |
| TINYBLOB   | 最大长度为255 (28-1)字节              |
| BLOB       | 最大长度为65535 (216-1)字节           |
| MEDIUMBLOB | 最大长度为16777215 (224-1)字节        |
| LONGBLOB   | 最大长度为4294967295或4GB (231-1)字节 |

> BLOB 是二进制字符串，TEXT 是非二进制字符串，两者均可存放大容量的信息。BLOB 主要存储图片、音频信息等，而 TEXT 只能存储纯文本文件。



#### 存储引擎

[mysql中innodb和myisam对比及索引原理区别](https://blog.csdn.net/qq_27607965/article/details/79925288)

[MyISAM与InnoDB 的区别（9个不同点）](https://blog.csdn.net/qq_35642036/article/details/82820178?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.compare&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.compare)

| 存储引擎 | 描述                           |
| -------- | ------------------------------ |
| InnoDB   | 具备外键支持功能的事务处理引擎 |
| MyISAM   | 主要的非事务处理存储引擎       |

| 特性         | MyISAM | InnoDB | MEMORY |
| ------------ | ------ | ------ | ------ |
| 存储限制     | 有     | 支持   | 有     |
| 事务安全     | 不支持 | 支持   | 不支持 |
| 锁机制       | 表锁   | 行锁   | 表锁   |
| B树索引      | 支持   | 支持   | 支持   |
| 哈希索引     | 不支持 | 不支持 | 支持   |
| 全文索引     | 支持   | 不支持 | 不支持 |
| 集群索引     | 不支持 | 支持   | 不支持 |
| 数据缓存     |        | 支持   | 支持   |
| 索引缓存     | 支持   | 支持   | 支持   |
| 数据可压缩   | 支持   | 不支持 | 不支持 |
| 空间使用     | 低     | 高     | N/A    |
| 内存使用     | 低     | 高     | 中等   |
| 批量插入速度 | 高     | 低     | 高     |
| 支持外键     | 不支持 | 支持   | 不支持 |

区别：

MyISAM

- 不需要事务支持（不支持）
- 并发相对较低（锁定机制问题）
- 数据修改相对较少（阻塞问题），以读为主
- 数据一致性要求不是非常高

InnoDB 

- 需要事务支持（具有较好的事务特性）
- 行级锁定对高并发有很好的适应能力，但需要确保查询是通过索引完成
- 数据更新较为频繁的场景
- 数据一致性要求较高
- 硬件设备内存较大，可以利用InnoDB较好的缓存能力来提高内存利用率，尽可能减少磁盘 IO



**为什么MyISAM会比Innodb 的查询速度快**

InnoDB 在做SELECT的时候，要维护的东西比MYISAM引擎多很多；

1）InnoDB 要缓存数据和索引，MyISAM只缓存索引块，这中间还有换进换出的减少

2）innodb寻址要映射到块，再到行，MyISAM记录的直接是文件的OFFSET，定位比INNODB要快

3）InnoDB 还需要维护MVCC一致；虽然你的场景没有，但他还是需要去检查和维护

**MVCC ( Multi-Version Concurrency Control )多版本并发控制**

InnoDB ：通过为每一行记录添加两个额外的隐藏的值来实现MVCC，这两个值一个记录这行数据何时被创建，另外一个记录这行数据何时过期（或者被删除）。但是InnoDB并不存储这些事件发生时的实际时间，相反它只存储这些事件发生时的系统版本号。这是一个随着事务的创建而不断增长的数字。每个事务在事务开始时会记录它自己的系统版本号。每个查询必须去检查每行数据的版本号与事务的版本号是否相同。让我们来看看当隔离级别是REPEATABLE READ时这种策略是如何应用到特定的操作的

**SELECT InnoDB必须每行数据来保证它符合两个条件**

1、InnoDB必须找到一个行的版本，它至少要和事务的版本一样老(也即它的版本号不大于事务的版本号)。这保证了不管是事务开始之前，或者事务创建时，或者修改了这行数据的时候，这行数据是存在的。

2、这行数据的删除版本必须是未定义的或者比事务版本要大。这可以保证在事务开始之前这行数据没有被删除。

#### 表操作语法

```sql
alter table tableName
{ ADD COLUMN <列名> <类型> <first|after 列名>
| CHANGE COLUMN <旧列名> <新列名> <新列类型>
| ALTER COLUMN <列名> { SET DEFAULT <默认值> | DROP DEFAULT }
| MODIFY COLUMN <列名> <类型>
| DROP COLUMN <列名>
| RENAME TO <新表名>
| CHARACTER SET <字符集名>
| COLLATE <校对规则名> }
```

+ **查看表结构**

  ```sql
  DESC tableName;
  
  +-------+------------------------+------+-----+---------+----------------+
  | Field | Type                   | Null | Key | Default | Extra          |
  +-------+------------------------+------+-----+---------+----------------+
  | id    | int(11)                | NO   | PRI | NULL    | auto_increment |
  | wjmc  | varchar(200)           | YES  |     | NULL    |                |
  | ys    | int(11)                | YES  |     | 0       |                |
  | lx    | set('磁介质','纸介质')   | YES  |     | NULL    |                |
  +-------+------------------------+------+-----+---------+----------------+
  ```

  各个字段的含义如下：

  - Null：表示该列是否可以存储 NULL 值。
  - Key：表示该列是否已编制索引。PRI 表示该列是表主键的一部分，UNI 表示该列是 UNIQUE 索引的一部分，MUL 表示在列中某个给定值允许出现多次。
  - Default：表示该列是否有默认值，如果有，值是多少。
  - Extra：表示可以获取的与给定列有关的附加信息，如 AUTO_INCREMENT 等。

#### 约束

​	查看约束（查看见表语句）

```sql
SHOW CREATE TABLE <数据表名>;
```


1. 主键约束

   - 每个表只能定义一个主键。
   - 主键值必须唯一标识表中的每一行，且不能为 NULL，即表中不可能存在有相同主键值的两行数据。这是唯一性原则。
   - 一个字段名只能在联合主键字段表中出现一次。
   - 联合主键不能包含不必要的多余字段。当把联合主键的某一字段删除后，如果剩下的字段构成的主键仍然满足唯一性原则，那么这个联合主键是不正确的。这是最小化原则。

   ```sql
   --修改表时添加主键约束
   ALTER TABLE <数据表名> ADD PRIMARY KEY(<字段名>);
   
   --删除主键
   ALTER TABLE <数据表名> DROP PRIMARY KEY;
   ```

   修改表时要设置表中某个字段的主键约束时，要确保设置成主键约束的字段中值不能够有重复的，并且要保证是非空的。

   **单字段主键**

   ```sql
   --创建表时 定义字段的同时指定主键
   <字段名> <数据类型> PRIMARY KEY [默认值]
   --创建表时 义完所有字段之后指定主键
   [CONSTRAINT <约束名>] PRIMARY KEY [字段名]
   ```

   **联合主键**

   主键是由一张表中多个字段组成的，例如：学生选课中选课表主键可设置学生id课程id联合主键为主键

   ```sql
   PRIMARY KEY [字段1，字段2，…,字段n]
   ```

   

   **自增**

   ```sql
   字段名 数据类型 AUTO_INCREMENT
   
   --设置初始值
   )AUTO_INCREMENT=100;
   ```

   - 一个表中只能有一个字段使用 AUTO_INCREMENT 约束，<font  color='red'>且该字段必须有唯一索引，以避免序号重复（即为主键或主键的一部分）。</font>
   - AUTO_INCREMENT 约束的字段必须具备 NOT NULL 属性。
   - AUTO_INCREMENT 约束的字段只能是整数类型（TINYINT、SMALLINT、INT、BIGINT 等）。
   - AUTO_INCREMENT 约束字段的最大值受该字段的数据类型约束，如果达到上限，AUTO_INCREMENT 就会失效。

2. 外键约束<font color='red'>（不建议使用）</font>

   + 级联问题 每次级联delete或update的时候，都要级联操作相关的外键表，不论有没有这个必要，由其在高并发的场景下，这会导致性能瓶颈
   + 增加数据库压力 外键等于把数据的一致性事务实现，全部交给数据库服务器完成，并且有了外键，当做一些涉及外键字段的增，删，更新操作之后，需要触发相关操作去检查，而不得不消耗资源
   + 死锁问题 高并发大流量事务场景，使用外键还可能容易造成死锁
   + 开发不方便 有外键时，无论开发还是维护，需要手工维护数据时，都不太方便，要考虑级联因素

3. 唯一约束

   唯一约束与主键约束有一个相似的地方，就是它们都能够确保列的唯一性。与主键约束不同的是，<font color='red'>唯一约束在一个表中可以有多个，并且设置唯一约束的列是允许有空值的，虽然只能有一个空值。</font>

   ```sql
   --添加唯一约束
   ALTER TABLE <数据表名> ADD CONSTRAINT <唯一约束名> UNIQUE(<列名>);
   --删除唯一约束
   ALTER TABLE <表名> DROP INDEX <唯一约束名>;
   ```

4. 检查约束

   将 CHECK 约束子句置于表中某个列的定义之后，则这种约束也称为基于列的 CHECK 约束。

   若将 CHECK 约束子句置于所有列的定义以及主键约束和外键定义之后，则这种约束也称为基于表的 CHECK 约束。该约束可以同时对表中多个列设置限定条件。

   ```sql
   ALTER TABLE tb_emp7 ADD CONSTRAINT <检查约束名> CHECK(<检查约束>)
   ALTER TABLE <数据表名> DROP CONSTRAINT <检查约束名>;
   ```

5. 非空约束

   ```sql
   --创建表时
   <字段名> <数据类型> NOT NULL;
   --添加非空约束
   ALTER TABLE <数据表名>CHANGE COLUMN <字段名><字段名> <数据类型> NOT NULL;
   --删除非空约束
   ALTER TABLE <数据表名>CHANGE COLUMN <字段名> <字段名> <数据类型> NULL;
   ```

   

6. 默认值约束

   默认值约束通常用在已经设置了非空约束的列

   ```sql
   --创建表时
   <字段名> <数据类型> DEFAULT <默认值>;
   --修改表时添加默认值
   ALTER TABLE <数据表名> CHANGE COLUMN <字段名> <数据类型> DEFAULT <默认值>;
   --删除默认值约束
   ALTER TABLE <数据表名> CHANGE COLUMN <字段名> <字段名> <数据类型>;
   ```

   

#### 比较

1. <font color='red'>字符串的比较是不区分大小写的</font>
2. = 若有一个或两个操作数为 NULL，则比较运算的结果为 NULL
3. = 若一个操作数为字符串，另一个操作数为数字，则 MySQL 可以自动将字符串转换为数字。
4. <> 如果两侧操作数有一个是 NULL，那么返回值也是 NULL。
5. <= 如果两侧操作数有一个是 NULL，那么返回值也是 NULL。
6. IN  expr in ('str' , null)   expr 依次与待选值比较(或的关系)，有一个相同则返回1  <font color='red'>注意:null与任意比较返回null</font>
7. NOT IN expr in ('str',null) expr 依次与待选值比较（与的关系），有一个值相同返回0 <font color='red'>注意：null与任意比较返回null</font>

#### 位运算

位运算一般用于操作整数，对整数进行位运算才有实际的意义。整数在内存中是以补码形式存储的，正数的补码形式和原码形式相同，而负数的补码形式和它的原码形式是不一样的。

#### 关键字执行顺序

```sql
(7)     SELECT 
(8)     DISTINCT <select_list>
(1)     FROM <left_table>
(3)     <join_type> JOIN <right_table>
(2)     ON <join_condition>
(4)     WHERE <where_condition>
(5)     GROUP BY <group_by_list>
(6)     HAVING <having_condition>
(9)     ORDER BY <order_by_condition>
(10)    LIMIT <limit_number>
```

#### 函数及流程控制语句

1. 数值函数

   | 函数名称        | 作用                                                       |
   | --------------- | :--------------------------------------------------------- |
   | ABS             | 求绝对值                                                   |
   | MOD             | 求余                                                       |
   | CEIL 和 CEILING | 两个函数功能相同，都是返回不小于参数的最小整数，即向上取整 |
   | FLOOR           | 向下取整，返回值转化为一个BIGINT                           |
   | RAND            | 生成一个0~1之间的随机数，传入整数参数是，用来产生重复序列  |
   | ROUND           | 对所传参数进行四舍五入                                     |

2. 字符串函数

   | 函数名称                                                 | 作 用                                                        |
   | -------------------------------------------------------- | ------------------------------------------------------------ |
   | [LENGTH](http://c.biancheng.net/mysql/length.html)       | 计算字符串长度函数，返回字符串的字节长度                     |
   | [CONCAT](http://c.biancheng.net/mysql/concat.html)       | 合并字符串函数，返回结果为连接参数产生的字符串，参数可以使一个或多个 |
   | [INSERT](http://c.biancheng.net/mysql/insert.html)       | 替换字符串函数 <br>INSERT(s1，x，len，s2) 返回字符串 s1，s1从x位置删除len长度字符插入s2<br>x 超过字符串长度直接返回s1<br> len 大于剩余的长度直接删除全部<br>若任何一个参数为 NULL，则返回值为 NULL |
   | [LOWER](http://c.biancheng.net/mysql/lower.html)         | 若 x 超过字符串长度，则返回值为原始字符串。假如 len 的长度大于其他字符串的长度，则从位置 x 开始替换。若任何一个参数为 NULL，则返回值为 NULL。将字符串中的字母转换为小写 |
   | [UPPER](http://c.biancheng.net/mysql/upper.html)         | 将字符串中的字母转换为大写                                   |
   | [LEFT](http://c.biancheng.net/mysql/left.html)           | 从左侧字截取符串，返回字符串左边的若干个字符<br>LEFT(s，n) 函数返回字符串 s 最左边的 n 个字符 |
   | [RIGHT](http://c.biancheng.net/mysql/right.html)         | 从右侧字截取符串，返回字符串右边的若干个字符                 |
   | [TRIM](http://c.biancheng.net/mysql/trim.html)           | 删除字符串左右两侧的空格                                     |
   | [REPLACE](http://c.biancheng.net/mysql/replace.html)     | 字符串替换函数，返回替换后的新字符串<br>REPLACE(s，s1，s2) 使用字符串 s2 替换字符串 s 中所有的字符串 s1。 |
   | [SUBSTRING](http://c.biancheng.net/mysql/substring.html) | 截取字符串，返回从指定位置开始的指定长度的字符换<br>SUBSTRING(s，n，len)    len 可选参数，不选到结尾<br>从字符串 s 的第n个位置截取长度为len的字符串，n为负数从结尾往前数向后截取len个字符 |
   | [REVERSE](http://c.biancheng.net/mysql/reverse.html)     | 字符串反转（逆序）函数，返回与原始字符串顺序相反的字符串     |

3. 日期函数

   | 函数名称                                                     | 作 用                                                        |
   | ------------------------------------------------------------ | ------------------------------------------------------------ |
   | [CURDATE 和 CURRENT_DATE](http://c.biancheng.net/mysql/curdate_current_date.html) | 两个函数作用相同，返回当前系统的日期值                       |
   | [CURTIME 和 CURRENT_TIME](http://c.biancheng.net/mysql/curtime_current_time.html) | 两个函数作用相同，返回当前系统的时间值                       |
   | [NOW 和 SYSDATE](http://c.biancheng.net/mysql/now_sysdate.html) | 两个函数作用相同，返回当前系统的日期和时间值                 |
   | [UNIX_TIMESTAMP](http://c.biancheng.net/mysql/unix_timestamp.html) | 获取UNIX时间戳函数，返回一个以 UNIX 时间戳为基础的无符号整数 |
   | [FROM_UNIXTIME](http://c.biancheng.net/mysql/from_unixtime.html) | 将 UNIX 时间戳转换为时间格式，与UNIX_TIMESTAMP互为反函数     |
   | [MONTH](http://c.biancheng.net/mysql/month.html)             | 获取指定日期中的月份                                         |
   | [YEAR](http://c.biancheng.net/mysql/year.html)               | 获取年份，返回值范围是 1970〜2069                            |
   | [TIME_TO_SEC](http://c.biancheng.net/mysql/time_to_sec.html) | 将时间参数转换为秒数                                         |
   | [SEC_TO_TIME](http://c.biancheng.net/mysql/sec_to_time.html) | 将秒数转换为时间，与TIME_TO_SEC 互为反函数                   |
   | [DATE_ADD 和 ADDDATE](http://c.biancheng.net/mysql/date_add_adddate.html) | 两个函数功能相同，都是向日期添加指定的时间间隔               |
   | [DATE_SUB 和 SUBDATE](http://c.biancheng.net/mysql/date_sub_subdate.html) | 两个函数功能相同，都是向日期减去指定的时间间隔               |
   | [ADDTIME](http://c.biancheng.net/mysql/addtime.html)         | 时间加法运算，在原始时间上添加指定的时间                     |
   | [SUBTIME](http://c.biancheng.net/mysql/subtime.html)         | 时间减法运算，在原始时间上减去指定的时间                     |
   | [DATEDIFF](http://c.biancheng.net/mysql/datediff.html)       | 获取两个日期之间间隔，返回参数 1 减去参数 2 的值             |

4. 聚合函数

   | 函数名称                                         | 作用                                                         |
   | ------------------------------------------------ | ------------------------------------------------------------ |
   | [MAX](http://c.biancheng.net/mysql/max.html)     | 查询指定列的最大值                                           |
   | [MIN](http://c.biancheng.net/mysql/min.html)     | 查询指定列的最小值                                           |
   | [COUNT](http://c.biancheng.net/mysql/count.html) | 统计查询结果的行数<br>COUNT(*)、COUNT(1) 计算表中总的行数，无论某列有数值或者为空值。<br>COUNT（字段名）计算指定列下总的行数，计算时将忽略空值的行。<br>[SQL优化 COUNT](./SQL优化.md) |
   | [SUM](http://c.biancheng.net/mysql/sum.html)     | 求和，返回指定列的总和 (忽略计算中的 NULL 值)                |
   | [AVG](http://c.biancheng.net/mysql/avg.html)     | 求平均值，返回指定列数据的平均值(忽略计算中的 NULL 值,也不平均) |

5. 流程控制函数

   | 函数名称                                           | 作用                                                         |
   | -------------------------------------------------- | ------------------------------------------------------------ |
   | [IF](http://c.biancheng.net/mysql/if.html)         | 判断，流程控制 <br>IF(expr,v1,v2)<br>其中：表达式 expr 得到不同的结果，当 expr 为真是返回 v1 的值，否则返回 v2. |
   | [IFNULL](http://c.biancheng.net/mysql/ifnull.html) | 判断是否为空<br>IFNULL(v1,v2)<br>其中：如果 v1 不为 NULL，则 IFNULL 函数返回 v1; 否则返回 v2 的结果。 |
   | [CASE](http://c.biancheng.net/mysql/case.html)     | 搜索语句<br>简单CASE句：CASE  <表达式>    WHEN <值1> THEN <操作>    WHEN <值2> THEN <操作>    ...    ELSE <操作> END ;<br>CASE  WHEN <条件1> THEN <命令>     WHEN <条件2> THEN <命令>     ...     ELSE commands END CASE<br>简单 CASE 语句仅允许将表达式的值与一组不同的值进行匹配。 为了执行更复杂的匹配，如范围，则可以使用可搜索 CASE 语句。 可搜索 CASE 语句等同于 IF 语句，但是它的构造更加可读。 |
   
6. 流程控制语句

   | 流程控制语句名称       | 示例                                                         | 说明                                                         |
   | ---------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
   | if 语句                | IF search_condition THEN statement_list<br/>[ELSEIF search_condition THEN statement_list]...    <br/>[ELSE statement_list] <br/>END IF |                                                              |
   | case语句               | CASE case_value    <br/>WHEN when_value THEN statement_list     <br/>[WHEN when_value THEN statement_list]...     <br/>[ELSE statement_list] <br/>END CASE |                                                              |
   | loop leave iterate语句 | add_num:LOOP     <br/>SET @count=@count+1;    <br/>IF @count=100 THEN        <br/>LEAVE add_num;     <br/>ELSE IF MOD(@count,3)=0 THEN         <br/>ITERATE add_num;   END IF  <br/>SELECT * FROM employee;<br/>END LOOP add_num; | LOOP 只实现了一个简单的循环，并不进行条件判断,可以使某些特定的语句重复执行。<br/>LEAVE 语句主要用于跳出循环控制。<br>ITERATE 是“再次循环”的意思，用来跳出本次循环，直接进入下一次循环。 |
   | repeat语句             | [begin_label:] REPEAT<br/>statement_list<br/>UNTIL search_condition<br/>END REPEAT [end_label] | REPEAT 语句是有条件控制的循环语句<br/>begin_label和end_label相同可以省略<br/>search_condition 参数表示结束循环的条件，满足该条件时循环结束。 |
   | while语句              | [begin_label:] WHILE search_condition DO<br/>statement list<br/>END WHILE [end label] | begin_label和end_label可省略<br/>WHILE 语句也是有条件控制的循环语句。<br/>search_condition 参数表示循环执行的条件，满足该条件时循环执行 |

#### 查询语句

 1. 查询语句书写顺序

    ```sql
    SELECT
    DISTINCT {* | <字段列名>}
    [
    FROM <表 1>, <表 2>…
    [WHERE <表达式>
    [GROUP BY <group by definition>
    [HAVING <expression> [{<operator> <expression>}…]]
    [ORDER BY <order by definition>]  --ASC 升序（默认） DESC
    [LIMIT[<offset>,] <row count>]
    ]
    ```

2. DISTINCT 去重
   - DISTINCT 关键字只能在 SELECT 语句中使用。
   - 在对一个或多个字段去重时，DISTINCT 关键字`必须`在所有字段的最前面。

3. 查询条件
   
- <font color='red'>XOR</font>：记录满足其中一个条件，并且不满足另一个条件时，才会被查询出来。
  
4. 模糊查询

   + **注意大小写**。MySQL 默认是不区分大小写的。如果区分大小写<font color='red'>（like binary 't%'）</font>，像“Tom”这样的数据就不能被“t%”所匹配到。

   + **注意尾部空格**，尾部空格会干扰通配符的匹配。例如，“T% ”就不能匹配到“Tom”。(匹配以T开头空格结尾的字符串)

   + **注意 NULL**。“%”通配符可以到匹配任意字符，但是不能匹配 NULL。也就是说 “%”匹配不到 tb_students_info 数据表中值为 NULL 的记录。

   + %匹配任意个  _匹配一个

     <font color='red'>注意 </font>

   + 不要过度使用通配符，如果其它操作符能达到相同的目的，应该使用其它操作符。因为 MySQL 对通配符的处理一般会比其他操作符花费更长的时间。

   + 在确定使用通配符后，除非绝对有必要，否则<font color='red'>不要把它们用在字符串的开始处</font>。把通配符置于搜索模式的开始处，搜索起来是最慢的。

   + 仔细注意通配符的位置。如果放错地方，可能不会返回想要的数据。

5. 分组查询

   + GROUP BY 与 GROUP_CONCAT()  <br>GROUP BY 关键字可以和 GROUP_CONCAT() 函数一起使用。GROUP_CONCAT() 函数会把每个分组的字段值都显示出来。

     ```sql
     select lx,group_concat(id) from bw_602 group by lx;
     +---------------+------------------+
     | lx            | group_concat(id) |
     +---------------+------------------+
     | NULL          | 3                |
     | 纸介质         | 4                |
     | 磁介质,纸介质   | 1,2              |
     +---------------+------------------+
     ```

   + GROUP BY 与 WITH ROLLUP <br>WITH POLLUP 关键字用来在所有记录的最后加上一条记录，这条记录是上面所有记录的总和，即统计记录数量。

     ```sql
     select lx,group_concat(id) from bw_602 group by lx with rollup;
     +---------------+------------------+
     | lx            | group_concat(id) |
     +---------------+------------------+
     | NULL          | 3                |
     | 纸介质         | 4                |
     | 磁介质,纸介质   | 1,2              |
     | NULL          | 3,4,1,2          |
     +---------------+------------------+
     ```


#### 多表连接

 1. 交叉连接**CROSS JOIN 可省略**（笛卡尔积）

    交叉连接时使用 WHERE 子句，MySQL 会<font color='red'>先生成两个表的笛卡尔积</font>，然后再选择满足 WHERE 条件的记录。一般情况下不建议使用交叉连接。

2. 内连接

   内连接使用 **INNER JOIN**（简写JOIN） 关键字连接两张表，并使用 ON 子句来设置连接条件(利用条件表达式来消除交叉连接的某些数据行)。如果没有连接条件，INNER JOIN 和 CROSS JOIN 在语法上是等同的，两者可以互换。

   也称为等值连接，返回两张表都满足条件的部分

3. 外连接

   + 左外连接 **LEFT OUTER JOIN**

     取左边的表的全部，右边的表按条件，符合的显示，不符合则显示null

   + 右外连接 **RIGHT OUTER JOIN**

     取右边的表的全部，左边的表按条件，符合的显示，不符合则显示null

   + 全外连接 **FULL JOIN**（全外连接和交叉连接的结果是不同的）

   **outer join 的执行过程分为4步**

   1. 先对两个表执行交叉连接(笛卡尔积)

   2. 应用on筛选器

   3. <font color='red'>添加外部行</font>

   4. 应用where筛选器

   5. 例子

      ```sql
      select * from bw_602 a left join bw_t b on a.id=b.id where a.wjmc ='文件名称';
      --先on筛选完再添加null的数据项 再where筛选
      +------+----------+-----+------+--------+------+------+------+------+
      | id   | wjmc     | zrr | ys   | lx     | id   | name | ref  | def  |
      +------+----------+-----+------+--------+------+------+------+------+
      |    4 | 文件名称  |     |   50 | 纸介质  | NULL | NULL | NULL | NULL |
      +------+----------+-----+------+--------+------+------+------+------+
      select * from bw_602 a left join bw_t b on a.id=b.id and a.wjmc ='文件名称'
      --先on筛选完再添加null的数据项
      +------+--------------+-----+------+---------------+------+------+------+------+
      |    4 | 文件名称      |     |   50 | 纸介质         | NULL | NULL | NULL | NULL |
      |    3 | 测试         |     | NULL | NULL          | NULL | NULL | NULL | NULL |
      |    1 | 测试文件      |     |  200 | 磁介质,纸介质   | NULL | NULL | NULL | NULL |
      |    2 | 测试文件3333  |     |  100 | 磁介质,纸介质   | NULL | NULL | NULL | NULL |
      +------+--------------+-----+------+---------------+------+------+------+------+
      ```
   
      表连接（内连接和外连接等）都可以用子查询替换，但反过来却不一定，有的子查询不能用表连接来替换。子查询比较灵活、方便、形式多样，适合作为查询的筛选条件，而表连接更适合于查看连接表的数据。
   
      [不推荐使用 子查询和JOIN](./SQL优化.md)

#### REGEXP正则表达式查询

```sql
属性名 REGEXP '匹配方式'
```

| 选项         | 说明                                  | 例子                                       | 匹配值示例                 |
| ------------ | ------------------------------------- | ------------------------------------------ | -------------------------- |
| ^            | 匹配文本的开始字符                    | '^b' 匹配以字母 b 开头的字符串             | book、big、banana、bike    |
| $            | 匹配文本的结束字符                    | 'st$' 匹配以 st 结尾的字符串               | test、resist、persist      |
| .            | 匹配任何单个字符                      | 'b.t' 匹配任何 b 和 t 之间有一个字符       | bit、bat、but、bite        |
| *            | 匹配零个或多个在它前面的字符          | 'f*n' 匹配字符 n 前面有任意个字符 f        | fn、fan、faan、abcn        |
| +            | 匹配前面的字符 1 次或多次             | 'ba+' 匹配以 b 开头，后面至少紧跟一个 a    | ba、bay、bare、battle      |
| <字符串>     | 匹配包含指定字符的文本                | 'fa' 匹配包含‘fa’的文本                    | fan、afa、faad             |
| [字符集合]   | 匹配字符集合中的任何一个字符          | '[xz]' 匹配 x 或者 z                       | dizzy、zebra、x-ray、extra |
| [^]          | 匹配不在括号中的任何字符              | '[^abc]' 匹配任何不包含 a、b 或 c 的字符串 | desk、fox、f8ke            |
| 字符串{n,}   | 匹配前面的字符串至少 n 次             | 'b{2}' 匹配 2 个或更多的 b                 | bbb、bbbb、bbbbbbb         |
| 字符串 {n,m} | 匹配前面的字符串至少 n 次， 至多 m 次 | 'b{2,4}' 匹配最少 2 个，最多 4 个 b        | bbb、bbbb                  |

```sql
select * from bw_t where name regexp  '^测试'; --以测试开头
select * from bw_t where name regexp  '[^测试check]'; --不包好测试时check
```

#### 数据操作插入修改删除

1. insert into...from

   ```sql
   insert into bw_717 (id,wjmc) select id,name from bw_t where id=200;
   ```

2. update 可以跟order by 控制更新顺序 跟limit控制更新条数

   ```sql
   UPDATE <表名> SET 字段 1=值 1 [,字段 2=值 2… ] [WHERE 子句 ][ORDER BY 子句] [LIMIT 子句]
   update bw_717 set zrr='曾杰文' order by id desc limit 1;
   ```

3. delete 可以跟order by 控制更新顺序 跟limit控制删除条数

   ```sql
   DELETE FROM <表名> [WHERE 子句] [ORDER BY 子句] [LIMIT 子句]
   delete from bw_717  order by id desc limit 1;
   ```

   

4. delete、truncate、DROP

   + 不需要该表时，用 DROP；当仍要保留该表，但要删除所有记录时，用 TRUNCATE；当要删除部分记录时，用 DELETE。

   + DELETE 是逐行一条一条删除记录的；TRUNCATE 则是直接删除原来的表，再重新创建一个一模一样的新表，而不是逐行删除表中的数据，执行数据比 DELETE 快。
   + DELETE 删除数据后，配合事件回滚可以找回数据；TRUNCATE 不支持事务的回滚，数据删除后无法找回。
   + DELETE 删除数据后，系统不会重新设置自增字段的计数器；TRUNCATE 清空表记录后，系统会重新设置自增字段的计数器。
   + DELETE 的使用范围更广，因为它可以通过 WHERE 子句指定条件来删除部分数据；而 TRUNCATE 不支持 WHERE 子句，只能删除整体。
   + DELETE 会返回删除数据的行数，但是 TRUNCATE 只会返回 0，没有任何意义。

#### 多表联合更新

```sql
update bw_t join bw_717 on bw_t.id = bw_717.id set bw_t.ref='ref2',bw_717.wjmc=bw_t.name;
update bw_t left join bw_717 on bw_t.id=bw_717.id set bw_t.def=2,bw_717.ys=10;
```

#### 处理无效数据值

```sql
--查看模式
select @@sql_mode;
--插入或更新数据时执行更严格的检查，那么可以启用以下两种 SQL 模式中的一种
SET sql_mode = 'STRICT_ALL_TABLES' ;
SET sql_mode = 'STRICT_TRANS_TABLES';
```

默认情况下，MySQL 会按照以下规则来处理越界（即超出取值范围）的值和其他非正常值：

- 对于数值列或 TIME 列，超出合法取值范围的那些值将被截断到取值范围最近的那个端点，并把结果值存储起来。

- 对于除 TIME 列以外的其他类型列，非法值会被转换成与该类型一致的“零”值。

- 对于字符串列（不包括 ENUM 或 SET），过长的字符串将被截断到该列的最大长度。

- 给 ENUM 或 SET 类型列进行赋值时，需要根据列定义里给出的合法取值列表进行。如果把不是枚举成员的值赋给 ENUM列，那么列的值就会变成空字符串。如果把包含非集合成员的子字符串的值赋给 SET 列，那么这些字符串会被清理，剩余的成员才会被赋值给列。

  如果在执行增删改查等语句时发生了上述转换，那么 MySQL 会给出警告消息。在执行完其中的某一条语句之后，可以使用 `SHOW WARNINGS` 语句来查看警告消息的内容

#### SQL注入

1. 预防SQL注入
   + 使用java PreparedStatement（PreparedStatement能防止注入，是因为它把单引号转义了，变成了\'，这样一来，就无法截断SQL语句，进而无法拼接SQL语句）
   + 使用正则表达式过滤传入的参数；
   + 对用户输入的数据类型做严格限制；(对用户输入的内容要时刻保持警惕,只有客户端的验证等于没有验证)
   + 在应用发布之前建议使用专业的SQL注入检测工具进行检测，以及时修补被发现的SQL注入漏洞。网上有很多这方面的开源工具，例<font color='red'>sqlmap、SQLninja</font>等。
   + 避免网站打印出SQL错误信息，比如类型错误、字段不匹配等，把代码里的SQL语句暴露出来，以防止攻击者利用这些错误信息进行SQL注入。(永远不要把服务器错误信息暴露给用户)

#### 视图

```sql
--创建
CREATE VIEW <视图名> AS <SELECT语句>
--查看
DESC 视图名;
SHOW CREATE VIEW 视图名;
--修改
ALTER VIEW <视图名> AS <SELECT语句>
--删除
DROP VIEW <视图名1> [ , <视图名2> …]
```

#### 索引

索引就是根据表中的一列或若干列按照一定顺序建立的列值与记录行之间的对应关系表，实质上是一张描述索引列的列值与原表中记录行之间一 一对应关系的有序表。

+ **索引语法**

1. 创建索引

   ```sql
   --1.在已有表上创建索引
   --<长度>：可选项。指定使用列前的 length 个字符来创建索引。使用列的一部分创建索引有利于减小索引文件的大小，节省索引列所占的空间。
   --ASC|DESC：可选项。ASC指定索引按照升序来排列，DESC指定索引按照降序来排列，默认为ASC
   CREATE <索引名> ON <表名> (<列名> [<长度>] [ ASC | DESC])
   ALTER TABLE tablename ADD INDEX [<索引名>] [<索引类型>] (<列名>,…)
   ALTER TABLE tablename ADD PRIMARY KEY [<索引类型>] (<列名>,…)
   ALTER TABLE tablename ADD UNIQUE [ INDEX | KEY] [<索引名>] [<索引类型>] (<列名>,…)
   --2.建表同时创建索引
   CONSTRAINT PRIMARY KEY [索引类型] (<列名>,…)
   KEY | INDEX [<索引名>] [<索引类型>] (<列名>,…)
   UNIQUE [ INDEX | KEY] [<索引名>] [<索引类型>] (<列名>,…) --唯一索引
   ```

2. 查看索引

   ```sql
   SHOW INDEX FROM <表名> [ FROM <数据库名>]
   ```

   | 参数         | 说明                                                         |
   | ------------ | ------------------------------------------------------------ |
   | Table        | 表示创建索引的数据表名，这里是 tb_stu_info2 数据表。         |
   | Non_unique   | 表示该索引是否是唯一索引。若不是唯一索引，则该列的值为 1；若是唯一索引，则该列的值为 0。 |
   | Key_name     | 表示索引的名称。                                             |
   | Seq_in_index | 表示该列在索引中的位置，如果索引是单列的，则该列的值为 1；如果索引是组合索引，则该列的值为每列在索引定义中的顺序。 |
   | Column_name  | 表示定义索引的列字段。                                       |
   | Collation    | 表示列以何种顺序存储在索引中。在 MySQL 中，升序显示值“A”（升序），若显示为 NULL，则表示无分类。 |
   | Cardinality  | 索引中唯一值数目的估计值。基数根据被存储为整数的统计数据计数，所以即使对于小型表，该值也没有必要是精确的。基数越大，当进行联合时，MySQL 使用该索引的机会就越大。 |
   | Sub_part     | 表示列中被编入索引的字符的数量。若列只是部分被编入索引，则该列的值为被编入索引的字符的数目；若整列被编入索引，则该列的值为 NULL。 |
   | Packed       | 指示关键字如何被压缩。若没有被压缩，值为 NULL。              |
   | Null         | 用于显示索引列中是否包含 NULL。若列含有 NULL，该列的值为 YES。若没有，则该列的值为 NO。 |
   | Index_type   | 显示索引使用的类型和方法（BTREE、FULLTEXT、HASH、RTREE）。   |
   | Comment      | 显示评注。                                                   |

3. 删除索引

   ```sql
   DROP INDEX <索引名> ON <表名>
   ```

   

+ **访问数据库表的行数据两种方式：**

1. 顺序访问

   顺序访问是在表中实行全表扫描，从头到尾逐行遍历，直到在无序的行数据中找到符合条件的目标数据。顺序访问实现比较简单，但是当表中有大量数据的时候，效率非常低下。

2. 索引访问

   索引访问是通过遍历索引来直接访问表中记录行的方式。使用这种方式的前提是对表建立一个索引，在列上创建了索引之后，查找数据时可以直接根据该列上的索引找到对应记录行的位置，从而快捷地查找到数据。索引存储了指定列数据值的指针，根据指定的排序顺序对这些指针排序。

+ **索引优缺点**

1. 优点
   - 通过创建唯一索引可以保证数据库表中每一行数据的唯一性。
   - 可以给所有的 MySQL 列类型设置索引。
   - 可以大大加快数据的查询速度，这是使用索引最主要的原因。
   - 在实现数据的参考完整性方面可以加速表与表之间的连接。
   - 在使用分组和排序子句进行数据查询时也可以显著减少查询中分组和排序的时间

2. 缺点

   - 创建和维护索引组要耗费时间，并且随着数据量的增加所耗费的时间也会增加。

   - 索引需要占磁盘空间，除了数据表占数据空间以外，每一个索引还要占一定的物理空间。如果有大量的索引，索引文件可能比数据文件更快达到最大文件尺寸。

   - 当对表中的数据进行增加、删除和修改的时候，索引也要动态维护，这样就降低了数据的维护速度。

     <font color='red'>注:</font>索引可以提高查询速度，但是会影响插入记录的速度。因为，向有索引的表中插入记录时，数据库系统会按照索引进行排序，这样就降低了插入记录的速度，插入大量记录时的速度影响会更加明显。这种情况下，最好的办法是先删除表中的索引，然后插入数据，插入完成后，再创建索引。

+ **索引创建原则**

  1. 查询频次较高的表，且数据量较大的表建立索引

  2. 索引的字段经常出现在where 子句中的字段，如果where 子句的条件比较多，应该挑选最常用的，过滤效果最好的建立索引

  3. 尽量使用唯一索引，区分度越高，索引效率越高

  4. 索引可以有效提升查询的效率，但是索引不是越多越好，索引越多，维护索引的代价自然水涨船高，对于数据的更新等管理操作，比较频繁的表来说，索引过多会导致相当高的维护代价，较低dml的操作效率

  5. 使用短索引，索引创建后也是使用硬盘来存储，因此提升索引的访问的i/o效率，也可以提升总体的访问效率，假如构成索引的字段总长度比较短，那么在给定大小的存储块内可以存储更多的索引值，相应的可以提升mysql访问索引的i/o效率

  6. 利用最左前缀，N个列组合而成的索引，那么相当于创建了N个索引，如果查询时where子句中使用了组成该索引的前几个字段，那么这条件查询sql可以利用组合索引提升查询的效率.

     总结：1.表数据量大频繁作为查询条件的字段 2.尽量使用唯一索引区分度高 3.频繁更新的字段不适合做索引 4.使用短索引 5.利用最左前缀
     
     

+ **索引类型和种类**

  1. 索引类型

     索引类型：FULLTEXT，HASH，BTREE，RTREE。
     + FULLTEXT
       即为全文索引，目前只有MyISAM引擎支持。其可以在CREATE TABLE ，ALTER TABLE ，CREATE INDEX 使用，不过目前只有 CHAR、VARCHAR ，TEXT 列上可以创建全文索引。

     + HASH
       由于HASH的唯一（几乎100%的唯一）及类似键值对的形式，很适合作为索引。
       HASH索引可以一次定位，不需要像树形索引那样逐层查找,因此具有极高的效率。但是即只在“=”和“in”条件下高效，对于范围查询、排序及组合索引仍然效率不高。

     + BTREE
       BTREE索引就是一种将索引值按一定的算法，存入一个树形的数据结构中（二叉树），每次查询都是从树的入口root开始，依次遍历node，获取leaf。MySQL里默认和最常用的索引类型。

     + RTREE
       RTREE在MySQL很少使用，仅支持geometry数据类型，支持该类型的存储引擎只有MyISAM、BDb、InnoDb、NDb、Archive几种。
       相对于BTREE，RTREE的优势在于范围查找。

  2. 索引种类

     + 普通索引：仅加速查询
     + 唯一索引：加速查询 + 列值唯一（可以有null）
     + 主键索引：加速查询 + 列值唯一（不可以有null）+ 表中只有一个
     + 组合索引：多列值组成一个索引，专门用于组合搜索，其效率大于索引合并
     + 全文索引：对文本的内容进行分词，进行搜索
       ps.
       索引合并，使用多个单列索引组合搜索
       覆盖索引，select的数据列只用从索引中就能够取得，不必读取数据行，换句话说查询列要被所建的索引覆盖

+ **索引是否能命中**

  + 命中索引

    WHERE和JOIN中出现的列需要建立索引并且条件未<，<=，=，>，>=，BETWEEN，IN，以及后匹配的LIKE才会使用索引

  + 命不中索引的情况

    1. like 前匹配（'%文字' '_文字'）

    2. 列使用函数 where reverse(name) 对筛选列进行了运算

    3. or 中有为建索引的列

    4. 类型不一致(where name = 999 name为字符串类型) <font color='red'>如果是主键或索引是整数类型，则还是会走索引</font>

    5. 当根据索引排序时候，选择的映射列中有不是索引，则不走索引<font color='red'>主键排序，则还是走索引</font>

    6. 组合索引 最左前缀（name,index) 

       ```sql
       where name='' and index =1 --走索引
       where index='' and name =1 --走索引
       where name='' or index =1 --走索引
       where index='' or name =1 --不走索引
       where name='' --走索引
       where index=1 --不走索引
       ```

+ **大数据分页**

  原(弊端：每一条select语句都会从1遍历至当前位置，若跳转到第100页，则会遍历1000条记录)

  ```sql
  SELECT * FROM table_name LIMIT 0,10;--第一页
  SELECT * FROM table_name LIMIT 10,10;--第二页
  SELECT * FROM table_name LIMIT 20,10;--第三页
  ```

  改进(优点：若已知每页的max_id和min_id，则可以通过主键索引来快速定位)

  ```sql
  --下一页SELECT * FROM table_name WHERE id in (SELECT id FROM table_name WHERE id > max_id LIMIT 10);
  --上一页SELECT * FROM table_name WHERE id in (SELECT id FROM table_name WHERE id < min_id ORDER BY id DESC LIMIT 10);
  --5.7子查询中不支持limit 需要再加一层
  --This version of MySQL doesn't yet support 'LIMIT & IN/ALL/ANY/SOME subquery'
  select * from smzt where id in (select t.id from (select id from smzt where id>1000000 limit 10) as t);
  ```

  局限：1. 只能一页一页查询

  			2. id是顺序的
     			3. 前端要传给后台最新一页的最后一个id
              			4. 后端不能任意排序

+ **SQL注意事项**

  > 避免使用select *
  > - 创建表时尽量时 char 代替 varchar
  > - 表的字段顺序固定长度的字段优先
  > - 组合索引代替多个单列索引（经常使用多个条件查询时）
  > - 尽量使用短索引
  > - 使用连接（JOIN）来代替子查询(Sub-Queries)
  > - 连表时注意条件类型需一致
  > - 索引散列值（重复多）不适合建索引，例：性别不适合

#### 存储过程

+ **存储过程操作**

  ```sql
  --1、创建存储过程
  delimiter //   --修改sql结束符
  create procedure psmzt(in id int(11))
  begin
  select * from smzt where id = id;
  end
  //
  
  --参数类别
  [ IN | OUT | INOUT ] <参数名> <类型>
  
  --2、查看存储过程
  SHOW PROCEDURE STATUS LIKE 存储过程名;
  SHOW CREATE PROCEDURE 存储过程名;
  SELECT * FROM information_schema.routines WHERE routine_name='ShowStuScore'; --存储过程存在information_schema库下routines表中
  --3、删除存储过程
  DROP PROCEDURE [ IF EXISTS ] <过程名>
  ```

+ **存储函数**

  ```sql
  --创建存储函数
  delimiter //
  create function func_smzt(id int(11))
  returns varchar(30)
  comment '涉密载体查询'
  begin
  return (select name from smzt where smzt.id=id);
  end
  //
  ```

  

+ **变量**

  ```sql
  --定义变量 可以定义多个
  DECLARE var_name[,...] type [DEFAULT value]
  --变量赋值 可以多个
  SET var_name = expr[,var_name = expr]...
  --SELECT..INTO 为变量赋值
  SELECT col_name [...] INTO var_name[,...] FROM table_name WEHRE condition
  ```

  

#### 事务

​	[查看事务](./MySQL事务.md)

#### 字符集及校对规则

​	字符集（Character set）定义了字符和二进制的对应关系，为字符分配了唯一的编号。常见的字符集有 ASCII、GBK、IOS-8859-1 等。

​	字符编码（Character encoding）也可以称为字集码，规定了如何将字符的编号存储到计算机中。

​	校对规则（Collation）也可以称为排序规则，是指在同一个字符集内字符之间的比较规则。字符集和校对规则是一对多的关系，每个字	符集都有一个默认的校对规则。字符集和校对规则相辅相成，相互依赖关联。

#### 用户管理

+ 创建用户

  ```sql
  --1.
  --用户:创建用户账号，格式为 user_name'@'host_name。这里的user_name是用户名，host_name为主机名，即用户连接 MySQL 时所用主机的名字。如果在创建的过程中，只给出了用户名，而没指定主机名，那么主机名默认为“%”，表示一组主机，即对所有主机开放权限
  --IDENTIFIED BY:于指定用户密码。
  --PASSWORD 'password':PASSWORD 表示使用哈希值设置密码，该参数可选。如果密码是一个普通的字符串，则不需要使用 PASSWORD 关键字。'password' 表示用户登录时使用的密码，需要用单引号括起来。
  
  CREATE USER <用户> [ IDENTIFIED BY [ PASSWORD ] 'password' ] [ ,用户 [ IDENTIFIED BY [ PASSWORD ] 'password' ]]
  
  create user mabin identified by password '*6BB4837EB74329105EE4568DDA7DC67ED2CA2AD9';
  create user mabin@localhost identified by password '123456';
  
  --2.
  --insert 方式新增用户
  --ssl_cipher、x509_issuer 和 x509_subject 这 3 个字段没有默认值，所以向 user 表插入新记录时，一定要设置这 3 个字段的值，否则 INSERT 语句将不能执行。
  INSERT INTO mysql.user(Host, User,  authentication_string, ssl_cipher, x509_issuer, x509_subject) VALUES ('hostname', 'username', PASSWORD('password'), '', '', '');
  
  --3.
  --GRANT语句新建用户
  GRANT priv_type ON database.table TO user [IDENTIFIED BY [PASSWORD] 'password']
  --创建名为 test3 的用户，主机名为 localhost，密码为 test3。该用户对所有数据库的所有表都有 SELECT 权限。
  GRANT SELECT ON*.* TO 'test3'@localhost IDENTIFIED BY 'test3';
  
  --FLUSH 命令让用户生效
  FLUSH PRIVILEGES;
  ```

+ 修改用户

  ```sql
  RENAME USER <旧用户> TO <新用户>
  RENAME USER 'test1'@'localhost'TO 'testUser1'@'127.0.0.1'
  ```

+ 删除用户

  ```sql
  --使用 DROP USER 语句删除普通用户
  DROP USER <用户1> [ , <用户2> ]…
  DROP USER 'test1'@'localhost';
  
  --使用DELETE语句删除普通用户
  DELETE FROM mysql.user WHERE Host='hostname' AND User='username';
  ```

+ 查看用户权限

  ```sql
  SELECT * FROM mysql.user;
  
  SHOW GRANTS FOR root@localhost;
  ```

+ 授权

  ```sql
  GRANT priv_type [(column_list)] ON database.table
  TO user [IDENTIFIED BY [PASSWORD] 'password']
  [, user[IDENTIFIED BY [PASSWORD] 'password']] ...
  [WITH with_option [with_option]...]
  ```

  + priv_type 参数表示权限类型；
  + columns_list 参数表示权限作用于哪些列上，省略该参数时，表示作用于整个表；
  + database.table 用于指定权限的级别；
  + user 参数表示用户账户，由用户名和主机名构成，格式是“'username'@'hostname'”；
  + IDENTIFIED BY 参数用来为用户设置密码；
  + password 参数是用户的新密码。
  + WITH 关键字后面带有一个或多个 with_option 参数。这个参数有 5 个选项，详细介绍如下：
    - GRANT OPTION：被授权的用户可以将这些权限赋予给别的用户；
    - MAX_QUERIES_PER_HOUR count：设置每个小时可以允许执行 count 次查询；
    - MAX_UPDATES_PER_HOUR count：设置每个小时可以允许执行 count 次更新；
    - MAX_CONNECTIONS_PER_HOUR count：设置每小时可以建立 count 个连接;
    - MAX_USER_CONNECTIONS count：设置单个用户可以同时具有的 count 个连接。

  + 在 GRANT 语句中可用于指定权限级别的值有以下几类格式（ON后面的）：
    - *：表示当前数据库中的所有表。
    - *.*：表示所有数据库中的所有表。
    - db_name.*：表示某个数据库中的所有表，db_name 指定数据库名。
    - db_name.tbl_name：表示某个数据库中的某个表或视图，db_name 指定数据库名，tbl_name 指定表名或视图名。
    - db_name.routine_name：表示某个数据库中的某个存储过程或函数，routine_name 指定存储过程名或函数名。
    - TO 子句：如果权限被授予给一个不存在的用户，MySQL 会自动执行一条 CREATE USER 语句来创建这个用户，但同时必须为该用户设置密码。

  附：

  <权限类型>可以指定为以下值：

  | 权限名称                       | 对应user表中的字段 | 说明                                                       |
  | ------------------------------ | ------------------ | ---------------------------------------------------------- |
  | SELECT                         | Select_priv        | 授予用户可以使用 SELECT 语句进行访问特定表的权限           |
  | INSERT                         | Insert_priv        | 授予用户可以使用 INSERT 语句向一个特定表中添加数据行的权限 |
  | DELETE                         | Delete_priv        | 授予用户可以使用 DELETE 语句从一个特定表中删除数据行的权限 |
  | DROP                           | Drop_priv          | 授予用户可以删除数据表的权限                               |
  | UPDATE                         | Update_priv        | 授予用户可以使用 UPDATE 语句更新特定数据表的权限           |
  | ALTER                          | Alter_priv         | 授予用户可以使用 ALTER TABLE 语句修改数据表的权限          |
  | REFERENCES                     | References_priv    | 授予用户可以创建一个外键来参照特定数据表的权限             |
  | CREATE                         | Create_priv        | 授予用户可以使用特定的名字创建一个数据表的权限             |
  | INDEX                          | Index_priv         | 授予用户可以在表上定义索引的权限                           |
  | ALL 或 ALL PRIVILEGES 或 SUPER | Super_priv         | 所有的权限名                                               |

+ 删除用户权限

  ```sql
  REVOKE priv_type [(column_list)]...
  ON database.table
  FROM user [, user]...
  
  --priv_type 参数表示权限的类型；
  --column_list 参数表示权限作用于哪些列上，没有该参数时作用于整个表上；
  --user 参数由用户名和主机名构成，格式为“username'@'hostname'”。
  
  revoke delete on *.* from mabin@127.0.0.1;
  ```

+ 登录

  ```sql
  --登录
  mysql -h hostname|hostlP -P port -u username -p
  --登录并切到指定库
  mysql -h hostname|hostlP -P port -u username -p DatabaseName
  --执行某条语句后退出
  mysql -h hostname|hostlP -P port -u username -p DatabaseName -e "SQL语句"
  
  mysql -h127.0.0.1 -P3306 -uroot -p123456 hz717 -e "select * from aqzsk"
  ```

+ 修改密码

  ```sql
  --1.root 修改普通用户和自己密码
  SET PASSWORD FOR 'username'@'hostname' = PASSWORD ('newpwd');
  --2.root 修改普通用户和自己密码
  UPDATE MySQL.user SET authentication_string = PASSWORD("newpwd") WHERE User = "username" AND Host = "hostname";
  FLUSH PRIVILEGES;--加载权限

  --3.root和普通用户修改自己密码
  SET PASSWORD = PASSWORD('newpwd');
  
  --4.grant
  GRANT USAGE ON *.* TO 'user'@’hostname’ IDENTIFIED BY 'newpwd';
  ```
  
  ```sql
  --修改root密码 密码要用""
  mysqladmin -u username -h hostname -p password "newpwd"
  mysqladmin -P3306 -uroot -p password "123456"
  ```
  
  

#### MySQL权限控制原理

权限按照以下权限表的顺序得到数据库权限：user→db→tables_priv→columns_priv→procs_priv。在这几个权限表中，权限范围依次递减，全局权限覆盖局部权限。

[说明：http://c.biancheng.net/view/8011.html](http://c.biancheng.net/view/8011.html)

#### MySQL日志

```sql
--日志输出位置
set global log_output='FILE,TABLE';
```

1. 通用日志

   ```sql
   --通用日志表mysql.general_log
   --通用日志是否开启
   SHOW VARIABLES like 'general_log';
   --通用日志输出文件
   SHOW VARIABLES like 'general_log_file';
   --通用日志输出位置
   SHOW VARIABLES like 'log_output';
   ```

2. 慢SQL日志

   ```sql
   --慢SQL日志表mysql.slow_log
   --慢SQL日志是否开启
   show variables like 'slow_query_log';
   --通用日志输出文件
   show variables like 'slow_query_log_file';
   --慢SQL超过多少才记录 --设置完成之后需要重新连接生效
   show variables like 'long_query_time';
   SET GLOBAL long_query_time=0.1;
   ```

MySQL优化

​	优化查询速度、更新速度、优化MySQL服务器。

 	1. 定位低效的SQL

+ 如果慢查询日志中记录内容很多，可以使用 mysqldumpslow 工具（ MySQL 客户端安装自带,使用需要安装perl环境）来对慢查询日志进行分类汇总。

  ```sql
  mysqldumpslow mysql_master-slow.log
  Reading mysql slow query log from mysql_master-slow.log
  Count: 2 Time=11.00s (22s) Lock=0.00s (0s) Rows=1.0 (2), root[root]@mysql_master
  select count(N) from t_user;
  ```

  对于 SQL 文本完全一致，只是变量不同的语句， mysqldumpslow 将会自动视为同一个语句进行统计，变量值用 N 来代替。这个统计结果将大大增加用户阅读慢查询日志的效率，并迅速定位系统的 SQL 瓶颈。

+ show processlist 

  命令查看当前 MySQL 在进行的线程，包括线程的状态、是否锁表等，可以实时地查看 SQL 的 执行情况，同时对一些锁表操作进行优化。

	|列|描述|
	| ---- | ------------------------------------------------------------ |
	|ID列|一个标识，你要kill一个语句的时候很有用，用命令杀掉此查询 /*/mysqladmin kill 进程号。|
	| user列 | 显示当前用户，如果不是root，这个命令就只显示你权限范围内的sql语句。|
	| host列 | 显示这个语句是从哪个ip的哪个端口上发出的。用于追踪出问题语句的用户。|                                                           |
	| db列  | 显示这个进程目前连接的是哪个数据库。|
	|command列|显示当前连接的执行的命令，一般就是休眠（sleep），查询（query），连接（connect）。|
	|time列|此这个状态持续的时间，单位是秒。|
	|state列|显示使用当前连接的sql语句的状态，很重要的列，后续会有所有的状态的描述，请注意，state只是语句执行中的某一个状态，一个 sql语句，以查询为例，可能需要经过copying to tmp table，Sorting result，Sending data等状态才可以完成|
	|info列|显示这个sql语句，因为长度有限，所以长的sql语句就显示不全，但是一个判断问题语句的重要依据。|
	
	| state列意义 | 描述 |
	| ----------- | ---- |
	|            Checking table            |正在检查数据表（这是自动的）。|
	|Copying to tmp table on disk|　由于临时结果集大于tmp_table_size，正在将临时表从内存存储转为磁盘存储以此节省内存。|
	|Creating tmp table|正在创建临时表以存放部分查询结果。|
	|deleting from main table|服务器正在执行多表删除中的第一部分，刚删除第一个表。|
	|deleting from reference tables|服务器正在执行多表删除中的第二部分，正在删除其他表的记录。|
	|Flushing tables|正在执行FLUSH TABLES，等待其他线程关闭数据表。|
	|Killed|发送了一个kill请求给某线程，那么这个线程将会检查kill标志位，同时会放弃下一个kill请求。MySQL会在每次的主循环中检查kill标志位，不过有些情况下该线程可能会过一小段才能死掉。如果该线程程被其他线程锁住了，那么kill请求会在锁释放时马上生效。|
	|Locked|被其他查询锁住了。|
	|Sending data|正在处理SELECT查询的记录，同时正在把结果发送给客户端。|
	|Sorting for group|正在为GROUP BY做排序。|
	|Sorting for order|正在为ORDER BY做排序。|
	|Opening tables|这个过程应该会很快，除非受到其他因素的干扰。例如，在执ALTER TABLE或LOCK TABLE语句行完以前，数据表无法被其他线程打开。正尝试打开一个表。|
	|Removing duplicates|正在执行一个SELECT DISTINCT方式的查询，但是MySQL无法在前一个阶段优化掉那些重复的记录。因此，MySQL需要再次去掉重复的记录，然后再把结果发送给客户端。|
	|Searching rows for update|正在讲符合条件的记录找出来以备更新。它必须在UPDATE要修改相关的记录之前就完成了。|
	|Sleeping|正在等待客户端发送新请求.|
	|System lock|正在等待取得一个外部的系统锁。如果当前没有运行多个mysqld服务器同时请求同一个表，那么可以通过增加--skip-external-locking参数来禁止外部系统锁。|
	|Updating|正在搜索匹配的记录，并且修改它们。|
	|User Lock|正在等待GET_LOCK()。|

表结构结构优化方式：

 1. 选择合适的数据类型

    + 使用可以存下数据的最小数据类型
    + 使用简单的数据类型。Int要比varchar类型在mysql处理上简单（使用int存储日期时间:FROM_UNIXTIME()将int类型时间戳转换成日期时间格式  UNIX_TIMESTAMP()将日期时间格式转换成int类型 、bigint来存储IP地址，利用INET_ATON() 将IP格式转换成int INET_NTOA()将int格式转换成正常IP格式）
    + 尽可能使用not null定义字段
    + 尽量少用text类型，非用不可时最好考虑分表

 2. 表的范式化和反范式化

 3. 表的垂直拆分

    + 把不常用的字段单独存放到一个表中。
    + 把大字段独立存放到一个表中。
    + 把经常一起使用的字段放到一起。

 4. 水平拆分 

    + 表的水平拆分是为了解决单表的数据量过大问题，水平拆分的表每个表的结构都是完全一致的

     常用的水平拆分方法：

      1、对customer_id进行hash运算，如果要拆分成5个表则使用`mod(custoneer_id,5)`取出0-4个	值

      2、针对不同的hashID把数据存到不同的表中