[toc]

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

#### 函数

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