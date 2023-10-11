# 

# 三层交换机互联实验

## 实验目的

本实验的目的是通过配置三层交换机实现两个不同 VLAN 中的设备互通。

​	

## 实验拓扑

![](https://pic.imgdb.cn/item/64b652391ddac507cc8f932c.jpg)

​	

## 实验步骤

1. 开启三层交换机的三层功能：进入全局配置模式，开启三层交换机的三层功能。

   ```
   Switch(config)# ip routing
   ```

2. 创建VLAN：在交换机上创建两个 VLAN ，分别为 VLAN 10 和 VLAN 20。

   ```
   Switch(config)# vlan 10                          // 创建 VLAN 10
   Switch(config)# vlan 20                          // 创建 VLAN 20
   ```

3. 配置接口和VLAN关联：将每个PC所连接的接口与相应的VLAN关联起来。

   ```
   Switch(config)# interface fastEthernet 0/1        // 进入接口 FastEthernet 0/1
   Switch(config-if)# switchport mode access         // 配置接口为 Access 模式
   Switch(config-if)# switchport access vlan 10      // 将接口划分到 VLAN 10
   Switch(config-if)# exit                           // 退出接口配置模式
   
   Switch(config)# interface fastEthernet 0/2        // 进入接口 FastEthernet 0/2
   Switch(config-if)# switchport mode access         // 配置接口为 Access 模式
   Switch(config-if)# switchport access vlan 20      // 将接口划分到 VLAN 20
   Switch(config-if)# exit                           // 退出接口配置模式
   ```

4. 配置三层接口：为交换机配置一个三层接口，并将其与两个VLAN进行关联。

   ```
   Switch(config)# interface vlan 10                 // 进入 VLAN 10 接口
   Switch(config-if)# ip address 192.168.0.1 255.255.255.0    // 配置 VLAN 10 接口的 IP 地址
   Switch(config-if)# no shutdown                    // 启用 VLAN 10 接口
   Switch(config-if)# exit                           // 退出 VLAN 10 接口配置模式
   
   Switch(config)# interface vlan 20                 // 进入 VLAN 20 接口
   Switch(config-if)# ip address 192.168.1.1 255.255.255.0    // 配置 VLAN 20 接口的 IP 地址
   Switch(config-if)# no shutdown                    // 启用 VLAN 20 接口
   Switch(config-if)# exit                           // 退出 VLAN 20 接口配置模式
   ```

5. ​	保存配置。

   ```
   Switch#write
   ```

​	

## 实验验证

完成上述配置后，您可以使用以下方法验证实验的成功：

1. 确保 PC 的 IP地址、子网掩码和默认网关正确配置，并且 PC 之间没有防火墙或 ACL 等阻止通信的限制。
2. 尝试从一个 VLAN 中的 PC Ping 另一个 VLAN 中的PC，验证它们之间的互通性。
3. 在交换机上检查 VLAN 接口和端口的状态，确保它们正常工作。

​	

## 结论

通过配置三层交换机实现两个不同 VLAN 中的设备互通，建立了 VLAN 间的互联。这种设置允许两个 VLAN 中的设备之间进行通信，扩展了网络的灵活性和功能。
