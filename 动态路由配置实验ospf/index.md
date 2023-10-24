# 动态路由配置实验—OSPF

# 动态路由配置实验—OSPF

**1、 实验目的** 

1) 掌握 OSPF 的基本配置
2) 熟悉 OSPF 三张表的内容和作用
3) 掌握 OSPF 的 DR 和 BDR 的作用及选举
4) 理解 RID 的概念
5) 理解 OSPF 中两个组播地址的作用

**2、 拓扑结构**

<img src="https://pic.imgdb.cn/item/64e3296d661c6c8e54d06ca6.jpg" style="zoom:67%;" /> 

**3、 实验需求** 

1) 参照逻辑拓扑，使用合适的线缆完成物理拓扑的搭建
2) 完成各路由器的基本配置，实现各直连设备之间可以互 ping 对方，要求C 类地址作为 R1、R2 和 R3 的互联网段地址，B 类地址作为 R3 和 R4 的互连网段地址，具体网段及路由器各接口地址可以自己规划

3) 设置 PC1 的地址为 192.168.1.1/24，网关为 192.168.1.254，设置 PC2 的地址为 172.16.2.1/24，网关为 172.16.2.254

4) 在各路由器上各启用一个 loopback 接口，地址为各设备的编号，如 R1 的loopback 地址为 1.1.1.1/24，R2 的 loopback 地址为 2.2.2.2/24，其他以此类推

5) 全网启用 OSPF，进程设置为 110，并将整个环境划分入区域 0，要求在R4 上使用接口级命令的方式启用 OSPF，其他设备上使用常规的 router ospf 方式启用 OSPF

6) 使用合适的命令查看 OSPF 的邻居表、拓扑表以及路由表，并测试 PC1 与PC2 之间的连通性

7) 观察以太网环境与串行链路环境的 OSPF 邻居表的区别
8) 使用合适的命令找出并记录各个路由器的 RID
9) 观察 R1/R2/R3 之间 DR/BDR/DROTHER 的相应角色，然后采取合适的方式将 R2 设置为该以太网环境的 DR

10) 任意选取两个路由器的 OSPF 拓扑表，观察这两个路由器之间的 OSPF 拓扑表是否一致

11) 指定 R2 的 RID 为 1.2.3.4，R3 的 RID 为 5.6.7.8，使用相关命令观察和验证各路由器 RID 的变化

12) 在 R1 上添加 192.168.100.0/24 的网段，在 R4 上添加 172.16.100.0/24 的网段，完成必要配置实现这两个网络之间的连通性

​	

​	

### 1.参照逻辑拓扑，使用合适的线缆完成物理拓扑的搭建

![](https://pic.imgdb.cn/item/64e4290c661c6c8e549fcfae.jpg)

​	

### 2.完成各路由器的基本配置，实现各直连设备之间可以互 ping 对方，要求C 类地址作为 R1、R2 和 R3 的互联网段地址，B 类地址作为 R3 和 R4 的互连网段地址，具体网段及路由器各接口地址可以自己规划

<img src="https://pic.imgdb.cn/item/64e42d7d661c6c8e54a1b1ed.jpg" style="zoom:80%;" />

<img src="https://pic.imgdb.cn/item/64e42dc0661c6c8e54a1cc27.jpg" style="zoom:80%;" />

<img src="https://pic.imgdb.cn/item/64e42dee661c6c8e54a1da0e.jpg" style="zoom:80%;" />

<img src="https://pic.imgdb.cn/item/64e46a1a661c6c8e54b82ad4.jpg" style="zoom:80%;" />

​	

### 3.设置 PC1 的地址为 192.168.1.1/24，网关为 192.168.1.254，设置 PC2 的地址为 172.16.2.1/24，网关为 172.16.2.254

<img src="https://pic.imgdb.cn/item/64e46a4c661c6c8e54b838ab.jpg" style="zoom: 50%;" />

<img src="https://pic.imgdb.cn/item/64e46a69661c6c8e54b8443f.jpg" style="zoom:50%;" />

​	

### 4.在各路由器上各启用一个 loopback 接口，地址为各设备的编号，如 R1 的loopback 地址为 1.1.1.1/24，R2 的 loopback 地址为 2.2.2.2/24，其他以此类推

R1的配置：

```
<R1>sys
[R1]int LoopBack 0
[R1-LoopBack0]ip address 1.1.1.1 24
```

> 其他路由器的配置以此类推，只是配置的IP不同

​	

### 5.全网启用 OSPF，进程设置为 110，并将整个环境划分入区域 0，要求在R4 上使用接口级命令的方式启用 OSPF，其他设备上使用常规的 routerospf 方式启用 OSPF

R1的配置：

```
[R1]ospf 110
[R1-ospf-110]area 0
[R1-ospf-110-area-0.0.0.0]network 192.168.123.0 0.0.0.255
```

R2的配置：

```
[R2]ospf 110	
[R2-ospf-110]area 0
[R2-ospf-110-area-0.0.0.0]network 192.168.123.0 0.0.0.255
[R2-ospf-110-area-0.0.0.0]network 172.16.24.0 0.0.0.255
```

R3的配置：

```
[R3]ospf 110
[R3-ospf-110]area 0
[R3-ospf-110-area-0.0.0.0]network 192.168.123.0 0.0.0.255
[R3-ospf-110-area-0.0.0.0]network 192.168.1.0 0.0.0.255
```

R4的配置：

```
[R4]ospf 110 router-id 4.4.4.4
[R4-ospf-110]qui
[R4]qui
<R4>reset ospf process 
[R4]int s2/0/0
[R4-Serial2/0/0]ospf enable 110 area 0
[R4-ospf-110-area-0.0.0.0]qui
[R4-ospf-110]qui
[R4]int g0/0/0
[R4-GigabitEthernet0/0/0]ospf enable 110 area 0
[R4-GigabitEthernet0/0/0]ospf 110
[R4-ospf-110]area 0
```

​	

### 6.使用合适的命令查看 OSPF 的邻居表、拓扑表以及路由表，并测试 PC1 与PC2 之间的连通性

查看邻居表的命令：

```
[]display ospf peer brief 
```

<img src="https://pic.imgdb.cn/item/64e47c2a661c6c8e54bfa996.jpg" style="zoom:80%;" />

<img src="https://pic.imgdb.cn/item/64e47c86661c6c8e54bfd996.jpg" style="zoom:80%;" />

<img src="https://pic.imgdb.cn/item/64e47ccd661c6c8e54c005dd.jpg" style="zoom:80%;" />

<img src="https://pic.imgdb.cn/item/64e47cf9661c6c8e54c01683.jpg" style="zoom:80%;" />

查看拓扑表的命令：

```
[]display ospf lsdb 
```

查看路由表的命令：

```
[]display ip routing-table
```



<img src="https://pic.imgdb.cn/item/64e47b73661c6c8e54bf2778.jpg" style="zoom:50%;" />

​	

### 7.观察以太网环境与串行链路环境的 OSPF 邻居表的区别

```
[]display ospf peer 
```

​	

### 8.使用合适的命令找出并记录各个路由器的 RID

```
[]display ospf peer brief 
```

<img src="https://pic.imgdb.cn/item/64e47c2a661c6c8e54bfa996.jpg" style="zoom:80%;" />

<img src="https://pic.imgdb.cn/item/64e47c86661c6c8e54bfd996.jpg" style="zoom:80%;" />

<img src="https://pic.imgdb.cn/item/64e47ccd661c6c8e54c005dd.jpg" style="zoom:80%;" />

<img src="https://pic.imgdb.cn/item/64e47cf9661c6c8e54c01683.jpg" style="zoom:80%;" />

​	

### 9.观察 R1/R2/R3 之间 DR/BDR/DROTHER 的相应角色，然后采取合适的方式将 R2 设置为该以太网环境的 DR

观察路由器当前状态：

```
[]display ospf peer
```

将 R2 设置为该以太网环境的 DR：

```
[R2]int g0/0/0
[R2-GigabitEthernet0/0/0]ospf dr-priority 100
[R2-GigabitEthernet0/0/0]quit
[R2]quit
<R2>reset ospf process
```

​	

### 10.任意选取两个路由器的 OSPF 拓扑表，观察这两个路由器之间的 OSPF 拓扑表是否一致

查看拓扑表的命令：

```
[]display ospf lsdb 
```

<img src="https://pic.imgdb.cn/item/64e4886f661c6c8e54c42fc0.jpg" style="zoom:67%;" />

<img src="https://pic.imgdb.cn/item/64e486d6661c6c8e54c35178.jpg" style="zoom:67%;" />

​	

### 11.指定 R2 的 RID 为 1.2.3.4，R3 的 RID 为 5.6.7.8，使用相关命令观察和验证各路由器 RID 的变化

```
[R2]ospf 110 router-id 1.2.3.4
```

```
[R3]ospf 110 router-id 5.6.7.8
```

<img src="https://pic.imgdb.cn/item/64e4881f661c6c8e54c41914.jpg" style="zoom:67%;" />

<img src="https://pic.imgdb.cn/item/64e48891661c6c8e54c43860.jpg" style="zoom:67%;" />

​	

### 12.在 R1 上添加 192.168.100.0/24 的网段，在 R4 上添加 172.16.100.0/24 的网段，完成必要配置实现这两个网络之间的连通性

```
[R1]int LoopBack 1
[R1-LoopBack1]ip address 192.168.100.1 24
[R1-LoopBack1]192.168.100.1 0.0.0.255
[R1-LoopBack1]ospf enable area 0
```

```
[R4]int LoopBack 1
[R4-LoopBack1]ip address 172.16.100.4 24
[R4-LoopBack1]172.16.100.4 0.0.0.255
[R4-LoopBack1]ospf enable 110 area 0
```


