[TOC]

#### 1. 表空间和用户

1. **创建数据表空间**

   ```SQL
   create tablespace 数据表空间名称
       logging
       datafile  '路 径 ( 预 先 创 建 好 ) \数 据 文 件 名 称.dbf'
        size 100M  --指定大小为100M
        reuse autoextend on next 20m maxsize 1024M --允许自动扩展数据文件，数据大小超过100M，自动增长20M，最大可增长到1024M，若不扩展，则替换语句为 ：reuse autoextend off
        /**下面是可选指令，看需求而定*/
        extent management local autoallocate--本地管理方式(1、autoallocate:自动分配；2、uniform size *m：定制分配； 
        segment space management auto  --段管理方式（1、auto：自动管理；2、manual：手动管理）
   --临时表空间
   create temporary tablespace 临时表空间名称
       tempfile  '路 径 (预 先 创 建 好 ) \临 时 文 件 名 称.dbf' 
       size 20M;
   ```

2. **创建用户并指定表空间**

   ```sql
   create user 用户名 identified by 密码
   default tablespace 数据表空间名称
   temporary tablespace 临时表空间名称
   ```

3. **用户授权**

   ```sql
   grant connect,resource,dba to 用户名;
   /**
   
   除此之外还可授权：
   
            create table
            ,select any table
            ,select any dictionary 
            ,create public synonym
            ,create sequence
            ,create trigger 
            ,create type
            ,create view
            ,create indextype
            ,create job
            ,create materialized view
            ,create procedure
   
   */
   ```

4. **删除表空间以及对应的数据文件**

   ```mysql
   drop tablespace 表空间名称 including contents and datafiles; --执行该语句，再到本地把指定路径下的数据文件删除即可
   ```

5. **修改表空间**

   ```sql
   （1）该把表空间设置为脱机状态
            alter tablespace 表空间名称 offline normal;
   （2）创建新的文件夹，并且复制旧的test01.dbf到新的文件夹下
           alter tablespace 表空间名称 rename datafile
                  '旧路径\test01.dbf'
           to
                  '新路径\test02.dbf';
   （3）最后把表空间设置为联机状态
            alter tablespace 表空间名称 online；
   ```

6. **修改用户表空间**

   ```sql
   alter user 用户名  default tablespace 新的表空间;
   ```

7. **删除用户**

   ```sql
   drop user  用户名 cascade  --只删除用户，不删除表空间
   ```


#### 2. oracle12c 数据库未打开: 仅允许在固定表/视图中查询问题

​	[原文](https://www.cnblogs.com/hanmk/p/8553481.html)

```sql
sqlplus sys/orcl@192.168.0.1/orcl as sysdba
--打开数据库
alter database open;

--自动启动PDB Pluggable Database SYS用户创建如下触发器即可：
CREATE TRIGGER open_all_pdbs AFTER STARTUP ON DATABASE
BEGIN
  EXECUTE IMMEDIATE 'alter pluggable database all open';
END open_all_pdbs;
```

#### 3. ORA-28040: No matching authentication protocol问题

在sqlnet.ora文件追加：

```sql
--低版本
SQLNET.ALLOWED_LOGON_VERSION=8
--高版本
SQLNET.ALLOWED_LOGON_VERSION_SERVER=n
SQLNET.ALLOWED_LOGON_VERSION_CLIENT=n
```



