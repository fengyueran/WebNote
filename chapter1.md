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
```
2) 卸载包 - npm uninstall packageName
```
//卸载当前工程bebel
$ npm uninstall babel
//卸载全局bebel
$ npm uninstall babel -g
```
