- 安装webpack
```
npm install --dave-dev webpack
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