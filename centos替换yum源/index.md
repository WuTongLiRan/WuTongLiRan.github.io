# CentOS替换yum源

# CentOS替换yum源

注：从物理机粘贴文字内容到虚拟机终端的快捷键是`Shift+Insert`

## 备份官方yum源配置文件

官方yum源配置文件在：

`/etc/yum.repos.d/CentOS-Base.repo`

查看yum源配置文件的命令：

```
cat /etc/yum.repos.d/CentOS-Base.repo
```

备份命令：

```
cp /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.bak
```

## 下载阿里云源配置，覆盖原文件

命令：

```
curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
```

## 清理缓存并生成新的缓存

清理缓存命令：

```
yum clean all
```

生成新的缓存命令：

```
yum makecache
```

如果出现"Failed connect to mirrors.aliyuncs.com:80; Connection refused"的错误，重试即可。

## 更新软件（不是必要操作）

注意：这一步会更新操作系统中所有软件到最新版，不是必要操作。而且网速慢的情况下，会非常耗时，谨慎操作。

命令：

```
sudo yum -y update
```

如果要中断，就按`Ctrl+C`。

