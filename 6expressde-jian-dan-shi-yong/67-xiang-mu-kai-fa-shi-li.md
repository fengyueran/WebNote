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

