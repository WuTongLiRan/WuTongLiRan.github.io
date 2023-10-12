# Linux文件压缩及解压缩

# 文件压缩及解压缩

## 打包和压缩的区别

**打包：**多个文件变成一个文件——减少文件个数

**压缩：**缩小一个文件的体积——减少文件体积  

## 常见压缩格式

**HTTP：**gzip

gzip、 deflate(zlib的格式)、 br(Brotli)、identity(不压缩）

**Windows：**zip、rar、7z

**Linux：**gzip（tar.gz=.tgz）、bzip2（.bz2）、zip

> 压缩后体积: tar.bz2 < tgz < tar
>
> 压缩解压时间: tar.bz2 < tar < tgz

## tar命令(Tape Archive)

### tar常用选项

| 选项 | 作用                               | 单词    |
| ---- | ---------------------------------- | ------- |
| `-c` | 创建打包文件                       | create  |
| `-v` | 显示打包或解包的详细信息           | verbose |
| `-f` | 指定文件名称，必须放到所有选项后面 | file    |
| `-z` | 压缩或解压缩（.gz）                | -       |
| `-j` | 压缩或解压缩（.bz2）               | -       |
| `-x` | 解包                               | -       |
| `-C` | 解压缩到指定目录                   | -       |

### tar用法

| 操作           | 命令示例                        |
| -------------- | ------------------------------- |
| 打包（不压缩） | `tar -cvf test.tar test/`       |
| 解包           | `tar -xvf test.tar`             |
| 打包并gz压缩   | `tar -zcvf test.tar.gz test/`   |
| 解压           | `tar -zxvf test.tar.gz`         |
| 解压到指定目录 | `tar -zxvf test.tar.gz -C aaa`  |
| 打包并bz2压缩  | `tar -jcvf test.tar.bz2 test/`  |
| 解压           | `tar -jxvf test.tar.bz2 test/`  |
| 解压到指定目录 | `tar -jxvf test.tar.bz2 -C aaa` |

### tar其他操作

| 操作         | 命令示例                                                     |
| ------------ | ------------------------------------------------------------ |
| 仅查看不解压 | `tar -tf test.tar`                                           |
| 追加文件     | `tar -rf test.tar *.gif`                                     |
| 替换文件     | `tar -uf test.tar huaji.gif`                                 |
| 加密         | `tar -zcf - *.txt | openssl des3 -salt -k 123456 | dd of=test.des3` |
| 解密         | `dd if=test.des3 | openssl des3 -d -k 123456 | tar zxf -`    |

## zip命令

### zip用法

| 操作             | 命令示例                        |
| ---------------- | ------------------------------- |
| 压缩             | `zip test.zip *.txt`            |
| 解压缩           | `unzip test.zip`                |
| 解压缩到指定目录 | `unzip test.zip -d bbb`         |
| 添加密码         | `zip -rP 123456 test.zip *.txt` |
| 使用密码解压     | `unzip -P 123456 test.zip`      |

