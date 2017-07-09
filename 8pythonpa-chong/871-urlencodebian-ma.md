当我们的网络请求有中文字符，如
```
https://tieba.baidu.com/f?ie=utf-8&kw=网络爬虫&fr=search
```
直接请求该url显然是不行的，所以需要转换，这里用Urlencode,urlencode()函数原理就是首先把中文字符转换为十六进制，然后在每个字符前面加一个标识符%
```
https://tieba.baidu.com/f?ie=utf-8&kw=%E7%BD%91%E7%BB%9C%E7%88%AC%E8%99%AB&fr=search
```

**中文字符按什么编码格式进行转化成十六进制呢？**
**utf-8、gb2312、gbk urlencode编码**
- utf-8与utf-8 urlencode区别
```
import urllib

country = u'中国'

country.encode('utf-8')
'\xe4\xb8\xad\xe5\x9b\xbd'

urllib.quote(country.encode('utf-8'))
'%E4%B8%AD%E5%9B%BD'
```
- gb2312与gb2312 urlencode区别
```
import urllib

country = u'中国'

country.encode('gb2312')
'\xd6\xd0\xb9\xfa'

urllib.quote(country.encode('gb2312'))
'%D6%D0%B9%FA'
```
案例

模拟出 拉勾网 如下url地址：
http://www.lagou.com/jobs/list_Python?px=default&city=%E5%8C%97%E4%BA%AC&district=%E6%9C%9D%E9%98%B3%E5%8C%BA&bizArea=%E6%9C%9B%E4%BA%AC#filterBox
# -*- coding: utf-8 -*-
import urllib
import chardet

city=u'北京'.encode('utf-8')
district=u'朝阳区'.encode('utf-8')
bizArea=u'望京'.encode('utf-8')

query={
    'city':city,
    'district':district,
    'bizArea':bizArea
}

print chardet.detect(query['city'])
{'confidence': 0.7525, 'encoding': 'utf-8'}

print urllib.urlencode(query)
city=%E5%8C%97%E4%BA%AC&bizArea=%E6%9C%9B%E4%BA%AC&district=%E6%9C%9B%E4%BA%AC

print 'http://www.lagou.com/jobs/list_Python?px=default&'+urllib.urlencode(query)+'#filterBox'
http://www.lagou.com/jobs/list_Python?px=default&city=%E5%8C%97%E4%BA%AC&bizArea=%E6%9C%9B%E4%BA%AC&district=%E6%9C%9B%E4%BA%AC#filterBox
模拟出 阿里巴巴 如下url地址：
https://s.1688.com/selloffer/offer_search.htm?keywords=%CA%D6%BB%FA%BC%B0%C5%E4%BC%FE%CA%D0%B3%A1


# -*- coding: utf-8 -*-
import urllib
import chardet

keywords=u'手机及配件市场'.encode('gbk')

query={
    'keywords':keywords,
}
print chardet.detect(query['keywords'])
{'confidence': 0.99, 'encoding': 'GB2312'}
print urllib.urlencode(query)
keywords=%CA%D6%BB%FA%BC%B0%C5%E4%BC%FE%CA%D0%B3%A1

print 'https://s.1688.com/selloffer/offer_search.htm?'+urllib.urlencode(query)
https://s.1688.com/selloffer/offer_search.htm?keywords=%CA%D6%BB%FA%BC%B0%C5%E4%BC%FE%CA%D0%B3%A1