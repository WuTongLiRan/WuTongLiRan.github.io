# 

# 访问控制列表配置实验—ACL协议

## 实验目的

本实验的目的是给原本能互通的两台设备配置ACL，使其不能互通。

​	

## 实验拓扑

![](https://pic.imgdb.cn/item/64ba35d51ddac507cc4fd3c5.jpg)

​	

## 实验步骤

1. 配置两台PC的基本信息。

2. 配置路由器的两个接口信息(记得启动接口)。

3. 这个时候两台PC已经能互通了，接下来是配置ACL使其不能互通：

   ```
   //方法一：
   Router>en                                          // 进入特权模式
   Router#configure terminal                          // 进入全局配置模式
   
   Router(config)#access-list 2 deny 192.168.2.0 0.0.0.255  // 创建访问控制列表：2，拒绝来自 192.168.2.0/24 网段的流量，0.0.0.255是反掩码
   //因为互通需要两边互相发送和接受数据，所以随便拒绝一方的流量就能达到断绝通信的效果
   
   Router(config)#int f0/0                            // 进入 FastEthernet 0/0 接口配置模式
   //随便进入一个接口应用ACL，道理同上
   
   Router(config-if)#ip access-group 2 in             // 将 ACL 2 应用于入方向（即流入接口）的流量
   //in和out都可以，道理同上
   
   Router(config-if)#end                              // 退出接口配置模式
   Router#write                                      // 保存配置
   
   
   
   //方法二：
   Router>en                                     // 进入特权模式
   Router#configure terminal                     // 进入全局配置模式
   Router(config)#ip access-list standard wangduan2  // 创建一个名为 "wangduan2" 的标准访问控制列表（ACL）
   Router(config-std-nacl)#deny 192.168.2.0 0.0.0.255  // 在 "wangduan2" ACL 中添加拒绝条目，拒绝来自 192.168.2.0/24 网段的流量
   Router(config-std-nacl)#permit any            // 在 "wangduan2" ACL 中添加允许条目，允许其他所有流量通过
   Router#write                                  // 保存配置
   ```

   

