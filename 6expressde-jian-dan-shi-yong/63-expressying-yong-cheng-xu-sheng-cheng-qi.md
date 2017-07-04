使用应用程序生成器工具—— express-generator 快速创建应用程序框架
安装 express-generator
```
$ npm install express-generator -g
```
使用 -h 选项显示所有命令选项
生成example2
```
$ express example2
```
![](/assets/6.3.1-1.png)

按照提示进入example2 目录，运行 npm install ,然后在命令行输入 DEBUG=myapp:* npm start
启动应用。打开 http://localhost:3000 可以看到: