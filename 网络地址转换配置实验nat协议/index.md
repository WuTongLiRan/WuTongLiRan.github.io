# 网络地址转换配置实验—NAT协议

# 网络地址转换配置实验—NAT协议

## NAT的类型

- **SNAT（源地址转换）：**用于将源IP地址从内部私有网络转换为外部公共网络的IP地址。它允许多个内部主机共享同一个外部公共IP地址，通过修改IP报文的源地址来实现。**SNAT通常用于让内部主机访问互联网**，同时隐藏了内部网络的真实IP地址，提供了一定程度的安全性。

  > 为了上网

- **DNAT（目标地址转换）：**用于将目标IP地址从外部公共网络转换为内部私有网络的IP地址。它允许外部主机访问内部网络中的特定服务或资源，通过修改IP报文的目标地址来实现。**DNAT通常用于将外部请求映射到内部服务器(端口映射)，以提供对外服务**，比如Web服务器、邮件服务器等。

  > 为了内网服务器被访问到

​	

### SNAT常见的三种类型

- **静态转换(常用)：**指将内部网络的私有IP地址转换为公有IP地址，IP地址对**是一对一的**，是一成不变的，某个私有IP地址只转换为某个公有IP地址。借助于静态转换，可以实现外部网络对内部网络中某些特定设备（如服务器）的访问。
- **动态转换：**指将内部网络的私有IP地址转换为公用IP地址时，**IP地址是不确定的，是随机的**，所有被授权访问上Internet的私有IP地址可随机转换为指定的合法IP地址集。
- **端口多路复用：**指改变外出数据包的源端口并进行端口转换，即端口地址转换（PAT，Port Address Translation).采用端口多路复用方式。内部网络的所有主机均可共享一个合法外部IP地址实现对Internet的访问，从而可以最大限度地节约IP地址资源。

​	

## 静态NAT配置

### 实验拓扑

![](https://pic.imgdb.cn/item/64bb54a51ddac507cc9b5f33.jpg)

​	

### 实验步骤

1. 配置PC0的基本信息。

2. 配置Router0 & RouterISP的接口地址：

   ```
   // Router0
   
   Router>en
   Router#configure terminal 
   
   Router(config)#int f0/0
   Router(config-if)#ip address 192.168.20.254 255.255.255.0
   Router(config-if)#no shutdown 
   Router(config-if)#exit
   
   Router(config)#int f0/1
   Router(config-if)#ip address 154.12.12.1 255.255.255.252
   Router(config-if)#no shutdown
   
   
   
   // RouterISP
   
   Router>en
   Router#configure terminal 
   
   Router(config)#int f0/0
   Router(config-if)#ip address 154.12.12.2 255.255.255.252
   Router(config-if)#no shutdown
   
   ```

   

3. 配置ACL划分内外网：

   ```
   Router(config)#ip access-list standard neiwang      // 创建一个标准ACL，名称为neiwang
   Router(config-std-nacl)#permit 192.168.20.0 0.0.0.255  // 允许192.168.20.0/24网段的流量通过ACL
   Router(config-std-nacl)#exit                         // 退出标准ACL配置模式
   
   Router(config)#interface FastEthernet0/0            // 进入FastEthernet0/0接口配置模式
   Router(config-if)#ip nat inside                      // 将接口设置为NAT的内部接口
   Router(config-if)#exit                              // 退出接口配置模式
   
   Router(config)#interface FastEthernet0/1            // 进入FastEthernet0/1接口配置模式
   Router(config-if)#ip nat outside                     // 将接口设置为NAT的外部接口
   
   ```

   

4. 配置NAT将内网转化为外网：

   ```
   
   Router(config)#ip nat inside source list neiwang interface fastEthernet 0/1
   
   /*
   *ip nat inside: 配置内部网络的源地址，这些地址需要进行网络地址转换(NAT)。
   *source: 指定要配置源地址转换规则。
   *list neiwang: 使用名为"neiwang"的标准ACL，该ACL包含了内部网络的IP地址范围，用于指定需要进行转换的地址。
   *interface fastEthernet 0/1: 指定FastEthernet 0/1接口的IP地址作为转换后的源地址，这将成为NAT转换后数据流量出去的接口。
   */
   ```

   
