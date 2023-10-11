# SQL注入之数据类型

# SQL注入之数据类型

## 1、数字型注入点

- 结构 ：http://xxx.com/users.php?id=1
-  SQL 语句原型： `select * from 表名 where id=1` 
- 爆破：`id=1 and 1=1`
- 爆破后的SQL 语句：`select * from 表名 where id=1 and 1=1`

​	

## 2、 字符型注入点

- 结构 ：http://xxx.com/users.php?name=admin
-  SQL 语句原型大概为：`select * from 表名 where name='admin'`
- 爆破：`admin' and 1=1 #` 
- 爆破后的SQL 语句：`select * from 表名 where name='admin' and 1=1 #'` 

​	

## 3、 搜索型注入点

- 结构 ：一般在链接地址中有 `"keyword=关键字"` 有的不显示在的链接地址里面，而是直接通过搜索框表单提交。
-  SQL 语句原型大概为：`select * from 表名 where 字段 like '%关键字%'` 
- 爆破：`关键字%' or 1=1 #` 
- 爆破后的SQL 语句：`select * from 表名 where 字段 like '%关键字%' or 1=1 #%`

​	

## 4、 xx型注入点

- SQL 语句原型大概为：`select * from 表名 where username=('admin')` 
- 爆破：`'admin') and 1=1 #` 
- 爆破后的SQL 语句：`select * from 表名 where username=(''admin') and 1=1 #')`
- 常见的闭合符号：`'`     `"`   ` %`     `(`      `{`
