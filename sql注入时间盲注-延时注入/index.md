# SQL注入—时间盲注 (延时注入)

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
