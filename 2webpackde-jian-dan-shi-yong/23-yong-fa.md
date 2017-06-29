**主要有三种途径**

1.CLI
CLI即Command Line Interface，顾名思义，也就是命令行用户界面，例如：
 ```
 $ webpack entry.js ./dist/bundle.js
 ```
 
2.配置文件
如果我们直接执行webpack，不加任何参数（且当前目录不存在配置文件），则会显示webpack的帮助信息，里面有非常多的参数可用。事实上，除了我们前面这种出于演示的目的直接在命令行中写参数之外，大部分生产环境下使用时都会需要加上非常多的参数，导致整个命令非常长，既不利于记忆编写，也不利于传播交接等。因此，一般会将配置项写在同目录的webpack.config.js中，然后执行webpack即可，webpack会从该配置文件中读取参数，此时不需要在命令行中传入任何参数。

```
//执行时webpack会去寻找当前目录下的webpack.config.js当作配置文件使用
webpack

// 也可以用参数-c指定配置文件
webpack -c mycofnig.js
```

   
 配置文件实例：
 ```
 module.exports = {
   entry: "./example1/entry.js",//入口文件
   output: {
      path: __dirname,//刷出地址
      filename: "bundle.js" //输出文件名
},
  module: {
    loaders: [
    { test: /\.css$/, loader:   "style!css" }
     ]
   }
};

 ```
