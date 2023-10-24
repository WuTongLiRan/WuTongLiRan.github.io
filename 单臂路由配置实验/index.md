# 单臂路由配置实验

# 单臂路由配置实验

## 1. 单臂路由概述

单臂路由是指在路由器的**一个接口**上通过**配置子接口**（或“逻辑接口”，并不存在真正物理接口）的方式，**实现原来相互隔离的不同 VLAN 之间的互联互通。**

​	

### 1.1 链路状态

- 交换机**连接主机的端口为 Access 链路。**
- 交换机**连接路由器的端口为 Trunk 链路。**

​	

### 1.2 子接口

- 路由器的物理接口可以被划分成为多个逻辑接口
- 每个子接口对应一个 VLAN 网段网关

​	

​	

## 2. 单臂路由实验

![](https://pic.imgdb.cn/item/64b60ffe1ddac507cc9ca06f.jpg)

### 2.1 PC配置

- PC 0
  <img src="https://pic.imgdb.cn/item/64b6025f1ddac507cc6ca0d1.jpg" style="zoom:80%;" />
- PC 1
  <img src="https://pic.imgdb.cn/item/64b602d71ddac507cc6e47a3.jpg" style="zoom:80%;" />

​	

### 2.2 交换机配置

```css
Switch> enable                     // 进入特权模式
Switch# configure terminal         // 进入全局配置模式
Switch(config)# vlan 10            // 创建 VLAN 10
Switch(config-vlan)# vlan 20       // 创建 VLAN 20
Switch(config-vlan)# exit          // 退出 VLAN 配置模式
Switch(config)# interface fastEthernet 0/1     // 进入 0/1 接口
Switch(config-if)# switchport mode access      // 将接口设置为 Access 模式
Switch(config-if)# switchport access vlan 10   // 将接口设置为 VLAN 10
Switch(config-if)# exit                        // 退出接口配置模式
Switch(config)# interface fastEthernet 0/2     // 进入 0/2 接口
Switch(config-if)# switchport mode access      // 将接口设置为 Access 模式
Switch(config-if)# switchport access vlan 20   // 将接口设置为 VLAN 20
Switch(config-if)# exit                        // 退出接口配置模式
Switch(config)# interface fastEthernet 0/3     // 进入 0/3 接口
Switch(config-if)# switchport mode trunk       // 将接口设置为 Trunk 模式
Switch(config-if)# end                         // 退出配置模式
Switch# write                                  // 保存配置更改
```

​	

### 2.3 路由器配置

```css
Router# enable                            // 进入特权模式
Router# configure terminal                // 进入全局配置模式
Router(config)# interface f0/0.1          // 进入接口 f0/0 的子接口 1
Router(config-subif)# encapsulation dot1q 10     // 配置子接口的 VLAN 标记为 10
Router(config-subif)# ip address 192.168.10.254 255.255.255.0   // 配置子接口的 IP 地址为 192.168.10.254/24
Router(config-subif)# no shutdown         // 启用子接口
Router(config-subif)# exit                // 退出子接口配置模式
Router(config)# interface f0/0.2          // 进入接口 f0/0 的子接口 2
Router(config-subif)# encapsulation dot1q 20     // 配置子接口的 VLAN 标记为 20
Router(config-subif)# ip address 192.168.20.254 255.255.255.0   // 配置子接口的 IP 地址为 192.168.20.254/24
Router(config-subif)# no shutdown         // 启用子接口
Router(config-subif)# exit                // 退出子接口配置模式
Router(config)# interface f0/0             // 进入接口 f0/0
Router(config-if)# no shutdown            // 启用接口 f0/0
Router(config-if)# end                    // 退出配置模式
Router# write                             // 保存配置更改
```

​	

### 2.4 测试连通性

- PC 0  ping  PC 1
  <img src="https://pic.imgdb.cn/item/64b61c5a1ddac507ccc69f7d.jpg" style="zoom:80%;" />
- PC 1  ping  PC 0
  <img src="https://pic.imgdb.cn/item/64b61cda1ddac507ccc841ce.jpg" style="zoom:80%;" />

