##python系列教程(翻译)

```
~# python
>>> import urllib
>>> from bs4 import BeautifulSoup
>>> url = urllib.urlopen("http://www.primalsecurity.net")
>>> output = BeautifulSoup(url.read(), 'lxml')
>>> output.title
<title>Primal Security Podcast</title>
>>>
```

这是一套python系列教程，学习本套教程不需要你有任何变成背景。教程由最简单的hello world到信息安全应用实例。逐个难点击破:

0x0 – [入门](https://github.com/smartFlash/pySecurity/blob/master/zh-cn/0x0.md)

0x0 – 入门 Pt.2

0×1 – 端口扫描

0x2 – 逆向shell

0x3 – 模糊测试

0x4 – Python转exe

0x5 – Web请求

0x6 – 爬虫

0x7 – Web扫描和利用

0x8 – Whois查询

0x9 – 系统命令调用

0xA – Python版的Metasploit

0xB – 伪终端

0xC – exp编写

用例1: CVE-2014-6271

用例2: CVE-2012-1823

用例3: CVE-2012-3152

用例4: CVE-2014-3704

原文地址:http://www.primalsecurity.net/tutorials/python-tutorials/
