## MySQL 笔记
### 安装
### 管理用户与权限
```sql
# 创建用户
create user identified by passwd; 
# 授予权限（同时可以创建用户）
grant priv on object to user identified by passwd;
# 查看用户
select name,host from mysql.user;
# 查看权限
show grants for user
# 收回权限
revoke priv on object from user
```
### 数据导入/导出
- mysqlimport
- mysql导入命令：`load data infile`
  
  `load data infile '...' into table mytable`

- mysql导出命令：`select into outfile '...'`

- 坑
  
  注意默认配置`secure-file-priv`的值会要求导入/导出数据的文件必须实在特定目录下面的。

  需要：把导入数据文件放到指定目录或修改`secure-file-priv`的值为`NULL`可以解决
