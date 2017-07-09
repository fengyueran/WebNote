**计算**
```
./hello.py
#coding:utf-8
a = 19 + 2
print a
```

**函数**
声明函数用def
```
def 函数名(参数1，参数2，...，参数n)：

    函数体
    
//冒号的意思就是下面好开始真正的函数内容了,不能少
def myFunction(a,b):
    c = a + b
    print c

if __name__=="__main__":
    myFunction(1,2)
```
其中__name__ = '__main__' 的作用，到底干嘛的？
有句话经典的概括了这段代码的意义：
>“Make a script both importable and executable”

意思就是说让你写的脚本模块既可以导入到别的模块中用，另外该模块自己也可执行。
这句话，可能一开始听的还不是很懂。下面举例说明：
先写一个模块：
```
#module.py
def main():
  print "we are in %s"%__name__
if __name__ == '__main__':
  main()
  ```
  这个函数定义了一个main函数，我们执行一下该py文件发现结果是打印出”we are in __main__“,说明我们的if语句中的内容被执行了，调用了main()：
但是如果我们从另我一个模块导入该模块，并调用一次main()函数会是怎样的结果呢？
```
#anothermodle.py
from module import main
main()
```
其执行的结果是：we are in module
但是没有显示”we are in __main__“,也就是说模块__name__ = '__main__' 下面的函数没有执行。
这样既可以让“模块”文件运行，也可以被其他模块引入，而且不会执行函数2次。这才是关键。

**if语句**
```
a = 3
if a == 3:
    print "it is three"
else:
    print "it is not three"
```

**数组**
```
a = [1, 2, "hello", 3]
print a
```

**for循环**
```
for i in a:
    print i
```

**字典：**
```
mydict = { "a":3, "b":4 }
mydict["c"] = 5
print mydict['c']
print mydict
```
**import模块引入**
```
import random
print random.randint(0,10)
```