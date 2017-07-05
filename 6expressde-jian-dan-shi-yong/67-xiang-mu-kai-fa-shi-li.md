**编写启动脚本**
上一节使用express命令自动建立项目，也可以不使用这个命令，手动新建所有文件。

先建立一个项目目录（假定这个目录叫做example6）。进入该目录，新建一个package.json文件，初始化项目的配置信息。
```
{
  "name": "example6",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "express": "^4.15.3"
  }
}

```
在项目目录中，新建文件index.js。项目的代码就放在这个文件里面。
```
var express = require('express');
var app = express();
```
上面代码首先加载express模块，赋给变量express。然后，生成express实例，赋给变量app。

接着，设定express实例的参数。
```
// 设定port变量，意为访问端口
app.set('port', process.env.PORT || 8000);

// 设定views变量，意为视图存放的目录
app.set('views', path.join(__dirname, 'views'));

// 设定view engine变量，意为网页模板引擎
app.set('view engine', 'jade');

app.use(express.favicon());
app.use(express.logger('dev'));
app.use(express.bodyParser());
app.use(express.methodOverride());
app.use(app.router);

// 设定静态文件目录，比如本地文件
// 目录为demo/public/images，访问
// 网址则显示为http://localhost:3000/images
app.use(express.static(path.join(__dirname, 'public')));
```
上面代码中的set方法用于设定内部变量，use方法用于调用express的中间件。

最后，调用实例方法listen，让其监听事先设定的端口（8000）。
```
app.listen(app.get('port'));
```
这时，运行下面的命令，就可以在浏览器访问http://localhost:8000。
```
node index.js
```
网页提示“Cannot GET /”，表示没有为网站的根路径指定可以显示的内容。所以，下一步就是配置路由。

**配置路由**
所谓“路由”，就是指为不同的访问路径，指定不同的处理方法。
1）指定根路径
在index.js之中，先指定根路径的处理方法。
```
app.get('/', function(req, res) {
   res.send('Hello World');
});
```
上面代码的get方法，表示处理客户端发出的GET请求。相应的，还有app.post、app.put、app.del（delete是JavaScript保留字，所以改叫del）方法。

get方法的第一个参数是访问路径，正斜杠（/）就代表根路径；第二个参数是回调函数，它的req参数表示客户端发来的HTTP请求，res参数代表发向客户端的HTTP回应，这两个参数都是对象。在回调函数内部，使用HTTP回应的send方法，表示向浏览器发送一个字符串。然后，运行下面的命令。
```
node index.js 
```
此时，在浏览器中访问http://localhost:8000，网页就会显示“Hello World”。

如果需要指定HTTP头信息，回调函数就必须换一种写法，要使用setHeader方法与end方法。
```
app.get('/', function(req, res){
  var body = 'Hello World';
  res.setHeader('Content-Type', 'text/plain');
  res.setHeader('Content-Length', body.length);
  res.end(body);
});
```