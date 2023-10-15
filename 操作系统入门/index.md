# 操作系统入门

# 操作系统入门

VMware的三种联网方式：NAT模式、桥接模式、仅主机模式

​	

物理机ping不通虚拟机，检查：

- 是否同卡
- 是否同网段
- 网关是否一致
- 避免IP冲突
- 防火墙是否关闭或者是否配置入站规则

​	

防火墙规则：对内允许所有，对外拒绝所有

 	

|                      | Windows                                                      | Linux                                                        |
| :------------------: | ------------------------------------------------------------ | ------------------------------------------------------------ |
|      查看主机名      | hostname                                                     | hostname                                                     |
|     查看当前用户     | whoami                                                       | whoami                                                       |
|       创建文件       | type nul > 文件名 //创建空文件<br />echo 内容 > 文件名 //创建有内容的文件<br />>> //追加 | touch 文件名 //创建空文件<br />echo 内容 > 文件名 //创建有内容的文件<br />>> //追加<br />vim 文件名 //创建有内容的文件 |
|       删除文件       | del 文件名                                                   | rm -f 文件名 //强制删除文件                                  |
|       创建目录       | mkdir/md 目录名 <br />md 1\2\3 //创建多级目录(注意是反斜杠\\) | mkdir 目录名 <br />mkdir -p 1/2/3 //创建多级目录(注意是斜杠/) |
|       删除目录       | rd 目录名 //删除空目录<br />rd /S /Q 目录名 //强制删除目录树 | rm -rf 目录名 //强制删除目录                                 |
|  列出当前目录下内容  | dir                                                          | ls <br />ls -a //查看隐藏目录<br />ls -l  ll  <br />ll -h //列出并显示文件具体大小 |
|       切换目录       | cd .. //切换到上一级目录                                     | cd .. //切换到上一级目录                                     |
|  打印/查看文件内容   | type 文件名                                                  | cat/more/tail 文件名                                         |
|         移动         | move 旧文件名 新文件名                                       | mv 旧文件名 新文件名                                         |
|         拷贝         | copy 旧文件名 新文件名<br />copy 1.txt+2.txt 3.txt //将1和2的内容复制到3里 | cp -p 旧文件名 新文件名 //复制文件的同时复制权限<br />       |
|   查看当前所在位置   |                                                              | pwd                                                          |
|     查看网络信息     | ipconfig /all                                                | ifconfig<br />ip address                                     |
|     查看用户信息     | net user                                                     | cat /etc/passwd //用户信息文件<br />cat /etc/shadow //密码信息文件 |
| 查看管理员组里的用户 | net localgroup administrators                                | cat /etc/group //组信息文件                                  |
|       创建用户       | net user 用户名 密码 /add                                    | useradd 用户名                                               |
|       删除用户       | net user 用户名 密码 /del                                    | userdel -r 用户名                                            |
| 将用户添加到管理员组 | net localgroup administrators 用户名 /add                    | usermod 用户名 -G GID/组名<br />gpasswd -a 用户名 组名       |
| 将用户从管理员组删除 | net localgroup administrators 用户名 /del                    | gpasswd -d 用户名 组名                                       |
|       锁定用户       | net user 用户名 /active:no                                   | usermod -L 用户名<br />password -l 用户名                    |
|       解锁用户       | net user 用户名 /active:yes                                  | usermod -U 用户名<br />password -u 用户名                    |
|       查看端口       | netstat -ano                                                 | netstat -antlp                                               |
|     查看所有进程     | tasklist                                                     | ps aux                                                       |
| 根据PID查看指定进程  | tasklist \| findstr "PID"                                    | ps anx \| grep "PID"                                         |
|       关闭进程       | taskkill                                                     | kill -9 PID                                                  |
|         关机         | shutdown                                                     | poweroff                                                     |
|         重启         | shutdown /r                                                  | reboot                                                       |
|       启动服务       |                                                              | systemctl start 服务                                         |
|       关闭服务       |                                                              | systemctl stop 服务                                          |
|       查看服务       |                                                              | systemctl status 服务                                        |
|       重启服务       |                                                              | systemctl restart 服务                                       |
|     开机自启服务     |                                                              | systemctl enable 服务                                        |
|     永久关闭服务     |                                                              | systemctl disable 服务                                       |
|       安装服务       |                                                              | yum -y install                                               |
|       删除服务       |                                                              | yum -y remove                                                |
|       重装服务       |                                                              | yum -y reinstall                                             |
|         压缩         |                                                              | gzip //.gz<br />bzip2 //.bz2                                 |
|        解压缩        |                                                              | gunzip<br />bunzip2                                          |
|         打包         |                                                              | tar                                                          |
|      打开注册表      | regedit                                                      |                                                              |
|    打开事件查看器    | eventvwr.msc                                                 |                                                              |
|     打开系统服务     | services.msc                                                 |                                                              |
|   打开本地安全策略   | secpol/msc                                                   |                                                              |
|      打开组策略      | gpedit.msc                                                   |                                                              |
|    打开本地组策略    | lusrmgr.msc                                                  |                                                              |
| 文件系统数据存储单元 | 簇                                                           | 块                                                           |
|       访问控制       | cacls                                                        |                                                              |
|   存放用户数据文件   | SAM                                                          | /etc/passwd<br />/etc/shadow                                 |
|   本地域名解析文件   | hosts                                                        | /etc/hosts                                                   |
|    查看dns服务器     |                                                              | /etc/resolv.conf                                             |

​	

常见的文件系统：NTFS、FAT16、FAT32、EXFAT

文件系统的载体：磁盘

​	

Windows的两种工作模式：工作组(workgroup)、域(域控、域用户)

域控DC：一台安装且运行AD的服务器

活动目录-AD：对帐户和资源进行统一管理的机制

活动目录的逻辑结构：域、域树、域林、组织单元OU(配置策略)

域的信任关系：单向不可传递

DNS在域中的作用：定位域控

​	

一个IP地址有65536个端口(0~65535)

| 服务   | 端口   |
| ------ | ------ |
| http   | 80     |
| https  | 443    |
| ftp    | 20、21 |
| ssh    | 22     |
| telnet | 23     |
| dhcp   | 67、68 |
| dns    | 53     |
| rdp    | 3389   |


