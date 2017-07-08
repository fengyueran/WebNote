###非结构化数据之lxml库

lxml 是一种使用 Python 编写的库,可以迅速、灵活地处理 XML ，支持 XPath (XML Path Language)
[lxml python 官方文档][1]

**学习目的**

利用上节课学习的XPath语法，来快速的定位 特定元素以及节点信息，目的是 提取出 HTML、XML 目标数据
如何安装
```
 pip install lxml

```
**初步使用**

首先我们利用lxml来解析 HTML 代码，先来一个小例子来感受一下它的基本用法。
使用 lxml 的 etree 库，然后利用 etree.HTML 初始化，然后我们将其打印出来。
```
from lxml import etree
text = '''
<div>
  <ul>
       <li class="item-0"><a href="link1.html">first item</a></li>
       <li class="item-1"><a href="link2.html">second item</a></li>
       <li class="item-inactive"><a href="link3.html"><span class="bold">third item</span></a></li>
       <li class="item-1"><a href="link4.html">fourth item</a></li>
       <li class="item-0"><a href="link5.html">fifth item</a>
   </ul>
</div>
'''
#Parses an HTML document from a string
html = etree.HTML(text)   
#Serialize an element to an encoded string representation of its XML tree
result = etree.tostring(html)
print result
```
所以输出结果是这样的
```
<html><body><div>
  <ul>
       <li class="item-0"><a href="link1.html">first item</a></li>
       <li class="item-1"><a href="link2.html">second item</a></li>
       <li class="item-inactive"><a href="link3.html"><span class="bold">third item</span></a></li>
       <li class="item-1"><a href="link4.html">fourth item</a></li>
       <li class="item-0"><a href="link5.html">fifth item</a>
   </li></ul>
</div>
</body></html>
```
不仅补全了 li 标签，还添加了 body，html 标签。

**XPath实例测试**

(1) 获取所有的 `<li>` 标签
```
print type(html)
result = html.xpath('//li')
print result
print len(result)
print type(result)
print type(result[0])
```
运行结果
```
<type 'lxml.etree._ElementTree'>
[<Element li at 0x1014e0e18>, <Element li at 0x1014e0ef0>, <Element li at 0x1014e0f38>, <Element li at 0x1014e0f80>, <Element li at 0x1014e0fc8>]
5
<type 'list'>
<type 'lxml.etree._Element'>
```
可见，每个元素都是 Element 类型;是一个个的标签元素，类似现在的实例
```
<Element li at 0x1014e0e18> Element类型代表的就是
<li class="item-0"><a href="link1.html">first item</a></li>
```
[注意］

Element类型是一种灵活的容器对象，用于在内存中存储结构化数据。
每个element对象都具有以下属性：
　　1. tag：string对象，标签，用于标识该元素表示哪种数据（即元素类型）。
　　2. attrib：dictionary对象，表示附有的属性。
　　3. text：string对象，表示element的内容。
　　4. tail：string对象，表示element闭合之后的尾迹。
实例

<tag attrib1=1>text</tag>tail
1     2        3         4
result[0].tag
result[0].text
result[0].tail
result[0].attrib
（2）获取 <li> 标签的所有 class
html.xpath('//li/@class')
运行结果

['item-0', 'item-1', 'item-inactive', 'item-1', 'item-0']
（3）获取 <li> 标签下属性 href 为 link1.html 的 <a> 标签
html.xpath('//li/a[@href="link1.html"]')
运行结果

[<Element a at 0x10ffaae18>]
（4）获取 <li> 标签下的所有 <span> 标签
注意这么写是不对的
html.xpath('//li/span')
因为 / 是用来获取子元素的，而 <span> 并不是 <li> 的子元素，所以，要用双斜杠
html.xpath('//li//span')
运行结果

[<Element span at 0x10d698e18>]
（5）获取 <li> 标签下的所有 class，不包括 <li>
html.xpath('//li/a//@class')
运行结果

['blod']
（6）获取最后一个 <li> 的<a> 的 href
html.xpath('//li[last()]/a/@href')
运行结果

['link5.html']
（7）获取 class 为 bold 的标签名
result = html.xpath('//*[@class="bold"]')
print result[0].tag
运行结果

span
开始练习

通过以上实例的练习，相信大家对 XPath 的基本用法有了基本的了解
实战项目

以腾讯招聘网站为例
http://hr.tencent.com/position.php?&start=10
from lxml import etree
import urllib2
import urllib
import json

request = urllib2.Request('http://hr.tencent.com/position.php?&start=10#a')
response =urllib2.urlopen(request)
resHtml = response.read()
output =open('tencent.json','w')

html = etree.HTML(resHtml)
result = html.xpath('//tr[@class="odd"] | //tr[@class="even"]')

for site in result:
    item={}

    name = site.xpath('./td[1]/a')[0].text
    detailLink = site.xpath('./td[1]/a')[0].attrib['href']
    catalog = site.xpath('./td[2]')[0].text
    recruitNumber = site.xpath('./td[3]')[0].text
    workLocation = site.xpath('./td[4]')[0].text
    publishTime = site.xpath('./td[5]')[0].text

    print type(name)
    print name,detailLink,catalog,recruitNumber,workLocation,publishTime
    item['name']=name
    item['detailLink']=detailLink
    item['catalog']=catalog
    item['recruitNumber']=recruitNumber
    item['publishTime']=publishTime

    line = json.dumps(item,ensure_ascii=False) + '\n'
    print line
    output.write(line.encode('utf-8'))
output.close()



[1]:http://lxml.de/index.html