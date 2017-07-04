**Hello World示例**
使用express创建的最简单的应用。与使用 Express Generator 生成的目录结构不同，
Hello World 项目只有一个文件。 Express Generator 会生成一个应用程序的目录架构，
若干 javascript 文件， jade 模板以及不同用途的子目录。

在 example1目录下，新建 app.js 并添加如下代码:
```
var express = require('express');
var app = express();
app.get('/',function(req,res){
   res.send('Hello World!');
});
app.listen(8000,function(){
   console.log('Example app listening on port 3000.');
});
```
应用程序启动了一个服务，并在3000端口监听连接。当请求该服务的根目录 / 时，应用程
序会返回 Hello World 字符串作为响应。对于其他任意请求路径，则返回 404 Not Found.
req (request) 和 res (response) 其实是相同的对象，均由 Node.js 提供。所以这两
个对象的使用方法不受 express 限制，你可以像在node中使用它们一样在express中随
意使用它们。 例如 req.pipe() ， req.on('data',callback) 这样调用。
最后，确保工作目录是 myapp 目录，在命令行运行 node app.js ,打开浏览器加载
http://localhost:3000 可以看到熟悉的问候语。