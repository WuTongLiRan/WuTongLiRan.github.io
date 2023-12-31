# RCE漏洞

# RCE漏洞

## 概述：
由于程序中预留了执行代码或者命令的接口，并 且提供了给用户使用的界面，导致被黑客利用，控制服务器。

​	

## RCE漏洞危害

1、获取服务器权限

2、获取敏感数据文件

3、写入恶意文件getshell

4、植入木马病毒勒索软件等

​	

## RCE漏洞相关函数

### 代码注入

`eval()`——把字符串 code 作为PHP代码执行

`assert()`——检查一个断言是否为 false

`preg_replace()`——执行一个正则表达式的搜索和替换

`create_function()`——创建一个匿名函数并且返回函数名创

`call_user_func()`/`call_user_func_array()`——把第一个参数作为回调函数调用

`usort()`/`uasort()`——使用用户自定义的比较函数对数组中的值进行排 序并保持索引关联

### 命令注入

`system()`——执行外部程序，并且显示输出

`exec()`/`shell_exec()`——通过 shell 环境执行命令，并且将完整的输出以 字符串的方式返回

`pcntl_exec()`——在当前进程空间执行指定程序

`passthru()`——执行外部程序并且显示原始输出

`popen()`——打开进程文件指针

`proc_open()`——执行一个命令，并且打开用来输入/输出的文件指针

​	

## Windows常用命令：

    ipconfig --- 查看网卡信息
    dir --- 查看当前目录下的所有文件
    cd .. --- 返回上级目录
    cd 路径 --- 前往某个路径
    echo "内容" > 文件名 --- 给某个文件写入内容。如果文件已存在，则该命令将会覆盖文件中的内容；如果文件不存在，则会创建新的文件并写入内容

### Windows命令拼接方式

```
command1 | command2 --- 命令1执行的结果作为命令2执行的参数
command1 || command2 ---- 命令1执行成功则命令2不执行(逻辑短路)
command1 & command2 --- 命令1和命令2都执行
command1 && command2 --- 命令1执行失败则命令2不执行(逻辑短路)
```

​	

## Linux常用命令：

    ifconfig --- 查看网卡信息
    ls --- 查看当前目录下的所有文件
    cd.. --- 返回上级目录
    cd 路径 --- 前往某个路径
    touch 文件名 --- 创建某个文件
    echo "内容" > 文件名 --- 给某个文件写入内容。如果文件已存在，则该命令将会覆盖文件中的内容；如果文件不存在，则会创建新的文件并写入内容

### Linux命令拼接方式

    command1 ; command2 --- 命令1和命令2都执行
    command1 | command2 --- 命令1执行的结果作为命令2执行的参数
    command1 || command2 ---- 命令1执行成功则命令2不执行(逻辑短路)
    command&  			--- 任务后台执行，与nohup命令功能差不多
    command1 && command2 --- 命令1执行失败则命令2不执行(逻辑短路)

​	

## 绕过限制

### Windows上查看文件内容的命令：

```bash
type 文件名
more 文件名
find "搜索词" 文件名 --- 用于查找文件内容
```

### Linux和类Unix系统上查看文件内容的命令：

```bash
cat 文件名
more 文件名
less 文件名
head 文件名 --- 显示文件开头的几行
tail 文件名 --- 显示文件结尾的几行
grep "搜索词" 文件名 --- 用于在文件中搜索文本模式
nl 文件名 --- 显示文件内容，并附带行号 类似于cat -n
tailf --- 用于实时监视文件内容变化的命令 类似于tail -f
```

### 空格的替代方式

- `$IFS$9`=>`cat$IFS$9flag.php`
- ``%09`=>`cat%09flag.php`
- `<`=>`cat>flag.php`
- `>`=>`cat<flag.php`
- `<>`=>`cat<>flag.php`
- `{,}`=>`{cat,flag.php}`
- `%20`(URL编码)
- `${ IFS}`=>`cat${ IFS}flag.php`
- `${IFS}`=>`cat${IFS}flag.php`


