##Redux实例

以下这张图表示了整个 React Redux App 的资料流程图（使用者与 View 互动 => dispatch 出 Action => Reducers 依据 action tyoe 分配到对应处理方式，回传新的 state => 通过 React Redux 传送给 React，React 重新绘制 View）：
![](/assets/5.8.1-1.png)


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
    "babel-preset-es2015": "^6.24.1",
    "babel-preset-react": "^6.24.1",
    "babel-preset-stage-1": "^6.24.1",
    "browser-sync": "^2.18.12",
    "eslint": "^3.19.0",
    "eslint-config-airbnb": "^15.0.1",
    "webpack": "^3.0.0",
    "webpack-dev-middleware": "^1.11.0",
    "webpack-hot-middleware": "^2.18.1"
  }
}
```