**主要有三种途径**

1.cli——即命令行形式，一般都会通过  package.json中写入scripts字段
 ```
 $ webpack entry.js ./dist/bundle.js
 ```
 
2.配置文件
在webpack.config.js文件中写入字段，webpack在启动时会搜索当前目录的 webpack.config.js 文件，这个文件是一个 node.js 模块，返回一个 json 格式的配置信息对象，或者通过 --config 选项来指定配置文件。
   - webpack.config.dev - debug配置
   - webpack.config.prod - release配置
   
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
