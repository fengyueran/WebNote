
webpack提供了俩种方式的热更新

webpack-dev-server 与 webpack-dev-middleware

**1.webpack-dev-server**

webpack-dev-server 是一个开发服务器

安装
```
npm install --save-dev webpack-dev-server 
```
在要server的静态文件目录执行 webpack-dev-server 命令即可

默认支持两种模式的自动刷新

- Iframe mode
- Inline mode

启用Inline mode的热更新很简单，执行如下即可
```
webpack-dev-server --inline --hot
```
这里在说下通过node api的形式开启热更新其实本质是一样的，只不过命令行形式帮我们做了处理，代码如下
```
var config = require("./webpack.config.js");
config.entry.app.unshift("webpack-dev-server/client?http://localhost:8080/", "webpack/hot/dev-server");
var compiler = webpack(config);
var server = new webpackDevServer(compiler, {
  hot: true,
  publicPath: "/assets/",
});
server.listen(8080);
```
同时需要添加
 new webpack.HotModuleReplacementPlugin() 插件到webpack config里

```
var path = require( "path" );
var webpack = require( 'webpack' )
module.exports = {
	entry: {
		app: [ "./src/main.js" ]
	},
	output: {
		path: path.resolve( __dirname, "build" ),
		publicPath: "/assets/",
		filename: "bundle.js"
	},
	module: {
		loaders: [
			{
				test: /\.css$/,
				loader: 'style-loader!css-loader'
			}
    	]
	},
	plugins: [
    	new webpack.HotModuleReplacementPlugin()
    ]
};
```

下面说下 webpack-dev-middleware

webpack-dev-middleware 是一个中间件，可以集成到现有服务里，主要是监控资源变动进行重编译。

所以要用到热更新还需要另一个中间件 webpack-hot-middleware

如果你用的是koa 可以用下面对应的中间件

koa-webpack-dev-middleware

koa-webpack-hot-middleware

如果用到的是koa2，可以这个中间件

koa-webpack-middleware

其实koa的中间件，都是依赖的上面的中间件，只是进行了包装符合koa的用法而已

koa2热更新 demo传送门

基本原理是dev-middleware监听变动，重新编译，hot-middleware监听编译事件，把变动通知到每一个通过Server Sent Events 连接的客户端，客户端接收到消息，检查本地是否是新的，如果过期了触发webpack的热更新，也就是把变动的代码拉过来动态替换掉。

其实如果看demo可以发现修改css文件是热更新，修改js是页面刷新

是因为style-loader支持了热更新，如果让我们js支持更新加上如下代码即可。

if(module.hot) {
    module.hot.accept()
}
它有个冒泡机制，变动的模块会向上传递，直到有accept的处理，否则刷新页面

具体可以看下这俩篇大概了解下

http://webpack.github.io/docs/hot-module-replacement.html

http://webpack.github.io/docs/hot-module-replacement-with-webpack.html

本文链接：https://gmiam.com/post/webpack-hot-replacement.html

-- EOF --

作者 admin 发表于 2016-05-09 12:00:00 ，并被添加「 nodejs webpack 」标签 ，最后修改于 2016-05-11 00:15:07

« Koa 2.0 使用与内部分析基于 Koa 2.0 打造自己的框架应用 Robot »
Comments

网友跟贴0人参与

抵制低俗，文明上网，登录发贴
快速登录：
发表跟贴
最新最热
网易云跟贴，有你更精彩
© 2017 -  GM's Blog  - gmiam.com 
Powered by ThinkJS & FireKylin 0.15.5