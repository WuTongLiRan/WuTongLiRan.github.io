# 

# Xshell远程连接Kali

> 虚拟机的复制/粘贴的快捷键是：`Ctrl+shift+C` / `Ctrl+shift+V`

​	

## 设置root用户密码

没设置过root用户密码的先设置密码，设置过则跳过这一步。

```bash
sudo passwd root
```

> 输入密码时是无回显的

​	

## 配置远程连接

进入root用户：

```bash
sudo su
```

编辑ssh配置文件：

```
vim /etc/ss h/ss hd_config
```

![](https://pic.imgdb.cn/item/64ce28b81ddac507cc920e76.jpg)

找到`#PermitRootLogin prohibit-password`，可以通过按`/`、输入`PermitRootLogin`来查找，找到后按回车定位到那一行

按`i`进入编辑模式，将`#PermitRootLogin prohibit-password`改成`PermitRootLogin yes`，也可以直接添加`PermitRootLogin yes`

![](https://pic.imgdb.cn/item/64ce29da1ddac507cc94a45a.jpg)

找到`#PasswordAuthentication yes`,可以按`Esc`退出编辑模式，按`/`、输入`PasswordAuthentication`来查找，找到后按回车定位到那一行

按`i`进入编辑模式，将`#`删除

按`Esc`退出编辑模式，输入`:wq`保存退出

> 保存退出：`:wq`
>
> 直接退出：`:q!`

​	

## 启动SSH服务

```
service  ssh  start
```

​	

## 查看IP

```
ifconfig
```

![](https://pic.imgdb.cn/item/64ce2c871ddac507cc9b7924.jpg)

​	

## Xshell远程连接

新建会话

![](https://pic.imgdb.cn/item/64ce2d151ddac507cc9cff93.jpg)

![](https://pic.imgdb.cn/item/64ce2d441ddac507cc9d9c7e.jpg)

<img src="https://pic.imgdb.cn/item/64ce2d6c1ddac507cc9e2855.jpg" style="zoom:67%;" />
