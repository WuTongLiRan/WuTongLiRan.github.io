# 动态路由配置实验—RIP协议 & OSPF协议

# 动态路由配置实验—RIP协议 & OSPF协议

​	

## 实验拓扑

![](https://pic.imgdb.cn/item/64b7d85d1ddac507cc570d6e.jpg)

​	

## 实验步骤

1. 配置每台PC的**IP、子网掩码、网关。**

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
   
   
   Router#write                     // 保存配置
   ```


4. 路由器配置(RIP协议)：

   ```
   //以Router1的配置为例，其他的Router配置方法一样(直连网段看拓扑图)
   
   Router>enable                                          // 进入特权模式
   Router#configure terminal                              // 进入全局配置模式
   Router(config)#router rip                              // 进入 RIP 路由器配置模式
   Router(config-router)#version 2                        // 配置 RIP 版本为 2
   Router(config-router)#no auto-summary                  // 禁用自动汇总
   
   //写入直连网段,会自己补全掩码(不同Router的直连网段不同，配置时做对应修改)
   Router(config-router)#network 192.168.5.0             // 配置 RIP 路由器接口 192.168.5.0/24
   Router(config-router)#network 192.168.1.0             // 配置 RIP 路由器接口 192.168.1.0/24
   
   Router(config-router)#end                              // 退出 RIP 路由器配置模式
   Router#write                                          // 保存配置到启动配置
   ```

5. 路由器配置(OSPF协议)：

   ```
   //以Router1的配置为例，其他的Router配置方法一样(直连网段看拓扑图)
   
   Router>enable                  // 进入特权模式
   Router#configure terminal      // 进入全局配置模式
   Router(config)#router ospf 1   // 开始配置 OSPF 进程 1(每个局域网的进程数要一样)
   
   //写入直连网段、掩码、区域(不同Router的直连网段不同，配置时做对应修改)
   Router(config-router)#network 192.168.1.0 255.255.255.0 area 0   // 将 192.168.1.0/24 网络加入到 OSPF 进程 1 的区域 0(一般是选择骨干区域:0)
   Router(config-router)#network 192.168.5.0 255.255.255.0 area 0   // 将 192.168.5.0/24 网络加入到 OSPF 进程 1 的区域 0
   
   Router(config-router)#end                       // 退出 OSPF 配置模式
   Router#write                                    // 保存配置
   ```

​	

## 实验验证

测试PC0能不能联通PC5：

<img src="https://pic.imgdb.cn/item/64b9096d1ddac507cce14779.jpg" style="zoom:67%;" />

​	

## 附件

- [动态路由—RIP.pkt (记得重命名为.pkt)](https://wutongliran.lanzoum.com/iHCkc1315p0b)

- [动态路由—OSPF.pkt (记得重命名为.pkt)](https://wutongliran.lanzoum.com/i9Jxo1317ygd)
