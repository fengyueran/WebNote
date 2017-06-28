ESLint 是一个插件化的 javascript 代码检测工具，它可以用于检查常见的 JavaScript 代码错误，也可以进行代码风格检查，这样我们就可以根据自己的喜好指定一套 ESLint 配置，然后应用到所编写的项目上，从而实现辅助编码规范的执行，有效控制项目代码的质量。

**1.安装eslint**
在开始使用 ESLint 之前，我们需要通过 NPM 来安装它：
```
$ npm install --save-dev eslint
```
**2.配置**

可以通过以下三种方式配置 ESLint:

- 使用 .eslintrc 文件（支持 JSON 和 YAML 两种语法）
.eslintrc 文件示例：
```
{
  "env": {
    "browser": true,
  },
  "parserOptions": {
    "ecmaVersion": 6,
    "ecmaFeatures": {
      "jsx": true
    }
  },
  "globals": {
    "angular": true,
  },
  "rules": {
    "camelcase": 2,
    "curly": 2,
    "brace-style": [2, "1tbs"],
    "quotes": [2, "single"],
    "semi": [2, "always"],
    "space-in-brackets": [2, "never"],
    "space-infix-ops": 2,
  }
}
```
.eslintrc 放在项目根目录，则会应用到整个项目；如果子目录中也包含 .eslintrc 文件，则子目录会忽略根目录的配置文件，应用该目录中的配置文件。这样可以方便地对不同环境的代码应用不同的规则。


- 在 package.json 中添加 eslintConfig 配置块；

 package.json 示例：
```
{
  "name": "mypackage",
  "version": "0.0.1",
  "eslintConfig": {
    "env": {
      "browser": true,
      "node": true
    }
  }
}
```


- 直接在代码文件中定义。

 代码文件内配置的规则会覆盖配置文件里的规则。
 禁用 ESLint：
 ```
 /* eslint-disable */
var obj = { key: 'value', }; // I don't care about IE8  
/* eslint-enable */
 ```
禁用一条规则：

```
/*eslint-disable no-alert */
alert('doing awful things');  
/* eslint-enable no-alert */
```
调整规则：
```
/* eslint no-comma-dangle:1 */
// Make this just a warning, not an error
var obj = { key: 'value', } 
```
详细使用方法可以参考文档：[Disabling Rules with Inline Comments - 使用行内注释禁用规则][6]


**3.实例**

创建index.js：

```
var run = function() {
  console.log('eslint')
}

run();
```
然后执行node index.js确保它是可以正确运行的。

接着我们执行以下命令来使用 ESLint 检查：
```
$ eslint index.js
```
可以看到，没有任何输出结果。这是因为我们没有指定任何的配置，除非这个文件是有语法错误，否则应该是不会有任何提示的。现在我们先使用内置的eslint:recommended配置，它包含了一系列核心规则，能报告一些常见的问题。

首先新建 ESLint 配置文件.eslintrc.js：
```
{
  extends: 'eslint:recommended',
}
```
重新执行eslint index.js可以看到输出了 2个错误：
```
error  Unexpected console statement  no-console 
error  'console' is not defined no-undef

✖ 2 problems (2 errors, 0 warnings)
```
这两条提示信息还是足够我们搞清楚是怎么回事的：

Unexpected console statement no-console - 不能使用console
'console' is not defined no-undef - console变量未定义，不能使用未定义的变量
针对第 1 条提示，我们可以禁用no-console规则。将配置文件.eslintrc.js改为这样：
```
module.exports = {
  extends: 'eslint:recommended',
  rules: {
    'no-console': 'off',
  },
};
```
说明：配置规则写在rules对象里面，key表示规则名称，value表示规则的配置，具体说明见下文。

重新执行检查还是提示no-undef：
```
error  'console' is not defined  no-undef

✖ 1 problem (1 error, 0 warnings)
```
这是因为 JavaScript 有很多种运行环境，比如常见的有浏览器和 Node.js，另外还有很多软件系统使用 JavaScript 作为其脚本引擎，比如 PostgreSQL 就支持使用 JavaScript 来编写存储引擎，而这些运行环境可能并不存在console这个对象。另外在浏览器环境下会有window对象，而 Node.js 下没有；在 Node.js 下会有process对象，而浏览器环境下没有。

所以在配置文件中我们还需要指定程序的目标环境：
```
module.exports = {
  extends: 'eslint:recommended',
  env: {
    node: true,
  },
  rules: {
    'no-console': 'off',
  },
};
```
再重新执行检查时，已经没有任何提示输出了，说明index.js已经完全通过了检查。


**规则**
每条规则有 3 个等级：off、warn和error。off表示禁用这条规则，warn表示仅给出警告，并不会导致检查不通过，而error则会导致检查不通过。

有些规则还带有可选的参数，比如comma-dangle可以写成[ "error", "always-multiline" ]；no-multi-spaces可以写成[ "error", { exceptions: { "ImportDeclaration": true }}]。

规则的详细说明文档可以参考这里：[Rules - 规则][1]

**使用共享的配置文件**

上文我们以eslint:recommended为基础配置，然后在此之上修改no-console这条规则。而在大多数时候，我们可能会根据自己个人或团队的习惯，定制更多的规则，比如限定缩进是2个空格和使用单引号的字符串等。而如果每一个项目都要这样写到.eslintrc.js文件上，管理起来会比较麻烦。

我们可以将定义好规则的.eslintrc.js文件存储到一个公共的位置，比如public-eslintrc.js：
```
module.exports = {
  "extends": 'eslint:recommended',
  "env": {
    "browser": true,
    "node":true
  },
  rules: {
     'indent': [ 'error', 2 ],
    },
}
```
然后将原来的.eslintrc.js文件改成这样：
```
module.exports = {
  extends: './public-eslintrc.js',
};
```
为了验证这样的修改是否生效，将index.js中的console.log('eslint');这一行前面多加一个空格，再执行 ESLint 检查：
```
 error  Expected indentation of 2 spaces but found 3  indent
 error  Unexpected console statement  
```
这时候提示的是缩进只能为 2 个空格，而文件的第 2 行却发现了 3 个空格，说明公共配置文件public-eslintrc.js已经生效了。

我们还可以使用已经发布到 NPM 上的 ESLint 配置，这些配置的模块名一般以eslint-config-为前缀，比如广受好评的airbnb。要使用这个配置，先执行以下命令安装它：
```
$ npm install --save-dev eslint-config-airbnb
```
然后将.eslintrc.js文件改成这样：
```
{
  # "extends": 'eslint:recommended',
  # "extends": './public-eslintrc.js',
  "extends": 'airbnb',

  "env": {
    "browser": true,
    "node":true
  },
  rules: {
    # "no-console": 'off',
    },
}
```
再执行 ESLint 检查: ./node_modules/.bin/eslint index.js ，可以看到输出如下的提示：
```
  1:1   error    Unexpected var, use let or const instead      no-var
  1:11  warning  Unexpected unnamed function                   func-names
  1:19  error    Missing space before function parentheses     space-before-function-paren
  2:5   error    Expected indentation of 2 spaces but found 4  indent
  2:5   warning  Unexpected console statement                  no-console
  2:26  error    Missing semicolon                             semi
  3:2   error    Missing semicolon                             semi

✖ 7 problems (5 errors, 2 warnings)
```
ESLint 配置文件中的extends还可以用来指定各种来源的配置引用，具体说明可以参考以下链接：

[Using a shareable configuration package - 使用共享的模块][2]
[Using the configuration from a plugin - 使用插件][3]
[Using a configuration file - 使用配置文件][4]
[Using "eslint:all" - 使用"eslint:all"][5]

**代码格式化**

在ESLint[规则列表][1]页面，我们发现有些规则的旁边会带有一个橙色扳手图标，表示在执行eslint命令时指定--fix参数可以自动修复该问题。

接着上文使用eslint-config-airbnb配置的检查，我们尝试在执行检查时添加--fix参数：
```
$ ./node_modules/.bin/eslint index.js --fix
```
执行完毕，没有发现任何提示。再打开index.js文件发现根规则已经变成了这样：

```
const run = function () {
  console.log('eslint');
};

run();
```
我们可以利用这个特性来自动格式化项目代码，这样就可以保证代码书写风格的统一。

**发布自己的配置**

关于「共享的配置文件」一小节已经提到，可以在extends中指定一个文件名，或者一个eslint-config-开头的模块名。为了便于共享，一般推荐将其发布成一个 NPM 模块。

其原理就是在载入模块时输出原来.eslintrc.js的数据。比如我们可以创建一个模块eslint-config-my用于测试。

新建文件eslint-config-my/index.js：

```
module.exports = {
  extends: 'eslint:recommended',
  env: {
    node: true,
    es6: true,
  },
  rules: {
    'no-console': 'off',
    'indent': [ 'error', 2 ],
    'quotes': [ 'error', 'single' ],
  },
};
```
再新建文件eslint-config-my/package.json：
```
{
  "name": "eslint-config-my",
  "version": "0.0.1",
  "main": "index.js"
}
```
为了能让eslint正确载入这个模块，我们需要执行npm link将这个模块链接到本地全局位置：

$ npm link eslint-config-my
然后将文件.eslintrc.js改成这样：
```
module.exports = {
  extends: 'my',
};
```
说明：在extends中，eslint-config-my可简写为my。

在执行eslint index.js检查，可看到没有任何错误提示信息，说明eslint已经成功载入了eslint-config-my的配置。如果我们使用npm publish将其发布到 NPM 上，那么其他人通过npm install eslint-config-my即可使用我们共享的这个配置。

关于共享 ESLint 配置的详细文档可参考：[Shareable Configs - 可共享的配置][7]







[1]:http://eslint.cn/docs/rules/
[2]:http://eslint.cn/docs/user-guide/configuring#using-a-shareable-configuration-package
[3]:http://eslint.cn/docs/user-guide/configuring#using-the-configuration-from-a-plugin
[4]:http://eslint.cn/docs/user-guide/configuring#using-a-configuration-file
[5]:http://eslint.cn/docs/user-guide/configuring#using-eslintall
[6]:http://eslint.cn/docs/user-guide/configuring#disabling-rules-with-inline-comments
[7]:http://eslint.cn/docs/developer-guide/shareable-configs
