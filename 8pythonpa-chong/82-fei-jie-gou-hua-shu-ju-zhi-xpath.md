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

![](/assets/8.2.1-1.png)

在下面的表格中，我们已列出了一些路径表达式以及表达式的结果：
![](/assets/8.2.1-2.png)

谓语条件（Predicates）
- 谓语用来查找某个特定的信息或者包含某个指定的值的节点。
- 所谓"谓语条件"，就是对路径表达式的附加条件
- 谓语是被嵌在方括号中，都写在方括号"[]"中，表示对节点进行进一步的筛选。

在下面的表格中，我们列出了带有谓语的一些路径表达式，以及表达式的结果：
![](/assets/8.2.1-3.png)

选取未知节点
XPath 通配符可用来选取未知的 XML 元素。
![](/assets/8.2.1-4.png)

在下面的表格中，我们列出了一些路径表达式，以及这些表达式的结果：
![](/assets/8.2.1-5.png)

选取若干路径
通过在路径表达式中使用“|”运算符，您可以选取若干个路径。
在下面的表格中，我们列出了一些路径表达式，以及这些表达式的结果：

![](/assets/8.2.1-6.png)


**XPath 高级用法**

- 模糊查询 contains
 
 目前许多web框架，都是动态生成界面的元素id，因此在每次操作相同界面时，ID都是变化的，这样为自动化测试造成了一定的影响。
 ```
<div class="eleWrapper" title="请输入用户名">
<input type="text" class="textfield" name="ID9sLJQnkQyLGLhYShhlJ6gPzHLgvhpKpLzp2Tyh4hyb1b4pnvzxFR!-166749344!1357374592067" id="nt1357374592068"  />
</div>
```
解决方法 使用xpath的匹配功能， //input[contains(@id,'nt')]

- 测试使用的XML
 ```
<Root>

<Person ID="1001" >

<Name lang="zh-cn" >张城斌</Name>

<Email xmlns="www.quicklearn.cn" > cbcye@live.com </Email>

<Blog>http://cbcye.cnblogs.com</Blog>

</Person>

<Person ID="1002" >

<Name lang="en" >Gary Zhang</Name>

<Email xmlns="www.quicklearn.cn" > GaryZhang@cbcye.com</Email>

<Blog>http://www.quicklearn.cn</Blog>

</Person>

</Root>
```
查询所有Blog节点值中带有 cn 字符串的Person节点
Xpath表达式：
/Root//Person[contains(Blog,'cn')]
2.查询所有Blog节点值中带有 cn 字符串并且属性ID值中有01的Person节点
Xpath表达式：
/Root//Person[contains(Blog,'cn') and contains(@ID,'01')]
学习笔记

1.依靠自己的属性，文本定位
   //td[text()='Data Import']

   //div[contains(@class,'cux-rightArrowIcon-on')]

   //a[text()='马上注册']

   //input[@type='radio' and @value='1']     多条件

   //span[@name='bruce'][text()='bruce1'][1]   多条件

    //span[@id='bruce1' or text()='bruce2']  找出多个

    //span[text()='bruce1' and text()='bruce2']  找出多个
2.依靠父节点定位
  //div[@class='x-grid-col-name x-grid-cell-inner']/div

  //div[@id='dynamicGridTestInstanceformclearuxformdiv']/div

  //div[@id='test']/input
3.依靠子节点定位
  //div[div[@id='navigation']]

  //div[div[@name='listType']]

  //div[p[@name='testname']]
4.混合型
  //div[div[@name='listType']]//img

  //td[a//font[contains(text(),'seleleium2从零开始 视屏')]]//input[@type='checkbox']
5.进阶部分
   //input[@id='123']/following-sibling::input   找下一个兄弟节点

   //input[@id='123']/preceding-sibling::span    上一个兄弟节点

   //input[starts-with(@id,'123')]               以什么开头

   //span[not(contains(text(),'xpath')）]        不包含xpath字段的span
6.索引

  //div/input[2]

  //div[@id='position']/span[3]

  //div[@id='position']/span[position()=3]

  //div[@id='position']/span[position()>3]

  //div[@id='position']/span[position()<3]

  //div[@id='position']/span[last()]

  //div[@id='position']/span[last()-1]
7.substring 截取判断
<div data-for="result" id="swfEveryCookieWrap"></div>
  //*[substring(@id,4,5)='Every']/@id  截取该属性 定位3,取长度5的字符 

  //*[substring(@id,4)='EveryCookieWrap']  截取该属性从定位3 到最后的字符 

  //*[substring-before(@id,'C')='swfEvery']/@id   属性 'C'之前的字符匹配

  //*[substring-after(@id,'C')='ookieWrap']/@id   属性'C之后的字符匹配
8.通配符*
  //span[@*='bruce']

  //*[@name='bruce']
9.轴

  //div[span[text()='+++current node']]/parent::div    找父节点

  //div[span[text()='+++current node']]/ancestor::div    找祖先节点
10.孙子节点
  //div[span[text()='current note']]/descendant::div/span[text()='123']

  //div[span[text()='current note']]//div/span[text()='123']          两个表达的意思一样
11.following pre
https://www.baidu.com/s?wd=xpath&pn=10&oq=xpath&ie=utf-8&rsv_idx=1&rsv_pq=df0399f30003691c&rsv_t=7dccXo734hMJVeb6AVGfA3434tA9U%2FXQST0DrOW%2BM8GijQ8m5rVN2R4J3gU
  //span[@class="fk fk_cur"]/../following::a       往下的所有a

  //span[@class="fk fk_cur"]/../preceding::a[1]    往上的所有a
xpath提取多个标签下的text

在写爬虫的时候，经常会使用xpath进行数据的提取，对于如下的代码：
<div id="test1">大家好！</div>
使用xpath提取是非常方便的。假设网页的源代码在selector中：
data = selector.xpath('//div[@id="test1"]/text()').extract()[0]
就可以把“大家好！”提取到data变量中去。
然而如果遇到下面这段代码呢？
<div id="test2">美女，<font color=red>你的微信是多少？</font><div>
如果使用：
data = selector.xpath('//div[@id="test2"]/text()').extract()[0]
只能提取到“美女，”；
如果使用：
data = selector.xpath('//div[@id="test2"]/font/text()').extract()[0]
又只能提取到“你的微信是多少？”
可是我本意是想把“美女，你的微信是多少？”这一整个句子提取出来。
<div id="test3">我左青龙，<span id="tiger">右白虎，<ul>上朱雀，<li>下玄武。</li></ul>老牛在当中，</span>龙头在胸口。<div>
而且内部的标签还不固定，如果我有一百段这样类似的html代码，又如何使用xpath表达式，以最快最方便的方式提取出来？
使用xpath的string(.)
以第三段代码为例：
data = selector.xpath('//div[@id="test3"]')
info = data.xpath('string(.)').extract()[0]
这样，就可以把“我左青龙，右白虎，上朱雀，下玄武。老牛在当中，龙头在胸口”整个句子提取出来，赋值给info变量。


[1]:http://www.w3school.com.cn/xpath/index.asp