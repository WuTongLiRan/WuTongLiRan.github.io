# 

# 【Cisco & eNSP】静态路由配置实验

> 温馨提示：配完不保存，故障泪两行

## 实验目的

通过本实验可以： 

1. 理解路由的作用 
2. 掌握静态路由的配置 
3. 理解路由表的含义 
4. 理解默认路由的作用与配置 
5. 理解默认路由的使用场合
6. 理解 traceroute 和 ping 命令的使用 

​	

## 拓扑结构

![](https://pic.imgdb.cn/item/64df3b6d661c6c8e548ab3d2.jpg)

​	

## 实验需求

1. 参照逻辑拓扑，使用合适的线缆完成物理拓扑的搭建 
2. 完成各路由器的基本配置，实现各直连设备之间可以互 ping 对方，主机和路由器接口的地址自己规划
3. 测试 PC1 与 PC2 两主机之间的连通性
4. 在 R1 上创建一条到达对端主机 PC2 所在网络的静态路由，要求使用下一跳方式实现 
5. 在 R3 上创建一条到达对端主机 PC1 所在网络的静态路由，要求使用出接口方式实现 
6. 观察 R1 和 R3 上的路由表，仔细观察使用出接口与使用下一跳方式的静态路由表项的不同 
7. 添加合适的配置，实现 PC1 与 PC2 之间的连通性，使用 ping 进行测试
8. 分别在 PC1 和 PC2 上使用 tracert 观察两主机通信过程的传输路径 
9. 添加合适的配置，实现每个路由器可以相互远程连接
10. 删除 R1 和 R3 上的静态路由条目，使用默认路由实现 PC1 与 PC2 之间的连通性

​	

​	

## 1.参照逻辑拓扑，使用合适的线缆完成物理拓扑的搭建

### eNSP

<img src="https://pic.imgdb.cn/item/64e01778661c6c8e54b87848.jpg" style="zoom: 80%;" />

### Cisco

<img src="https://pic.imgdb.cn/item/64e03efe661c6c8e542e9a46.jpg" style="zoom:80%;" />

​	

## 2.完成各路由器的基本配置，实现各直连设备之间可以互 ping 对方，主机和路由器接口的地址自己规划

### eNSP

PC1

<img src="https://pic.imgdb.cn/item/64df4657661c6c8e54aac539.jpg" style="zoom:67%;" />

PC2

<img src="https://pic.imgdb.cn/item/64df4f01661c6c8e54c29682.jpg" style="zoom: 67%;" />

R1

```
[R1]int g0/0/0
[R1-GigabitEthernet0/0/0]ip address 192.168.10.254 24

[R1-GigabitEthernet0/0/0]quit

[R1]int g0/0/1
[R1-GigabitEthernet0/0/1]ip address 192.168.12.1 24
```

> eNSP的接口默认是开启的

R2

```
[R2]int g0/0/0
[R2-GigabitEthernet0/0/0]ip address 192.168.12.2 24

[R2-GigabitEthernet0/0/0]quit

[R2]int s4/0/0
[R2-Serial4/0/0]ip address 192.168.23.2 24
```

R3

```
[R3]int s4/0/0
[R3-Serial4/0/0]ip address 192.168.23.3 24

[R3-Serial4/0/0]quit

[R3]int g0/0/0
[R3-GigabitEthernet0/0/0]ip address 192.168.20.254 24
```

### Cisco

PC1

<img src="https://pic.imgdb.cn/item/64e01932661c6c8e54bc28c0.jpg" style="zoom:67%;" />

PC2

<img src="https://pic.imgdb.cn/item/64e01980661c6c8e54bcdddc.jpg" style="zoom:67%;" />

R1

```
Router>en
Router#config
Router(config)#int f0/0
Router(config-if)#ip address 192.168.10.254 255.255.255.0
Router(config-if)#no shutdown 
Router(config-if)#ex

Router(config)#int f0/1
Router(config-if)#ip address 192.168.12.1 255.255.255.0
Router(config-if)#no shutdown 
```

R2

```
Router>en
Router#configure terminal 
Router(config)#int f0/0
Router(config-if)#ip address 192.168.12.2 255.255.255.0
Router(config-if)#no shutdown 
Router(config-if)#ex

Router(config)#int s0/1/0
Router(config-if)#ip address 192.168.23.2 255.255.255.0
Router(config-if)#no shutdown 
```

R3

```
Router>en
Router#config
Router(config)#int s0/1/0
Router(config-if)#ip address 192.168.23.3 255.255.255.0
Router(config-if)#no shutdown 
Router(config-if)#ex

Router(config)#int f0/0
Router(config-if)#ip address 192.168.20.254 255.255.255.0
Router(config-if)#no shutdown 
```

​	

## 3.测试 PC1 与 PC2 两主机之间的连通性

### eNSP

<img src="https://pic.imgdb.cn/item/64df5433661c6c8e54d349f1.jpg" style="zoom:67%;" />

### Cisco

![](https://pic.imgdb.cn/item/64e01d5a661c6c8e54c8148e.jpg)

​	

## 4.在 R1 上创建一条到达对端主机 PC2 所在网络的静态路由，要求使用下一跳方式实现

### eNSP

```
[R1]ip route-static 192.168.20.1 255.255.255.0 192.168.12.2
```

### Cisco

```
Router(config)#ip route 192.168.20.0 255.255.255.0 192.168.12.2
```

​	

## 5.在 R3 上创建一条到达对端主机 PC1 所在网络的静态路由，要求使用出接口方式实现

### eNSP

```
[R3]ip route-static 192.168.10.1 255.255.255.0 s4/0/0
```



### Cisco

```
Router(config)#ip route 192.168.10.0 255.255.255.0 s0/1/0
```

​	

## 6.观察 R1 和 R3 上的路由表，仔细观察使用出接口与使用下一跳方式的静态路由表项的不同

### eNSP

R1

```
[R1]display ip routing-table
```

<img src="https://pic.imgdb.cn/item/64df582e661c6c8e54dd55f3.jpg" style="zoom:67%;" />

R3

```
[R3]display ip routing-table
```

<img src="https://pic.imgdb.cn/item/64df57d9661c6c8e54dc8560.jpg" style="zoom:67%;" />

### Cisco

R1

```
Router#show ip route 
```

<img src="https://pic.imgdb.cn/item/64e03a3b661c6c8e5421e277.jpg" style="zoom:67%;" />

R3

<img src="https://pic.imgdb.cn/item/64e039eb661c6c8e5420ef03.jpg" style="zoom: 67%;" />





​	

## 7.添加合适的配置，实现 PC1 与 PC2 之间的连通性，使用 ping 进行测试

### eNSP

R2

```
[R2]ip route-static 192.168.20.1 255.255.255.0 192.168.23.3
[R2]ip route-static 192.168.10.1 255.255.255.0 192.168.12.1
```

测试连通性

<img src="https://pic.imgdb.cn/item/64df58ec661c6c8e54df4438.jpg" style="zoom: 80%;" />

### Cisco

R2

```
Router(config)#ip route 192.168.20.0 255.255.255.0 192.168.23.3
Router(config)#ip route 192.168.10.0 255.255.255.0 192.168.12.1
```

​	测试连通性

![](https://pic.imgdb.cn/item/64e03ae8661c6c8e5423cc9a.jpg)

​	

## 8.分别在 PC1 和 PC2 上使用 tracert 观察两主机通信过程的传输路径

### eNSP

PC1

```
PC>tracert 192.168.20.1
```

<img src="https://pic.imgdb.cn/item/64df59ac661c6c8e54e12c6c.jpg" style="zoom:80%;" />

PC2

```
PC>tracert 192.168.10.1
```

<img src="https://pic.imgdb.cn/item/64df59e7661c6c8e54e1c3c8.jpg" style="zoom:80%;" />

### Cisco

PC1

![](https://pic.imgdb.cn/item/64e03b2a661c6c8e542484ad.jpg)

PC2

![](https://pic.imgdb.cn/item/64e03b91661c6c8e54259e91.jpg)

​	

## 9.添加合适的配置，实现每个路由器可以相互远程连接

### eNSP

#### 实现路由器之间的通信

R1

```
[R1]ip route-static 192.168.23.3 255.255.255.0 192.168.12.2
```

R3

```
[R3]ip route-static 192.168.12.1 255.255.255.0 192.168.23.2
```



#### telnet password模式

在每个路由器上配置telnet password模式：

```
telnet server enable
user-interface vty 0 2
authentication-mode password
123456
protocol inbound  telnet
user privilege level 15
```

R1远程连接R2：

```
<R1>telnet 192.168.12.2
Password:123456
```

<img src="https://pic.imgdb.cn/item/64df8a91661c6c8e54906223.jpg" style="zoom:67%;" />

R1远程连接R3：

```
<R1>telnet 192.168.23.3
Password:123456
```

<img src="https://pic.imgdb.cn/item/64df8d56661c6c8e54999a50.jpg" style="zoom:67%;" />

R2连接R1、R2连接R3、R3连接R1、R3连接R2 都能成功

#### SSH AAA模式

拿R1举例，其他的路由器的配置方法也一样

```
[R1]stelnet server enable 
[R1]user-interface vty 3 4
[R1-ui-vty16-20]authentication-mode aaa
[R1-ui-vty16-20]protocol inbound ssh
[R1-ui-vty16-20]aaa
[R1-aaa]local-user r1 password cipher 123456
[R1-aaa]local-user r1 service-type ssh
[R1-aaa]local-user r1 level 15
[R1-aaa]quit
[R1]rsa local-key-pair create 
Confirm to replace them? (y/n)[n]:y
Input the bits in the modulus[default = 512]:1024
```

R1远程连接R2：

```
[R1]ssh client first-time enable
[R1]stelnet 192.168.12.2
```

<img src="https://pic.imgdb.cn/item/64e00f37661c6c8e54a81425.jpg" style="zoom:67%;" />

其他路由器也能相互使用ssh远程登录

### Cisco

#### 实现路由器之间的通信

R1

```
Router(config)#ip route 192.168.23.0 255.255.255.0 192.168.12.2
```

R3

```
Router(config)#ip route 192.168.12.0 255.255.255.0 192.168.23.2
```

#### telnet password模式

在每个路由器上配置telnet password模式：

```
en
configure terminal 
line vty 0 2
transport input telnet 
password 123456
login 
```

R1远程连接R3：

![](https://pic.imgdb.cn/item/64e03e29661c6c8e542c5e0b.jpg)

其他路由器也能相互使用telnet远程登录

#### SSH AAA模式

拿R3举例，其他的路由器的配置方法也一样

```
R3(config)#ip domain-name r3.com
R3(config)#crypto key generate rsa 
How many bits in the modulus [512]: 1024
R3(config)#username r3 privilege 15 password 123456
R3(config)#line vty 0 4
R3(config-line)#transport input all
R3(config-line)#login local
```

R1远程连接R3：

![](https://pic.imgdb.cn/item/64e0535e661c6c8e546ca3a8.jpg)

其他路由器也能相互使用ssh远程登录

​	

## 10.删除 R1 和 R3 上的静态路由条目，使用默认路由实现 PC1 与 PC2 之间的连通性

### eNSP

删除R1的静态路由

```
[R1]undo ip route-static 192.168.20.0 255.255.255.0 192.168.12.2
[R1]undo ip route-static 192.168.23.0 255.255.255.0 192.168.12.2
```

删除R3的静态路由

```
[R3]undo ip route-static 192.168.12.0 255.255.255.0 192.168.23.2
[R3]undo ip route-static 192.168.10.0 255.255.255.0 s4/0/0
```

R1配置默认路由

```
[R1]ip route-static 0.0.0.0 0.0.0.0 192.168.12.2
```

R3配置默认路由

```
[R3]ip route-static 0.0.0.0 0.0.0.0 192.168.23.2
```

<img src="https://pic.imgdb.cn/item/64e01722661c6c8e54b7c31b.jpg" style="zoom:67%;" />

### Cisco

删除R1的静态路由

```
R1(config)#no ip route 192.168.20.0 255.255.255.0
R1(config)#no ip route 192.168.23.0 255.255.255.0
```

删除R3的静态路由

```
[R3]undo ip route-static 192.168.12.0 255.255.255.0 192.168.23.2
[R3]undo ip route-static 192.168.10.0 255.255.255.0 s4/0/0
```

R1配置默认路由

```
R1(config)#ip route 0.0.0.0 0.0.0.0 192.168.12.2
```

R3配置默认路由

```
R3(config)#ip route 0.0.0.0 0.0.0.0 192.168.23.2
```

![](https://pic.imgdb.cn/item/64e056d4661c6c8e547744e4.jpg)


