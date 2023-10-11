# 

# SQL注入—报错盲注

## Xpath报错注入 (extractvalue和updatexml)

在MySQL高版本（大于5.1版本）中添加了对XML文档进行查询和修改的函数：

- updatexml()
- extractvalue()

>  当这两个函数在执行时，如果出现xml文档路径错误就会产生报错

​	

### updatexml()

- updatexml()是一个使用不同的xml标记匹配和替换xml块的函数。

- 作用：改变文档中符合条件的节点的值

- 语法： updatexml（XML_document，XPath_string，new_value） 
  第一个参数：代表string格式，为XML文档对象的名称，文中为Doc 
  第二个参数：代表路径，Xpath格式的字符串例如//title【@lang】 
  第三个参数：string格式，替换查找到的符合条件的数据

- updatexml使用时，当xpath_string格式出现错误，MySQL会爆出xpath语法错误（XPATH syntax error）

- 例如： select * from test where id = 1 and (updatexml(1,0x7e,3)); 
  由于0x7e是~，不属于Xpath语法格式，因此报出Xpath语法错误。

  

​	

### extractvalue()

- 此函数从目标XML中返回包含所查询值的字符串 
- 语法：extractvalue（XML_document，xpath_string） 
  第一个参数：string格式，为XML文档对象的名称 
  第二个参数：xpath_string（xpath格式的字符串） 
- select * from test where id=1 and (extractvalue(1,concat(0x7e,(select user()),0x7e)));
- extractvalue使用时当xpath_string格式出现错误，mysql则会爆出xpath语法错误（xpath syntax）
- ' union select 1,extractvalue(1,concat(0x7e,(select version())))%23
- ' or extractvalue(1,concat(0x7e,database())) or'
