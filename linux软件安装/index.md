# Linux软件安装

# Linux软件安装

## Windows软件安装流程

- 安装检查
- 释放文件
- 复制可执行文件
- DLL动态链接库/安装服务
- 注册表
- 开始菜单和快捷方式

![](https://pic.imgdb.cn/item/64a6ae751ddac507cc5e0a51.jpg)![](https://pic.imgdb.cn/item/64a6ae891ddac507cc5e5943.jpg)

## Linux可执行程序  

- /bin /sbin 
- /usr/bin 
- /usr/sbin  

## 脚本和程序的区别

**脚本：**

- 不需要编译的：Javascript、Python、Ruby……
- 解释型：边解释边执行

**程序：**

- 需要编译的：C、C++、Swift、Kotlin、Go……
- 编译型：计算机可以直接执行

## Linux软件常见安装方式  

| 主要派系       | Linux发行版              | 主要安装方式        |
| -------------- | ------------------------ | ------------------- |
| Redhat红帽派系 | Redhat、CentOS、Fedora等 | make、rpm、yum、dnf |
| Debian派系     | Kali、Ubuntu等           | deb、apt、dpkg      |
| FreeBSD派系    | FreeBSD                  | make、pkg、ports    |

## 源码安装

![](https://pic.imgdb.cn/item/64a6b04f1ddac507cc64d65c.jpg)

## Redhat红帽派系

### rpm安装（RedHat Package Manager）

![](https://pic.imgdb.cn/item/64a6b0891ddac507cc655870.jpg)

| 操作 | 命令                   | 说明                                |
| ---- | ---------------------- | ----------------------------------- |
| 查询 | `rpm -qa rpm -q 包名 ` | `q`：query                          |
| 安装 | `rpm -ivh 包名`        | `i`：install `v`：verbose `h`：hash |
| 升级 | `rpm -Uvh 包名 `       | `U`：安装或升级最新版               |
| 卸载 | `rpm -e 包名 `         | 需要先卸载依赖其的软件              |

### yum安装（Yellow dog Updater, Modified）

| 操作         | 命令                     |
| ------------ | ------------------------ |
| 列表         | `yum list yum list 包名` |
| 搜索         | `yum search 包名`        |
| 安装         | `yum install 包名`       |
| 升级         | `yum update 包名`        |
| 卸载         | `yum remove 包名`        |
| 更新所有软件 | `yum update`             |
| 清除缓存     | `yum clean all`          |
| 更新yum缓存  | `yum make cache`         |

| 选项  | 含义                    |
| ----- | ----------------------- |
| `-h`  | 显示帮助信息            |
| `-y`  | 对所有的提问都回答“yes" |
| `-c ` | 指定配置文件            |
| `-q ` | 安静模式                |
| `-v ` | 详细模式                |

### DNF安装（Dandified YUM）

| 区别         | DNF                                         | YUM                           |
| ------------ | ------------------------------------------- | ----------------------------- |
| 解析依赖关系 | 使用Libsolv                                 | 使用公开的API                 |
| API          | 有完整的API文档，能很容易地创建新功能       | 没有完整文档，创建新功能困难  |
| 开发语言     | C、C++、Python编写                          | 只用Ptyhon编写                |
| 使用范围     | Fedora、RHEL 8、CentOS 8、OEL 8、Mageia 6/7 | RHEL 6/7、CentOS 6/7、OEL 6/7 |
| 扩展的支持   | 支持各种扩展                                | 只支持基于Python的扩展        |
| 同步元数据   | 占用内存少                                  | 占用较多内存                  |
| 更新         | 包中包含不相关的依赖，则不会更新            | 在没有验证的情况下更新软件包  |
| 存储库不可用 | DNF将跳过它，并继续使用可用的存储库处理事务 | YUM会立即停止                 |
| 内核包的保护 | DNF不提供，可以删除内核包                   | 不允许你删除运行中的内核      |

## Debian派系

### apt安装  

| 操作 | 命令             |
| ---- | ---------------- |
| 搜索 | apt search 包名  |
| 安装 | apt install 包名 |
| 升级 | apt update 包名  |
| 卸载 | apt remove 包名  |

## FreeBSD派系  

### pkg安装（Package）

| 操作 | 命令             |
| ---- | ---------------- |
| 搜索 | pkg search 包名  |
| 安装 | pkg install 包名 |
| 升级 | pkg upgrade 包名 |
| 卸载 | pkg del 包名     |

## 软件版本管理

### update-alternatives

查看：

```
update-alternatives --display java
```

切换：

```
update-alternatives --config java
```

添加：

```
alternatives --install /usr/bin/java java 

/usr/local/jdk-11.0.2/bin/java

3

/usr/bin/java：注册地址，软链

java：服务名

/usr/local/jdk-11.0.2/bin/java：实际程序路径

3：优先级
```


