当我们的网络请求有中文字符，如
```
https://tieba.baidu.com/f?ie=utf-8&kw=网络爬虫&fr=search
```
直接请求该url显然是不行的，所以需要转换，这里用Urlencode,urlencode()函数原理就是首先把中文字符转换为十六进制，然后在每个字符前面加一个标识符%
```
https://tieba.baidu.com/f?ie=utf-8&kw=%E7%BD%91%E7%BB%9C%E7%88%AC%E8%99%AB&fr=search
```