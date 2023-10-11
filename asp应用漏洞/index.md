# ASP应用漏洞

# ASP应用漏洞

## 默认安装-MDB数据库泄漏下载

由于大部分ASP程序与ACCESS数据库搭建，但ACCESS无需连接，都在脚本文件中定义配置好数据库路径即用，不需要额外配置安装数据库，所以大部分提前固定好的数据库路径如默认未修改，当攻击者知道数据库的完整路径，可远程下载后解密数据实现攻击。

![](https://pic.imgdb.cn/item/64a129b71ddac507ccbd3ff9.jpg)

## HTTP.SYS（CVE-2015-1635）

### 1、漏洞描述

远程执行代码漏洞存在于 HTTP 协议堆栈 (HTTP.sys) 中，当 HTTP.sys 未正确分析经特殊设计的 HTTP 请求时会导致此漏洞。 成功利用此漏洞的攻击者可以在系统帐户的上下文中执行任意代码。

### 2、影响版本

Windows 7、Windows Server 2008 R2、Windows 8、Windows Server 2012、Windows 8.1 和 Windows Server 2012 R2

### 3、漏洞利用条件

安装了IIS6.0以上的Windows 7、Windows Server 2008 R2、Windows 8、Windows Server 2012、Windows 8.1 和 Windows Server 2012 R2版本

### 4、漏洞复现

Kali Linux root模式：

```
msfconsole

use auxiliary/dos/http/ms15_034_ulonglongadd

set rhosts xx.xx.xx.xx

set rport xx

run
```



## IIS短文件

### 1、漏洞描述

此漏洞实际是由HTTP请求中旧DOS 8.3名称约定(SFN)的代字符(~)波浪号引起的。它允许远程攻击者在Web根目录下公开文件和文件夹名称(不应该可被访问)。攻击者可以找到通常无法从外部直接访问的重要文件,并获取有关应用程序基础结构的信息。

### 2、漏洞成因:

为了兼容16位MS-DOS程序,Windows为文件名较长的文件(和文件夹)生成了对应的windows 8.3短文件名。在Windows下查看对应的短文件名,可以使用命令`dir /x`

### 3、应用场景：

后台路径获取，数据库文件获取，其他敏感文件获取等

### 4、利用工具：

https://github.com/irsdl/IIS-ShortName-Scanner

https://github.com/lijiejie/IIS_shortname_Scanner



## IIS文件解析

### IIS 6 解析漏洞

1. 该版本默认会将.asp;.jpg 此种格式的文件名，当成Asp解析
   如：logo.asp;.jpg 
2. 该版本默认会将.asp/目录下的所有文件当成Asp解析。
   如：xx.asp/logo.jpg

### IIS 7.x 解析漏洞

在一个文件路径(/xx.jpg)后面加上/xx.php会将/xx.jpg/xx.php 解析为php文件

**应用场景：**配合文件上传获取Webshell



## IIS写权限

前提条件：IIS版本<=6.0 、目录权限开启写入、开启WebDAV设置为允许

**应用：**使用postman发送put请求可以写入文件

相关文章：https://cloud.tencent.com/developer/article/2050105



## SQL注入-SQLMAP使用&ACCESS注入

ACCESS数据库无管理帐号密码，顶级架构为表名，列名（字段），数据，所以在注入猜解中一般采用字典猜解表和列再获取数据，猜解简单但又可能出现猜解不到的情况，由于Access数据库在当前安全发展中已很少存在，故直接使用SQLMAP注入。
