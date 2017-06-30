
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

或使用配置文件，首先，我们来看看基本的webpack.config.js的写法
```
    module.exports = {
        entry: './src/js/index.js',
        output: {
            path: './dist/js',
            filename: 'bundle.js'
        }
    }
```
配置文件提供一个入口和一个出口，webpack根据这个来进行js的打包和编译工作。虽然webpack提供了webpack --watch的命令来动态监听文件的改变并实时打包，输出新bundle.js文件，这样文件多了之后打包速度会很慢，此外这样的打包的方式不能做到hot replace，即每次webpack编译之后，你还需要手动刷新浏览器。

webpack-dev-server其中部分功能就能克服上面的2个问题。webpack-dev-server主要是启动了一个使用express的Http服务器。它的作用主要是用来伺服资源文件。此外这个Http服务器和client使用了websocket通讯协议，原始文件作出改动后，webpack-dev-server会实时的编译，但是最后的编译的文件并没有输出到目标文件夹，即上面配置的:
```
   output: {
        path: './dist/js',
        filename: 'bundle.js'
    }
```
注意：你启动webpack-dev-server后，你在目标文件夹中是看不到编译后的文件的,实时编译后的文件都保存到了内存当中。因此很多同学使用webpack-dev-server进行开发的时候都看不到编译后的文件
面来结合webpack的文档和webpack-dev-server里部分源码来说明下如何使用：

**启动：**
启动webpack-dev-server有2种方式：
- 通过cmd line
- 通过Node.js API

**配置：**
这个时候还要注意的一点就是在webpack.config.js文件里面，如果配置了output的publicPath这个字段的值的话，在index.html文件里面也应该做出调整。因为webpack-dev-server伺服的文件是相对publicPath这个路径的。因此，如果你的webpack.config.js配置成这样的：
```
  module.exports = {
        entry: './src/js/index.js',
        output: {
            path: './dist/js',
            filename: 'bundle.js'，
            publicPath: '/assets/'
        }
    }
```
那么，在index.html文件当中引入的路径也发生相应的变化:
```
 <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Demo</title>
    </head>
    <body>
        <script src="assets/bundle.js"></script>
    </body>
    </html>
```
如果在webpack.config.js里面没有配置output的publicPath的话，那么index.html最后引入的文件js文件路径应该是下面这样的。
```
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Demo</title>
    </head>
    <body>
        <script src="bundle.js"></script>
    </body>
    </html>
```
**Automatic Refresh:**
默认支持两种模式的自动刷新

- Iframe mode
- Inline mode

这2种模式配置的方式和访问的路径稍微有点区别，最主要的区别还是Iframe mode是在网页中嵌入了一个iframe，将我们自己的应用注入到这个iframe当中去，因此每次你修改的文件后，都是这个iframe进行了reload。
两种模式都支持热模块替换(Hot Module Replacement).热模块替换的好处是只替换更新的部分,而不是页面重载.
**inline模式**
inline模式下我们访问的URL不用发生变化,启用这种模式分两种情况:
1 当以命令行启动webpack-dev-server时,需要做两点：
- 在命令行中添加--inline命令
- 在webpack.config.js中添加devServer:{inline:true}

启用Inline mode的热更新很简单，执行如下即可
```
webpack-dev-server --inline --hot
```
2 当以Node.js API启动webpack-dev-server时,我们也需要做两点:
- 由于webpack-dev-server的配置中无inline选项,我们需要添加webpack-dev-server/client?http://«path»:«port»/到webpack配置的entry入口点中.
- 将webpack/hot/dev-server添加到webpack配置的entry入口点中

```
import webpack from 'webpack';
import config from './webpack-dev-server.config.js';
import webpackDevServer from 'webpack-dev-server';

config.entry.app.unshift("webpack-dev-server/client?http://localhost:8080", "webpack/hot/dev-server");
var compiler = webpack(config);
var server = new webpackDevServer(compiler, {
  hot: true,
  contentBase: "./src",//默认根目录是在webpack.config同一级的目录，去此目录寻找index.html
  // publicPath: "./",

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
比起直接编译，webpack-dev-middleware 多了一些好处:

- 不需要一直写入磁盘，所有产生的结果会直接存在内存
- 在监视模式(watch mode)下如果发生更改，middleware 会马上停止提供旧版的 bundle 並且会延迟请求的回应直到编译完成



webpack-hot-middleware是一个结合webpack-dev-middleware使用的middleware，它可以实现浏览器的无刷新更新（hot reload）。这也是webpack文档里常说的HMR（Hot Module Replacement）。
我们都知道 webpack dev server 有提供一种Hot Module Replacement/Hot Reloading 热替换的功能。在一般 webpack-dev-server 的时候我们会在 webpack.config.js 加入 new webpack.HotModuleReplacementPlugin() 来启用。

就是讓我們在一般的 server 上加上熱替換的功能，總結來說就是 webpack-dev-middleware + webpack-hot-middleware 即可讓我們用 express 客製一個有熱替換功能的 webpack 開發伺服器。
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
这种blob的形式可能会使得css里的url()引用的图片失效，因此建议用带http的绝对地址（这也只有开发环境会用到）。有关这个问题的详情，你可以查看github上的[issue][1]。

**使用browserSync热更新**
配置
```
/* eslint-disable */
import webpack from 'webpack';
import config from './webpack.config.dev';
import browserSync from 'browser-sync';
import webpackHotMiddleware from 'webpack-hot-middleware';
import webpackDevMiddleware from 'webpack-dev-middleware';

const bundler = webpack(config);

// Run Browsersync and use middleware for Hot Module Replacement
browserSync({
  startPath: 'index.html',
  server: {
    baseDir: ['./src', './'],//服务路径，也就是页面资源存放的路径
    middleware: [
      webpackDevMiddleware(bundler, {
        // Dev middleware can't access config, so we provide publicPath
        publicPath: config.output.publicPath,

        // pretty colored output
        stats: { colors: true },

        // Set to false to display a list of each file that is being bundled.
        noInfo: true,

        // for other settings see
        // http://webpack.github.io/docs/webpack-dev-middleware.html
      }),

      // bundler should be the same as above
      webpackHotMiddleware(bundler),

      // historyApiFallback(),
    ],
  },

  // no need to watch '*.js' here, webpack will take care of it for us,
  // including full page reloads if HMR won't work
  files: [

  ],
});
```


[1]:https://github.com/webpack-contrib/style-loader/issues/55