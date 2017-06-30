plugins是webpack中和loader同等地位的组成部分，他可以对我们生成的文件或者我们需要怎样生成文件进行跟细致化的处理。

```
new webpack.optimize.CommonsChunkPlugin("commons", "javascripts/commons.js")
```
该插件可以对entry中入口文件中的共同部分进行分离，达到在不同页面中可以共用同一套commons.js，利用缓存减少文件从服务器中获取。