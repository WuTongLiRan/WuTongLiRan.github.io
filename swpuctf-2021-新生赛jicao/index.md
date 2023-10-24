# [SWPUCTF 2021 新生赛]jicao


# [SWPUCTF 2021 新生赛]jicao

<img src="https://pic.imgdb.cn/item/653668a2c458853aefb8663b.jpg" alt="[SWPUCTF 2021 新生赛]jicao-1" width="80%;" />

```php
<?php
highlight_file('index.php');
include("flag.php");
$id=$_POST['id'];//通过POST给id传参
$json=json_decode($_GET['json'],true);//通过GET给json传参并把json格式解析成PHP数组格式，赋值给$json
if ($id=="wllmNB"&&$json['x']=="wllm")//id为"wllmNB"且json['x']为"wllm"
{echo $flag;}
?>
```

由以上代码可知，当 id 为 "wllmNB" 且 json['x'] 为 "wllm" 时才会输出 flag，即通过 GET 给 json传 {"x":"wllm"} 并通过 POST 给 id 传 "wllmNB"，借助 HackBar 插件来完成

![[SWPUCTF 2021 新生赛]jicao-2](https://pic.imgdb.cn/item/65366ae3c458853aefc01c33.jpg)

不知道为什么用 BurpSuite 没成功
