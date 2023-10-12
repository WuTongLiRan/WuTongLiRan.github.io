# Linux查看和处理文件内容

# Linux查看和处理文件内容

## 文本文件和二进制文件

**文本文件：**

ASCII、UTF-8、Unicode、ANSI txt、xml、conf、properties、yml等配置文件、日志文件、源代码  

**二进制文件：**

可执行程序、图片、音频、视频

## 查看文件内容

`cat`

全拼：concatenate [kənˈkætəneɪt] 连接

格式：`cat 文件名`

## 逐页查看文件内容

`more/less `(一般用`less`)

| 操作                   | 命令示例                   |
| ---------------------- | -------------------------- |
| 分页查看               | `more redis.conf`          |
| 第三行开始显示         | `more +3 test.log`         |
| 从出现 "bind" 开始显示 | `more +/bind test.log`     |
| 下一行                 | `Enter` 或 箭头下          |
| 上一行                 | `y` 或 箭头上              |
| 下一屏                 | `Space`（空格）或 `Ctrl+F` |
| 上一屏                 | `b`                        |
| 退出                   | `q` 或 `Ctrl+C` 或 `ZZ`    |

/要查找的内容  配合n键，从上往下查所有

?要查找的内容  配合n键，从下往上查所有  

## 查看文件开头部分/末尾部分

`head`命令用于显示文件的开头部分，默认情况下显示文件的前10行

`tail`命令用于显示文件的末尾部分，默认情况下显示文件的最后10行

```
head -n 10 redis.conf --显示redis.conf文件的前十行

tail -n 10 info.log  --显示info.log文件的后十行
```

## 文本搜索

全拼：Globally search a Regular Expression and Print

作用：全局搜索正则表达式并打印

格式：`grep 选项 模式 文件名`

## 管道符号pipe  

把前一个命令原本要输出到屏幕的数据当作是后一个命令的标准输入

`command1 | command2 | command3  `

```
netstat -an|grep 3306
```

> `netstat -an`：用于列出所有网络连接的详细信息
>
> `grep 3306`：则用于筛选出包含3306的行，即筛选出MySQL端口
>
> `netstat -an|grep 3306`：列出所有网络连接，并作为筛选3306的内容

## 统计文件的字节数、字数、行数

`wc`

全拼：word count 

`-l`或`--lines `显示行数

`-w`或`--words` 只显示字数  

```
ls -l|wc -l
```

> 统计当前目录下的文件数：
>
> `ls -l`:将当前目录下的文件名按一行一行列出
>
> `wc -l`:统计行数
>
> `ls -l|wc -l`:将当前目录下的文件名按一行一行列出，作为统计行数的内容。即统计当前目录下的文件数。

## 比较给定的两个文件的不同

`diff`

全拼：different

```
diff A.txt B.txt  -- 比较A.txt和B.txt的不同内容
```


