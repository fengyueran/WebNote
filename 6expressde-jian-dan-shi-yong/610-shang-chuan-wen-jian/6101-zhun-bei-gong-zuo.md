**安装必要模块**
```
{
  "name": "example10",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "babel-node app.js"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "express": "^4.15.3",
    "formidable": "^1.1.1",
    "rc-progress": "^2.2.1",
    "react": "^15.6.1",
    "react-dom": "^15.6.1",
    "react-fileupload-progress": "^0.4.0"
  },
  "devDependencies": {
    "babel-cli": "^6.24.1",
    "babel-core": "^6.25.0",
    "babel-loader": "^7.1.1",
    "babel-preset-es2015": "^6.24.1",
    "babel-preset-react": "^6.24.1",
    "babel-preset-stage-1": "^6.24.1",
    "webpack": "^3.0.0",
    "webpack-dev-middleware": "^1.11.0",
    "webpack-hot-middleware": "^2.18.1"
  }
}

```
webpack配置文件
```
//webpack.config.js
/* eslint-disable */
import path from 'path';
import webpack from 'webpack';

var publicPath = 'http://localhost:8000/';
var hotMiddlewareScript = 'webpack-hot-middleware/client?reload=true';

export default {
  entry: [
    hotMiddlewareScript,
    path.join(__dirname, './src/index.js')
  ],
  output: {
    filename: 'bundle.js',
    path: path.join(__dirname,"dist"),
    publicPath: publicPath,
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
    ]
};

```