###CSS Selector

CSS(即层叠样式表Cascading Stylesheet),Selector来定位（locate）页面上的元素（Elements）。Selenium官网的Document里极力推荐使用CSS locator，而不是XPath来定位元素，原因是CSS locator比XPath locator速度快.

**Beautiful Soup**

- 支持从HTML或XML文件中提取数据的Python库
- 支持Python标准库中的HTML解析器
- 还支持一些第三方的解析器lxml, 使用的是 Xpath 语法，推荐安装。

Beautiful Soup自动将输入文档转换为Unicode编码，输出文档转换为utf-8编码。你不需要考虑编码方式，除非文档没有指定一个编码方式，这时，Beautiful Soup就不能自动识别编码方式了。然后，你仅仅需要说明一下原始编码方式就可以了
- Beautiful Soup4 安装
官方文档链接:
[https://www.crummy.com/software/BeautifulSoup/bs4/doc/][1]

可以利用 pip来安装
pip install beautifulsoup4
安装解析器(上节课已经安装过)
Beautiful Soup支持Python标准库中的HTML解析器,还支持一些第三方的解析器,其中一个是 lxml .根据操作系统不同,可以选择下列方法来安装lxml:
另一个可供选择的解析器是纯Python实现的 html5lib , html5lib的解析方式与浏览器相同,可以选择下列方法来安装html5lib:
pip install html5lib
下表列出了主要的解析器：
[1]:https://www.crummy.com/software/BeautifulSoup/bs4/doc/