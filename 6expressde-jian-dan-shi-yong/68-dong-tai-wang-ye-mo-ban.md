网站真正的魅力在于动态网页，下面我们来看看，如何制作一个动态网页的网站。

**安装模板引擎**
Express支持多种模板引擎，这里采用Handlebars模板引擎的服务器端版本hbs模板引擎。

先安装hbs。
```
npm install hbs --save-dev
```

安装模板引擎之后，就要改写index.js。

```
//index.js文件

var express = require('express');
var app = express();

// 加载hbs模块
var hbs = require('hbs');

// 指定模板文件的后缀名为html
app.set('view engine', 'html');

// 运行hbs模块
app.engine('html', hbs.__express);

app.get('/', function (req, res){
	res.render('index');
});

app.get('/about', function(req, res) {
	res.render('about');
});

app.get('/article', function(req, res) {
	res.render('article');
});
```
上面代码改用render方法，对网页模板进行渲染。render方法的参数就是模板的文件名，默认放在子目录views之中，后缀名已经在前面指定为html，这里可以省略。所以，res.render(‘index’) 就是指，把子目录views下面的index.html文件，交给模板引擎hbs渲染。

**新建数据脚本**
渲染是指将数据代入模板的过程。实际运用中，数据都是保存在数据库之中的，这里为了简化问题，假定数据保存在一个脚本文件中。

在项目目录中，新建一个文件blog.js，用于存放数据。blog.js的写法符合CommonJS规范，使得它可以被require语句加载。
```
// blog.js文件

var entries = [
	{"id":1, "title":"第一篇", "body":"正文", "published":"6/2/2013"},
	{"id":2, "title":"第二篇", "body":"正文", "published":"6/3/2013"},
	{"id":3, "title":"第三篇", "body":"正文", "published":"6/4/2013"},
	{"id":4, "title":"第四篇", "body":"正文", "published":"6/5/2013"},
	{"id":5, "title":"第五篇", "body":"正文", "published":"6/10/2013"},
	{"id":6, "title":"第六篇", "body":"正文", "published":"6/12/2013"}
];

exports.getBlogEntries = function (){
   return entries;
}
 
exports.getBlogEntry = function (id){
   for(var i=0; i < entries.length; i++){
      if(entries[i].id == id) return entries[i];
   }
}
```
**新建网页模板**
接着，新建模板文件index.html。
```
<!-- views/index.html文件 -->

<h1>文章列表</h1>
 
{{#each entries}}
   <p>
      <a href="/article/{{id}}">{{title}}</a><br/>
      Published: {{published}}
   </p>
{{/each}}
```
模板文件about.html。
```
<!-- views/about.html文件 -->

<h1>自我介绍</h1>
 
<p>正文</p>
```
模板文件article.html。
```
<!-- views/article.html文件 -->

<h1>{{blog.title}}</h1>
Published: {{blog.published}}
 
<p/>
 
{{blog.body}}
```
可以看到，上面三个模板文件都只有网页主体。因为网页布局是共享的，所以布局的部分可以单独新建一个文件layout.html。
```
<!-- views/layout.html文件 -->

<html>
 
<head>
   <title>{{title}}</title>
</head>
 
<body>
 
	{{{body}}}
 
   <footer>
      <p>
         <a href="/">首页</a> - <a href="/about">自我介绍</a>
      </p>
   </footer>
    
</body>
</html>
```

**渲染模板**
最后，改写index.js文件。
```
var express = require('express');
var app = express();

// 加载hbs模块
var hbs = require('hbs');
// 加载数据模块
var blogEngine = require('./blog');
// 指定模板文件的后缀名为html
app.set('view engine', 'html');

// 运行hbs模块
app.engine('html', hbs.__express);

app.get('/', function(req, res) {
   res.render('index',{title:"最近文章", entries:blogEngine.getBlogEntries()});
});

app.get('/about', function(req, res) {
   res.render('about', {title:"自我介绍"});
});

app.get('/article/:id', function(req, res) {
   var entry = blogEngine.getBlogEntry(req.params.id);
   res.render('article',{title:entry.title, blog:entry});
});


app.listen(8000,function(){
   console.log('Example app listening on port 8000.');
});
```
