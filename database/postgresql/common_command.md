# 用于记录postgresql常用的命令

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