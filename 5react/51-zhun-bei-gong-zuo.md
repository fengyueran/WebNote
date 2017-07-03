- 安装eslint
```
npm install --dave-dev eslint
```
安装eslint-loder
```
npm install --dave-dev eslint-loader
```
创建.eslintrc
```
{
"env": {
  "node": true,
  "browser": true,
  },
"extends": "airbnb",
}
```
安装eslint-config-airbnb
```
npm install --save-dev eslint-config-airbnb
```

- 安装webpack
```
npm install --dave-dev webpack
```

 添加webpack.config.js文件
 
 ```
import path from 'path';
import webpack from 'webpack';
// reload=true 的意思是，如果碰到不能hot reload的情况，就整页刷新。
//publicPath设为'/'不热更新
const publicPath = 'http://localhost:3000/';
const hotMiddlewareScript = 'webpack-hot-middleware/client?reload=true';
export default {
  entry: [
    hotMiddlewareScript,
    path.join(__dirname, '/createButton.jsx'),
  ],
  output: {
    filename: 'bundle.js',
    path: path.join(__dirname,"dist"),
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
      // loader: 'eslint-loader', //指定使用什么loader，可以用字符串，也可以用数组
        },
        {
      test: /\.(jsx|js)$/, //test值为正则表达式，当文件路径匹配时启用
      loader: 'babel-loader', //指定使用什么loader，可以用字符串，也可以用数组
      exclude: /regexp/, //可以使用exclude来排除一部分文件
      }],
  },
  plugins: [
    new webpack.HotModuleReplacementPlugin(),
  ],
};
```
安装browser-sync
```
npm install --save-dev browser-sync
```
安装webpack-dev-middleware
```
npm install --save-dev webpack-dev-middleware
```
安装webpack-hot-middleware
```
npm install --save-dev webpack-hot-middleware
```


- 安装babel-cli
```
 npm install --dave-dev babel-cli
```
安装babel-loader
```
 npm install --dave-dev babel-loader
```
添加.bablerc文件
```
{
  "presets": ["es2015", "react", "stage-1"],
  "plugins": []
}
```
ES2015转码规则:
```
$ npm install --save-dev babel-preset-es2015
```
react转码规则:
```
$ npm install --save-dev babel-preset-react
```
ES7不同阶段语法提案的转码规则（共有4个阶段），选装一个
```
$ npm install --save-dev babel-preset-stage-1
```
- 安装react
```
npm install --save react
```
安装react-dom
```
npm install --save react-dom
```