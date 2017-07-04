`
**5. 构建actions**
```
const CORDOVA_DATA = 'CORDOVA_DATA';
const updateCordovaData = (username, data) => {
  return { type: CORDOVA_DATA, username, data };
};//类型 + 数据

const CARD_DATA = 'CARD_DATA';
const updateCardData = (data) => {
  return { type: CARD_DATA, data };
};

const ADD_TODO = 'ADD_TODO';
const addTodo = (text) => {
  return { type: ADD_TODO, text };
};

const COMPLETE_TODO = 'COMPLETE_TODO';
const completeTodo = (index) => {
  return { type: COMPLETE_TODO, index };
};

const SET_VISIBILITY_FILTER = 'SET_VISIBILITY_FILTER';
const setVisibilityFilter = (filter) => {
  return { type: SET_VISIBILITY_FILTER, filter };
};

const VisibilityFilters = {
  SHOW_ALL: 'SHOW_ALL',
  SHOW_COMPLETED: 'SHOW_COMPLETED',
  SHOW_ACTIVE: 'SHOW_ACTIVE'
};

export { updateCordovaData, updateCardData, addTodo, completeTodo, setVisibilityFilter, VisibilityFilters };

```
