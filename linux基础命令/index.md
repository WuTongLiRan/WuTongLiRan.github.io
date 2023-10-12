# Linux基础命令


# Linux基础命令

常用服务器架构：

- LAMP：Linux+Apache+mysql+PHP

- LNMP：Linux+Nginx+mysql+PHP

​	

## 基础命令

`[wpl@localhost ~]$`

- [当前用户@主机名    当前所在的位置]权限

- `~`：用户家目录

  - 普通用户的家目录在`/home`目录下

  - root的家目录在`/root`下

- `\`:根目录，所有的文件和目录都是置于根目录下，也就是从根目录出发

- `$`:普通权限

- `#`：管理员权限

​	

`su root`//切换到管理员用户

`hostname`  //查看主机名

修改主机名：

- `hostname 用户名`         //临时修改主机名 

- 永久修改主机名：去`/etc/hostname `修改配置

  ​	

`reboot`  //重启

`poweroff` //关机

`cd`  //切换目录

`pwd` //列出当前目录所在的路径

​	

**查询命令：**

`ls` //查看当前目录下的内容

`ls -l`=`ll` //详细的查看当前目录下的内容 

`cat` //查看文件的内容

`history` //查看历史命令

​	

**创建文件、目录:**

`touch 文件名`         //创建空文件

`echo 内容    > 文件名`    //创建有内容的文件 vim文本编辑器创建有内容的文件

`mkdir 目录名`    //创建目录

`mkdir -p 1/2/3` //创建多级目录

**删除文件、目录:**

`rm 文件名`        //删除文件 

`rm -f 文件名`         //强制删除文件 

`rm -r 目录名`         //删除目录 

`rm -rf 目录名`    //强制删除目录

**重命名、复制:**

`mv 旧文件名    新文件名`         //移动并重命名文件

`cp 1.txt 2.txt`     //复制1.txt并重命名为2.txt



​		

**SSH:**

`systemctl start sshd`  //开启ssh服务 

`systemctl status sshd`  //查看ssh服务的状态 

`systemctl stop sshd`    //停止ssh服务 

`systemctl enable sshd`  //开机自启ssh服务 

`systemctl disable sshd` //永久关闭ssh服务

**安装ssh服务**

1. `yum install openssh-server` --- 下载ssh服务

2. `vi /etc/ssh/sshd_config` --- 使用vi编辑SSH的配置文件 对ssh配置文件进行修改

​	

**设置自动登录root:**

1. `vim /etc/gdm/custom.conf`

2. ```
   [daemon]
   AutomaticLoginEnable=True  //开启自动登录功能 
   AutomaticLogin=root        //选择自动登录的用户 
   ```

   

3. `reboot`重启

