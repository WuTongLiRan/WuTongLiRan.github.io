# 

# ACL实验

ACL 的运用
1、实验目的通过本实验可以:
1) 掌握 ACL的作用
2) 理解基本和高级 ACL的区别
3) 熟悉基本和高级 ACL的配置
4) 掌握 ACL 的验证和查看命令
5) 理解命名 ACL 的作用与配置

2、拓扑结构

![](https://pic.imgdb.cn/item/65178f5cc458853aef376fdc.jpg)

3、 实验需求 
1) 参照逻辑拓扑，使用合适的线缆完成物理拓扑的搭建 
2) 在 SW1 和 SW2 上各创建三个 VLAN，VLAN10，VLAN20 和 VLAN30，分别将 PC1 和 PC4 加入 VLAN10，PC2 和 PC5 加入 VLAN20，PC3和PC6加入VLAN30，各 VLAN 的网段以及网关自己规划。
3) 完成各路由器的基本配置，实现各路由器之间可以互 ping 对方，路由器接口的地址自己规划。
4) 全网启用任意路由协议，完成必要的配置实现全网可达，即实现PC 之间可以互相访问。
5) 开启 R1 Telnet服务和 R3 上的 SSH 服务，用户名为r1和r3，并设置登录密码都为 huawei123
6) 使用基本 ACL 实现 PC1 不能访问 PC2，并使用 ping 命令验证结果
7）移除上述 ACL，使用基本ACL 实现只允许 R1连接R2的接口可以 远程登录到路由器 R3， 并验证结果(注意调用位置)
8) 移除上述 ACL，使用高级(扩展)ACL 实现 PC4 和 PC5 不能 ping 路由器 R1，但是可以访问到路由器 R1上的三个网关地址，其他主机对 R1 的访问不受限制，使用 ping 验证结果
9) 移除上述 ACL，使用高级 ACL 实现 PC1 可以 ping PC6，但禁止 PC6 ping PC1，其他的访问不受限制，使用 ping 验证结果
10 ) 移除上述 ACL，使用高级ACL实现禁止R3连接R2的接口使用Telnet登录R1，但不影响其他方式的通信。



## 1.

## 2.

- SW1

```
[SW1]vlan batch 10 20 30
[SW1]int e0/0/1
[SW1-Ethernet0/0/1]port link-type access 
[SW1-Ethernet0/0/1]port default vlan 10
[SW1-Ethernet0/0/1]int e0/0/2
[SW1-Ethernet0/0/2]port link-type access
[SW1-Ethernet0/0/2]port default vlan 20
[SW1-Ethernet0/0/2]int e0/0/3
[SW1-Ethernet0/0/3]port link-type access
[SW1-Ethernet0/0/3]port default vlan 30
[SW1-Ethernet0/0/3]int e0/0/4
[SW1-Ethernet0/0/4]port link-type trunk 
[SW1-Ethernet0/0/4]port trunk allow-pass vlan all 
```

- SW2

```
[SW2]vlan batch 10 20 30
[SW2]int e0/0/1
[SW2-Ethernet0/0/1]port link-type access 
[SW2-Ethernet0/0/1]port default vlan 10
[SW2-Ethernet0/0/1]int e0/0/2
[SW2-Ethernet0/0/2]port link-type access
[SW2-Ethernet0/0/2]port default vlan 20
[SW2-Ethernet0/0/2]int e0/0/3
[SW2-Ethernet0/0/3]port link-type access
[SW2-Ethernet0/0/3]port default vlan 30
[SW2-Ethernet0/0/3]int e0/0/4
[SW2-Ethernet0/0/4]port link-type trunk 
[SW2-Ethernet0/0/4]port trunk allow-pass vlan all 
```

## 3.

- R1

```
[R1]int g0/0/0.10
[R1-GigabitEthernet0/0/0.10]dot1q termination vid 10
[R1-GigabitEthernet0/0/0.10]ip add 192.168.10.254 24
[R1-GigabitEthernet0/0/0.10]arp broadcast enable 
[R1-GigabitEthernet0/0/0.10]int g0/0/0.20
[R1-GigabitEthernet0/0/0.20]dot1q termination vid 20
[R1-GigabitEthernet0/0/0.20]ip add 192.168.20.254 24
[R1-GigabitEthernet0/0/0.20]arp broadcast enable
[R1-GigabitEthernet0/0/0.20]int g0/0/0.30
[R1-GigabitEthernet0/0/0.30]dot1q termination vid 30
[R1-GigabitEthernet0/0/0.30]ip add 192.168.30.254 24
[R1-GigabitEthernet0/0/0.30]arp broadcast enable
[R1-GigabitEthernet0/0/0.30]int g0/0/1
[R1-GigabitEthernet0/0/1]ip add 172.16.12.1 24
```

- R2

```
[R2]int g0/0/0
[R2-GigabitEthernet0/0/0]ip add 172.16.12.2 24
[R2-GigabitEthernet0/0/0]int g0/0/1
[R2-GigabitEthernet0/0/1]ip add 172.16.23.2 24
```

- R3

```
[R3]int g0/0/0.10
[R3-GigabitEthernet0/0/0.10]dot1q termination vid 10
[R3-GigabitEthernet0/0/0.10]ip add 192.168.10.254 24
[R3-GigabitEthernet0/0/0.10]arp broadcast enable 
[R3-GigabitEthernet0/0/0.10]int g0/0/0.20
[R3-GigabitEthernet0/0/0.20]dot1q termination vid 20
[R3-GigabitEthernet0/0/0.20]ip add 192.168.20.254 24
[R3-GigabitEthernet0/0/0.20]arp broadcast enable
[R3-GigabitEthernet0/0/0.20]int g0/0/0.30
[R3-GigabitEthernet0/0/0.30]dot1q termination vid 30
[R3-GigabitEthernet0/0/0.30]ip add 192.168.30.254 24
[R3-GigabitEthernet0/0/0.30]arp broadcast enable
[R3-GigabitEthernet0/0/0.30]int g0/0/1
[R3-GigabitEthernet0/0/1]ip add 172.16.23.1 24
```

## 4.

- R1

```
[R1]ospf 110
[R1-ospf-110]area 0
[R1-ospf-110-area-0.0.0.0]network 192.168.10.0 0.0.0.255
[R1-ospf-110-area-0.0.0.0]network 192.168.20.0 0.0.0.255
[R1-ospf-110-area-0.0.0.0]network 192.168.30.0 0.0.0.255
[R1-ospf-110-area-0.0.0.0]network 172.16.12.0 0.0.0.255
```

- R2

```
[R2]ospf 110
[R2-ospf-110]area 0
[R2-ospf-110-area-0.0.0.0]network 172.16.12.0 0.0.0.255
[R2-ospf-110-area-0.0.0.0]network 172.16.23.0 0.0.0.255
```

- R3

```
[R3]ospf 110
[R3-ospf-110]area 0
[R3-ospf-110-area-0.0.0.0]network 192.168.10.0 0.0.0.255
[R3-ospf-110-area-0.0.0.0]network 192.168.20.0 0.0.0.255
[R3-ospf-110-area-0.0.0.0]network 192.168.30.0 0.0.0.255
[R3-ospf-110-area-0.0.0.0]network 172.16.23.0 0.0.0.255
```


