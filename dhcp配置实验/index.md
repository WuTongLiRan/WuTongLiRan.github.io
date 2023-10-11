# 

# DHCP配置实验笔记

## 实验目的
本实验的目的是在三层交换机上配置DHCP服务，为连接到相应VLAN的客户端提供自动IP地址分配功能。

​	

## 实验拓扑

![](https://pic.imgdb.cn/item/64b652391ddac507cc8f932c.jpg)

​	

## 实验步骤

```bash
Switch# configure terminal                      // 进入全局配置模式

Switch(config)# ip dhcp pool VLAN10         // 创建DHCP池 VLAN10，并进入DHCP配置模式
Switch(config-dhcp)# network 192.168.0.0 255.255.255.0    // 设置DHCP池的IP地址范围
Switch(config-dhcp)# default-router 192.168.0.254    // 配置默认网关
Switch(config-dhcp)# dns-server 114.114.114.114      // 配置DNS服务器
Switch(config-dhcp)# lease 1                        // 配置租约时间为 1天(非必须)
Switch(config-dhcp)# exit                            // 退出DHCP配置模式

Switch(config)# ip dhcp excluded-address 192.168.0.1 192.168.0.10   // 排除默认网关地址范围(非必须)
Switch(config)# interface vlan 10                     // 进入VLAN 10 接口配置模式
Switch(config-if)# ip dhcp server                     // 启用DHCP服务(Cisco交换机默认开启)
Switch(config-if)# exit                            // 退出VLAN 10 接口配置模式

Switch(config)# ip dhcp pool VLAN20         // 创建DHCP池 VLAN20_Pool，并进入DHCP配置模式
Switch(config-dhcp)# network 192.168.1.0 255.255.255.0    // 设置DHCP池的IP地址范围
Switch(config-dhcp)# default-router 192.168.1.254    // 配置默认网关
Switch(config-dhcp)# dns-server 114.114.114.114      // 配置DNS服务器
Switch(config-dhcp)# lease 1                        // 配置租约时间为 1天(非必须)
Switch(config-dhcp)# exit                            // 退出DHCP配置模式

Switch(config)# ip dhcp excluded-address 192.168.1.1 192.168.1.10   // 排除默认网关地址范围(非必须)
Switch(config)# interface vlan 20              // 进入VLAN 20 接口配置模式
Switch(config-if)# ip dhcp server              // 启用DHCP服务(Cisco交换机默认开启)

Switch(config)# end                           // 退出全局配置模式
Switch# write							     // 保存配置到启动配置
```

​	

## 实验验证

完成上述配置后，三层交换机将开始提供DHCP服务。您可以使用以下方法验证实验的成功：

1. 将连接到VLAN 10的第一个PC以及连接到VLAN 20的第二个PC设置为自动获取IP地址。
2. 在这两个PC上检查已获得的IP地址、默认网关和DNS服务器的设置。
