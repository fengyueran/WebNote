# node常用命令
Node.js是JavaScript的一种运行环境，是对Google V8引擎进行的封装，是一个服务器端的javascript的解释器。在Node.js上开发时时常需要应用到别人写好的包，这就需要npm，即nodejs的包管理器（package manager），通过npm就能使用大家上传到npm官网上的代码。

#####1.node常用命令
1）显示node版本 - node -v

 ```
 $ node -v
    -> v6.10.0
 //npm版本号   
 $ npm -v
    -> 3.10.10
 ```
 
 #####2.npm常用命令

1) 安装包 - npm install packageName
```
//当前工程安装bebel
$ npm install babel

//全局安装bebel
$ npm install babel -g

//安装某个版本
$ npm install babel@1.6.0

```
2) 卸载包 - npm uninstall packageName
```
//卸载当前工程bebel
$ npm uninstall babel
//卸载全局bebel
$ npm uninstall babel -g
```
3) 查看当前安装的包 - npm ls
```
├─┬ babel-cli@6.24.0
//查看安装的grep
$ npm ls | grep gulp
  ├─┬ gulp@3.0.0
  │ ├─┬ gulp-util@1.1.1
```
4) 查看安装的包的信息  - npm info
```
//查看babel的信息
$ npm info babel
├─┬ babel-cli@6.24.0
```
5) 初始化package.json  - npm init
```
//根据回答创建package.json文件(这个文件描述了项目相关信息)
$ npm init
{
  "name": "cordova",
  "version": "1.0.0",
  "description": "abc",
  "main": "npm start",
  //项目依赖
  "dependencies": {
    "underscore": "^1.8.3"
  },
  //项目开发时的依赖
  "devDependencies": {
    "babel": "^6.23.0",
    "babel-cli": "^6.24.0"
  },
  "scripts": {
    "test": "npm start"
  },
  "author": "",
  "license": "ISC"
}
```

6) 安装包并保存到package.json文件  - npm install packageName --save
```
//安装underscore并保存该依赖到package.json
$ npm install underscore --save
//删除underscore并从package.json中删除依赖
$ npm uninstall underscore --save
//安装babel并保存babel到package.json中的开发依赖
$ npm install babel --save-dev
```

