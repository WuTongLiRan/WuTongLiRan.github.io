# Windows搭建HTTP站点

# Windows搭建HTTP站点

> 以我的服务器——Windows 2012 R2 为例

## 安装IIS

打开 `服务器管理器`。

点击 "`添加角色和功能`"。

在向导中，选择 `"基于角色或特性的安装"`。

选择你的服务器。

![](https://pic.imgdb.cn/item/65040bb9661c6c8e54760205.jpg)

在 "角色" 窗口中，勾选 `"Web 服务器（IIS）"`。

<img src="https://pic.imgdb.cn/item/65040c30661c6c8e547636af.jpg" style="zoom:80%;" />

完成向导，安装 IIS。

<img src="https://pic.imgdb.cn/item/65040cfb661c6c8e5476aa4a.jpg" style="zoom: 67%;" />

​	

## 配置HTTP站点

在 `"管理工具"` 中找到 IIS。

![](https://pic.imgdb.cn/item/650417f2661c6c8e547f1bcd.jpg)

填写必要的内容

<img src="https://pic.imgdb.cn/item/650422e1661c6c8e5487119b.jpg" style="zoom:67%;" />

真机访问 8.130.91.80

![](https://pic.imgdb.cn/item/650422c7661c6c8e5486ebbb.jpg)

​	

## 配置HTTPS站点

### 部署 CA 服务器

新开一台 Windows 2012 R2 作为 CA 服务器

安装`AD证书服务`和`DNS服务`

<img src="https://pic.imgdb.cn/item/65046599661c6c8e54b99585.jpg" style="zoom: 67%;" />

角色服务中 勾选 `证书颁发机构 Web 注册`

<img src="https://pic.imgdb.cn/item/65046612661c6c8e54ba4971.jpg" style="zoom:67%;" />

一路“下一步”直到安装完毕

如下图所示在服务器管理器的右上角点击 `配置目标服务器上的 Active Directory证书服务`

![](https://pic.imgdb.cn/item/65046812661c6c8e54baa075.jpg)



<img src="https://pic.imgdb.cn/item/65046960661c6c8e54bbfed9.jpg" style="zoom:67%;" />

一路“下一步”直到配置完成

​	

### 获取CA证书

打开 Web服务器的`IIS管理器`，点击`服务器证书`

<img src="https://pic.imgdb.cn/item/65046ade661c6c8e54bcd336.jpg" style="zoom:67%;" />

点击右侧的`创建证书申请`，然后填上必填内容，点击下一步

![](https://pic.imgdb.cn/item/65046b79661c6c8e54bd83c7.jpg)

位长选择2048

<img src="https://pic.imgdb.cn/item/65046be0661c6c8e54bdfa71.jpg" style="zoom: 67%;" />

保存到指定地方

<img src="https://pic.imgdb.cn/item/65046ddb661c6c8e54bf7b8a.jpg" style="zoom:67%;" />

复制其内容

<img src="https://pic.imgdb.cn/item/65046e38661c6c8e54bfe03a.jpg" style="zoom: 50%;" />

浏览器访问`http://47.242.79.14/certsrv/`，点击`申请证书`

![](https://pic.imgdb.cn/item/650470f5661c6c8e54c1ea00.jpg)

点击`高级证书申请`

<img src="https://pic.imgdb.cn/item/65047219661c6c8e54c27ce0.jpg" style="zoom:67%;" />

粘贴内容，点击提交

<img src="https://pic.imgdb.cn/item/6504727d661c6c8e54c29de6.jpg" style="zoom:67%;" />

打开CA服务器，打开`管理工具`中的`证书颁发机构`

![](https://pic.imgdb.cn/item/65047340661c6c8e54c2ca4b.jpg)

点击左侧`挂起的申请`，右键申请，在`所有任务`中点击`颁发`

![](https://pic.imgdb.cn/item/6504744e661c6c8e54c413bb.jpg)

回到浏览器，点击`查看挂起的证书申请状态`

<img src="https://pic.imgdb.cn/item/65047625661c6c8e54c4d59f.jpg" style="zoom:67%;" />

<img src="https://pic.imgdb.cn/item/65047652661c6c8e54c4e57b.jpg" style="zoom:67%;" />

下载证书

<img src="https://pic.imgdb.cn/item/6504768f661c6c8e54c52ce9.jpg" style="zoom:67%;" />

然后把证书文件放到Web服务器上（也可以在Web服务器的浏览器里访问这个链接来下载证书）

​	

### 搭建HTTPS站点

打开Web服务器的`IIS管理器`

点击`服务器证书`，点击右侧的`完成证书申请`

![](https://pic.imgdb.cn/item/650475df661c6c8e54c4ce9e.jpg)

<img src="https://pic.imgdb.cn/item/6504772b661c6c8e54c5fd42.jpg" style="zoom:67%;" />

像之前一样右键点击`添加网站`

<img src="https://pic.imgdb.cn/item/6504784a661c6c8e54c6b367.jpg" style="zoom:67%;" />



<img src="https://pic.imgdb.cn/item/650478e2661c6c8e54c71691.jpg" style="zoom:67%;" />

