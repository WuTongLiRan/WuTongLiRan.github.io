# Shell脚本入门


# shell脚本入门

## hello world!

```shell
#!/bin/sh
a="hello world!"
echo "a is : $a"
```

运行结果：

```shell
a is : hello world!
```

`#！`指定要使用的shell

- `/bin/sh`执行过程中，若出现命令执行失败，则立即停止执行 
- `/bin/bash`执行过程中，若出现命令执行失败，仍会继续执行 
- 若不指定命令解释器，系统会默认使用`/bin/bash`

​	

## 脚本修改静态IP

```shell
#!/bin/bash
cd /etc/sysconfig/network-scripts/
echo "TYPE=Ethernet" > ifcfg-ens33
echo "BOOTPROTO=static" >> ifcfg-ens33
echo "IPADDR=192.168.1.100" >> ifcfg-ens33
echo "NETMASK=255.255.255.0" >> ifcfg-ens33
echo "GATEWAY=192.168.1.254" >> ifcfg-ens33
echo "DNS1=218.85.157.99" >> ifcfg-ens33
echo "DNS2=8.8.8.8" >> ifcfg-ens33
echo "DEFROUTE=yes" >> ifcfg-ens33
echo "NAME=ens33" >> ifcfg-ens33
echo "DEVICE=ens33" >> ifcfg-ens33
echo "ONBOOT=yes" >> ifcfg-ens33
systemctl restart network #重新启动网络服务
```

> ps:
>
> ```shell
> ifdown ens33 //关闭ens33网卡 
> ifup ens33   //开启ens33网卡
> ```

​	

## shell变量

- 命名只能使用英文字母，数字和下划线，首个字符不能以数字开头。
- 中间不能有空格，可以使用下划线`_`。
- 不能使用标点符号。不能使用bash里的关键字（可用help命令查看保留关键字）。

```shell
A=123 
echo $A
```

​	

## read

与Linux交互，可以通过脚本获取键盘输入的结果

- `-n`：限制读取N个字符就自动结束读取，如果没有读满N个字符就按下回车或遇到换行符，则也会结束读取。 
- `-N`：严格要求读满N个字符才自动结束读取，即使中途按下了回车或遇到了换行符也不结束。换行符或回车算一个字符。
- **`-p`：给出提示符。默认不支持`\n`换行，要换行需要特殊处理，见下文示例。**
  **例如，`-p "请输入密码："`**
- `-r`：禁止反斜线的转义功能。这意味着"\"会变成文本的一部分。
- `-s`：静默模式。输入的内容不会回显在屏幕上。

​	

## 交互式修改静态IP

```shell
#!/bin/bash
cd /etc/sysconfig/network-scripts/
read -p "IPADDR:" ip
read -p "GATEWAY:" gate
echo "TYPE=Ethernet" > ifcfg-ens33
echo "BOOTPROTO=static" >> ifcfg-ens33
echo "IPADDR=$ip" >> ifcfg-ens33
echo "NETMASK=255.255.255.0" >> ifcfg-ens33
echo "GATEWAY=$gate" >> ifcfg-ens33
echo "DNS1=218.85.157.99" >> ifcfg-ens33
echo "DNS2=8.8.8.8" >> ifcfg-ens33
echo "DEFROUTE=yes" >> ifcfg-ens33
echo "NAME=ens33" >> ifcfg-ens33
echo "DEVICE=ens33" >> ifcfg-ens33
echo "ONBOOT=yes" >> ifcfg-ens33
systemctl restart network
```

​	

## if

判断语句

```bash
#!/bin/bash

# 请用户输入一个数字
echo "请输入一个整数:"
read number

# 使用if语句判断输入的数字是否为正数、负数或零
if [ $number -gt 0 ]; then	# $ 前和 0 后的空格是必须的！！！
    echo "这是一个正数"
elif [ $number -lt 0 ]; then
    echo "这是一个负数"
else
    echo "这是零"
fi #if结束
```

- `-gt`（greater than）    大于
- `-lt`（less than）    小于
- `-eq`（equal）    等于
- `-ne`（not equal）    不等于
- `-le`（less than or equal）    小于等于 
- `-ge`（greater than or equal）大于等于

​	

## Linux下两个特殊文件

- /dev/zero:空字符填充设备
- /dev/null：黑洞文件（“位桶”）
  - 通常用于**丢弃数据**或将数据重定向到一个虚拟的黑洞，也就是什么都不做。这个设备文件不会存储数据，而是会立即丢弃所有写入它的内容。这在很多情况下非常有用，特别是当你**不想保留输出或不关心输出时**。

​	

## 存活主机探测-ping

```shell
#!/bin/bash

if ping -c2 -i0.5 -W2 192.168.10.2 > /dev/null; then
  echo "yes"
else
  echo "no"
fi
```

​	

## 循环语句

### for

```shell
#!/bin/bash

# 使用for循环遍历数字范围 1 到 5
for number in {1..5}
do
    echo "当前数字是: $number"
done
```

运行结果：

```shell
当前数字是: 1
当前数字是: 2
当前数字是: 3
当前数字是: 4
当前数字是: 5
```

```shell
#!/bin/bash
for ip in {1..254}
do
if ping -c2 -i0.5 -W2 192.168.10.$ip > /dev/null then
  echo "192.168.10.$ip is alive"
fi
done
```

​	

### while

```shell
#!/bin/bash

ip=0

while [ $ip -lt 254 ]
do
    let ip++	#递增 ip 变量的值
    
    if ping -c2 -i0.5 -W2 192.168.10.$ip > /dev/null; then
        echo "192.168.10.$ip is alive"
    fi
done
```

​	

## case-选择语句

```shell
#!/bin/bash

case $1 in
    1)
        echo 1
        ;;
    2)
        echo 2
        ;;
    3)
        echo 3
        ;;
    *)
        echo none
        ;;
esac
```

运行结果：

```shell
[root@os43 Desktop]# ./case.sh 2	# case.sh为$0  2为$1
2
[root@os43 Desktop]# ./case.sh 7 
none
```

​	

**Linux的内置变量**

- **$0**：表示当前脚本的名称或命令。
- **\$1,\$2, ... $n**：表示脚本或命令的参数。例如，\$1表示第一个参数，$2表示第二个参数，以此类推。


