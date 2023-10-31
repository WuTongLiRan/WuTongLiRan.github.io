# [suctf 2019]EasySQL


# [suctf 2019]EasySQL

![ [suctf 2019]EasySQL-1](https://pic.imgdb.cn/item/653b2f3ac458853aef8c57d5.jpg)

根据题目，应该要 sql 注入，先判断过滤了哪些东西

随便输个 1 ，Burpsuite 抓请求包发到 Intruder 模块，在要爆破的地方做记号

![ [suctf 2019]EasySQL-2](https://pic.imgdb.cn/item/653b3a11c458853aefaa5353.jpg)

跑字典，速度放慢点防止 429 报错

![ [suctf 2019]EasySQL-3](https://pic.imgdb.cn/item/653b3b2dc458853aefad784f.jpg)

Length 为 503 是什么也没回显

![ [suctf 2019]EasySQL-4](https://pic.imgdb.cn/item/653b3adcc458853aefac9414.jpg)

Length 为 510 回显 Nonono ，很显然是被过滤了

![ [suctf 2019]EasySQL-5](https://pic.imgdb.cn/item/653b3b97c458853aefaeaa2c.jpg)

Length 为 512 回显 Too long 

![ [suctf 2019]EasySQL-6](https://pic.imgdb.cn/item/653b3bd2c458853aefaf4e87.jpg)

除此之外的 Length 为正常回显，由此可以看出：输入数字回显数字、输入字符无回显、sql语句大部分被过滤( ; 和 select 未被过滤)

> 我这个字典在这题不是很适合，主要是演示一下思路

所以联合注入和盲注等是不太可能，因为 ; 未被过滤，尝试堆叠注入

`1;show databases;`回显出数据库的信息，存在堆叠注入

![ [suctf 2019]EasySQL-7](https://pic.imgdb.cn/item/653b3da3c458853aefb49a0e.jpg)

`1;show tables;`回显出表的信息

![ [suctf 2019]EasySQL-8](https://pic.imgdb.cn/item/653b3dd8c458853aefb54034.jpg)

由于 from 和 flag 都被过滤了，查询 flag 肯定不行，这时候要猜测后端的查询语句

由于：POST 提交 query 参数、只有一个 Flag 表、输入数字回显相同数字、输入字符无回显(因为 Flag 表没有这个字段所以没有回显)

初步猜测后端的代码可能是`select $_POST['query'] from Flag;`

但是这样的话由于 from 和 flag 都被过滤，题目就无解

所以后端的代码应该是`select $_POST['query']||flag from Flag;`(其他大佬猜出来的，我没猜出来😰)

> \$\_POST['query']||flag ：逻辑或运算，如果左边的是数字(真)，数字||flag 的结果就是那个数字
>
> **相关实验：**
> ![ [suctf 2019]EasySQL-9](https://pic.imgdb.cn/item/653b412ec458853aefbfa387.jpg)

**解法一：**

payload：`*,1`

![ [suctf 2019]EasySQL-10](https://pic.imgdb.cn/item/653b41ecc458853aefc200be.jpg)

后端代码：`select *,1||flag from Flag;`

既查询 * ( flag 表的所有数据)，也查询 1||flag 

> **相关实验：**
>
> ![ [suctf 2019]EasySQL-11](https://pic.imgdb.cn/item/653b432ac458853aefc5f07f.jpg)

**解法二：**

`set sql_mode=PIPES_AS_CONCAT;` 可以将 || 逻辑或的功能改成连接字符的功能，类似 concat

payload：`1;set sql_mode=PIPES_AS_CONCAT;select 1`

![ [suctf 2019]EasySQL-12](https://pic.imgdb.cn/item/653b4534c458853aefccaecd.jpg)

后端代码：`select 1;set sql_mode=PIPES_AS_CONCAT;select 1||flag from Flag;`

先查询1；再将 || 的功能改成连接功能；再查询 1||flag ，即把查询 1 的结果和查询 flag 的结果拼接起来，所以输出的是 1和1NSSCTF{xxxxxxxx}

> **相关实验：**
>
> ![ [suctf 2019]EasySQL-13](https://pic.imgdb.cn/item/653b459ec458853aefce0fc3.jpg)

