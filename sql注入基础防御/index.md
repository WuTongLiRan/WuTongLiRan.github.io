# 

# 2.6 SQL注入之基础防御

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

​	

## 自定义函数来检测关键字

## 安全防护软件 WAF
