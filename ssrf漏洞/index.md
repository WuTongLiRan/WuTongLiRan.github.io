# SSRF漏洞

# SSRF漏洞

SSRF (Server-Side Request Forgery)——服务器端请求伪造

​	

## 原理

SSRF的形成大多是由于服务端提供了从其他服务器应用获取数据的功能且没有对目标地址做过滤与限制。例如，黑客操作服务端从指定URL地址获取网页文本内容，加载指定地址的图片等，利用的是服务端的请求伪造。SSRF利用存在缺陷的Web应用作为代理攻击远程和本地的服务器。

​	

## 危害

1、扫描资产

2、获取敏感信息

3、攻击内网服务器（绕过防火墙）

4、访问大文件，造成溢出

5、通过Redis写入WebShell或建立反弹连接

​	

## 相关函数

curl_exec() 执行 cURL 会话

file_get_contents() 将整个文件读入一个字符串

fsockopen() 打开一个网络连接或者一个Unix套接字连接

​	

## 相关伪协议

file:// 从文件系统中获取文件内容，如`file:///etc/passwd`

dict:// 字典服务器协议，访问字典资源，如`dict:///ip:6739/info：`

sftp:// SSH文件传输协议或安全文件传输协议

ldap:// 轻量级目录访问协议

tftp:// 简单文件传输协议

gopher:// 分布式文档传递服务，可使用gopherus生成payload

​	

## payload

```
http://example.com/ssrf.php?url=file:///etc/passwd
http://example.com/ssrf.php?url=file:///C:/Windows/win.ini
```




