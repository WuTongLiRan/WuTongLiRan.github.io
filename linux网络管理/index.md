# Linux网络管理

# Linux网络管理

## 网络基本概念

### 物理地址(MAC地址) & 逻辑地址(IP地址)

**网卡**

- 计算机用于**连接网络**的硬件设备
- 网卡具有一个物理地址，被称为MAC地址
- 每个网卡都有一个**唯一**的MAC地址，类似于身份证号码

**MAC地址**

- MAC地址是一个物理地址，由网卡硬件制造时固化在设备中
- 网络设备上的一个唯一标识符，用于在**局域网中**标识和定位网卡设备

**IP地址**

- 做逻辑地址
- **网络中**用于唯一标识和定位设备的一种地址

   

### 公有IP地址 & 私有IP地址

- 局域网使用私有IP地址

- 互联网使用公有IP地址  

   

**公有IP地址的范围**

- A类的公有IP:
  1.0.0.0-9.255.255.255
  11.0.0.0-126.255.255.255
- B类的公有IP:
  128.0.0.0-172.15.255.255
  172.32.0.0-191.255.255.255
- C类的公有IP:
  192.0.0.0-192.168.255.255
  192.169.0.0-223.255.255.255

  

**私有IP地址的范围**

- A类私有IP地址:
  10.0.0.0-10.255.255.255
- B类私有IP地址:
  172.16.0.0-172.31.255.255
- C类私有IP地址:
  192.168.0.0-192.168.255.255

  

### NAT 网络地址转换

![](https://pic.imgdb.cn/item/64aa2ea61ddac507cca84f2a.jpg)

- NAT（Network Address Translation）是一种网络技术，用于在私有网络（例如家庭网络或企业网络）与公共网络（如互联网）之间进行通信。NAT的主要功能是将私有网络中的IP地址转换为公共网络可路由的IP地址，以便实现通信。
- NAT的主要目的是解决IPv4地址短缺的问题。

  

### IPv4 & IPv6

- IPv4（Internet Protocol version 4）和IPv6（Internet Protocol version 6）是互联网上常用的两种IP协议。
- IPv4是最早的IP协议版本，它使用32位（4字节）地址空间来唯一标识网络中的设备。IPv4地址由四个十进制数（0-255）组成，每个数之间使用句点分隔，例如192.168.0.1。IPv4地址空间有约42亿个可用地址，但随着互联网的迅速发展和设备的增加，IPv4地址短缺问题逐渐凸显。
- IPv6是为了解决IPv4地址短缺问题而开发的新一代IP协议。IPv6采用了128位（16字节）的地址空间，相比IPv4的地址空间大幅增加。IPv6地址由八组四位十六进制数（0-9，A-F）组成，每组之间使用冒号分隔，例如2001:0db8:85a3:0000:0000:8a2e:0370:7334。IPv6地址空间可提供约340万亿亿亿亿个地址，大大满足了未来互联网发展的需求。
- IPv4 & IPv6 的区别：
  - 地址表示：IPv4使用点分十进制表示，而IPv6使用冒号分隔的十六进制表示。
  - 地址空间：IPv4地址空间有限，而IPv6地址空间巨大，可以满足日益增长的互联网设备需求。
  - 地址分配：IPv4地址通常由ISP（Internet Service Provider，互联网服务提供商）分配，而IPv6地址分配更加灵活，并且支持自动配置。
  - 网络协议：IPv4和IPv6是两种不兼容的协议，但它们可以通过转换技术（如双栈、隧道）来实现互联互通。
  - 安全性和扩展性：IPv6在安全性和支持新的网络功能方面更加强大，并提供了更好的扩展性。

  

### 公网IP地址的分配

- 公网IP地址的分配是由互联网服务提供商（ISP，Internet Service Provider）负责进行的。

- 静态IP地址：静态IP地址是由ISP手动分配给用户的固定IP地址。每个静态IP地址都是唯一的，并且与特定的网络设备（如路由器、服务器）关联。静态IP地址适用于需要保持固定公网地址的场景，如托管网站、远程访问等。静态IP地址通常需要用户向ISP支付额外费用。

- 动态IP地址：动态IP地址是由ISP通过动态主机配置协议（DHCP，Dynamic Host Configuration Protocol）自动分配给用户的IP地址。每次用户连接到互联网时，ISP会自动分配一个可用的动态IP地址给用户的设备。动态IP地址是临时的，并且可能在下一次连接时发生变化。动态IP地址适用于大多数家庭网络和小型办公网络，因为它们更经济且易于管理。

  > 公网 IP地址通常是**动态分配**的

- 网络地址转换（NAT）：对于使用私有IP地址的设备（如家庭网络、企业内部网络），ISP通常会分配一个公网IP地址给网络中的路由器或网关设备。通过NAT技术，路由器将私有IP地址转换为公共IP地址，并为网络中的设备提供互联网访问。这种方式可以有效地共享一个公网IP地址给多个设备，并实现局域网内部的互联网连接。

  

### 动态IP & 静态IP  

- DHCP是一种网络协议，用于自动分配IP地址和其他网络配置信息给连接到网络的设备。
- 在静态配置中，设备的IP地址和其他网络配置信息是手动配置的。

  

**动态IP**

- 动态IP是由互联网服务提供商（ISP）动态分配给用户设备的IP地址。
- 每次设备连接到互联网时，ISP会为设备分配一个可用的IP地址，通常是从IP地址池中随机选择。
- 动态IP地址可能在每次重新连接时发生变化，因为它们是临时分配的，类似于你在公共电话亭中临时使用的电话号码。
- 动态IP适用于普通家庭用户和小型企业，因为它们经济实惠且供应量充足。
- 对于大多数网络活动（如网页浏览、电子邮件、在线娱乐等），动态IP足够满足需求。

  

**静态IP**

- 静态IP是由ISP手动分配给用户设备的固定IP地址。
- 静态IP是在网络设置中手动配置的，并且保持不变，无论设备是否重新连接到互联网。
- 静态IP地址通常用于需要长期稳定连接或特定网络服务的设备，如服务器、路由器、网络摄像头等。
- 使用静态IP地址可以方便地进行远程访问、设置端口转发和托管网络服务等。
- 静态IP通常需要额外付费，因为ISP需要保留和管理固定的IP地址资源。

  

### 环回地址

- 环回地址（Loopback Address）是计算机网络中的特殊IP地址，用于在同一设备内部进行网络通信。它通常被分配给一个名为"localhost"的网络接口，用于本地回环测试和网络应用程序的自我测试。

- IPv4中的环回地址是 127.0.0.1，也可以表示为 127.x.x.x（其中 x 是 0 到 255 之间的任意数）。

  IPv6中的环回地址是 ::1。

   

### 端口port  

- 作用：区分程序
- 范围：0 ~ 65535  
  - 系统端口： 0 ~ 1023
  - 用户端口， 也称为注册端口：1024 ~ 49151
  - 动态端口， 也称为私有或临时端口：49152 ~ 65535
  - ![](https://pic.imgdb.cn/item/64aa3f191ddac507ccc884fc.jpg)

  

### 域名Domain Name  

- 作用：替代IP，方便识记
- 域名如何转换为IP：DNS
- 域名与IP的数量关系：多对一

  

## 网络配置文件

| 文件                                         | 作用                       |
| -------------------------------------------- | -------------------------- |
| `/etc/sysconfig/network-scripts/ifcfgens33 ` | 网卡配置文件               |
| `/etc/sysconfig/network-scripts/ifcfg-lo `   | 环回地址配置文件           |
| `/etc/hosts `                                | 主机名与IP的映射           |
| `/etc/resolv.conf `                          | DNS配置，由`ens33`自动覆盖 |

  

## 查看及配置网络

### ifconfig  

- 全拼：network interfaces configuring
- 位于net-tools工具包
- 可以动态配置网络参数  

  

### ip

- 位于iproute工具包、
- 添加设备、启动停止网络设备、设置IP、设置网关……  

  

![](https://pic.imgdb.cn/item/64aa40a31ddac507cccd4dba.jpg)

  

## 连通性探测

### ping

- 全拼：Packet Internet Groper，因特网包探索器  

- 用于检测和测试网络连接以及测量与目标主机之间的延迟。

- 命令格式：`ping ip或域名`

  ```
  ping www.baidu.com
  ```

  

### telnet

- 用于在命令行界面中与远程主机建立Telnet连接并进行远程操作。
- 连接到远程主机(远程登陆)：`telnet 远程主机`。例如，`telnet 192.168.1.1`。
- 连接到指定端口(探测端口)：`telnet 远程主机 端口`。例如，`telnet 192.168.1.1 80`。
- 连接到远程主机并指定用户名：`telnet -l 用户名 远程主机`。例如，`telnet -l john 192.168.1.1`。

  

## 查看网络连接

### netstat (ss)

- 全拼：network statistics

- 查看程序的网络连接情况：` netstat -ap | grep ssh`

  > 查看程序ssh的网络连接情况

- 查看端口的网络连接情况：`netstat -ap | grep 3306`

  > 查看端口3306的网络连接情况

  

## 域名相关

### nslookup  

- `nslookup`是一种用于查询域名系统（DNS）记录的命令行工具。它可以用于获取特定主机的IP地址、解析域名、查询DNS记录等。

- 命令格式：`nslookup [选项] [域名]`

- 常见选项：

  - 无选项：在交互模式下启动nslookup，可以在命令提示符中直接输入域名进行查询。
  - `-type=记录类型`：指定要查询的DNS记录类型。常见的记录类型包括
    A（主机记录，获取主机的IP地址）
    CNAME（规范名称，获取别名的实际名称）
    MX（邮件交换记录，获取邮件服务器的优先级和地址）等。
  - `-query=记录类型`：与上述选项相同，用于指定查询的DNS记录类型。
  - `-debug`：显示更详细的调试信息。

- 使用示例：

  - 查询域名的IP地址：在命令提示符中输入`nslookup 域名`。例如，`nslookup example.com`。

  - 查询特定类型的DNS记录：在命令提示符中输入`nslookup -type=记录类型 域名`。

    ```
    nslookup -type=mx example.com
    ```

- 查询结果：`nslookup`命令会显示查询结果，包括查询的域名、对应的IP地址、DNS记录的TTL（生存时间）、查询时间等。

  如果查询失败或找不到域名的记录，会显示相应的错误信息。

  

### dig

- `dig`是一种用于在命令行中执行DNS（Domain Name System）查询的工具。它提供了详细的域名解析信息，包括域名的IP地址、DNS记录、域名服务器等。

- 命令格式：`dig [选项] [域名]`

- 常见选项：

  - 无选项：在默认模式下执行`dig`命令，查询指定域名的A记录（主机的IP地址）。
  - `+记录类型`：指定要查询的DNS记录类型。常见的记录类型包括
    A（主机记录，获取主机的IP地址）
    CNAME（规范名称，获取别名的实际名称）
    MX（邮件交换记录，获取邮件服务器的优先级和地址）等。
  - `@DNS服务器`：指定用于查询的特定DNS服务器。默认情况下，dig将使用操作系统配置的DNS服务器。
  - `+short`：以简洁的形式显示查询结果，只显示IP地址或最基本的信息。

- 使用示例：

  - 查询域名的IP地址：在命令提示符中输入`dig 域名`。例如

    ```
    dig example.com
    ```

    

  - 查询特定类型的DNS记录：在命令提示符中输入`dig +记录类型 域名`。例如

    ```
    dig +mx example.com
    ```

    

  - 查询特定DNS服务器：在命令提示符中输入`dig @DNS服务器 域名`。例如

    ```
    dig @8.8.8.8 example.com
    ```

    

- 查询结果：`dig`命令会显示详细的查询结果，包括查询的域名、对应的IP地址、DNS记录的TTL（生存时间）、查询时间等。查询结果中还包含有关DNS服务器和查询过程的信息。

  

### host

- 用于执行DNS（Domain Name System）查询并获取与指定域名相关的信息。它可用于查找域名的IP地址、反向解析IP地址、查询MX记录等。

- 语法：host [选项] [域名]

- 常见选项：

  - 无选项：在默认模式下执行host命令，查询指定域名的A记录（主机的IP地址）。
  - `-t 记录类型`：指定要查询的DNS记录类型。常见的记录类型包括
    A（主机记录，获取主机的IP地址）
    CNAME（规范名称，获取别名的实际名称）
    MX（邮件交换记录，获取邮件服务器的优先级和地址）等。
  - `-a`：查询所有可用的DNS记录类型。
  - `-4`：仅查询IPv4地址记录。
  - `-6`：仅查询IPv6地址记录。

- 使用示例：

  - 查询域名的IP地址：在命令提示符中输入`host 域名`。例如

    ```
    host example.com
    ```

    

  - 查询特定类型的DNS记录：在命令提示符中输入`host -t 记录类型 域名`。例如

    ```
    host -t mx example.com
    ```

    

  - 查询IPv6地址记录：在命令提示符中输入`host -6 域名`。例如

    ```
    host -6 example.com
    ```

    

- 查询结果：`host`命令会显示查询结果，包括查询的域名、对应的IP地址或其他DNS记录信息。它通常会显示多个记录，如果有多个可用的记录类型。

  

## 下载传输

### Xshell拖曳上传

  

### xftp & Filezilla & FlashFTP  双向传输

  

### sz

- `sz`命令是用于在Unix/Linux系统中通过串行连接（如终端仿真器）从本地主机向远程主机发送文件的命令。它是Zmodem协议的一部分，允许安全地将文件传输到远程主机。

  ```
  sz 文件名
  ```

  

  

### rz

- `rz`命令是用于在终端仿真器中接收（上传）文件到本地主机的命令。它通常与支持Zmodem协议的终端仿真器一起使用，如Minicom、SecureCRT或Xshell（通过Xftp工具）。
  - 在远程主机上，通过终端仿真器（如Xshell）与目标主机建立连接。
  - 在终端仿真器中，使用命令行界面进入要上传文件的目录。
  - 在命令行界面中，输入`rz`命令并按下回车键。
  - 终端仿真器会进入接收文件的等待状态。
  - 在本地主机上，通过支持Zmodem协议的文件传输工具（如sz、SecureCRT的rz命令）选择要上传的文件。
  - 拖拽或通过文件传输工具将文件发送到终端仿真器窗口。
  - 文件传输完成后，终端仿真器将接收并保存文件到当前目录中。

  

### VMware Tools 拖动传入

- VMware Tools是一套软件，提供了一些增强功能和驱动程序，用于增强在虚拟机中运行的操作系统的性能和功能。在使用VMware虚拟化平台（如VMware Workstation、VMware Fusion、VMware ESXi等）创建的虚拟机中，可以使用VMware Tools来实现拖动传输文件的功能。

  

### QQ双向传输

  

### wget

- wget是一个常用的命令行工具，用于从网络上下载文件。它支持HTTP、HTTPS和FTP等协议。

- 命令语法：`wget [选项] [URL]`

- 常见选项：

  - `-O 文件名`：将下载的文件保存为指定的文件名。
  - `-P 目录`：指定下载文件的保存目录。
  - `-c`：继续下载中断的文件。
  - `-r`：递归下载，下载指定URL页面上的所有链接。
  - `-np`：不跟踪父级目录，用于限制递归下载时的目录层级。
  - `-nH`：不保存远程主机的目录结构，将所有文件保存到当前目录。
  - `-nc`：不覆盖已存在的文件，跳过已下载的文件。
  - `-q`：安静模式，减少输出信息。
  - `-h`：显示wget命令的帮助信息和选项说明。

- 使用示例：

  - 下载文件到当前目录：`wget URL`。例如

    ```
    wget https://example.com/file.zip
    ```

    

  - 下载文件并指定保存目录：`wget -P 目录 URL`。例如

    ```
    wget -P /path/to/save https://example.com/file.zip
    ```

    

  - 断点续传下载：`wget -c URL`。例如

    ```
    wget -c https://example.com/largefile.zip
    ```

    

  - 递归下载整个网站：`wget -r URL`。例

    ```
    wget -r https://example.com
    ```

    

  

### scp

- 全拼：Secure Copy  

- 用于在本地主机和远程主机之间进行安全的文件传输。它使用SSH协议来加密传输数据，可以在不同的操作系统之间进行文件传输。

- 命令语法：`scp [选项] [源文件/目录] [目标位置]`

- 常见选项：

  - `-r`：递归复制整个目录。
  - `-p`：保留源文件的权限和时间戳。
  - `-P 端口`：指定远程SSH服务器的端口号，默认为22。
  - `-i 私钥文件`：指定使用的私钥文件来进行身份验证。
  - `-v`：显示详细的调试信息。

- 使用示例：

  - 从远程主机复制文件到本地主机：`scp 用户名@远程主机:源文件 目标位置`。例如

    ```
    scp user@example.com:/path/to/file.txt /local/path/
    ```

    

  - 从本地主机复制文件到远程主机：`scp 源文件 用户名@远程主机:目标位置`。例如

    ```
    scp /local/path/file.txt user@example.com:/path/to/
    ```

    

  - 复制整个目录：使用`-r`选项来递归复制整个目录。例如

    ```
    scp -r /local/directory user@example.com:/remote/directory
    ```

  

### curl

- 用于与网络服务器进行数据传输。它支持多种协议，包括HTTP、HTTPS、FTP等，并提供了丰富的功能，如发送HTTP请求、下载文件、上传文件、处理Cookie等。

- 命令语法：`curl [选项] [URL]`

- 常见选项：

  - `-X 请求方法`：指定HTTP请求的方法，如GET、POST、PUT等。
  - `-H "头信息"`：添加自定义的HTTP头信息，如Content-Type、User-Agent等。
  - `-d "数据"`：发送POST请求时，指定要发送的数据。
  - `-F "字段名=值"`：发送POST请求时，以表单方式发送数据。
  - `-o 文件名`：将服务器响应保存到指定的文件中。
  - `-O`：将服务器响应保存为默认的文件名。
  - `-L`：跟随重定向。
  - `-c "文件"`：使用指定的文件来保存和加载Cookie。
  - `-u 用户名:密码`：指定HTTP身份验证的用户名和密码。
  - `-s`：静默模式，不显示进度和错误信息。
  - `-v`：显示详细的调试信息。

- 使用示例：

  - 发送GET请求并显示服务器响应：`curl URL`。例如

    ```
    curl https://example.com
    ```

    

  - 发送POST请求并传递数据：`curl -X POST -d "data=hello" URL`。例如

    ```
    curl -X POST -d "username=admin&password=123" https://example.com/login
    ```

    

  - 下载文件到本地：`curl -o 文件名 URL`。例如

    ```
    curl -o image.jpg https://example.com/image.jpg
    ```

    

  - 上传文件到服务器：`curl -F "file=@本地文件路径" URL`。例如

    ```
    curl -F "file=@/path/to/file.txt" https://example.com/upload
    ```

    

## 防火墙设置

- iptables工具  

- 查看已开放的端口

  ```
  firewall-cmd --list-ports
  ```

  

- 开启80端口

  ```
  firewall-cmd --zone=public --add-port=80/tcp --permanent
  ```

  

- 重启防火墙

  ```
  firewall-cmd --reload
  ```

  

- 停止防火墙

  ```
  systemctl stop firewalld.service
  ```

  

- 禁止防火墙开机启动

  ```
  systemctl disable firewalld.service
  ```

  

- 删除规则

  ```
  firewall-cmd --zone=public --remove-port=80/tcp --permanent
  ```

  

  



  


