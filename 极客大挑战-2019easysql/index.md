# [极客大挑战 2019]EasySQL


# [极客大挑战 2019]EasySQL

初始样子：

![[极客大挑战 2019]EasySQL-1](https://pic.imgdb.cn/item/653361d5c458853aef1e1ea1.jpg)

题目为EasySQL，猜测为sql注入题

用户名和密码随便数个1，提示账户密码错误，看URL发现是GET型

![[极客大挑战 2019]EasySQL-2](https://pic.imgdb.cn/item/65336431c458853aef25bee2.jpg)

构造payload：`?username=1&password=1"`，页面无影响

![[极客大挑战 2019]EasySQL-3](https://pic.imgdb.cn/item/653364d2c458853aef27d598.jpg)

**构造payload：`?username=1&password=1'`，页面报错，说明`password`处存在注入点**

![[极客大挑战 2019]EasySQL-4](https://pic.imgdb.cn/item/65336838c458853aef338b69.jpg)

构造payload：`?username=1&password=1'--+`，页面报错

![[极客大挑战 2019]EasySQL-5](https://pic.imgdb.cn/item/6533688ec458853aef34b309.jpg)

**构造payload：`?username=1&password=1'%23`，页面正常(`%23`为注释符`#`的URL编码)，说明是字符型**

![[极客大挑战 2019]EasySQL-6](https://pic.imgdb.cn/item/6533691dc458853aef368c22.jpg)

想要得到flag需要绕过这登录，使用万能钥匙`or 1=1`会使得sql查询语句的where后面的语句直接为真

**构造payload：`?username=1&password=1'or 1=1%23`，绕过登录，得到flag**

![[极客大挑战 2019]EasySQL-7](https://pic.imgdb.cn/item/6533697ac458853aef383b71.jpg)
