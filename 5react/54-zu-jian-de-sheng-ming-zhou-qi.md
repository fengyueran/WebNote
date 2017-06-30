一般来说，一个组件类由 extends Component 创建，并且提供一个 render 方法以及其他可选的生命周期函数、组件相关的事件或方法来定义。+

一个简单的例子：
```
/*eslint-disable*/
import React, { Component, PropTypes } from 'react';

class LikeButton extends Component {
  constructor(props) {
    super(props);
    this.state = { liked: false };
  }

  handleClick() {
    this.setState({ liked: !this.state.liked });
  }

  render() {
    const text = this.state.liked ? 'like' : 'haven\'t liked';
    return (
      <button onClick={this.handleClick.bind(this)}>
          You {text} this. Click to toggle.
      </button>
    );
  }

}
LikeButton.defaultProps = {
  liked: false,
};
export default LikeButton;

```