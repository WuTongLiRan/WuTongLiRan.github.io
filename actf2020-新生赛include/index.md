# [ACTF2020 新生赛]Include


# [ACTF2020 新生赛]Include

初次访问：

![[ACTF2020 新生赛]Include-1](https://pic.imgdb.cn/item/65339878c458853aefda8840.jpg)

页面上就一个链接，点进去

![[ACTF2020 新生赛]Include-2](https://pic.imgdb.cn/item/65339939c458853aefdcfb6f.jpg)

点进链接后页面上就一句毫无鸟用的话，查看页面源码也没有其他的东西

![[ACTF2020 新生赛]Include-3](https://pic.imgdb.cn/item/65339e92c458853aefed23a0.jpg)

也就是说前端确确实实就只输出了这一句话

再看URL，发现是用`file`伪协议包含`flag.php`这个文件来输出的，看这文件的名字应该就是含有flag的文件，为什么页面上没有显示？

有一种可能是flag{xxxx}被放在`flag.php`的注释里，当php执行的时候会跳过注释，所以可以通过`php://filter`这个伪协议将`flag.php`里的源码以其他形式输出，这样子注释就不再因为按php的正常执行而被跳过

> [PHP支持的协议和封装协议](https://www.php.net/manual/zh/wrappers.php.php)
>
> `php://filter` 是 PHP 中用于数据流过滤和操作的伪协议。它允许你在读取和写入数据流时应用各种过滤器来修改、处理或转换数据。
>
> `php://filter` 的一般语法如下：
>
> ```
> php://filter/[read/write]=<过滤器>/resource=<要过滤的数据流>
> ```
>
> - `php://filter` ：固定格式
> - `[read/write]`：读取数据流 / 写入数据流
> - `<过滤器>`：将数据流怎么处理，例如`convert.base64-encode`是将数据流转化成Base64编码
>
> [PHP可用过滤器列表](https://www.php.net/manual/zh/filters.php)

**构造payload：`?file=php://filter/read=convert.base64-encode/resource=flag.php`**

意思是用`php://filter`这个伪协议读取(`read`) 服务器上的 `flag.php` 文件的内容并以Base64编码的形式(`convert.base64-encode`)返回

这样会使`flag.php`里的注释不再因为按php的正常执行被跳过，而会以Base64编码的形式显现出来

![[ACTF2020 新生赛]Include-4](https://pic.imgdb.cn/item/6533a787c458853aef08b467.jpg)

最后这串Base64编码的数据解码得到flag

```php
<?php
echo "Can you find out the flag?";
//flag{a60bbd47-fc37-42aa-b7e0-e01caed4ad9c}
```

​	

​	

**参考文章**

- [[ACTF2020 新生赛]Include 1](https://blog.csdn.net/weixin_45642610/article/details/112427044)
- [PHP支持的协议和封装协议](https://www.php.net/manual/zh/wrappers.php.php)
- [PHP可用过滤器列表](https://www.php.net/manual/zh/filters.php)


