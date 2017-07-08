###XPath 语言

XPath（XML Path Language）是XML路径语言,它是一种用来定位XML文档中某部分位置的语言。

**学习目的**

将HTML转换成XML文档之后，用XPath查找HTML节点或元素
比如用“/”来作为上下层级间的分隔，第一个“/”表示文档的根节点（注意，不是指文档最外层的tag节点，而是指文档本身）。
比如对于一个HTML文件来说，最外层的节点应该是"/html"。

**XPath开发工具**

chrome插件 XPath Helper
可以支持在网页点击元素生成xpath，就省去了自己去查找xpath的功夫，也便于未来做到所点即所得的功能
 只要按住Ctrl + Shift+ X就会出来相应窗口，将鼠标移至想要的元素再按Shift就会出来结果了。

[XPath语法参考文档][1]

**XPath语法**

XPath 是一门在 XML 文档中查找信息的语言。
XPath 可用来在 XML 文档中对元素和属性进行遍历。
```
<?xml version="1.0" encoding="ISO-8859-1"?>

<bookstore>

<book>
  <title lang="eng">Harry Potter</title>
  <price>29.99</price>
</book>

<book>
  <title lang="eng">Learning XML</title>
  <price>39.95</price>
</book>

</bookstore>
```
选取节点 XPath 使用路径表达式在 XML 文档中选取节点。节点是通过沿着路径或者 step 来选取的。
下面列出了最有用的路径表达式：






[1]:http://www.w3school.com.cn/xpath/index.asp