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
**import**
```
import random
print random.randint(0,10)
```