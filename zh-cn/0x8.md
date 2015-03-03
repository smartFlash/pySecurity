##Whois自动查询

这一章将教大家一些技巧性的东西,教大家使用Cymru's团队提供的[whois模块](https://pypi.python.org/pypi/cymruwhois/1.0)来做一个whois信息查询工具，使用这个模块可以帮你节省大量的的时间,废话少说,现在就让我们开始吧!

首先你需要安装这个模块并且可以使用之前我们讲过的dir函数去看看这个模块提供了那些功能:

```
>>> from cymruwhois import Client
>>> c = Client()
>>> dir(c)
['KEY_FMT', '__doc__', '__init__', '__module__', '_begin', '_connect', '_connected', '_disconnect', '_lookupmany_raw', '_readline', '_sendline', 'c', 'cache', 'disconnect', 'get_cached', 'host', 'lookup', 'lookupmany', 'lookupmany_dict', 'port', 'read_and_discard']
>>>
```

现在我们使用lookup函数来查询一个单独的IP地址，在后面我们会使用"lookupmany"来查询一个IP数组列表:

```
>>> google = c.lookup('8.8.8.8') #译者注:国内会被GFW
>>> google
<cymruwhois.record instance: 15169|8.8.8.8|8.8.8.0/24|US|GOOGLE - Google Inc.,US>
>>> type(google)
<type 'instance'>
>>>
```

现在我们有一个cymruwhois.record的实例,可以从中提取出下面这些信息:

```
>>>
>>> dir(google)
['__doc__', '__init__', '__module__', '__repr__', '__str__', 'asn', 'cc', 'ip', 'owner', 'prefix']
>>> google.ip
'8.8.8.8'
>>> google.owner
'GOOGLE - Google Inc.,US'
>>> google.cc
'US'
>>> google.asn
'15169'
>>> google.prefix
'8.8.8.0/24'
>>>
```

我们以前思考处理多个ip列表的时候是使用for循环来处理的,但在这里我们并不需要使用for循环去遍历整个数组列表,我们可以使用Cymru团队提供的"lookupmany"函数去代替自己写的循环代码,下面将演示了一个比较复杂的脚本:从一个文件里面读取到IP列表,然后执行whois信息查询:

我们通常使用tcpdump,BPF还有bash-fu来处理IP列表,下面我们抓取了SYN包里面"tcp[13]=2"的ip并且通过awk的通道stdout与stdin把第六个元素使用" awk ‘{print $6}’"抓取出来，然后把使用awk抓取出来的ip写入到一个文件里面:

```
~$ tcpdump -ttttnnr t.cap tcp[13]=2 | awk '{print $6}' | awk -F "." '{print $1"."$2"."$3"."$4}' > ips.txt
reading from file t.cap, link-type LINUX_SLL (Linux cooked)
~$ python ip2net.py -r ips.txt
[+] Querying from:  ips.txt
173.194.0.0/16       # -   173.194.8.102 (US) - GOOGLE - Google Inc.,US
~$
```

现在让我看看ip2net.py这个脚本,里面有注释可以帮助你快速的理解这个脚本究竟是干些什么:

**ip2net.py**

```
#!/usr/bin/env python
import sys, os, optparse
from cymruwhois import Client
 
def look(iplist):
    c=Client() # 创建一个Client的实例类
    try:
        if ips != None:
            r = c.lookupmany_dict(iplist) # 利用lookupmany_dict()函数传递IP列表
            for ip in iplist: #遍历lookupman_dict()传入的值
                net = r[ip].prefix; owner = r[ip].owner; cc = r[ip].cc #从字典里面获取连接后的信息
                line = '%-20s # - %15s (%s) - %s' % (net,ip,cc,owner) #格式化输出
                    print line
    except:pass
 
def checkFile(ips): # 检查文件是否能够读取
        if not os.path.isfile(ips):
                print '[-] ' + ips + ' does not exist.'
                sys.exit(0)
        if not os.access(ips, os.R_OK):
                print '[-] ' + ips + ' access denied.'
                sys.exit(0)
        print '[+] Querying from:  ' +ips
 
def main():
        parser = optparse.OptionParser('%prog '+ \
        '-r <file_with IPs> || -i <IP>')
        parser.add_option('-r', dest='ips', type='string', \
                help='specify target file with IPs')
    parser.add_option('-i', dest='ip', type='string', \
        help='specify a target IP address')
        (options, args) = parser.parse_args()
    ip = options.ip      # Assigns a -i <IP> to variable 'ip'
    global ips; ips = options.ips # 赋值-r <fileName> 给变量 'ips'
        if (ips == None) and (ip == None): # 如果缺少参数就输出使用手册
                print parser.usage
                sys.exit(0)
        if ips != None:    #检查ips
        checkFile(ips)    #检查文件是否能够读取
        iplist = []    # 创建ipslist列表对象
            for line in open(ips, 'r'): # 解析文件内容
            iplist.append(line.strip('\n')) # 添加一行新内容并且删除换行字符
        look(iplist)    # 调用look()函数
 
    else:    # 执行lookup()函数并且把内容存储到变量 'ip'
        try:
            c=Client()
            r = c.lookup(ip)
                    net = r.prefix; owner = r.owner; cc = r.cc
                    line = '%-20s # - %15s (%s) - %s' % (net,ip,cc,owner)
                    print line
        except:pass
 
if __name__ == "__main__":
      main()
```


