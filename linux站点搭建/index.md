# Linux站点搭建(更新中)


# Linux站点搭建

## 准备工作

> 确定两台主机为同一种联网方式（例如都为 NAT 模式）

1. 关闭虚拟机本身的DHCP功能
   <img src="https://pic.imgdb.cn/item/65280886c458853aeff55130.jpg" style="zoom: 67%;" />
2. 搭建本地源

③关闭防火墙

systemctl stop firewalld.service  //关闭防火墙 

systemctl disable firewalld.service  //永久关闭防火墙

④关闭selinux（安全策略）

setenforce 0 //临时关闭 

永久关闭：

cd /etc/selinux/config 

内容：

SELINUX=disabled
