**底层：http模块**
Express框架建立在node.js内置的http模块上。http模块生成服务器的原始代码如下。
```
var http = require("http");

var app = http.createServer(function(request, response) {
  response.writeHead(200, {"Content-Type": "text/plain"});
  response.end("Hello world!");
});

app.listen(3000, "localhost");
```
上面代码的关键是http模块的createServer方法，表示生成一个HTTP服务器实例。该方法接受一个回调函数，该回调函数的参数，分别为代表HTTP请求和HTTP回应的request对象和response对象。

Express框架的核心是对http模块的再包装。上面的代码用Express改写如下。
```
var express = require('express');
var app = express();

app.get('/', function (req, res) {
  res.send('Hello world!');
});

app.listen(3000);
```
比较两段代码，可以看到它们非常接近。原来是用http.createServer方法新建一个app实例，现在则是用Express的构造方法，生成一个Epress实例。两者的回调函数都是相同的。Express框架等于在http模块之上，加了一个中间层。

**什么是中间件**
简单说，中间件（middleware）就是处理HTTP请求的函数。它最大的特点就是，一个中间件处理完，再传递给下一个中间件。App实例在运行过程中，会调用一系列的中间件。

每个中间件可以从App实例，接收三个参数，依次为request对象（代表HTTP请求）、response对象（代表HTTP回应），next回调函数（代表下一个中间件）。每个中间件都可以对HTTP请求（request对象）进行加工，并且决定是否调用next方法，将request对象再传给下一个中间件。

一个不进行任何操作、只传递request对象的中间件，就是下面这样。
```
function uselessMiddleware(req, res, next) {
  next();
}
```
上面代码的next就是下一个中间件。如果它带有参数，则代表抛出一个错误，参数为错误文本。
```
function uselessMiddleware(req, res, next) {
  next('出错了！');
}
```
抛出错误以后，后面的中间件将不再执行，直到发现一个错误处理函数为止。

**use方法**
use是express注册中间件的方法，它返回一个函数。下面是一个连续调用两个中间件的例子。
```
var express = require("express");
var http = require("http");

var app = express();

app.use(function(request, response, next) {
  console.log("In comes a " + request.method + " to " + request.url);
  next();
});

app.use(function(request, response) {
  response.writeHead(200, { "Content-Type": "text/plain" });
  response.end("Hello world!\n");
});

app.listen(8000,function(){
   console.log('Example app listening on port 8000.');
});
```