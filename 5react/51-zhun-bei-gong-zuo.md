- 安装webpack
```
npm install --dave-dev webpack
```
添加webpack.config.js文件
```
1
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