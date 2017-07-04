**创建reducers**

rootreducer下共3个：
- appstate
- cardstate
- cordovasate

**rootreducer:**
```
import { combineReducers } from 'redux';
import { cardState } from './cardstate';
import { cordovaState } from './cordovastate';
import { visibilityFilter, todos } from './appstate';

const reducers = {
  cardState,
  cordovaState,
  visibilityFilter,
  todos,
};

const rootReducer = combineReducers(reducers);

export { rootReducer };
```
**appstate:**
```
import { combineReducers } from 'redux';

const ADD_TODO = 'ADD_TODO';
const COMPLETE_TODO = 'COMPLETE_TODO';
const SET_VISIBILITY_FILTER = 'SET_VISIBILITY_FILTER';
export const VisibilityFilters = {
  SHOW_ALL: 'SHOW_ALL',
  SHOW_COMPLETED: 'SHOW_COMPLETED',
  SHOW_ACTIVE: 'SHOW_ACTIVE',
};
const { SHOW_ALL } = VisibilityFilters;

const visibilityFilter = (state = SHOW_ALL, action) => {
  switch (action.type) {
    case SET_VISIBILITY_FILTER:
      return action.filter;
    default:
      return state;
  }
};


const todos = (state = [], action) => {
  switch (action.type) {****
    case ADD_TODO:
      return [
        ...state,
        {
          text: action.text,
          completed: false,
        },
      ];
    case COMPLETE_TODO:
      return [
        ...state.slice(0, action.index),
        Object.assign({}, state[action.index], {
          completed: true,
        }),
        ...state.slice(action.index + 1),
      ];
    default:
      return state;
  }
};

export { visibilityFilter, todos };

```

**cardstate:**
```
const CARD_DATA = 'CARD_DATA';

const cardState = (state = { number: 0 }, action) => {
  switch (action.type) {
    case CARD_DATA:
      {
        return { ...state, ...action.data };
      }
    default:
      return state;
  }
};
export { cardState };
```
**cordovastate:**
```

const CORDOVA_DATA = 'CORDOVA_DATA';

const cordovaState = (state = {}, action) => {
  switch (action.type) {
    case CORDOVA_DATA:
      {
        return { ...state, caseId: action.caseId };
      }
    default:
      return state;
  }
};

export { cordovaState };//待使用
```
