# [极客大挑战 2019]Havefun


# [极客大挑战 2019]Havefun

初始样子：

![[极客大挑战 2019]Havefun-1](https://pic.imgdb.cn/item/653393b0c458853aefcb38ee.jpg)

没什么可以点的地方，查看页面源代码，发现了一些提示

![[极客大挑战 2019]Havefun-2](https://pic.imgdb.cn/item/653394cbc458853aefcf08a0.jpg)

```php
 $cat=$_GET['cat'];		//从URL的GET参数中获取名为"cat"的值，存在变量cat中
 echo $cat;				//输出变量cat
 if($cat=='dog'){		//如果变量cat的值为"dog"则输出"Syc{cat_cat_cat_cat}"
     echo 'Syc{cat_cat_cat_cat}';
 }
```

根据这个提示，在URL里给"cat"参数传个"dog"值：`?cat=dog`，得到flag

![[极客大挑战 2019]Havefun-3](https://pic.imgdb.cn/item/653397b4c458853aefd82a0d.jpg)
