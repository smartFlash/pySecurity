##入门(2)

这一章将继续讲解一些基础的Python脚本概念,我们将把代码写入到一个脚本里面，函数，类和sys模块。

**Python脚本框架**

下面是一个开始写Python脚本的基础例子，开始部分，我么告诉系统需要使用那一个解释器"#!/usr/bin/env python",然后我们通过"def main():"声明一个main函数,最后2行代码有mian()的首先运行。你可以定义在你的脚本里面定义其他函数，这样使得你的代码更容易的理解和修改维护：

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

 Py