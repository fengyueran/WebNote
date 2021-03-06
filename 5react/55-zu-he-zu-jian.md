组合组件

使用组件的目的就是通过构建模块化的组件，相互组合组件最后组装成一个复杂的应用。
在 React 组件中要包含其他组件作为子组件，只需要把组件当作一个 DOM 元素引入就可以了。
一个例子：一个显示用户头像的组件 Avatar 包含两个子组件 ProfilePic 显示用户头像和 ProfileLink 显示用户链接：
```
import React from 'react';
import { render } from 'react-dom';

const ProfilePic = (props) => {
  return (
    <img src={'http://graph.facebook.com/' + props.username + '/picture'} />
  );
}

const ProfileLink = (props) => {
  return (
    <a href={'http://www.facebook.com/' + props.username}>
      {props.username}
    </a>
  );
}

const Avatar = (props) => {
  return (
    <div>
      <ProfilePic username={props.username} />
      <ProfileLink username={props.username} />
    </div>
  );
}

render(
  <Avatar username="pwh" />,
  document.getElementById('example')
);
```
通过 props 传递值。
循环插入子元素

如果组件中包含通过循环插入的子元素，为了保证重新渲染 UI 的时候能够正确显示这些子元素，每个元素都需要通过一个特殊的 key 属性指定一个唯一值。具体原因见这里，为了内部 diff 的效率。
key 必须直接在循环中设置：
```
const ListItemWrapper = (props) => <li>{props.data.text}</li>;

const MyComponent = (props) => {
  return (
    <ul>
      {props.results.map((result) => {
        return <ListItemWrapper key={result.id} data={result}/>;
      })}
    </ul>
  );
}
```
你也可以用一个 key 值作为属性，子元素作为属性值的对象字面量来显示子元素列表，虽然这种用法的场景有限，参见Keyed Fragments，但是在这种情况下要注意生成的子元素重新渲染后在 DOM 中显示的顺序问题。+

实际上浏览器在遍历一个字面量对象的时候会保持顺序一致，除非存在属性值可以被转换成整数值，这种属性值会排序并放在其他属性之前被遍历到，所以为了防止这种情况发生，可以在构建这个字面量的时候在 key 值前面加字符串前缀，比如：
```
render() {
  var items = {};

  this.props.results.forEach((result) => {
    // If result.id can look like a number (consider short hashes), then
    // object iteration order is not guaranteed. In this case, we add a prefix
    // to ensure the keys are strings.
    items['result-' + result.id] = <li>{result.text}</li>;
  });

  return (
    <ol>
      {items}
    </ol>
   );
}
```