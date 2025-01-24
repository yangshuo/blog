# 用于记录postgresql常用的命令
## 用户相关操作
1. 创建用户
   ```sql
   -- 创建用户
   create user username with password 'password';
   
   --分配创建数据库的权限
   alter user username with createdb;
   -- 分配登陆的权限
   alter user username with login;
   
   -- 修改用户密码
   alter user username with password 'password'
   
   -- 创建管理员
   CREATE ROLE username WITH
     LOGIN
     SUPERUSER
     INHERIT
     CREATEDB
     CREATEROLE
     REPLICATION;
   ```
2. 创建数据库，并赋予权限
   ```sql
   -- 创建数据库,并指定所有者
   create database dbname owner username;
   
   -- 把一个已经存在的数据库的权限赋予某个用户
   grant all on database dbname to username;
   
   ```

## 数据库统计相关操作
* 查询单个数据库大小
```sql
select pg_size_pretty(pg_database_size('postgres')) as db_size;
```
* 查询素有数据库大小
```sql
select datname, pg_size_pretty(pg_database_size(datname)) as db_size from pg_database;
```
* 查询单个表大小
```sql
select 
   pg_size_pretty(pg_total_relation_size('table_name')) as table_full_size, -- 表大小（包括索引）
   pg_size_pretty(pg_relation_size('table_name')) as table_data_size -- 表大小（不包括索引)
```
* 查询所有表大小
```sql
select relname, pg_size_pretty(pg_relation_size(relid)) as table_size from pg_stat_user_tables;
```
* 查询表空间大小
```sql
select pg_size_pretty(pg_tablespace_size('table_space_name')) as table_size_size
```

* 查询所有表空间大小
```sql
select spcname, pg_size_pretty(pg_tablespace_size(spcname)) from pg_tablespace ;
```