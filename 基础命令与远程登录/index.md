# 基础命令与远程登录

# 基础命令与远程登录

## 本地登录---利用console口

1. 需要一根console线

2. console线一端连接自己的电脑，另一端连接设备上的console口

   > console口：专门用来管理设备的接口，并不传输数据  ---  管理口

3. 登录软件：SecureCRT、putty、Xshell

4. 登录协议：serial协议

   > serial：串口通讯协议，主从双方必须一致，才可以有效的传输

5. com口：去设备管理器上查看

6. 波特率：一般选择9600

​	

## Cisco模拟器

### 交换机的命令配置

#### 命令模式

- 一般用户配置模式(简称**用户模式**)
  > 什么也干不了

- 特权用户配置模式(简称**特权模式**)

- 全局配置模式

- 接口配置模式

- VLAN配置模式

- 线路配置模式

#### 命令模式相互关系

##### 图片1
<img src="https://pic.imgdb.cn/item/64b207c51ddac507cc26a12d.jpg" style="zoom:80%;" />

##### 图片2
<img src="https://pic.imgdb.cn/item/64b2080e1ddac507cc27c622.jpg" style="zoom:80%;" />

#### 交换机模式切换

```
Switch>enable		//切换到特权模式

Switch#config terminal		//切换到全局配置模式

Switch(config)#interface f0/1		//进入接口f0/1

Switch(config-if)#no shutdown		//激活接口f0/1
```

​	

### 将交换机的1口设置成 VLAN 10 口子

#### 1. 进入全局配置模式

```
Switch>enable	//切换到特权模式

Switch#config	//切换到全局配置模式

Switch(config)#
```

#### 2. 创建VLAN 10 后退出

```
Switch(config)#vlan 10	//创建VLAN 10

Switch(config-vlan)#exit	//退出VLAN模式

Switch(config)#
```

#### 3. 进入接口模式

```
Switch(config)#interface fastEthernet 0/1	//进入1口

Switch(config-if)#
```

#### 4. 配置交换机为VLAN 10

```
Switch(config-if)#switchport access  vlan 10	//将1口划为VLAN 10

Switch(config-if)#no shutdown	//启用(打开)接口
```

#### 5. 特权模式下查看配置

```
Switch(config-if)#exit	//退出1口的配置模式

Switch(config)#exit	//退出配置模式

Switch#show run	//显示当前交换机的运行配置
```

​	

## eNSP模拟器

### 交换机/路由器的命令配置

#### 命令模式

- 用户模式
- 全局模式

> 比Cisco设备少一种

```
<Huawei>	//用户模式
<Huawei>system-view          //进入全局模式
[Huawei]	//全局模式
```

#### 常用命令

```
[Huawei]sysname R1       //修改设备名称
[R1]int g0/0/0            //进入g0/0/0接口
[R1]display  current-configuration      //查看设备的所有配置信息
[R1]quit           //退回到上一级
<R1>save              //保存配置  华为一定要在用户模式下才可以保存
[R1]dis ip routing-table           //查看路由表
[R1-GigabitEthernet0/0/0]display this        //查看当前模式下的配置
[R1]dis ip interface brief              //查看接口ip信息
[R1]dis interface brief             //查看接口信息
```

​	

#### 设备安全加固

##### password模式

```
<r2>system-view              // 切换至全局配置模式
[r2]user-interface console 0     // 进入控制台接口 0
[r2-ui-console0]authentication-mode password    // 配置密码认证方式
Please set the login password (maximum length 16): 123    // 设置登录密码
```

##### AAA模式

```
<r2>system-view            // 切换至全局配置模式
[r2]user-interface console 0       // 进入控制台接口 0
[r2-ui-console0]authentication-mode aaa    // 设置 AAA 认证方式
[r2]aaa         // 进入 AAA 配置模式
[r2-aaa]local-user qq password cipher 123456    // 创建本地用户 qq，密码为 123456
[r2-aaa]local-user qq service-type terminal     // 配置用户 qq 为终端登录类型
[r2-aaa]local-user qq level 15       // 为用户 qq 设置权限等级，一定要在设备安全加固时使用 AAA 模式时给用户设置适当等级
```

​	

#### 配置Telnet

##### password模式

```
[r2]telnet server enable           // 启用 Telnet 服务
[r2]user-interface vty 0 4           // 进入 vty 虚拟链路
[r2-ui-vty0-4]authentication-mode password           // 配置身份认证模式为 password
Please configure the login password (maximum length 16): 456           // 配置登录密码
[r2-ui-vty0-4]protocol inbound telnet           // 允许使用 Telnet 进行远程登录
[r2-ui-vty0-4]user privilege level 15           // 设置登录后的管理级别为 15
```

#####  AAA方式

```
[r2-ui-vty0-4]authentication-mode aaa           // 配置身份认证模式为 aaa
[r2-ui-vty0-4]protocol inbound telnet           // 允许使用 Telnet 进行远程登录
[r2]aaa           // 进入 aaa 模式，创建本地用户
[r2-aaa]local-user r2 password cipher 123456           // 创建本地用户的用户名和密码
[r2-aaa]local-user r2 service-type telnet           // 选择本地用户所服务的协议类型
[r2-aaa]local-user r2 level 15           // 设备本地用户等级为 15
```

​	

#### 配置SSH

通过 SSH 方式（使用 22 号端口）进行远程登录。首先需启用 SSH 服务和创建 RSA 密钥对。

```
[R1]stelnet server enable           // 启用 SSH 服务
[R1]user-interface vty 0 4           // 进入 vty 虚拟接口
[R1-ui-vty0-4]authentication-mode aaa           // 配置身份认证模式为 aaa
[R1-ui-vty0-4]protocol inbound ssh           // 设置该虚拟链路给 SSH 使用
[R1]aaa           // 进入 aaa 创建本地用户
[R1-aaa]local-user r1 password cipher 123456           // 创建本地用户的用户名和密码
[R1-aaa]local-user r1 service-type ssh           // 选择本地用户所服务的协议类型为 SSH
[R1-aaa]local-user r1 level 15           // 设备本地用户等级为 15
[R1]rsa local-key-pair create           // 创建加密算法 RSA，生成公钥和私钥
Confirm to replace them? (y/n)[n]:y
Input the bits in the modulus[default = 512]:1024
```

#### SSH 客户端登录

使用 SSH 客户端可以通过 SSH 协议进行远程登录。

```
[r2]ssh client first-time enable           // SSH 客户端首次启用
[r2]stelnet IP地址           // 登录该 IP 地址下的设备
```


