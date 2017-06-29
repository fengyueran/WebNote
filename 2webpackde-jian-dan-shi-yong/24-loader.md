**使用loader**

oader是webpack中一个重要的概念，它是指用来将一段代码转换成另一段代码的webpack插件。晕了没？为什么需要将一段代码转换成另一段代码呢？这是因为webpack实际上只能处理JS文件，如果需要使用一些非JS文件（比如Coffee Script），就需要将它转换成JS再require进来。当然，这个代码转换的过程能做的远不止是Coffee->JS这么简单，稍后将看到它的无穷魅力。
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