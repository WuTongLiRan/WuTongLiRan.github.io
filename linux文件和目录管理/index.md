# Linux文件和目录管理

# Linux文件和目录管理  

## 命令格式

![](https://pic.imgdb.cn/item/64a53b771ddac507cc822644.jpg)

`Command Options Arguments`

命令            选项        参数  

- `Options`选项：命令的行为方式
- `Arguments`参数：命令的对象  



**Linux命令文档：**

[](https://wangchujiang.com/linux-command)

[](https://www.linuxcool.com)

## 文件与目录管理

### 列出目录内容和属性  

命令：`ls`

全拼：`list`

格式：`ls 选项 文件名`

```
 ls -a 

ll --block-size=M
```

### 打印工作路径

命令：`pwd`

全拼：`print working directory`

格式：`pwd`

```
pwd
```

### 切换工作目录

命令：`cd`

全拼：`change directory`

格式：`cd 相对路径或者绝对路径`

| 符号     | 指代                                         |
| -------- | -------------------------------------------- |
| 绝对路径 | 由根目录 `/` 开始写起                        |
| 相对路径 | 从当前所在的工作目录开始写起                 |
| `/`      | 根目录                                       |
| `.`      | 代表当前目录                                 |
| `~`      | 代表用户工作目录，例如 `vim ~/.bashrc`       |
| `../`    | 代表上一级目录                               |
| `../../` | 上上一级目录，以此类推，超出范围的时候代表根 |

```
cd ../../
```

### 查看文件类型  

命令：`file`

格式：`file 选项 文件或目录`

```
file -i 文件名
```

### 复制文件或目录

命令：`cp`

全拼：`copy`

格式：`cp 选项 源文件 目标文件`

`-R/r`：递归处理，将指定目录下的所有文件与子目录一并处理；

`-f`：强行复制文件或目录，不论目标文件或目录是否已存在；

```
cp -r Pictures/ pic
```

### 查找文件或者目录  

find格式：`find 目录 选项 名字或模式`

`-name` 名字

```
find /etc -name a*

find / -name "aaa" 2>/dev/null
```

`-type` 类型参数

`f `普通文件，`d` 目录

```
find /root -type f
```

`-size`大小

```
find /root -type f -size 10M+
```

`-exec command`把find找到的内容作为命令的参数去执行

`{}`就是找到的内容

```
find . -name "*.txt" -exec rm -rf {} \; --（包括子目录） 

find . -name aaa -exec mv {} bbb \;
```

### 其他查找命令  

`whereis` ：查找二进制程序、代码等相关文件路径

`which`：查找并显示给定命令的绝对路径

`locate`：updatedb程序每天会跑一次，建立文件索引  

### 创建目录

命令：`mkdir`

全拼：`make direcotry`

格式：`mkdir 选项 目录名`

```
mkdir test

mkdir -p /usr/local/soft/redis
```

### 移动或者重命名

命令：`mv`

全拼：`move`

格式：`mv 选项 原文件 新文件`

```
mv 1.txt 2.txt 

mv /a/1.txt /b/1.txt
```

### 删除文件

命令：`rm`

全拼：`remove`

格式：`rm 选项 (多个)文件名`

删除空目录：`rmdir`

`-r` 递归（连同子文件夹一起删除） -f 强制删除

```
find . -name "a.json" -exec rm -rf {}
```

> - `find .`：这个部分表示从当前目录开始进行查找。
> - `-name "a.json"`：这个部分指定了要查找的文件名为 "a.json"。可以根据实际需求修改文件名。
> - `-exec rm -rf {}`：这个部分表示对于找到的每一个文件，执行` rm -rf {}`命令进行删除操作。
>   - `rm` 是一个用于删除文件或目录的命令。
>   - `-rf` 是 `rm` 命令的选项，其中 `-r` 表示递归删除，即删除目录及其内容，`-f` 表示强制执行删除操作，无需确认。
>   - `{}` 是一个占位符，表示 `find` 命令找到的文件。
>
> `find . -name "a.json" -exec rm -rf {}` 命令的作用是从当前目录开始递归查找名为 "a.json" 的文件，并将找到的每一个文件都删除，包括目录及其内容。

### 创建空文件

命令：`touch`

格式：`touch 选项 文件名`

```
touch a.txt
```

## 挂载和链接

### 挂载mount

一个目录树怎么使用多个磁盘？  

![](https://pic.imgdb.cn/item/64a5411f1ddac507cc8f3c82.jpg)

原路径：`/dev/sdb1 `  挂载到： `/sdb-u`

```
mkdir /sdb-u

mount /dev/sdb1 /sdb-u
```

挂载后：![](https://pic.imgdb.cn/item/64a5413f1ddac507cc8f8375.jpg)

### 链接

命令：`ln`

全拼：`link`

格式：`ln 源文件 链接文件`

创建硬链接： 

```
ln 1.php hard.php
```

**注意：**

- 用户不能给目录创建硬链接
- 只有相同的文件系统才可以创建硬链接（tmpfs NTFS FAT32）

### 软链接

查看软链接：

```
 ll /usr/bin/nc
```

创建软链接：

```
ln -s /usr/local/phpstudy/system/phpstudyctl /usr/bin/study
```

使用：`study`

**源文件删除，软连接失效**

