# [第五空间 2021]WebFTP


# [第五空间 2021]WebFTP

![[第五空间 2021]WebFTP-1](https://pic.imgdb.cn/item/6537c737c458853aef45142a.jpg)

用Dirsearch目录扫描`python dirsearch.py -u "http://node4.anna.nssctf.cn:28680/" -e *`

![[第五空间 2021]WebFTP-2](https://pic.imgdb.cn/item/6537c67cc458853aef4026aa.jpg)

访问phpinfo.php，CTRL+F查询"flag"

![[第五空间 2021]WebFTP-3](https://pic.imgdb.cn/item/6537c849c458853aef4aebe6.jpg)

> 这flag的位置莫名其妙的，还是通过看其他师傅的WriteUp才知道flag藏在phpinfo里
>
> 一开始我是在README.md里拿到默认的admin账号密码，尝试进了后台，在后台里找了半天啥也没找到🤨 (不过听说当时在比赛的时候用默认密码是进不去的，现在的复现环境里才能进)
