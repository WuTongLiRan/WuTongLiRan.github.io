# [SWPUCTF 2021 新生赛]Do_you_know_http


# [SWPUCTF 2021 新生赛]Do_you_know_http

提示"请使用 WLLM 浏览器"

![[SWPUCTF 2021 新生赛]Do_you_know_http-1](https://pic.imgdb.cn/item/6537a72cc458853aefd264e9.jpg)

利用BurpSuite抓包伪造User-Agent:WLLM

![[SWPUCTF 2021 新生赛]Do_you_know_http-2](https://pic.imgdb.cn/item/6537b616c458853aef0180fd.jpg)

> - [User-Agent](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/User-Agent)
>
> User-Agent：用来让网络协议的对端来识别发起请求的用户代理软件的应用类型、操作系统、软件开发商以及版本号。
>
> 说白了User-Agent就是用来告知服务器你是用哪个浏览器访问的

来到了a.php，提示只能通过本地来访问

![[SWPUCTF 2021 新生赛]Do_you_know_http-3](https://pic.imgdb.cn/item/6537b635c458853aef01e690.jpg)

伪造X-Forwarded-For:127.0.0.1

![[SWPUCTF 2021 新生赛]Do_you_know_http-4](https://pic.imgdb.cn/item/6537bab5c458853aef10d8c2.jpg)

![[SWPUCTF 2021 新生赛]Do_you_know_http-5](https://pic.imgdb.cn/item/6537bad1c458853aef1140d4.jpg)

> - [X-Forwarded-For](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/X-Forwarded-For)
>
> X-Forwarded-For：请求标头是一个事实上的用于标识通过代理服务器连接到 web 服务器的客户端的原始 IP 地址的标头。
>
> 说白了X-Forwarded-For就是用来告知服务器你的IP



**参考文章**

[HTTP | MDN - MDN Web Docs](https://developer.mozilla.org/zh-CN/docs/Web/HTTP)


