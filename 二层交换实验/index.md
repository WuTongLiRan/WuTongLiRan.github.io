# 二层交换实验


# 二层交换实验

**1、 实验目的** 

通过本实验可以： 

1. 理解二层交换的基本原理
2. 掌握交换机的基本配置 
3. 掌握交换机VLAN划分
4. 掌握实现不同局域网的通信
5. 理解交换机端口安全
6. 掌握DHCP的基本配置

 

**2、 拓扑结构**

<img src="https://pic.imgdb.cn/item/64e715ba661c6c8e54a9b40f.jpg" style="zoom: 80%;" /> 

 

**3、 实验需求** 

1. 参照逻辑拓扑，使用合适的线缆完成物理拓扑的搭建 
2. PC0，PC1和PC2属于一个局域网，Server0和Server1分别属于vlan100和vlan200。
3. 在交换机上创建相应的VLAN，并且将相应的接口加入到相应的局域网，完成基本配置
4. 为了安全考虑服务器IP地址均为手动配置，公司服务器IP地址为：192.168.100.100/24，网关为192.168.100.254。财务服务器IP地址为192.168.200.100/24，网关为192.168.200.254。其余主机IP地址均为动态获取，IP地址自己规划
5. 完成必要配置实现各主机与各服务器可以相互通信
6. 在交换机连接服务器的接口配置端口安全，只能允许合法服务器接入，非法网络设备接入接口将关闭

​	

​	
#### 1. 参照逻辑拓扑，使用合适的线缆完成物理拓扑的搭建

​	<img src="https://pic.imgdb.cn/item/64e72b48661c6c8e54b172bf.jpg" style="zoom:80%;" />

#### 2. PC0，PC1和PC2属于一个局域网，Server0和Server1分别属于vlan100和vlan200。

​	

#### 3. 在交换机上创建相应的VLAN，并且将相应的接口加入到相应的局域网，完成基本配置

SW2的配置：

```
[SW2]vlan 100                 // 创建 VLAN 100
[SW2]vlan 200                 // 创建 VLAN 200

[SW2]int e0/0/1               // 进入以太网接口 0/0/1 的配置模式
[SW2-Ethernet0/0/1]port link-type trunk       // 配置接口为 trunk 模式，用于传送多个 VLAN
[SW2-Ethernet0/0/1]port trunk allow-pass vlan 1 100 200  // 允许通过的 VLAN 为 VLAN 1、100 和 200
[SW2-Ethernet0/0/1]quit      // 退出以太网接口配置模式

[SW2]int e0/0/2               // 进入以太网接口 0/0/2 的配置模式
[SW2-Ethernet0/0/2]port link-type access      // 配置接口为 access 模式，用于连接单个 VLAN 的设备
[SW2-Ethernet0/0/2]port default vlan 100      // 将默认 VLAN 设置为 VLAN 100
[SW2-Ethernet0/0/2]quit      // 退出以太网接口配置模式

[SW2]int e0/0/6               // 进入以太网接口 0/0/6 的配置模式
[SW2-Ethernet0/0/6]port link-type access     // 配置接口为 access 模式，用于连接单个 VLAN 的设备
[SW2-Ethernet0/0/6]port default vlan 200     // 将默认 VLAN 设置为 VLAN 200
```

SW1的配置：

```
[SW1]vlan 100                   // 创建 VLAN 100
[SW1-vlan100]vlan 200            // 进入 VLAN 100 配置视图后创建 VLAN 200
[SW1-vlan200]quit                // 退出 VLAN 200 配置视图

[SW1]int g0/0/1                  // 进入 GigabitEthernet0/0/1 接口的配置模式
[SW1-GigabitEthernet0/0/1]port link-type trunk    // 配置接口为 trunk 模式，用于传送多个 VLAN
[SW1-GigabitEthernet0/0/1]port trunk allow-pass vlan 1 100 200  // 允许通过的 VLAN 为 VLAN 1、100 和 200
[SW1-GigabitEthernet0/0/1]quit  // 退出接口配置模式

[SW1]int vlan 1                  // 进入 VLAN 1 接口的配置模式
[SW1-Vlanif1]ip address 192.168.10.254 24  // 配置接口 IP 地址为 192.168.10.254/24

[SW1-Vlanif1]int vlan 100        // 进入 VLAN 100 接口的配置模式
[SW1-Vlanif100]ip address 192.168.100.254 24  // 配置接口 IP 地址为 192.168.100.254/24

[SW1-Vlanif100]int vlan 200      // 进入 VLAN 200 接口的配置模式
[SW1-Vlanif200]ip address 192.168.200.254 24  // 配置接口 IP 地址为 192.168.200.254/24
```

​	

#### 4. 为了安全考虑服务器IP地址均为手动配置，公司服务器IP地址为：192.168.100.100/24，网关为192.168.100.254。财务服务器IP地址为192.168.200.100/24，网关为192.168.200.254。其余主机IP地址均为动态获取，IP地址自己规划

<img src="https://pic.imgdb.cn/item/64e7247d661c6c8e54af1557.jpg" style="zoom:67%;" />

<img src="https://pic.imgdb.cn/item/64e724bc661c6c8e54af27c7.jpg" style="zoom:67%;" />

SW1的配置：

```
[SW1]ip pool PC                 // 创建一个名为 PC 的 IP 地址池
[SW1-ip-pool-pc]network 192.168.10.0 mask 24  // 配置 IP 地址池的网络地址和子网掩码
[SW1-ip-pool-pc]gateway-list 192.168.10.254  // 配置默认网关地址
[SW1-ip-pool-pc]dns-list 114.114.114.114     // 配置 DNS 服务器地址
[SW1-ip-pool-pc]quit            // 退出 IP 地址池配置视图

[SW1]dhcp enable                // 启用全局 DHCP 服务

[SW1]int vlan1                  // 进入 VLAN 1 接口的配置模式
[SW1-Vlanif1]dhcp select global  // 配置 VLAN 1 接口使用全局的 DHCP 服务
```

PC0：

<img src="https://pic.imgdb.cn/item/64e72860661c6c8e54b02567.jpg" style="zoom:80%;" />

PC1：

<img src="https://pic.imgdb.cn/item/64e72877661c6c8e54b02a63.jpg" style="zoom:80%;" />

PC2：

<img src="https://pic.imgdb.cn/item/64e728a8661c6c8e54b0343f.jpg" style="zoom:80%;" />

​	

#### 5. 完成必要配置实现各主机与各服务器可以相互通信

都可以相互ping通

​	

#### 6. 在交换机连接服务器的接口配置端口安全，只能允许合法服务器接入，非法网络设备接入接口将关闭

SW2的配置：

```
[SW2]int e0/0/2                     // 进入以太网接口 0/0/2 的配置模式
[SW2-Ethernet0/0/2]port-security enable      // 启用端口安全性功能
[SW2-Ethernet0/0/2]port-security mac-address sticky   // 将学习到的 MAC 地址添加到允许列表中
[SW2-Ethernet0/0/2]port-security protect-action shutdown   // 配置触发保护操作为关闭端口
[SW2-Ethernet0/0/2]port-security max-mac-num 1   // 设置最大允许的 MAC 地址数量为 1
[SW2-Ethernet0/0/2]quit           // 退出以太网接口配置模式

[SW2]int e0/0/6                     // 进入以太网接口 0/0/6 的配置模式
[SW2-Ethernet0/0/6]port-security enable      // 启用端口安全性功能
[SW2-Ethernet0/0/6]port-security mac-address sticky   // 将学习到的 MAC 地址添加到允许列表中
[SW2-Ethernet0/0/6]port-security protect-action shutdown   // 配置触发保护操作为关闭端口
[SW2-Ethernet0/0/6]port-security max-mac-num 1   // 设置最大允许的 MAC 地址数量为 1
```


