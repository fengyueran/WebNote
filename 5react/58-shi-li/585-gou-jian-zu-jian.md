**构建组件**
需要构建组件如下图：
入口组件App下分为四个部分：
```
App:
   1. AddTodo
   2. TodoList
   3. Footer
   4. Card
```
![](/assets/5.8-3.png)

**App:**
```
import React, { Component, PropTypes } from 'react';
import { connect } from 'react-redux';
import { addTodo, completeTodo, setVisibilityFilter, VisibilityFilters } from '../actions/action';
import AddTodo from './addtodo';
import TodoList from './todolist';
import Footer from './footer';
import Card from '../containers/cardcontainer';

class App extends Component {
  render() {
    // Injected by connect() call:
    const { dispatch, visibleTodos, visibilityFilter } = this.props;
    return (
      <div>
        <AddTodo
          onAddClick={text =>
            dispatch(addTodo(text))
          } />
        <TodoList
          todos={visibleTodos}
          onTodoClick={index =>
            dispatch(completeTodo(index))
          } />
        <Footer
          filter={visibilityFilter}
          onFilterChange={nextFilter =>
            dispatch(setVisibilityFilter(nextFilter))
          } />
        <Card />
      </div>
    );
  }
}

App.propTypes = {
  visibleTodos: PropTypes.arrayOf(PropTypes.shape({
    text: PropTypes.string.isRequired,
    completed: PropTypes.bool.isRequired
  }).isRequired).isRequired,
  visibilityFilter: PropTypes.oneOf([
    'SHOW_ALL',
    'SHOW_COMPLETED',
    'SHOW_ACTIVE'
  ]).isRequired,
  dispatch: PropTypes.func,
};

const selectTodos = (todos, filter) => {
  switch (filter) {
    case VisibilityFilters.SHOW_ALL:
      return todos;
    case VisibilityFilters.SHOW_COMPLETED:
      return todos.filter(todo => todo.completed);
    case VisibilityFilters.SHOW_ACTIVE:
      return todos.filter(todo => !todo.completed);
    default:
  }
};

const select = (state) => {
  return {
    visibleTodos: selectTodos(state.todos, state.visibilityFilter),
    visibilityFilter: state.visibilityFilter
  };
};

// 包装 component ，注入 dispatch 和 state 到其默认的 connect(select)(App) 中；
export default connect(select)(App);

```
store更新时会调用select方法。

**AddTodo:**

```
/* eslint-disable */
import React, { Component, PropTypes } from 'react';

class AddTodo extends Component {
 render() {
   return (
     <div>
       <input type='text' ref='input' />
       <button onClick={(e) => this.handleClick(e)}>
         Add
       </button>
     </div>
   );
 }

  handleClick(e) {
    const node = this.refs.input
    const text = node.value.trim();
    this.props.onAddClick(text);
    node.value = '';
  }
}

AddTodo.propTypes = {
  onAddClick: PropTypes.func.isRequired,
};

export default AddTodo;

```

**TodoList:**
```
/* eslint-disable */
import React, { Component, PropTypes } from 'react';
import Todo from './todo';

class TodoList extends Component {
  render() {
    const { todos } = this.props
    return (
      <ul>
        {this.props.todos.map((todo, index) =>
          (<Todo
            {...todo}
            key={index}
            onClick={() => this.props.onTodoClick(index)} />)
        )}
      </ul>
    )
  }
}

TodoList.propTypes = {
  onTodoClick: PropTypes.func.isRequired,
  todos: PropTypes.arrayOf(PropTypes.shape({
    text: PropTypes.string.isRequired,
    completed: PropTypes.bool.isRequired,
  }).isRequired).isRequired
};

export default TodoList;

```
**Footer:**
```
/*eslint-disable */
import React, { Component, PropTypes } from 'react'

class Footer extends Component {
  renderFilter(filter, name) {
    if (filter === this.props.filter) {
      return name
    }

    return (
      <a href='#' onClick={e => {
        e.preventDefault()
        this.props.onFilterChange(filter)
      }}>
        {name}
      </a>
    )
  }

  render() {
    return (
      <p>
        Show:
        {' '}
        {this.renderFilter('SHOW_ALL', 'All')}
        {', '}
        {this.renderFilter('SHOW_COMPLETED', 'Completed')}
        {', '}
        {this.renderFilter('SHOW_ACTIVE', 'Active')}
        .
      </p>
    )
  }
}

Footer.propTypes = {
  onFilterChange: PropTypes.func.isRequired,
  filter: PropTypes.oneOf([
    'SHOW_ALL',
    'SHOW_COMPLETED',
    'SHOW_ACTIVE'
  ]).isRequired
}

export default Footer;

```
**Card:**
```
/* eslint-disable */
import React, { Component, PropTypes } from 'react';
import { updateCardData } from '../actions/action';

 class Card extends Component {
   stateChange() {
      return this.props.number;
   }
  getRandomNum(Min,Max) {
    var Range = Max - Min;
    var Rand = Math.random();
    return(Min + Math.round(Rand * Range));
  }
  handleClick() {
    global.store.dispatch(updateCardData({ number:this.getRandomNum(0,100) }))
  }
  render() {
      return (
        <button onClick={this.handleClick.bind(this)}>
            I like the number: {this.stateChange()}.
        </button>
      );
  }
}
Card.propTypes = {
  number: PropTypes.number,
};

export default Card;

```

这个Card还需要添加connect方法来生成与Redux store关联起来的新组件，如下：在container中
```
import { connect } from 'react-redux';

import Card from '../components/card';
import { cardSelector } from '../selectors/selector';

const mapStateToProps = state => cardSelector;
//store更新调用mapStateToProps
const CardContainer = connect(
  mapStateToProps,
)(Card);

export default CardContainer;

```
上面的cardSelector：
```
const cardSelector = (state) => {
  return { number: state.cardState.number };
};

export { cardSelector };
```
