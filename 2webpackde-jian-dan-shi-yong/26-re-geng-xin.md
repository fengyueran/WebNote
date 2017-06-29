
webpack提供了俩种方式的热更新

webpack-dev-server 与 webpack-dev-middleware

**1.webpack-dev-server**

它是一个静态资源服务器，只用于开发环境。

一般来说，对于纯前端的项目（全部由静态html文件组成），简单地在项目根目录运行webpack-dev-server，然后打开html，修改任意关联的源文件并保存，webpack编译就会运行，并在运行完成后通知浏览器刷新。

和直接在命令行里运行webpack不同的是，webpack-dev-server会把编译后的静态文件全部保存在内存里，而不会写入到文件目录内。这样，少了那个每次都在变的webpack输出目录，会不会觉得更清爽呢？

如果在请求某个静态资源的时候，webpack编译还没有运行完毕，webpack-dev-server不会让这个请求失败，而是会一直阻塞它，直到webpack编译完毕。这个对应的效果是，如果你在不恰当的时候刷新了页面，不会看到错误，而是会在等待一段时间后重新看到正常的页面，就好像“网速很慢”。


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

**2.webpack-dev-middleware**

webpack-dev-middleware是一个处理静态资源的middleware。前面说的webpack-dev-server，实际上是一个小型Express服务器，它也是用webpack-dev-middleware来处理webpack编译后的输出。

webpack-hot-middleware是一个结合webpack-dev-middleware使用的middleware，它可以实现浏览器的无刷新更新（hot reload）。这也是webpack文档里常说的HMR（Hot Module Replacement）。

webpack配置文件部分:

```
import path from 'path';
import webpack from 'webpack';
// reload=true 的意思是，如果碰到不能hot reload的情况，就整页刷新。
const hotMiddlewareScript = 'webpack-hot-middleware/client?reload=true';
// __dirname是当前运行的js所在的目录
import path from 'path';
import webpack from 'webpack';
// reload=true 的意思是，如果碰到不能hot reload的情况，就整页刷新。
const publicPath = 'http://localhost:3000/';
const hotMiddlewareScript = 'webpack-hot-middleware/client?reload=true';
// __dirname是当前运行的js所在的目录
export default {
  entry: [
    hotMiddlewareScript,
    path.join(__dirname, '/app/index.js'),
  ],
  output: {
    filename: 'bundle.js',
    path: `${__dirname}/dist`,
    publicPath,
  },
  devtool: 'source-map',
  resolve: {
          // resolve 指定可以被 import 的文件后缀
    extensions: ['.js', '.jsx'],
  },
  module: {
    // loaders是一个数组，每个元素都用来指定loader
    loaders: [{
      test: /\.(jsx|js)$/, // test值为正则表达式，当文件路径匹配时启用
      loaders: ['babel-loader', 'eslint-loader'], // 指定使用什么loader，可以用字符串，也可以用数组
      exclude: /regexp/, //可以使用exclude来排除一部分文件
    }],

  },
  plugins: [
    new webpack.HotModuleReplacementPlugin(),
    new webpack.NoEmitOnErrorsPlugin(),
  ],
};


```
在这个配置文件中，还有一个要点是publicPath不是/这样的值，而是http://localhost:3000/这样的绝对地址。这是因为，在使用?sourceMap的时候，style-loader会把css的引入做成这样：
![](/2webpackde-jian-dan-shi-yong/26-1.png)
这种blob的形式可能会使得css里的url()引用的图片失效，因此建议用带http的绝对地址（这也只有开发环境会用到）。有关这个问题的详情，你可以查看github上的issue。