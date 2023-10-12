# CentOS设置静态IP

# CentOS设置静态IP

## 前言：

在安装好CentOS虚拟机以后，一般我们会通过Xshell连接到虚拟机，而不是直接使用虚拟机里面的终端（Terminal）输入命令。

如果使用默认的动态分配IP，虚拟机**每次开机以后IP都会改变**，Xshell连接需要修改IP。所以这里我们需要将IP设置成静态IP，只要虚拟机开机即可连接。

**静态IP配置并不是必须的**

问题：如果网络环境发生变化，比如从有线变成无线，或者电脑从办公室移动到家里，主机IP（网段）发生了变化，需要重新设置虚拟机的静态IP吗？

答案是不需要，不影响物理机与虚拟机的连接。

## 一、查看物理机IP

- `win+R`打开cmd，输入`ipconfig`

- 首先要确定你的上网方式是**有线**还是**无线**

- 如果你的电脑用的是**有线网络（插了网线）**，就找到**“以太网适配器 以太网”**的IPv4地址：

  ![](https://pic.imgdb.cn/item/64a3e4181ddac507cc247eb5.jpg)

  > 192.168.3.10

- 如果是**无线网络（WiFi上网）**，则找到**“无线局域网适配器WLAN”**的IPv4地址：

  ![](https://pic.imgdb.cn/item/64a3e4da1ddac507cc271c26.jpg)

  > 192.168.1.5

## 二、虚拟机网络设置

- 首先是网络模式，点击虚拟机，编辑虚拟机设置：
  ![](https://pic.imgdb.cn/item/64a3e55a1ddac507cc288311.jpg)

- 网络适配器，网络连接需要选择：自定义——VMnet8（NAT模式），保存。
  ![](https://pic.imgdb.cn/item/64a3e58d1ddac507cc290f5f.jpg)
- 打开“编辑”——“虚拟网络编辑器”
  ![](https://pic.imgdb.cn/item/64a3e5bc1ddac507cc29b058.jpg)
  **注意：NAT网络模式对应的虚拟网卡是VMnet8。**
  ![](https://pic.imgdb.cn/item/64a3e5e21ddac507cc2a171a.jpg)

- 这里是灰色的，不能编辑怎么办？点右下角的“更改设置”，窗口会重新打开。

  有三次地方要修改：

  子网IP（虚拟机网段）、NAT网段、DHCP网段
  ![](https://pic.imgdb.cn/item/64a3e6201ddac507cc2acdf5.jpg)
  这三个地方，都只需要修改第三位就行了。比如你的真机ip为192.168.1.5，第三位是1就都设置成1。
  ![](https://pic.imgdb.cn/item/64a3e7e61ddac507cc2efae3.jpg)
  子网IP的最后一位必须是0。
  NAT设置，IP第三位和子网保持一致，网关最后一位必须是2

  ![](https://pic.imgdb.cn/item/64a3e85c1ddac507cc2ff65f.jpg)
  如果改了以后网络不通，可以点左下角的“还原默认设置”，再修改。
  ![](https://pic.imgdb.cn/item/64a3e8c11ddac507cc30dbb7.jpg)

虚拟机的网络设置好以后，下面是网络配置文件。

注意：网络配置文件的网段，必须要和虚拟机网络配置里面的网段一致。

比如设置了192.168.1.x，后面就必须填写192.168.1.x。

## 三、CentOS网络配置文件

### 1、打开终端

启动虚拟机，以**root用户登录**（必须是root用户，否则没权限修改配置文件）。

Applications——System Tools找到Terminal（终端）

![](https://pic.imgdb.cn/item/64a3ea361ddac507cc34235a.jpg)

如果不是root用户也可以输入指令

```
su root
```

然后输入密码（不可见）：123456即可进入root模式

![](https://pic.imgdb.cn/item/64a3e9c81ddac507cc332d92.jpg)

### 2、编辑配置文件

输入命令（复制以后在终端里面`Shift+Insert`粘贴）：

```
vi /etc/sysconfig/network-scripts/ifcfg-ens33
```

按回车打开配置文件。

如果弹出下面这个窗口，说明你之前没有保存就退出了，或者有多个窗口在同时操作ens33文件。

![](https://pic.imgdb.cn/item/64a3ea941ddac507cc34f739.jpg)

解决办法:

1. 按E或Enter继续编辑。

2. 删掉这个临时文件，下次就没有提示了：

   ```
   cd /etc/sysconfig/network-scripts/
   rm -rf .ifcfg-ens33.sw*
   ```



对于第一次使用vi编辑器的同学来说，需要注意，VI有两种模式，一种是“命令模式”，可以执行命令，一种是“编辑模式”，可以修改文本。

当我们用vi打开文本的时候，是命令模式，不能修改文本。

这个时候需要按“i”进入编辑模式。

![](https://pic.imgdb.cn/item/64a3eb091ddac507cc35ec3c.jpg)

此时左下角出现-- INSERT --提示。

我们用键盘上下左右键，移动光标，到需要修改的位置。

### 3、修改配置文件内容

对于初次安装的CentOS操作系统来说，有几个需要修改的地方：

1. BOOTPROTO需要改成static
2. ONBOOT改成yes
3. 添加IPADDR/NETMASK/DNS1/GATEWAY
   ![](https://pic.imgdb.cn/item/64a3ec591ddac507cc38d7bb.jpg)
   - IPADDR就是静态IP地址,网段跟物理机不同即可。比如物理机的IP是192.168.1.5，修改后两位，比如.131 （最后一位随便写，建议从130以后开始）
   - 网关固定255.255.255.0
   - DNS1固定 114.114.114.114
   - 网关最后一位必须是2，前面三位跟物理机一致

### 4、退出和保存

上面的操作都是在编辑模式中进行的。

如果不小心改错了，想要放弃修改怎么办？

这个时候需要按Esc回到命令模式。

在命令模式下，左下角的`-- INSERT --`消失了。

如果放弃修改重来，输入（注意全部是英文符号），回车

`:q!`

![](https://pic.imgdb.cn/item/64a3ecbe1ddac507cc39d757.jpg)

如果要保存修改的结果，输入（英文符号），回车

`:wq`

### 5、重启网络

网络配置文件修改以后需要重启网络才能生效，命令：（重要！每次修改了ens33文件都要重启网络）

```
service network restart
```

### 6、测试网络

测试网络：

1. 物理机与虚拟机连通性
   打开cmd，ping虚拟机的IP，比如

   ```
   ping 192.168.1.131
   ```

    （Ctrl+C退出）
   这是正常情况：
   ![](https://pic.imgdb.cn/item/64a3ed6a1ddac507cc3b9792.jpg)
   如果卡住了，或者请求超时，说明物理机和虚拟机网络不通

2. 虚拟机是否能访问互联网
   终端输入 ping baidu.com （Ctrl+C退出）
   这是正常情况：
   ![](https://pic.imgdb.cn/item/64a3edb81ddac507cc3c697b.jpg)
   如果卡住了，或者提示Name or service not known，是网络不通或者DNS配置错误

3. 虚拟机与物理机连通性
   比如前面看到的物理机IP是192.168.1.5
   在终端输入

   ```
   ping 192.168.1.5
   ```

   

