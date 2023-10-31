# [NISACTF 2022]easyssrf


# [NISACTF 2022]easyssrf

![[NISACTF 2022]easyssrf-1](https://pic.imgdb.cn/item/653bd706c458853aef4dcdd4.jpg)

题目已经告知利用 SSRF 漏洞，输入 file:///etc/passwd ，失败，提示换个路径

![[NISACTF 2022]easyssrf-2](https://pic.imgdb.cn/item/653bd789c458853aef526797.jpg)

输入 file:///flag ，得到提示 fl4g

![[NISACTF 2022]easyssrf-3](https://pic.imgdb.cn/item/653bd7bfc458853aef541b3c.jpg)

输入 file:///fl4g ，得到提示 ha1x1ux1u.php

![[NISACTF 2022]easyssrf-4](https://pic.imgdb.cn/item/653bd80bc458853aef56b33b.jpg)

访问 ha1x1ux1u.php

![[NISACTF 2022]easyssrf-5](https://pic.imgdb.cn/item/653bd831c458853aef57c5fa.jpg)

```php
<?php
highlight_file(__FILE__);
error_reporting(0);

// 从GET请求参数中获取名为 "file" 的值
$file = $_GET["file"];

// 检查 "file" 参数中是否包含字符串 "file"
if (stristr($file, "file")) {
  die("你败了.");
}

// 试图读取文件的内容并将其输出
echo file_get_contents($file);
?>
```

payload：`?file=../../../flag`

![[NISACTF 2022]easyssrf-6](https://pic.imgdb.cn/item/653bd967c458853aef5e33fe.jpg)

> **无基础可看：**[SSRF漏洞原理攻击与防御(超详细总结)](https://blog.csdn.net/qq_43378996/article/details/124050308)
