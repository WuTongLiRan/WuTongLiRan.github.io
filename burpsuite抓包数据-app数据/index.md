# 

# BurpSuite抓包数据 & APP数据

## 一、电脑端抓包

### 1. 安装证书

#### 1.1 导出证书

![](https://pic.imgdb.cn/item/64c0db3d1ddac507ccf2a907.jpg)

![](https://pic.imgdb.cn/item/64c0dc051ddac507ccf4d19f.jpg)

​	

#### 1.2 导入证书

![](https://pic.imgdb.cn/item/64c0dcbc1ddac507ccf6cbee.jpg)

![](https://pic.imgdb.cn/item/64c0dcf31ddac507ccf72f31.jpg)

![](https://pic.imgdb.cn/item/64c0dd9a1ddac507ccf8742e.jpg)

​	

### 2. 设置代理

![](https://pic.imgdb.cn/item/64c104e41ddac507cc439fab.jpg)

### 3. 开始抓包

​	

​	

​	

## 二、手机端/手机模拟器 抓包

### 1. 安装证书

1. 把导出的`.der`证书文件的后缀改成cer
2. 将证书文件传到手机或手机模拟器里
3. 点击证书文件，`凭据用途`选择：`WLAN`
   <img src="https://pic.imgdb.cn/item/64c106841ddac507cc467671.jpg" style="zoom:67%;" />

​	

### 2. 开启电脑 Burp 监听

1. 查看电脑IP地址
   ![](https://pic.imgdb.cn/item/64c109bd1ddac507cc4bde34.jpg)
2. 启动BurpSuite，打开 Proxy——Options——Add，端口随意填写，绑定地址选择电脑 IP 地址
   ![](https://pic.imgdb.cn/item/64c10c4a1ddac507cc50d2b6.jpg)

​	

### 3. 配置手机 WiFi 代理

1. **手机与电脑相同WIFI**
2. 设置手机的WIFI代理
   ![](https://pic.imgdb.cn/item/64c10e0a1ddac507cc543ccc.jpg)
   填写BurpSuite监控的IP(电脑IP)和端口
   ![](https://pic.imgdb.cn/item/64c10e651ddac507cc5521e0.jpg)

​	

### 4. 开始抓包
