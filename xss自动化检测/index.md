# 

# XSS自动化检测

## XSSer

### Kali安装XSSer

```bash
git clone https://github.com/epsylon/xsser.git --- 下载XSSer安装包
cd xsser
python3 setup.py install --- 安装
```

​	

### 命令

```
（一）、Options:
–version          	   显示程序的版本号
  -h, --help             显示帮助信息
  -s, --statistics 	   显示高级显示输出结果
  -v, --verbose      激活冗余模式输出结果
  --gtk                加载 XSSer GTK 接口

（二）、特别的用法:
你可以选择 Vector(s) 和 Bypasser(s) 结合特殊的用法来注入代码：  
–imx=IMX       	   利用XSS代码植入来创建一个假的图象
–fla=FLASH     	  利用XSS代码植入来创建一个假的swf 
–xst=XST XST - Cross Site Tracing (–xst http(s)://host.com)

（三）、选择目标:
至少有一个选择必须被指定来设置来源以获得目标(s)的url。
-u URL, --url=URL   	 键入目标URL进行分析
-i READFILE         	  从一个文件中读取URL
-d DORK                   利用搜索引擎傻瓜式的搜索URL
–De=DORK_ENGINE    		傻瓜式的利用搜索引擎 (bing, altavista,yahoo, baidu, yandex, youdao, webcrawler, ask, etc.查看 dork.py 文件来核对有效的搜索引擎)

（四）、 现测HTTP/HTTPS的连接类型:
These options can be used to specify which parameter(s) we want to use
like payload to inject code.   
-g GETDATA            输入一个负荷来进行审计，使用 GET参数 (例如: ‘/menu.php?q=’)
-p POSTDATA         输入一个负荷来进行审计，使用 POST 参数(例如: ‘foo=1&bar=’)
-c CRAWLING         目标URL的爬行数目(s): 1-99999
–Cw=CRAWLER_WIDTH  爬行深度: 1-5
–Cl                            本地目标URL爬行 (默认 TRUE)

（五）、 配置请求:
这些选项被用来制定如何攻击目标和使用负荷. 你有多重选择:   
–head Send a HEAD request before start a test
–cookie=COOKIE     改变你的HTTP Cookie header
–user-agent=AGENT  改变你的 HTTP User-Agent header (默认 SPOOFED)
–referer=REFERER   使用别的HTTP Referer header (默认 NONE)
–headers=HEADERS   额外的 HTTP headers 换行隔开
–auth-type=ATYPE   HTTP 认证类型 (基本值类型或者摘要)
–auth-cred=ACRED   HTTP 认证证书 (值 name:password)
–proxy=PROXY       使用代理服务器 (tor:http://localhost:8118)
–timeout=TIMEOUT   设定时间延迟 (默认 30)
–delay=DELAY       设定每一个 HTTP request值 (默认 8)
–threads=THREADS   最大数目的 HTTP requests并发 (默认 5)
–retries=RETRIES   连接超时重试 (默认 3)

（六）、 系统验校器:
这些选项对于有过滤器的XSS攻击很有效和或者重复所利用的代码:   
–hash                如果目标重复内容，则每次检测hash(对预知可能错误的结果非常有用)
–heuristic         启发式的设置才检测那些脚本会被过滤: ;/<>"’=

（七）、 选择攻击向量(s):
这些选项被用在特殊的 XSS向量源代码来注入到每一个负荷中。非常重要的, 如果你不想尝试通用的XSS注入代码,请使用默认参数. 只有一个选项:   
–payload=SCRIPT    OWN  - 插入你手动构造的XSS 语句-
–auto              AUTO - 从文件中插入 XSSer ‘报告’ 向量

（八）、选择Bypasser(s):
这些选项用来编码所选择的攻击向量，如果目标使用反XSS过滤器代码和IPS规则，则尝试绕过所有的目标上的反XSS 过滤器代码和入侵防御系统规则，总之, 能结合其他的技巧 来提供编码:   
–Str                 使用 String.FromCharCode()方法
–Une               使用 Unescape() 函数
–Mix                最小的 String.FromCharCode() 函数 和 Unescape()函数
–Dec               使用小数编码
–Hex               使用16进制编码
–Hes               使用带分号的16进制编码
–Dwo              编码IP地址向量为双字节
–Doo               编码IP地址向量为八进制
–Cem=CEM       手动尝试不同的字符编码(反向混淆效果更好) -> (例如: ‘Mix,Une,Str,Hex’)

（九）、 特殊的技巧:
这些选项被用来尝试不同的XSS 技巧. 你可以进行多重选择:   
–Coo               COO - 跨站脚本Cookies注入
–Xsa                XSA -   跨站Agent 脚本
–Xsr                 XSR -    跨站 Referer 脚本
–Dcp               DCP - DCP注入
–Dom              DOM - DOM注入
–Ind                 IND - HTTP 包含代码的快速响应
–Anchor           ANC - 使用影子攻击负荷 (DOM 影子!)

（十）、 Select Final injection(s):
这些选项在攻击目标中用于特殊代码注入 Important, if you want to exploit on-the-wild your discovered vulnerabilities. Choose only one option:   
–Fp=FINALPAYLOAD   OWN    - 手动插入注入代码-
–Fr=FINALREMOTE    REMOTE - 远程插入注入代码

（十一）、 Special Final injection(s):
These options can be used to execute some ‘special’ injection(s) in vulnerable target(s). You can select multiple and combine with your final code (except with DCP code):
–Anchor ANC - Use ‘Anchor Stealth’ payloader (DOM shadows!)
–B64 B64 - Base64 code encoding in META tag (rfc2397)
–Onm             ONM - 使用 MouseMove() 事件注入代码
–Ifr                   IFR - 使用 资源标签注入代码 
–Doss              DOSs   - XSS 对服务端的拒绝服务攻击注入
–Dos               DOS    - XSS 对客户端的拒绝服务攻击注入
–B64               B64    - META标签 Base64编码(rfc2397)

（十二）、 混杂模式:
–silent              禁止控制台输出结果
–update            检查XSSer 最新稳定版本
–save                直接输入结果到模版文件 (XSSlist.dat)
–xml=FILEXML 输出 'positives’到一个XML文件 (–xml filename.xml)
–publish             输出 'positives’本地网络 (identi.ca)
–short=SHORTURLS   显示最后的短代码 (tinyurl, is.gd)
–launch              发现的每个XSS都在浏览器进行测试

（十三）、 Reporting:
–save Export to file (XSSreport.raw)
–xml=FILEXML Export to XML (–xml file.xml)

（十四）、Miscellaneous:
–silent Inhibit console output results
–alive=ISALIVE Set limit of errors before check if target is alive
–update Check for latest stable version
```

​	

## 检测XSS漏洞

```bash
xsser -u"http://192.168.172.63/DVWA/vulnerabilities/" -g"xss_r/?name=XSS" --cookie="security=low; PHPSESSID=4ljpm33nju2du0nitcmg2a3d0s" -s -v --reverse-check
```

- -u：URL地址
- -g/-p：GET/POST方法
- --cookie：若当前需要身份认证才能访问的需要用到该参数
- -s：提交请求的数量的统计
- -v：显示详细信息--reverse-check：基于反向连接是否被建立来检测是否存在漏洞

​	

​	

## XSStrike

### Kali安装XSSer

```bash
git clone https://github.com/s0md3v/XSStrike.git --- 下载XSStrike
cd XSStrike
pip3 install -r requirements.txt --- 更新依赖模块
```

​	

## 检测XSS漏洞

```bash
python3 xsstrike.py -u "http://192.168.172.63/pikachu-master/vul/xss/xss_reflected_get.php?message=xss&submit=submit" 
```


