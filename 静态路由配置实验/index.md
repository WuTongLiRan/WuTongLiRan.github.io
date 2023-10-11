# 

# 静态路由配置实验

## 实验目的

本实验的目的是在网络中配置静态路由，以实现不同网络之间的互连和通信。

​	

## 实验拓扑

![](https://pic.imgdb.cn/item/64b7d85d1ddac507cc570d6e.jpg)

​	

## 实验步骤

1. 配置每台PC的**IP、子网掩码、网关。**

2. 顺便复习一下VLAN的配置：(与静态路由实验无关)

   ```
   Switch>enable                                       // 进入特权模式
   
   Switch#configure terminal                           // 进入全局配置模式
   
   Switch(config)#vlan 10                              // 创建 VLAN 10
   Switch(config-vlan)#vlan 20                         // 创建 VLAN 20
   Switch(config-vlan)#exit                            // 退出 VLAN 配置模式
   
   Switch(config)#interface fastEthernet 0/1            // 进入 0/1 接口配置模式
   Switch(config-if)#switchport mode access             // 将接口设置为 Access 模式
   Switch(config-if)#switchport access vlan 10         // 配置接口的 VLAN 为 VLAN 10
   Switch(config-if)#no shutdown                       // 打开接口
   Switch(config-if)#exit                              // 退出接口配置模式
   
   Switch(config)#interface fastEthernet 0/2            // 进入 0/2 接口配置模式
   Switch(config-if)#switchport mode access             // 将接口设置为 Access 模式
   Switch(config-if)#switchport access vlan 20         // 配置接口的 VLAN 为 VLAN 20
   Switch(config-if)#no shutdown                       // 打开接口
   Switch(config-if)#exit                              // 退出接口配置模式
   
   Switch(config)#end                                  // 退出全局配置模式
   
   Switch#write                                        // 保存配置
   ```

3. 配置路由器(以Router1的配置为例):
   <img src="https://pic.imgdb.cn/item/64b7ce361ddac507cc31a91c.jpg" style="zoom:67%;" />
   <img src="https://pic.imgdb.cn/item/64b7cefb1ddac507cc347287.jpg" style="zoom:67%;" />

4. 配置每个路由器的接口地址 & 设置 DCE 的时钟为 64000：

   ```
   //以Router1的配置为例，其他的Router配置方法一样(接口和IP看拓扑图)
   
   Router>enable                            // 进入特权模式
   Router#configure terminal                // 进入全局配置模式
   Router(config)#interface FastEthernet 0/0   // 进入 FastEthernet 0/0 接口配置模式
   Router(config-if)#ip address 192.168.5.1 255.255.255.0   // 配置接口 IP 地址
   Router(config-if)#no shutdown             // 打开接口
   Router(config-if)#exit                    // 退出接口配置模式
   
   Router(config)#interface serial 0/1/0     // 进入序列接口 0/1/0 配置模式
   Router(config-if)#ip address 192.168.1.1 255.255.255.0   // 配置接口 IP 地址
   Router(config-if)#clock rate 64000        // 设置时钟速率(该命令在DCE设备上使用)
   Router(config-if)#no shutdown             // 打开接口
   Router(config-if)#end                     // 退出到特权模式
   
   
   Router#write                              // 保存配置
   ```

   

5. 在路由器1上配置静态路由：

   ```
   Router>enable                                         // 进入特权模式
   Router#configure terminal                             // 进入全局配置模式
   
   //配置非直连网段的静态路由
   Router(config)#ip route 192.168.4.0 255.255.255.0 192.168.1.2   // 配置静态路由到目的网络 192.168.4.0/24
   Router(config)#ip route 192.168.2.0 255.255.255.0 192.168.1.2   // 配置静态路由到目的网络 192.168.2.0/24
   Router(config)#ip route 192.168.3.0 255.255.255.0 192.168.1.2   // 配置静态路由到目的网络 192.168.3.0/24
   
   Router(config)#end                                   // 退出全局配置模式
   Router#write                                         // 保存配置到启动配置
   ```

6. 在路由器2上配置静态路由：
   ```
   Router>enable                                         // 进入特权模式
   Router#configure terminal                             // 进入全局配置模式
   Router(config)#ip route 192.168.5.0 255.255.255.0 192.168.1.1   // 配置静态路由到目的网络 192.168.5.0/24，下一跳为 192.168.1.1
   Router(config)#ip route 192.168.3.0 255.255.255.0 192.168.2.2   // 配置静态路由到目的网络 192.168.3.0/24，下一跳为 192.168.2.2
   Router(config)#end                                   // 退出全局配置模式
   Router#write                                         // 保存配置到启动配置
   ```

7. 在路由器3上配置静态路由：

   ```
   Router>enable
   Router#configure terminal
   Router(config)#ip route 192.168.4.0 255.255.255.0 192.168.2.1
   Router(config)#ip route 192.168.1.0 255.255.255.0 192.168.2.1
   Router(config)#ip route 192.168.5.0 255.255.255.0 192.168.2.1
   Router(config)#end
   Router#write
   ```

   

8. 确认静态路由配置是否生效：

   ```
   Router1# show ip route
   ```

​	

## 路由表讲解

路由表是路由器上存储的用于路由选择的关键信息。它列出了网络的连接方式以及如何到达目标网络的下一跳。

静态路由的配置项由以下部分组成：

- 目标网络：表示要到达的网络地址。
- 子网掩码：用于确定目标网络的范围。
- 下一跳地址：指示路由器发送数据包到达目标网络时要经过的下一跳路由器的IP地址。

​	

## 实验验证

测试PC2能不能联通PC5：

<img src="https://pic.imgdb.cn/item/64b7d8221ddac507cc563483.jpg" style="zoom:67%;" />



​	

## 附件

[静态路由文件下载  记得重命名.pkt](https://wutongliran.lanzoum.com/ia5A512zbwbi)

​	

***参考文章：***

***[https://blog.csdn.net/qq_43499381/article/details/109860375 ](https://blog.csdn.net/qq_43499381/article/details/109860375)***

***[https://blog.csdn.net/Chiong_Z/article/details/118228643](https://blog.csdn.net/Chiong_Z/article/details/118228643)***


