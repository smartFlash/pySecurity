##入门(2)

这一章将继续讲解一些基础的Python脚本概念,我们将把代码写入到一个脚本里面，函数，类和sys模块。

**Python脚本框架**

下面是一个开始写Python脚本的基础例子，开始部分，我么告诉系统需要使用那一个解释器"#!/usr/bin/env python",然后我们通过"def main():"声明一个main函数,最后2行代码有mian()的先执行。你可以定义在你的脚本里面定义其它函数，这样使得你的代码更容易的理解和修改维护：

```
#!/usr/bin/python
import <module1>, <module2>
 
def myFunction():
 
def main():
        myFunction()
 
if __name__=="__main__":
        main()
 ```

 **函数**

 一种常见的写法是把每个功能函数分开写，执行一些操作之后然后返回结果。下面的这个伪代码演示的例子就能够很清晰的解释这个概念:
 ```
# 声明函数/逻辑处理
def MyFunction:
  ...do work...
  return output
 
#在main函数里面调用:
def main():
  output = MyFunction(input)
 ```

 **类**

 Python类开始使用的时候会有点困难，因为它是教你以何种方式设计你的代码，如果你掌握类的概念那么你就可以把数据和定义按照类的逻辑分组，这样类就拥有了属性和与之想关联的方法。当你定义一个类之后，你可以创建一个新的类，然后继承之前创建的类的属性和与之相关联的方法，这编程就叫做面向对象编程。

 如果你感到迷惑，那么我建议你先不要去学习类，实际上，你并不需要类。但它可以让你的代码减少冗余。下面我们将定义个新的类"Domain"使用"class"关键字，当你实例化Domain类型对象的时候，它的类型有多种方式去定义:

```
>>> import os
>>> class Domain:
...     def __init__(self, domain, port, protocol):
#通过两个内部变量存储变量
...       self.domain=domain
...       self.port=port
...       self.protocol=protocol
#构造一个url的方法
...     def URL(self):
...       if self.protocol == 'https':
...         URL = 'https://'+self.domain+':'+self.port+'/'
...       if self.protocol == 'http':
...         URL = 'http://'+self.domain+':'+self.port+'/'
...         return URL
# 调用os.system中主机命令lookup去解析域名
...     def lookup(self):
...       os.system("host "+self.domain)
...
>>>
>>> domain=Domain('google.com', '443', 'https')
>>>
>>> dir(domain)
['URL', '__doc__', '__init__', '__module__', 'ip', 'lookup', 'port', 'protocol']
>>> domain.URL()
'https://8.8.8.8:443/'
>>> domain.ip
'8.8.8.8'
>>> domain.port
'443'
>>> domain.protocol
'https'
>>> domain.lookup()
google.com has address 74.125.228.233
google.com has address 74.125.228.227
google.com has address 74.125.228.232
```

正如你所看到的，当你实例化一个Domian类之后你可以运行类中的方法。再次说声，这个概念最初的时候很容易混乱，尤其是当你刚刚Python和编程的时候。尝试一下去实现一个新的类在你的Python脚本里面，我发现这是掌握这个概念最好的途径。

**使用sys处理命令行输入值**

最好我们来介绍一下sys模块，它可以让你读取从命令终端输入的值并且帮你引入到脚本里面，它的语法很简单，sys.agrv[0]就是一个实际的脚本名，并在命令行指定的每个参数后面分配一个下标。下面是一个简单的例子:

```
#!/usr/bin/python
import sys

script = sys.argv[0]
ip = sys.argv[1]
port = sys.argv[2]

print "[+] The script name is: "+script
print "[+] The IP is: "+ip+" and the port is: "+port

```
当执行这个脚本的时候，并且后面跟三个参数执行之后的结果如下:

```
~$ python sys.py 8.8.8.8 53
[+] The script name is: sys.py
[+] The IP is: 8.8.8.8 and the port is: 53
```

上面的只是一个例子，大家可以继续去研究其它Python模块，因为它们能够放你用最简单的方式解决你遇到的问题。下一章将会介绍使用Python进行网络连接并且写出一个基础的扫描器.


