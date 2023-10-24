# [SWPUCTF 2021 新生赛]easy_md5


# [SWPUCTF 2021 新生赛]easy_md5

![[SWPUCTF 2021 新生赛]easy_md5-1](https://pic.imgdb.cn/item/653672c3c458853aefdea9d2.jpg)

**完整的代码注释放在文末**

来看关键代码块

```php
if ($name != $password && md5($name) == md5($password)){
        echo $flag;
    } 
```

要满足两个字符串不相等、但经过md5加密后的结果是相同的，才会输出flag

正常情况下想找到两个不同的字符串，且通过MD5处理后的结果相同，几乎是不可能的

所以只能通过php中 == 的特性来解决

PHP中==是判断值是否相等，若两个变量的类型不相等，则会转化为相同类型后再进行比较。PHP在处理哈希字符串的时候，会把**以0e开头并且后面字符均为纯数字的哈希值**都解析为0

MD5加密后以0E开头的字符串：

- QNKCDZO
- 240610708
- s878926199a
- s155964671a

所以只要随便选以上两个字符串，一个通过GET传给name、一个通过POST传给password，即可得到flag。可以借助HackBar来完成

![[SWPUCTF 2021 新生赛]easy_md5-2](https://pic.imgdb.cn/item/653677f4c458853aeff6c626.jpg)



---
**完整代码注释：**

```php
<?php
// 显示当前 PHP 文件的源代码
highlight_file(__FILE__);

// 包含一个名为 'flag2.php' 的文件
include 'flag2.php';

// 检查是否设置了 'name' 和 'password' 参数
if (isset($_GET['name']) && isset($_POST['password'])) {
    // 从 GET 请求中获取 'name' 参数并从 POST 请求中获取 'password' 参数
    $name = $_GET['name'];
    $password = $_POST['password'];

    // 检查以下条件：
    // 1. 'name' 和 'password' 不相等（$name != $password）
    // 2. 'name' 和 'password' 经过 MD5 散列后的结果相同（md5($name) == md5($password)）
    if ($name != $password && md5($name) == md5($password)) {
        // 如果条件成立，输出 'flag'，这意味着 'flag2.php' 中的内容将被显示
        echo $flag;
    } else {
        // 如果条件不成立，输出 "wrong!"
        echo "wrong!";
    }
} else {
    // 如果未设置 'name' 和 'password' 参数，输出 "wrong!"
    echo 'wrong!';
}
?>
```

​	

​	

**参考文章**

[PHP中MD5常见绕过](https://blog.csdn.net/iczfy585/article/details/106081299)
