**1. 初始化项目**
```
$ npm init
```

**2. 安装所需模块**
```
{
  "name": "example",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "babel-node server.js"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "react": "^15.6.1",
    "react-dom": "^15.6.1",
    "react-redux": "^5.0.5",
    "redux": "^3.7.1"
  },
  "devDependencies": {
    "babel-cli": "^6.24.1",
    "babel-core": "^6.25.0",
    "babel-loader": "^7.1.1",
    "babel-preset-es2015": "^6.24.1",
    "babel-preset-react": "^6.24.1",
    "babel-preset-stage-1": "^6.24.1",
    "browser-sync": "^2.18.12",
    "eslint": "^3.19.0",
    "eslint-config-airbnb": "^15.0.1",
    "eslint-loader": "^1.8.0",
    "webpack": "^3.0.0",
    "webpack-dev-middleware": "^1.11.0",
    "webpack-hot-middleware": "^2.18.1"
  }
}
```

**3. 构建工程目录**
首先在根目录建立src，放置 script的source。
```
actions:存放所有action
components:存放所有组件
containers:负责和store互动取得state
reducers:存放所有reducer
selectors:返回需要的state

```
其余配置文件都放到根目录下，.babelrc、.eslintrc、webpack.config.js等的配置参考2、3、4章节。
![](/assets/5.8-2.png)

**4. 配置文件**
webpack.config.js:
```
/* eslint-disable */
import path from 'path';
import webpack from 'webpack';
// reload=true 的意思是，如果碰到不能hot reload的情况，就整页刷新。
const publicPath = 'http://localhost:3002/';
const hotMiddlewareScript = 'webpack-hot-middleware/client?reload=true';
// __dirname是当前运行的js所在的目录
export default {
  entry: [
    hotMiddlewareScript,
    path.join(__dirname, './src/index'),
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
.eslintrc:
```
{
"env": {
"node": true,
"browser": true,
},
"extends": "airbnb",
"rules": {
  "no-unused-vars": 'off',
   },
}

```
