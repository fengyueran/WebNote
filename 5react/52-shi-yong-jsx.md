使用 JSX

利用 JSX 编写 DOM 结构，可以用原生的 HTML 标签，也可以直接像普通标签一样引用 React 组件。这两者约定通过大小写来区分，小写的字符串是 HTML 标签，大写开头的变量是 React 组件。
使用 HTML 标签：
```
import React from 'react';
import { render } from 'react-dom';

var myDivElement = <div className="foo" />;
render(myDivElement, document.getElementById('mountNode'));
```
HTML 里的 class 在 JSX 里要写成 className，因为 class 在 JS 里是保留关键字。同理某些属性比如 for 要写成 htmlFor。
使用组件：
```
import React from 'react';
import { render } from 'react-dom';
import MyComponent from './MyComponet';

var myElement = <MyComponent someProperty={true} />;
render(myElement, document.body);
```