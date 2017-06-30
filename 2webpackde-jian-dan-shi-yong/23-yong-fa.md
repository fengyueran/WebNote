**主要有三种途径**

1.CLI
CLI即Command Line Interface，顾名思义，也就是命令行用户界面，例如：
 ```
 $ webpack entry.js ./dist/bundle.js
 ```
 
2.配置文件
如果我们直接执行webpack，不加任何参数（且当前目录不存在配置文件），则会显示webpack的帮助信息，里面有非常多的参数可用。事实上，除了我们前面这种出于演示的目的直接在命令行中写参数之外，大部分生产环境下使用时都会需要加上非常多的参数，导致整个命令非常长，既不利于记忆编写，也不利于传播交接等。因此，一般会将配置项写在同目录的webpack.config.js中，然后执行webpack即可，webpack会从该配置文件中读取参数，此时不需要在命令行中传入任何参数。

```
//执行时webpack会去寻找当前目录下的webpack.config.js当作配置文件使用
webpack

// 也可以用参数--config指定配置文件
webpack --config mycofnig.js
```
配置文件webpack.config.js的写法则是：
```
module.exports = {
    // 配置项
};
```
 值得注意的是，配置文件是一个真正的JS文件，因此配置项只要是一个对象即可，并不要求是JSON。也就意味着你可以使用表达式，也可以进行动态计算，或者甚至使用继承的方式生成配置项。  
 配置文件实例：
 ```
 module.exports = {
   entry: "./example1/entry.js",//入口文件
   output: {
      path: __dirname,//输出地址，必须是绝对地址
      filename: "bundle.js" //输出文件名
      publicPath: './',//网站运行时的访问路径 
},
//webpack在构建包的时候会按目录的进行文件的查找，resolve属性中的extensions数组中用于配置程序可以自行补全哪些文件后缀：
  resolve: {//
      	//查找module的话从这里开始查找
      	root: '/pomy/github/flux-example/src', //绝对路径
      	//自动扩展文件后缀名，意味着我们require模块可以省略不写后缀名
      	extensions: ['', '.js', '.json', '.scss'],
      	//模块别名定义，方便后续直接引用别名，无须多写长长的地址
      	alias: {
      		AppStore : 'js/stores/AppStores.js',//后续直接 require('AppStore') 即可
      		ActionType : 'js/actions/ActionType.js',
      		AppAction : 'js/actions/AppAction.js'
      	}
      }
  module: {
    loaders: [
    { test: /\.css$/, loader:   "style!css" }
     ]
   }
};
 ```
 
 **resolve:**
alias:
```
alias: {
              AppStore : 'js/stores/AppStores.js',//后续直接 require('AppStore') 即可
              ActionType : 'js/actions/ActionType.js',
              AppAction : 'js/actions/AppAction.js'
     }

```
webpack的alias的作用，通过key，value的形式，将模块名和路径对应起来，不管是相对路径还是绝对路径，因此，在模块引用的时候，利用require引用的模块可以不用通过相对路径或者绝对路径的方式，而是直接通过require('模块名')的方式进行引用。

**entry**
Entry配置项告诉Webpack应用的根模块或起始点在哪里，它的值可以是字符串、数组或对象。这看起来可能令人困惑，因为不同类型的值有着不同的目的。
像绝大多数app一样，倘若你的应用只有一个单一的入口，entry项的值你可以使用任意类型，最终输出的结果都是一样的。
在webpack中，所有的js文件都可以通过一个js文件进行引用，如下面的例子那样：
```
/**
* index.js
*/
require('moduleA');
require('../moduleB');
require('../index.less');

var $ = require('jquery');
```
```
/**
* webpack.config.js
*/
{
entry: {
indexA: ['./app/index.js']
}
}
```

entry：数组类型
但是，如果你想添加多个彼此不互相依赖的文件，你可以使用数组格式的值。


```
var path = require('path')
module.exports = {
  entry:{
    pageA: './pageA.js',
    pageB: './pageB.js',
    //支持数组形式，将加载数组中的所有模块，但以最后一个模块作为输出
    //该方法可以添加多个彼此不互相依赖的文件 
    pageC: ['./pageA.js', './pageB.js'],
  },
  output:{
          filename: '[name].bundle.js',
          path: path.join(__dirname, "/dist"),
        },
  resolve: {
          //resolve 指定可以被 import 的文件后缀
           extensions: ['.js', '.jsx']
        },
};
``` 
entry：对象类型
现在，假设你的应用是多页面的（multi-page application）而不是SPA，有多个html文件（index.html和profile.html）。然后你通过一个对象告诉Webpack为每一个html生成一个bundle文件。当使用多入口点的时候，需要重载output.filename，否责每个入口点都写入到同一个输出文件里面了。使用[name]来得到入口点名字。


```
var path = require('path')
module.exports = {
  entry:{
    pageA: './pageA.js',
    pageB: './pageB.js',
  },
  output:{
          filename: '[name].bundle.js',
          path: path.join(__dirname, "/dist"),
        },
};
``` 




**output**
path
```
path: './dist',
```
 webpack打包后，生成的js文件，css文件，字符文件，图片文件会打包放在path字段所指定的文件目录中。
 
** filename**
```
entry: {
    indexA: ['./assets/javascripts/index.js']
},
output: {
    path: './build/public',
    publicPath: '/',
    filename: '/javascripts/[name].js'
}
```
确定webpack生成的js文件的准确路径和文件名，其文件路径根据之前的path和filename进行合成，文件为entry中的指定的index.js文件的模块名，如上面配置项所示，index.js文件生成的路径为 build/public/javascripts/indexA.js



 3.API
 API则是指将webpack作为Node.js模块使用，例如：
 ```
 webpack({
    // 配置项
    entry:'main.js',
    ...
},callback);
 ```
这种用法并不少见，尤其是在和构建工具（如Gulp）搭配使用的时候，都是使用的这种方式。你也可以自己写一段程序来调用webpack。相比CLI模式而言，使用API模式会更加灵活，因为可以与你的各种工具进行集成，甚至共享一些环境变量然后动态生成打包配置等等，在复杂项目中会很有用。
同理，API模式中的配置项对象也是一个真正的对象。