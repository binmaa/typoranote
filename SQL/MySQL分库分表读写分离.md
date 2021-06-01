[TOC]

#### 一.分库分表原因

[原文：https://blog.csdn.net/wlg9527/article/details/105700526](https://blog.csdn.net/wlg9527/article/details/105700526)

##### 1. 影响数据库性能原因

+ 数据量

  MySQL单库数据量在5000万以内性能比较好,超过阈值后性能会随着数据量的增大而变弱。

  MySQL单表的数据量是500w-1000w之间性能比较好,超过1000w性能也会下降。

+ 磁盘

  因为单个服务的磁盘空间是有限制的,如果并发压力下,所有的请求都访问同一个节点,肯定会对磁盘IO造成非常大的影响。

+ 数据库连接

   海量用户同时访问同一数据库（根据不同的条件）

##### 2.解决方法

​	[垂直分库，垂直分表，水平分库，水平分表概念](https://blog.csdn.net/weixin_44062339/article/details/100491744)

+ 垂直拆分

  根据业务进行拆分，比如可以将一张表中的多个字段拆成两张表，一张是不经常更改的，一张是经常改的。

+ 水平拆分

  根据表来进行分割：比如user表可以拆分为user0,、user1、user2、user3、user4等

##### 3. 分库分表之后如何实现联合查询

使用第三方中间件来实现，比如：mycat、shading-jdbc

##### 4. 原理

当客户端发送一条sql查询：select * from user;此时中间件会根据有几个子表，拆分成多个语句：select * from user1;select * from user2;select * from user3等多条语句查询，然后将查询的结果返回给中间件，然后汇总给客户端。这些语句是并发执行的，所以效率会很高哦。同过拆分字段查询可以直接路由到对应的表不需要进行广播式查询。

##### 5. 常用的分库分表策略

+ hash取模

  假设有用户表user,将其分成3个表user0,user1,user2.路由规则是对3取模,当uid=1时,对应到的是user1,uid=2时,对应的是user2.

+ 范围分片

  从1-10000一个表,10001-20000一个表。

+ 地理位置分片
  华南区一个表,华北一个表。
+ 时间分片
  按月分片，按季度分片等等,可以做到冷热数据。

##### 6. 分库分表之后造成的负面影响

+ 分布式事务问题

  - 1.使用分布式事务中间件
  - 2.使用MySQL自带的针对跨库的事务一致性方案(XA),不过性能要比单库的慢10倍左右。
  - 3.能否避免掉跨库操作(比如将用户和商品放在同一个库中)

+ 跨库join的问题

  分库分表后表之间的关联将受到限制，将无法join位于不同分库的表，也无法join分表粒度不同的表，造成业务查询次数变多，

  解决办法：

  - 1、全局表：基础数据，所有的库都拷贝一份
  - 2、字段冗余：
  - 3、系统层组装:分别查询所有，然后组装起来，较复杂

+ 横向扩容问题

  当我们使用HASH取模做分表的时候,针对数据量的递增,可能需要动态的增加表,此时就需要考虑因为reHash导致数据迁移的问题。

+ 结果集合并、排序的问题

  因为我们是将数据分散存储到不同的库、表里的,当我们查询指定数据列表时,数据来源于不同的子库或者子表,就必然会引发结果集合并、排序的问题。如果每次查询都需要排序、合并等操作,性能肯定会受非常大的影响。可以考虑走缓存



#### 分库分表插件选择

[https://www.cnblogs.com/fyy-hhzzj/p/9044775.html](https://www.cnblogs.com/fyy-hhzzj/p/9044775.html)

##### 1. Mycat安装

​	[下载Mycat1.6.5+(支持单库分表)](http://dl.mycat.org.cn/1.6.5/)

​	解压后直接cd到bin目录

```shell
#安装服务
mycat install

#启动
mycat start

#停止
mycat stop

#重启
mycat restart
```

如果在启动时发现异常，在logs目录中查看日志。

- wrapper.log 为程序启动的日志，启动时的问题看这个
- mycat.log 为脚本执行时的日志，SQL脚本执行报错后的具体错误内容,查看这个文件。mycat.log是最新的错误日志，历史日志会根据时间生成目录保存。

##### 2. 配置

+ 目录介绍

  | 目录   | 说明                                        |
  | ------ | ------------------------------------------- |
  | bin    | mycat命令，启动、重启、停止等               |
  | catlet | catlet为Mycat的一个扩展功能                 |
  | conf   | Mycat 配置信息,重点关注                     |
  | lib    | Mycat引用的jar包，Mycat是java开发的         |
  | logs   | 日志文件，包括Mycat启动的日志和运行的日志。 |

+ 配置

  Mycat的配置文件都在conf目录里面，这里介绍几个常用的文件：

  | 文件         | 说明                                                         |
  | ------------ | ------------------------------------------------------------ |
  | wrapper.conf | 脚本配置文件，可以控制mycat jvm的一些启动参数（如-Xms和-Xmx这些） |
  | server.xml   | 配置mycat服务的参数，比如端口号，myact用户名和密码使用的逻辑数据库等 |
  | schema.xml   | 配置数据库的信息，例如逻辑数据库名称，物理上真实的数据源以及表和数据源之间的对应关系和路由策略等。 |
  | rule.xml     | 配置路由策略，主要有分片的片键，拆分的策略（取模还是按区间划分等） |

##### 3.读写分离配置（mycat并不能实现主从复制，需使用mysql实现主从复制）

Mycat的架构其实很好理解，Mycat是代理，Mycat后面就是物理数据库。和Web服务器的Nginx类似。对于使用者来说，访问的都是Mycat，不会接触到后端的数据库。

+ server.xml

  ```xml
  <!--name="mycat用户名"-->
  <user name="root" defaultAccount="true">
          <!---password：密码 schemas：逻辑库名 privileges：权限-->
      <property name="password">1qaz2wsx</property>
      <property name="schemas">TESTDB</property>
  </user>
  ```

+ schema.xml

  ```xml
  <!-- 数据库配置，与server.xml中的数据库对应 -->
  <!--<schema name="逻辑数据库名，与server.xml中的schema对应" checkSQLschema="数据库前缀相关设置，建议看文档，这里暂时设为false" sqlMaxLimit="select 时默认的limit，避免查询全表" dataNode="默认库">-->
  <schema name="TESTDB" checkSQLschema="false" sqlMaxLimit="100" dataNode="dn1">
      <!-- auto sharding by id (long) -->
      <!--<table name="表名，物理数据库中表名" 
     primaryKey="主键字段名，自动生成主键时需要设置" 
     dataNode="表存储到哪些节点，多个节点用逗号分隔。节点为下文dataNode设置的name" 
     autoIncrement="是否自增"
     rule="分片规则名，具体规则下文rule详细介绍" />-->
      <table name="user" primaryKey="id" dataNode="dn1" />
  </schema>
  <!-- 分片配置 -->
  <!-- 节点 <dataNode name="dn1$0-743" dataHost="localhost1" database="db$0-743"
    /> -->
  <!--<dataNode name="节点名，与table中dataNode对应" dataHost="物理数据库名，与datahost中name对应" database="物理数据库中数据库名" />-->
  <dataNode name="dn1" dataHost="localhost3308" database="TEST_DB" />
  
  <!-- 物理数据库配置 -->
  <!--<dataHost name="物理数据库名，与dataNode中dataHost对应" maxCon="1000" minCon="10" balance="均衡负载的方式"
       writeType="写入方式" dbType="	数据库类型" dbDriver="native" switchType="1"  slaveThreshold="100">
    <heartbeat>心跳检测语句，注意语句结尾的分号要加。</heartbeat>-->
  <dataHost name="localhost3308" maxCon="1000" minCon="10" balance="1"
            writeType="0" dbType="mysql" dbDriver="native" switchType="1"  slaveThreshold="100">
      <heartbeat>select user()</heartbeat>
      <!-- can have multi write hosts -->
      <writeHost host="hostM1" url="localhost:3308" user="root"
                 password="1qaz2wsx">
          <readHost host="hostM1" url="localhost:3307" user="root" password="1qaz2wsx" />
      </writeHost>
  </dataHost>
  ```

##### 4.分库分表

+ schema.xml

  ```xml
  <!-- 数据库配置，与server.xml中的数据库对应 -->
  <!--<schema name="逻辑数据库名，与server.xml中的schema对应" checkSQLschema="数据库前缀相关设置，建议看文档，这里暂时设为false" sqlMaxLimit="select 时默认的limit，避免查询全表">-->
  <schema name="TESTDB" checkSQLschema="false" sqlMaxLimit="100">
      <!-- auto sharding by id (long) -->
      <!--<table name="表名，物理数据库中表名" 
     primaryKey="主键字段名，自动生成主键时需要设置" 
     dataNode="表存储到哪些节点，多个节点用逗号分隔。节点为下文dataNode设置的name" 
     autoIncrement="是否自增"
     rule="分片规则名，具体规则下文rule详细介绍" />-->
      <table name="user" primaryKey="id" dataNode="dn1,dn2" rule="mod-long" />
  </schema>
  <!-- 分片配置 -->
  <!-- 节点 <dataNode name="dn1$0-743" dataHost="localhost1" database="db$0-743"/> -->
  <!--<dataNode name="节点名，与table中dataNode对应" dataHost="物理数据库名，与datahost中name对应" database="物理数据库中数据库名" />-->
  <dataNode name="dn1" dataHost="localhost3308" database="TEST_DB" />
  <dataNode name="dn2" dataHost="localhost3307" database="TEST_DB" />
  
  <!-- 物理数据库配置 -->
  <!--<dataHost name="物理数据库名，与dataNode中dataHost对应" maxCon="1000" minCon="10" balance="均衡负载的方式"
       writeType="写入方式" dbType="	数据库类型" dbDriver="native" switchType="1"  slaveThreshold="100">
    <heartbeat>心跳检测语句，注意语句结尾的分号要加。</heartbeat>-->
  <dataHost name="localhost3308" maxCon="1000" minCon="10" balance="0"
            writeType="0" dbType="mysql" dbDriver="native" switchType="1"  slaveThreshold="100">
      <heartbeat>select user()</heartbeat>
      <!-- can have multi write hosts -->
      <writeHost host="hostM1" url="localhost:3308" user="root"
                 password="1qaz2wsx">
      </writeHost>
  </dataHost>
  
  <dataHost name="localhost3307" maxCon="1000" minCon="10" balance="0"
            writeType="0" dbType="mysql" dbDriver="native" switchType="1"  slaveThreshold="100">
      <heartbeat>select user()</heartbeat>
      <!-- can have multi write hosts -->
      <writeHost host="hostM1" url="localhost:3307" user="root"
                 password="1qaz2wsx">
      </writeHost>
  </dataHost>
  ```

+ rule.xml

  ```xml
  <tableRule name="mod-long">
      <rule>
          <!--分片字段-->
          <columns>id</columns>
          <!--分片方法-->
          <algorithm>mod-long</algorithm>
      </rule>
  </tableRule>
  <function name="mod-long" class="io.mycat.route.function.PartitionByMod">
      <!-- how many data nodes 分片数 -->
      <property name="count">2</property>
  </function>
  ```

##### 5.单库分表(及读写分离)

+ schema.xml

  ```xml
  <schema name="TESTDB" checkSQLschema="false" sqlMaxLimit="100" dataNode="dn1">
      <!-- subTables水平分片表：department_1 department_2-->
      <table name="department" primaryKey="id" subTables="department_$1-2" dataNode="dn1" rule="mod-long" />
      <table name="user" primaryKey="id" type="global" dataNode="dn1"/>
  </schema>
  <dataNode name="dn1" dataHost="localhost1" database="test_db" />
  
  <dataHost name="localhost1" maxCon="1000" minCon="10" balance="1"
            writeType="0" dbType="mysql" dbDriver="native" switchType="1"  slaveThreshold="100">
      <heartbeat>select user()</heartbeat>
      <!-- can have multi write hosts -->
      <writeHost host="hostM1" url="localhost:3307" user="root"
                 password="1qaz2wsx">
          <readHost host="hostS3" url="localhost:3308" user="root" password="1qaz2wsx" />
      </writeHost>
  </dataHost>
  ```

+ rule.xml

  ```xml
  <tableRule name="mod-long">
      <rule>
          <!--分片字段-->
          <columns>id</columns>
          <!--分片方法-->
          <algorithm>mod-long</algorithm>
      </rule>
  </tableRule>
  <function name="mod-long" class="io.mycat.route.function.PartitionByMod">
      <!-- how many data nodes 分片数 -->
      <property name="count">2</property>
  </function>
  ```

##### 6.Mycat不适用场景

[https://blog.csdn.net/gaobudong1234/article/details/79581846](https://blog.csdn.net/gaobudong1234/article/details/79581846)

+ 非分片字段查询

  Mycat中的路由结果是通过**分片字段**和**分片方法**来确定的。**5.单库分表**中`department`表以id分片，分片方法为id值取模，根据模值确定在`department_0` `department_1`的某个分片中，查询条件有id可以将查询具体落到某个分片中，<font color='red'>查询条件没有id字段mycat进行广播将查询发送到所有节点</font>。会极大消耗Mycat和MySQL数据库资源。

+ 分页排序

  + Mycat对分页的处理

    ```sql
    select * from department limit 1;
    ```

    没有通过分片字段检索，广播到所有分片，并接收各个分片的返回结果。

    <font color="red">但Mycat向应用返回的结果集取决于哪个分片最先返回结果给Mycat，同一个SQL，在Mycat上执行时会有不同的返回结果。</font>

    <font color="red">解决该问题方式添加排序条件</font>

    ```sql
    select * from department order by name limit 1;
    ```

    有排序呢条件的情况下，Mycat接收到各个DB节点的返回结果后，对其进行最小堆运算，计算出所有结果集中最小的一条记录 返回给应用。

  + Mycat 对**有偏移量的排序分页**时

    Mycat在处理 **有偏移量的排序分页** 时是另外一套逻辑——**改写SQL** 

    Mycat在下发有 **limit m,n** 的SQL语句时会对其进行改写，改写成 **limit 0, m+n** 来保证查询结果的逻辑正确性。所以，Mycat发送到后端DB上的SQL语句是

    ```sql
    select * fromtable order by id limit 1,2;
    --改写成
    select * fromtable order by id limit 0,3;
    ```

    经过最小堆计算后得到最小序列 ，然后返回偏移量为1的两个结果。

    <font color="red">改写的缺陷</font>

    虽然Mycat返回了正确的结果，但是仔细推敲发现这类操作的处理逻辑是及其消耗（浪费）资源的。应用需要的结果集为2条，Mycat中需要处理的结果数为6条。也就是说，对于有 **t** 个DB节点的全分片 **limit m, n** 操作，Mycat需要处理的数据量为 **(m+n)\*t** 个。比如实际应用中有50个DB节点，要执行limit 1000,10操作，则Mycat处理的数据量为 **50500** 条，返回结果集为**10**，当偏移量更大时，内存和CPU资源的消耗则是数十倍增加。

+ 任意表JOIN

  Mycat时如果要进行表JOIN操作，要确保两个表的关联字段具有相同的数据分布

+ 分布式事务（弱XA事务）

  Mycat并没有根据二阶段提交协议实现 **XA事务**，而是只保证 **prepare** 阶段数据一致性的 **弱XA事务** ，实现过程如下：

  应用开启事务后Mycat标识该连接为非自动提交，比如前端执行

  `begin`后Mycat不会立即把命令发送到DB节点上，等后续下发SQL时，Mycat从连接池获取非自动提交的连接去执行。Mycat会等待各个节点的返回结果，如果都执行成功，Mycat给该连接标识为 **Prepare Ready** 状态，如果有一个节点执行失败，则标识为 **Rollback** 状态。执行完成后Mycat等待前端发送 *commit* 或 *rollback* 命令。发送 *commit* 命令时，Mycat检测当前连接是否为 **Prepare Ready** 状态，若是，则将 *commit* 命令发送到各个DB节点。

  这一阶段是无法保证一致性的，如果一个DB节点在 *commit* 时故障，而其他DB节点 *commit* 成功，Mycat会一直等待故障DB节点返回结果。Mycat只有收到所有DB节点的成功执行结果才会向前端返回 *执行成功* 的包，此时Mycat只能一直 *waiting* 直至*TIMEOUT*，导致事务一致性被破坏。

#### MySQL主从同步

https://blog.csdn.net/cool_summer_moon/article/details/106112560

主从复制可以水平扩展数据库的负载能力，容错，高可用，数据备份。

##### 1. MySQL安装

​	[MySQL安装：https://blog.csdn.net/weixin_40845192/article/details/117164835](https://blog.csdn.net/weixin_40845192/article/details/117164835)

##### 2. 原理

+ master的I/O线程将数据写入binlog中；

+ slave的I/O线程从master的binlog中读取数据，写入自己的Relay_Log_File日志中；

+ slave的SQL线程从Relay_Log_File日志中解析sql，完成数据的复制。

##### 3. 应用场景 

+ 从服务器作为主服务器的实时数据备份

+ 主从服务器实现读写分离（主写从读），从服务器实现负载均衡

+ 把多个从服务器根据业务重要性进行拆分访问（从服务器根据业务进行拆分）

##### 4. 搭建

+ 主库

  1. 修改配置文件 my.ini

     ```mysql
     ## 主从复制 主库
     # 开启bin-log二进制日志
     log-bin=mysql.bin
     # 使binlog在每N次binlog写入后与硬盘同步
     sync-binlog=1
     # 1天时间自动清理二进制日志
     expire_logs_days=1
     # 需要同步的数据库
     binlog-do-db=test_db
     # 不需要同步的数据库
     binlog-ignore-db = mysql 
     binlog-ignore-db = information_schema 
     binlog-ignore-db = performance_schema 
     binlog-ignore-db = sys
     # Error Logging.
     log-error="WIN-1QQDRPR5R2M.err"
     # Server Id.
     server-id=1
     ```

  2. 创建数据同步账户

     ```sql
     --登陆 mysql -u授权账户名 -p密码 -P端口
     mysql -uroot -p123456 -P3307
     --创建账户并授权
     grant replication slave on *.* to 'master'@'127.0.0.1' identified by '123456';
     --刷新权限
     flush privileges;
     ```

  3. 重启MySQL服务，查看binlog日志名及数据同步位置

     ```sql
     --停止服务
     net stop MySQL3307
     --启动服务
     net start MySQL3307
     --登陆 mysql -u授权账户名 -p密码 -P端口
     mysql -uroot -p123456 -P3307
     --显示主服务信息
     show master status \G;
     ---------------------1.row-----------------------
                  File: mysql.000001
              Position: 2574
     ```

+ 从库

  1. 修改配置文件

     ```mysql
     ## 主从复制 从库
     # Server Id.
     server-id=110
     # bind-address=127.0.0.1
     log_bin=mysql-bin
     # 二进制日志自动删除的天数
     expire_logs_days=1my
     ```

  2. 重启MySQL服务，slave与master建立连接，进行数据同步

     ```sql
     --停止服务
     net stop MySQL3308
     --启动服务
     net start MySQL3308
     --登陆 mysql -u授权账户名 -p密码 -P端口
     mysql -uroot -p123456 -P3308
     
     --slave与master建立连接，进行数据同步
     change master to master_host='127.0.0.1',master_user='master',master_password='123456', MASTER_PORT = 3307, master_log_file='mysql.000001',master_log_pos=2574;
     
     --启动同步
     start slave;
     ```

  3. 查看从库状态

     ```mysql
     show slave status \G;
     *************************** 1. row ***************************
                    Slave_IO_State: Waiting for master to send event
                       Master_Host: 127.0.0.1
                       Master_User: master
                       Master_Port: 3307
                     Connect_Retry: 60
                   Master_Log_File: mysql.000001
               Read_Master_Log_Pos: 6793
                    Relay_Log_File: һҶ֮Çï-relay-bin.000004
                     Relay_Log_Pos: 3698
             Relay_Master_Log_File: mysql.000001
                  Slave_IO_Running: Yes
                 Slave_SQL_Running: Yes
     ```

     `Slave_IO_Running: Yes` `Slave_SQL_Running: Yes` 表示主从同步启动

  4. 错误日志，及主从连接出错，重新配置

     默认位置：data文件夹下\主机名.err

     ```mysql
     --停止连接
     stop slave;
     --清除连接设置
     reset slave all;
     ```

  5. 从库进行了写操作，主从中止问题

     + 方式一 从新设置链接

       ```sql
       --主库
       --查看主服务信息
       show master status \G;
       ---------------------1.row-----------------------
                    File: mysql.000001
                Position: 5555
       
       --从库         
       --停止连接
       stop slave;
       --清除连接设置
       reset slave all;
       --slave与master重新建立连接，进行数据同步
       change master to master_host='127.0.0.1',master_user='master',master_password='123456', MASTER_PORT = 3307, master_log_file='mysql.000001',master_log_pos=5555;
       ```

     + 方式二 

       ```mysql
       #从库 
       #停止slave服务
       mysql> stop slave;
       #跳过一步错误，后面的数字可变，如果还不行可执行多次
       set global sql_slave_skip_counter =1;
       
       #开始slave服务
       start slave;
       #查看slave状态
       show slave status\G
       ```

##### 2. 主从同步延迟的原因及解决

​	[https://blog.csdn.net/hao_yunfeng/article/details/82392261](https://blog.csdn.net/hao_yunfeng/article/details/82392261)

+ 原因

  1. MySQL数据库主从同步延迟原理mysql主从同步原理：主库针对写操作，顺序写binlog，从库单线程去主库顺序读”写操作的binlog”，从库取到binlog在本地原样执行（随机写），来保证主从数据逻辑上一致。mysql的主从复制都是单线程的操作，主库对所有DDL和DML产生binlog，binlog是顺序写，所以效率很高，slave的Slave_IO_Running线程到主库取日志，效率比较高，下一步，问题来了，slave的Slave_SQL_Running线程将主库的DDL和DML操作在slave实施。DML和DDL的IO操作是随即的，不是顺序的，成本高很多，还可能可slave上的其他查询产生lock争用，由于Slave_SQL_Running也是单线程的，所以一个DDL卡主了，需要执行10分钟，那么所有之后的DDL会等待这个DDL执行完才会继续执行，这就导致了延时。有朋友会问：“主库上那个相同的DDL也需要执行10分，为什么slave会延时？”，答案是**master可以并发，Slave_SQL_Running线程却不可以**。

  2. MySQL数据库主从同步延迟是怎么产生的？当主库的TPS并发较高时，产生的DDL数量超过slave一个sql线程所能承受的范围，那么延时就产生了，当然还有就是可能与slave的大型query语句产生了锁等待。首要原因：数据库在业务上读写压力太大，CPU计算负荷大，网卡负荷大，硬盘随机IO太高次要原因：读写binlog带来的性能影响，网络传输延迟。

+ 解决

  1. 架构方面
     + 业务的持久化层的实现采用分库架构，mysql服务可平行扩展，分散压力。
     + 单个库读写分离，一主多从，主写从读，分散压力。这样从库压力比主库高，保护主库
     + 服务的基础架构在业务和mysql之间加入memcache或者redis的cache层。降低mysql的读压力。
     + 不同业务的mysql物理上放在不同机器，分散压力。
     + 使用比主库更好的硬件设备作为slave总结，mysql压力小，延迟自然会变小。

  2. 硬件方面

     + 采用好服务器，比如4u比2u性能明显好，2u比1u性能明显好。

     + 存储用ssd或者盘阵或者san，提升随机写的性能。

     + 主从间保证处在同一个交换机下面，并且是万兆环境。

       总结，硬件强劲，延迟自然会变小。一句话，缩小延迟的解决方案就是花钱和花时间。

  3. mysql主从同步加速
     + sync_binlog在slave端设置为0 （sync_binlog=0，表示MySQL不控制binlog的刷新，由文件系统自己控制它的缓存的刷新。这时候的性能是最好的，但是风险也是最大的。一旦系统Crash，在binlog_cache中的所有binlog信息都会被丢失。）
     + –logs-slave-updates 从服务器从主服务器接收到的更新不记入它的二进制日志。
     + 直接禁用slave端的binlog
     + slave端，如果使用的存储引擎是innodb，innodb_flush_log_at_trx_commit =2（默认值1的意思是每一次事务提交或事务外的指令都需要把日志写入（flush）硬盘）