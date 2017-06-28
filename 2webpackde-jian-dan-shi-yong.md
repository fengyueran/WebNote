####1.webpack安装与配置
这里用node-npm来安装webpack
- 新建工程目录-WebPackDemo
- 在工程目录中初始化配置npm init，生成package.json文件
```
{
  "name": "webpackdemo",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC"
}
```
- 安装webpack
```
npm install webpack --save-dev
```
- 配置webpack
可以有三种途径

 1) cli——即命令行形式，一般都会通过  package.json中写入scripts字段
 ```
 $ webpack entry.js ./dist/bundle.js
 ```
 
 2) 配置文件webpack.config.js文件中写入字段，webpack在启动时会读取它，并依据其工作。
默认情况下，会搜索当前目录的 webpack.config.js 文件，这个文件是一个 node.js 模块，返回一个 json 格式的配置信息对象，或者通过 --config 选项来指定配置文件。
   - webpack.config.dev - debug配置
   - webpack.config.prod - release配置

- node api——其实配置文件也算node api，更广义的来讲，node api是一套配置文件的生成系统，根据不同的输入（从cli传参数--a=b），实现不同的配置。
 
####2.配置webpack-dev-server这个服务器工具
webpack-dev-server可以让浏览器实时刷新，显示我们对文件的改动。
webpack-dev-server是一个小型的node.js Express服务器,为webpack打包生成的资源文件提供Web服务。webpack-dev-server发送关于编译状态的消息到客户端，客户端根据消息作出响应。
- 安装webpack-dev-server
```
npm install webpack-dev-server --save-dev
```
- 配置服务（选择其一）
 - 1.配置文件 ——Node API
 - cmd指令（推荐）
 
####2.实例
 [demo地址][1]
 从空白文件夹example1开始，创建以下文件：增加 entry.js:
 ```
 alert('hello world');
 ```
增加 index.html
```
<html>
<head>
<title>webpack.toobug.net</title>
<script src="./bundle.js"></script>
</head>
<body>
</body>
</html>
```
然后执行以下命令：
```
$ ./example1/entry.js ./example/bundle.js
```
 
 
 [1]:https://github.com/fengyueran/WebPackDemo