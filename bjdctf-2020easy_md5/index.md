# [BJDCTF 2020]easy_md5


# [BJDCTF 2020]easy_md5

![[BJDCTF 2020]easy_md5-1](https://pic.imgdb.cn/item/653ce6d2c458853aef598f02.jpg)

抓响应包，看到一个 hint ：`select * from 'admin' where password=md5($pass,true)`

> 这里的提示不够准确，最准确的应该是 select * from 'admin' where password='md5($pass,true)'
>
> (应该是这样)

![[BJDCTF 2020]easy_md5-2](https://pic.imgdb.cn/item/653d2062c458853aefca9b6a.jpg)

不管输入什么都会经过 MD5 加密，所以要 sql 注入的话得保证输入的东西经 MD5 加密后是： `'or '真`，这样才能构成万能钥匙，即 `password=''or'真'`

可以写一段脚本去跑，看哪个字符串经过 MD5 加密后符合这个格式的，这里就借用其他大佬的payload：ffifdyop

ffifdyop 经 MD5 加密后是 `'or'6ɝ驡r,魢`，刚好可以作为万能钥匙

![[BJDCTF 2020]easy_md5-3](https://pic.imgdb.cn/item/653d390cc458853aef8392de.jpg)

> 在 MySQL 中，字符串前面有数字会按数字的真假来算，例如 6xxxxxxx 为真
>
> **相关实验：**
>
> ![[BJDCTF 2020]easy_md5-4](https://pic.imgdb.cn/item/653d3985c458853aef86bdf9.jpg)

输入 ffifdyop 查询后来到这个界面

![[BJDCTF 2020]easy_md5-5](https://pic.imgdb.cn/item/653d39d5c458853aef88c803.jpg)

查看页面源码

![[BJDCTF 2020]easy_md5-6](https://pic.imgdb.cn/item/653d39fdc458853aef89d1e3.jpg)

得到提示：

```php
$a = $GET['a'];
$b = $_GET['b'];

if($a != $b && md5($a) == md5($b)){
    header('Location: levell14.php');
```

代码的意思是要符合：两不同的字符串经 MD5 加密后相同，才定向到 levell14.php 

**方法一：利用 php 的弱比较特性来绕过 ==**

> php 中 == 是判断值是否相等，若两个变量的类型不相等，则会转化为相同类型后再进行比较。 php 在处理哈希字符串的时候，会把**以 0e 开头并且后面字符均为纯数字的哈希值**都解析为 0
>
> MD5 加密后以 0e 开头的字符串：
>
> - QNKCDZO
> - 240610708
> - s878926199a
> - s155964671a

所以只要随便选以上两个字符串，通过 GET 传给 'a' 和 'b' 参数 ，即可定向到 levell14.php 

**方法二：利用 md5() 函数无法处理数组(会返回NULL)来实现绕过 ==**

> NULL==NULL 为真
>
> **相关实验：**
>
> ![[BJDCTF 2020]easy_md5-7](https://pic.imgdb.cn/item/653d41adc458853aefb63dca.jpg)

所以 payload :  `/?a[]=1&&b[]=2`

**方法三：MD5 碰撞**

> 下面的任意两组字符串内容不同，但 MD5 结果相同：
>
> ```
> %af%13%76%70%82%a0%a6%58%cb%3e%23%38%c4%c6%db%8b%60%2c%bb%90%68%a0%2d%e9%47%aa%78%49%6e%0a%c0%c0%31%d3%fb%cb%82%25%92%0d%cf%61%67%64%e8%cd%7d%47%ba%0e%5d%1b%9c%1c%5c%cd%07%2d%f7%a8%2d%1d%bc%5e%2c%06%46%3a%0f%2d%4b%e9%20%1d%29%66%a4%e1%8b%7d%0c%f5%ef%97%b6%ee%48%dd%0e%09%aa%e5%4d%6a%5d%6d%75%77%72%cf%47%16%a2%06%72%71%c9%a1%8f%00%f6%9d%ee%54%27%71%be%c8%c3%8f%93%e3%52%73%73%53%a0%5f%69%ef%c3%3b%ea%ee%70%71%ae%2a%21%c8%44%d7%22%87%9f%be%79%6d%c4%61%a4%08%57%02%82%2a%ef%36%95%da%ee%13%bc%fb%7e%a3%59%45%ef%25%67%3c%e0%27%69%2b%95%77%b8%cd%dc%4f%de%73%24%e8%ab%66%74%d2%8c%68%06%80%0c%dd%74%ae%31%05%d1%15%7d%c4%5e%bc%0b%0f%21%23%a4%96%7c%17%12%d1%2b%b3%10%b7%37%60%68%d7%cb%35%5a%54%97%08%0d%54%78%49%d0%93%c3%b3%fd%1f%0b%35%11%9d%96%1d%ba%64%e0%86%ad%ef%52%98%2d%84%12%77%bb%ab%e8%64%da%a3%65%55%5d%d5%76%55%57%46%6c%89%c9%df%b2%3c%85%97%1e%f6%38%66%c9%17%22%e7%ea%c9%f5%d2%e0%14%d8%35%4f%0a%5c%34%d3%73%a5%98%f7%66%72%aa%43%e3%bd%a2%cd%62%fd%69%1d%34%30%57%52%ab%41%b1%91%65%f2%30%7f%cf%c6%a1%8c%fb%dc%c4%8f%61%a5%93%40%1a%13%d1%09%c5%e0%f7%87%5f%48%e7%d7%b3%62%04%a7%c4%cb%fd%f4%ff%cf%3b%74%28%1c%96%8e%09%73%3a%9b%a6%2f%ed%b7%99%d5%b9%05%39%95%ab
> 
> %af%13%76%70%82%a0%a6%58%cb%3e%23%38%c4%c6%db%8b%60%2c%bb%90%68%a0%2d%e9%47%aa%78%49%6e%0a%c0%c0%31%d3%fb%cb%82%25%92%0d%cf%61%67%64%e8%cd%7d%47%ba%0e%5d%1b%9c%1c%5c%cd%07%2d%f7%a8%2d%1d%bc%5e%2c%06%46%3a%0f%2d%4b%e9%20%1d%29%66%a4%e1%8b%7d%0c%f5%ef%97%b6%ee%48%dd%0e%09%aa%e5%4d%6a%5d%6d%75%77%72%cf%47%16%a2%06%72%71%c9%a1%8f%00%f6%9d%ee%54%27%71%be%c8%c3%8f%93%e3%52%73%73%53%a0%5f%69%ef%c3%3b%ea%ee%70%71%ae%2a%21%c8%44%d7%22%87%9f%be%79%6d%c4%61%a4%08%57%02%82%2a%ef%36%95%da%ee%13%bc%fb%7e%a3%59%45%ef%25%67%3c%e0%27%69%2b%95%77%b8%cd%dc%4f%de%73%24%e8%ab%66%74%d2%8c%68%06%80%0c%dd%74%ae%31%05%d1%15%7d%c4%5e%bc%0b%0f%21%23%a4%96%7c%17%12%d1%2b%b3%10%b7%37%60%68%d7%cb%35%5a%54%97%08%0d%54%78%49%d0%93%c3%b3%fd%1f%0b%35%11%9d%96%1d%ba%64%e0%86%ad%ef%52%98%2d%84%12%77%bb%ab%e8%64%da%a3%65%55%5d%d5%76%55%57%46%6c%89%c9%5f%b2%3c%85%97%1e%f6%38%66%c9%17%22%e7%ea%c9%f5%d2%e0%14%d8%35%4f%0a%5c%34%d3%f3%a5%98%f7%66%72%aa%43%e3%bd%a2%cd%62%fd%e9%1d%34%30%57%52%ab%41%b1%91%65%f2%30%7f%cf%c6%a1%8c%fb%dc%c4%8f%61%a5%13%40%1a%13%d1%09%c5%e0%f7%87%5f%48%e7%d7%b3%62%04%a7%c4%cb%fd%f4%ff%cf%3b%74%a8%1b%96%8e%09%73%3a%9b%a6%2f%ed%b7%99%d5%39%05%39%95%ab
> 
> %af%13%76%70%82%a0%a6%58%cb%3e%23%38%c4%c6%db%8b%60%2c%bb%90%68%a0%2d%e9%47%aa%78%49%6e%0a%c0%c0%31%d3%fb%cb%82%25%92%0d%cf%61%67%64%e8%cd%7d%47%ba%0e%5d%1b%9c%1c%5c%cd%07%2d%f7%a8%2d%1d%bc%5e%2c%06%46%3a%0f%2d%4b%e9%20%1d%29%66%a4%e1%8b%7d%0c%f5%ef%97%b6%ee%48%dd%0e%09%aa%e5%4d%6a%5d%6d%75%77%72%cf%47%16%a2%06%72%71%c9%a1%8f%00%f6%9d%ee%54%27%71%be%c8%c3%8f%93%e3%52%73%73%53%a0%5f%69%ef%c3%3b%ea%ee%70%71%ae%2a%21%c8%44%d7%22%87%9f%be%79%ed%c4%61%a4%08%57%02%82%2a%ef%36%95%da%ee%13%bc%fb%7e%a3%59%45%ef%25%67%3c%e0%a7%69%2b%95%77%b8%cd%dc%4f%de%73%24%e8%ab%e6%74%d2%8c%68%06%80%0c%dd%74%ae%31%05%d1%15%7d%c4%5e%bc%0b%0f%21%23%a4%16%7c%17%12%d1%2b%b3%10%b7%37%60%68%d7%cb%35%5a%54%97%08%0d%54%78%49%d0%93%c3%33%fd%1f%0b%35%11%9d%96%1d%ba%64%e0%86%ad%6f%52%98%2d%84%12%77%bb%ab%e8%64%da%a3%65%55%5d%d5%76%55%57%46%6c%89%c9%df%b2%3c%85%97%1e%f6%38%66%c9%17%22%e7%ea%c9%f5%d2%e0%14%d8%35%4f%0a%5c%34%d3%73%a5%98%f7%66%72%aa%43%e3%bd%a2%cd%62%fd%69%1d%34%30%57%52%ab%41%b1%91%65%f2%30%7f%cf%c6%a1%8c%fb%dc%c4%8f%61%a5%93%40%1a%13%d1%09%c5%e0%f7%87%5f%48%e7%d7%b3%62%04%a7%c4%cb%fd%f4%ff%cf%3b%74%28%1c%96%8e%09%73%3a%9b%a6%2f%ed%b7%99%d5%b9%05%39%95%ab
> ```

任选两个传给 'a' 参数和 'b' 参数

![[BJDCTF 2020]easy_md5-8](https://pic.imgdb.cn/item/653d4596c458853aefc8e885.jpg)


---


> **方法四：**无视这段代码，直接去 URL 访问 levell14.php ，也可以访问，但是作为练习就不投机取巧了


---

来到 levell14.php 
![[BJDCTF 2020]easy_md5-9](https://pic.imgdb.cn/item/653d3ad0c458853aef8f3ca5.jpg)

```php
<?php
error_reporting(0);
include "flag.php";

highlight_file(__FILE__);

// 检查是否通过POST请求发送了 'param1' 和 'param2' 参数，并且这两个参数的值不相等，
// 同时 'param1' 和 'param2' 经过MD5哈希后的结果相等
if ($_POST['param1'] !== $_POST['param2'] && md5($_POST['param1']) === md5($_POST['param2'])) {
    // 如果条件满足，输出 $flag 变量的值
    echo $flag;
}
?>
```

这里的代码和之前的不一样，== 是弱比较、=== 是强比较

**方法一：利用 md5() 函数无法处理数组(会返回NULL)来实现绕过 === (和之前绕过 == 的方法一样)**

>NULL===NULL 为真
>
>**相关实验：**
>
>![[BJDCTF 2020]easy_md5-10](https://pic.imgdb.cn/item/653d42e9c458853aefbcc3a2.jpg)

所以用 POST 给 'param1' 和 'param2' 参数分别传个数组类型就能得到 flag

![[BJDCTF 2020]easy_md5-11](https://pic.imgdb.cn/item/653d441bc458853aefc27e73.jpg)

**方法二：MD5 碰撞(同上)**


