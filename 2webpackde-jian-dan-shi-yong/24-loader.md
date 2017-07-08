

loader是webpack中一个重要的概念，它是指用来将一段代码转换成另一段代码的webpack插件。晕了没？为什么需要将一段代码转换成另一段代码呢？这是因为webpack实际上只能处理JS文件，如果需要使用一些非JS文件（比如Coffee Script），就需要将它转换成JS再require进来。当然，这个代码转换的过程能做的远不止是Coffee->JS这么简单，稍后将看到它的无穷魅力。
>虽然本质上说，loader也是插件，但因为webpack的体系中还有一个专门的名词就叫插件（plugins），为避免混淆，后面不再将loader与插件混淆说，后文中这将是两个相互独立的概念。

**用途**
webpack处理的主要对象是JS文件，而我们知道JS文件是可以做很多事情的，比如编译Less/SASS/CoffeeScript，比如在页面中插入一段HTML，比如修改替换文本文件等等。既然如此，我们能不能将这些能力集中到webpack上来呢，比如我在require('a.coffee')的时候，webpack自动帮我把它编译成JS文件，再当成JS文件引入进来？或者更极端一点，require('a.css')的时候，直接将CSS变成一段JS，用这段JS将样式插入DOM中呢？+

这些，正是loader要做的事情。

**用法**
>coffee就是指coffee script啦，可以算是JS的一种方言，深受ruby/python党喜爱。虽然笔者并不使用coffee，也不提倡使用，但这个loader相对比较典型，容易理解。在生产环境中，更提倡使用ES6编写代码，然后使用babel-loader编译成ES5代码使用，但因为babel 6改成了插件式的，配置项略多，为了简单起见，先不使用这个。

coffee-loader的作用就是在引用.coffee文件时，自动转换成JS文件，这样可以省去额外的将.coffee变成.js的过程。

首先我们准备一个index.js：
```
var hello = require('coffee-loader!./coffee.js');
hello.sayHello();
```
值得注意的是这里在require参数最前面加了coffee!，这个表示使用coffee-loader来处理文件内容。详细的机制稍后说明。
接下来，准备coffee.js：
```
class Hello
    constructor: (@name) ->

    sayHello: () ->
        alert "hello #{@name}"

module.exports = new Hello 'world'
```
接下来，我们需要安装coffee-loader。webpack的每一个loader命名都是xxx-loader，在安装的时候需要将对应的loader装到项目目录下：
```
npm install coffee-loader --save-dev
```
然后编译：
```
webpack index.js bundle.js
```
打HTML，即可看到我们写的代码生效了。


coffee.js编译后的代码也可以拿出来看一下：
```
function(module, exports) {

    var Hello;

    Hello = (function() {
      function Hello(name) {
        this.name = name;
      }

      Hello.prototype.sayHello = function() {
        return alert("hello " + this.name);
      };

      return Hello;

    })();

    module.exports = new Hello('world');


/***/ }
```

**串联**
loader是可以串联使用的，也就是说，一个文件可以先经过A-loader再经过B-loader最后再经过C-loader处理。而在经过所有的loader处理之前，webpack会先取到文件内容交给第一个loader。以我们后面经常会涉及的一个例子说明：
```
require('style!css!./style.css');
```
这个例子的意思是将style.css文件内容先经过css-loader处理（路径处理、import处理等），然后经过style-loader处理（包装成JS文件，运行的时候直接将样式插入DOM中。）
参数

loader还可以接受参数，不同的参数可以让loader有不同的行为（前提是loader确实支持不同的行为），具体每个loader支持什么样的参数可以参考loader的文档。同样以coffee-loader为例，它可以指定一个名为literate的参数，意思是……我也不知道，猜测可能是编译非完整的coffee script文件（例如内嵌在markdown中的代码片段等），如果你知道是什么意思，请告诉我。
在上面的用法中，参数的指定方式和url很像，要通过?来指定，例如指定literate参数需要这样写：
```
require('coffee?literate=1!./a.coffee');
```
这段代码中指定了literate参数为1，如果不写=1的话，默认值为true。（coffee-loader官方并没有说可以为1，而是只写了literate，没有值）。

**loader使用方法**
loader的使用有三种方法，分别是：
- 在require中显式指定，即上面看到的用法
- 在配置项（webpack.config.js）中指定
- 在命令行中指定
第一种我们已经看过了，不再说明。
第二种，在配置项中指定是最灵活的方式，它的指定方式是这样：

```
module: {
    // loaders是一个数组，每个元素都用来指定loader
    loaders: [{
        test: /\.jade$/,    //test值为正则表达式，当文件路径匹配时启用
        loader: 'jade',    //指定使用什么loader，可以用字符串，也可以用数组
        exclude: /regexp/, //可以使用exclude来排除一部分文件

        //可以使用query来指定参数，也可以在loader中用和require一样的用法指定参数，如`jade?p1=1`
        query: {
            p1:'1'
        }
    },
    {
        test: /\.css$/,
        loader: 'style!css'    //loader可以和require用法一样串联
    },
    {
        test: /\.css$/,
        loaders: ['style', 'css']    //也可以用数组指定loader
    }]
}

```
在命令行中指定参数的用法用得较少，可以这样写：
```
webpack --module-bind jade --module-bind 'css=style!css'
````
使用--module-bind指定loader，如果后缀和loader一样，直接写就好了，比如jade表示.jade文件用jade-loader处理，如果不一样，则需要显示指定，如css=style!css表示分别使用css-loader和style-loader处理.css文件。

**常用loader**
- babel-loader 
```
 loaders: [
            { test: /\.js|jsx$/, loaders: ['babel'] }
        ]
```
 编译后缀名为 .js 或者 .jsx 的文件，这样你就可以在这两种  
 类型的文件中自由使用 JSX 和 ES6 了。

-  css-loader
css-loader会遍历 CSS 文件，然后找到 url() 表达式然后处理他们

- style-loader
会把原来的 CSS 代码插入页面中的一个 style 标签中。



[demo地址][1]



[1]:https://github.com/fengyueran/WebPackDemo.git