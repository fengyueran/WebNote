基本路由
路由( Routing )关系到客户端请求某个特定 URI 或 path 时，应用程序决定如何响应的问
题。 每个路由均可拥有一个或多个处理函数，当路由匹配时就会被执行。
路由定义采取如下结构：
app.METHOD(PATH,HANDLER)
app 是 express 的实例
METHOD 是某个 HTTP 方法，小写形式
PATH 是服务器上的某个路径
HANDLER 是路由匹配时候要执行的处理函数
本教程假设已经创建了 express 的实例 app ，并且服务处于运行状态。如果你对创建
和启动应用程序还不熟悉的话，请参考 Hello World 示例
下面展示如何定义一些简单的路由:
以 get 方法请求根路由时，在主页显示 Hello World!
```
app.get('/',function(req,res){
res.send('Hello World!');
});
```
以 post 方法请求根路由时，在主页显示 ' Got a POST request '
```
app.post('/', function (req, res) {
res.send('Got a POST request');
});
```
以 put 方法请求 /user 路由时，显示 Got a PUT request at /user
```
app.put('/user', function (req, res) {
res.send('Got a PUT request at /user');
});
```
以 DELETE 方法请求 /user 路由时，显示 Got a DELETE request at /user
```
app.delete('/user', function (req, res) {
res.send('Got a DELETE request at /user');
});
```
这时，最好就把路由放到一个单独的文件中，比如新建一个routes子目录。
```
// routes/index.js

module.exports = function (app) {
  app.get('/', function (req, res) {
    res.send('Hello world');
  });
  app.get('/customer', function(req, res){
    res.send('customer page');
  });
  app.get('/admin', function(req, res){
    res.send('admin page');
  });
};
```
更多有关路由的细节，参看[路由指南][1]

[1]:http://expressjs.com/en/guide/routing.html