# 二层冗余综合实验

# 二层冗余综合实验

**1、 实验目的** 

通过本实验可以： 

1. 理解二层交换的基本原理
2. 掌握交换机的基本配置 
3. 掌握交换机VLAN划分
4. 掌握实现不同局域网的通信
5. 掌握VRRP以及链路捆绑
6. 掌握DHCP的基本配置 

 

**2、 拓扑结构**

<img src="https://pic.imgdb.cn/item/64f3eb13661c6c8e54f914b3.jpg" style="zoom:67%;" />

**3、 实验需求** 

1. 参照逻辑拓扑，使用合适的线缆完成物理拓扑的搭建 

2. 完成相应配置，PC1与PC3在一个局域网里，PC2与PC4在一个局域网里，并捆绑三层交换机之间的端口

3. 在R1上完成相应配置，并启用动态地址池，实现各个主机可以获取到地址并且能够相互通信

4. 开启虚拟网关冗余协议，R1作为PC1与PC3局域网的主路由器，R2作为PC2与PC4局域网的主路由器 

5. 合理划分R1,R2,R3之间的地址，全网启用路由协议，实现主机与R3之间的通信

6. 分析此时PC2能否获取到动态地址，并进行检验

7. 关闭R2的Master接口，观察日志信息，分析此时网络环境是否正常

 

#### 参照逻辑拓扑，使用合适的线缆完成物理拓扑的搭建

![](https://pic.imgdb.cn/item/64f3f3f6661c6c8e54fa8cec.jpg)

#### 完成相应配置，PC1与PC3在一个局域网里，PC2与PC4在一个局域网里，并捆绑三层交换机之间的端口

- ASW1

```
[ASW1]vlan batch 10 20

[ASW1]int e0/0/1
[ASW1-Ethernet0/0/1]port link-type access 
[ASW1-Ethernet0/0/1]port default vlan 10
[ASW1-Ethernet0/0/1]quit

[ASW1]int e0/0/2
[ASW1-Ethernet0/0/2]port link-type access 	
[ASW1-Ethernet0/0/2]port default vlan 20

[ASW1]int e0/0/3
[ASW1-Ethernet0/0/3]port link-type trunk 
[ASW1-Ethernet0/0/3]port trunk allow-pass vlan all 
```

- ASW2

```
[ASW2]vlan batch 10 20

[ASW2]int e0/0/1	
[ASW2-Ethernet0/0/1]port link-type access 
[ASW2-Ethernet0/0/1]port default vlan 10
[ASW2-Ethernet0/0/1]quit

[ASW2]int e0/0/2
[ASW2-Ethernet0/0/2]port link-type access 
[ASW2-Ethernet0/0/2]port default vlan 20

[ASW2]int e0/0/3
[ASW2-Ethernet0/0/3]port link-type trunk 
[ASW2-Ethernet0/0/3]port trunk allow-pass vlan all
```

- DSW1

```
[DSW1]vlan batch 10 20

[DSW1]int g0/0/1
[DSW1-GigabitEthernet0/0/1]port link-type trunk 
[DSW1-GigabitEthernet0/0/1]port trunk allow-pass vlan all 
[DSW1-GigabitEthernet0/0/1]quit

[DSW1]int g0/0/4	
[DSW1-GigabitEthernet0/0/4]port link-type  trunk 
[DSW1-GigabitEthernet0/0/4]port trunk allow-pass vlan all 
[DSW1-GigabitEthernet0/0/4]quit

[DSW1]int Eth-Trunk 1
[DSW1-Eth-Trunk1]port link-type  trunk 	
[DSW1-Eth-Trunk1]port trunk allow-pass vlan all 
[DSW1-Eth-Trunk1]quit

[DSW1]int g0/0/2
[DSW1-GigabitEthernet0/0/2]eth-trunk 1
[DSW1-GigabitEthernet0/0/2]quit

[DSW1]int g0/0/3
[DSW1-GigabitEthernet0/0/3]eth-trunk 1
```

- DSW2

```
[DSW2]vlan batch 10 20

[DSW2]int g0/0/1
[DSW2-GigabitEthernet0/0/1]port link-type trunk 
[DSW2-GigabitEthernet0/0/1]port trunk allow-pass vlan all 
[DSW2-GigabitEthernet0/0/1]quit

[DSW2]int g0/0/4	
[DSW2-GigabitEthernet0/0/4]port link-type  trunk 
[DSW2-GigabitEthernet0/0/4]port trunk allow-pass vlan all 
[DSW2-GigabitEthernet0/0/4]quit

[DSW2]int Eth-Trunk 1
[DSW2-Eth-Trunk1]port link-type  trunk 	
[DSW2-Eth-Trunk1]port trunk allow-pass vlan all 
[DSW2-Eth-Trunk1]quit

[DSW2]int g0/0/2
[DSW2-GigabitEthernet0/0/2]eth-trunk 1
[DSW2-GigabitEthernet0/0/2]quit

[DSW2]int g0/0/3
[DSW2-GigabitEthernet0/0/3]eth-trunk 1
```



#### 在R1、R2上完成相应配置，并启用动态地址池，实现各个主机可以获取到地址并且能够相互通信

- R1

```
[R1]ip pool vlan10
[R1-ip-pool-vlan10]network 192.168.10.0 mask 24
[R1-ip-pool-vlan10]gateway-list 192.168.10.254
[R1-ip-pool-vlan10]dns-list 8.8.8.8
[R1-ip-pool-vlan10]quit

[R1]ip pool vlan20
[R1-ip-pool-vlan20]network 192.168.20.0 mask 24
[R1-ip-pool-vlan20]gateway-list 192.168.20.254
[R1-ip-pool-vlan20]dns-list 8.8.8.8
```

- R2

```
[R2]ip pool vlan10
[R2-ip-pool-vlan10]network 192.168.10.0 mask 24
[R2-ip-pool-vlan10]gateway-list 192.168.10.254
[R2-ip-pool-vlan10]dns-list 8.8.8.8
[R2-ip-pool-vlan10]quit

[R2]ip pool vlan20
[R2-ip-pool-vlan20]network 192.168.20.0 mask 24
[R2-ip-pool-vlan20]gateway-list 192.168.20.254
[R2-ip-pool-vlan20]dns-list 8.8.8.8
```



#### 开启虚拟网关冗余协议，R1作为PC1与PC3局域网的主路由器，R2作为PC2与PC4局域网的主路由器

- R1

```
[R1]int g0/0/0.10
[R1-GigabitEthernet0/0/0.10]dot1q termination vid 10
[R1-GigabitEthernet0/0/0.10]ip address 192.168.10.253 24
[R1-GigabitEthernet0/0/0.10]arp broadcast enable 
[R1-GigabitEthernet0/0/0.10]vrrp vrid 10 virtual-ip 192.168.10.254
[R1-GigabitEthernet0/0/0.10]qui


[R1]int g0/0/0.20
[R1-GigabitEthernet0/0/0.20]dot1q termination vid 20
[R1-GigabitEthernet0/0/0.20]ip address 192.168.20.252 24
[R1-GigabitEthernet0/0/0.20]arp broadcast enable 
[R1-GigabitEthernet0/0/0.20]vrrp vrid 20 virtual-ip 192.168.20.254
[R1-GigabitEthernet0/0/0.20]quit

[R1]dhcp enable 

[R1]int g0/0/0.10	
[R1-GigabitEthernet0/0/0.10]dhcp select global 
[R1-GigabitEthernet0/0/0.10]qui

[R1]int g0/0/0.20
[R1-GigabitEthernet0/0/0.20]dhcp select  global 
```

- R2

```
[R2]int g0/0/0.10
[R2-GigabitEthernet0/0/0.10]dot1q termination vid 10
[R2-GigabitEthernet0/0/0.10]ip address 192.168.10.252 24
[R2-GigabitEthernet0/0/0.10]arp broadcast enable 
[R2-GigabitEthernet0/0/0.10]vrrp vrid 10 virtual-ip 192.168.10.254
[R2-GigabitEthernet0/0/0.10]quit

[R2]int g0/0/0.20
[R2-GigabitEthernet0/0/0.20]dot1q termination vid 20
[R2-GigabitEthernet0/0/0.20]ip address 192.168.20.253 24
[R2-GigabitEthernet0/0/0.20]arp broadcast enable 
[R2-GigabitEthernet0/0/0.20]vrrp vrid 20 virtual-ip 192.168.20.254
[R2-GigabitEthernet0/0/0.20]quit

[R2]dhcp enable 

[R2]int g0/0/0.10	
[R2-GigabitEthernet0/0/0.10]dhcp select global 
[R2-GigabitEthernet0/0/0.10]quit

[R2]int g0/0/0.20
[R2-GigabitEthernet0/0/0.20]dhcp select  global 
```



#### 合理划分R1,R2,R3之间的地址，全网启用路由协议，实现主机与R3之间的通信

- R1

```
[R1]int g0/0/1
[R1-GigabitEthernet0/0/1]ip address 172.16.13.1 24
[R1-GigabitEthernet0/0/1]quit

[R1]ospf 110 router-id 1.1.1.1
[R1-ospf-110]area 0
[R1-ospf-110-area-0.0.0.0]network 172.16.13.0 0.0.0.255
[R1-ospf-110-area-0.0.0.0]network 192.168.10.0 0.0.0.255
[R1-ospf-110-area-0.0.0.0]network 192.168.20.0 0.0.0.255
```

- R3

```
[R3]ospf 110
[R3-ospf-110]area 0
[R3-ospf-110-area-0.0.0.0]quit

[R3]int g0/0/0
[R3-GigabitEthernet0/0/0]ip add 172.16.13.2 24
[R3-GigabitEthernet0/0/0]ospf enable 110 area 0
[R3-GigabitEthernet0/0/0]quit

[R3]int g0/0/1
[R3-GigabitEthernet0/0/1]ip add 172.16.23.2 24
[R3-GigabitEthernet0/0/1]ospf enable 110 area 0
[R3-GigabitEthernet0/0/1]quit
```



#### 分析此时PC2能否获取到动态地址，并进行检验

![](https://pic.imgdb.cn/item/65178d79c458853aef3694fc.jpg)

#### 关闭R3的Master接口，观察日志信息，分析此时网络环境是否正常

- R3

```
[AR3]int g0/0/1.20
[AR3-GigabitEthernet0/0/1.20]shutdown
```


