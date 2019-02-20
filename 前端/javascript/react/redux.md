---
typora-copy-images-to: img
typora-root-url: img
---

# redux

## 1 简介

react是一个视图层框架,需要使用redux 数据层框架来管理数据

在不使用数据框架管理数据是如下图:5在与3传递数据   5->2->1->3

![1550218582527](/1550674391769.png)

在使用数据框架管理数据后:5->store->3

![1550218872108](/1550674405761.png)

所以我们需要使用redux进行数据的管理,redux的原理如下图

![1550218702525](/1550674431472.png)



## 2 安装

```shell
npm install redux  --save
```

## 3 详解

### 3.1 store

创建store

```js
import {createStore} from 'redux'
import reducer from "./reducer"

const store = createStore(
    reducer,
);

export default store
```

### 3.2 reduser

```js
const defaultState={
    inputValue:"",
    list:[]
};
export default (state=defaultState,action)=> {
  const newState = JSON.parse(JSON.stringify(state));
  switch (action.type) {
    case "":
      return newState;
    default:
      return newState;
  }
}
```

### 3.3 createAction

```js
import {ADD_ITEM} from "./actionTypes";

export const getAddItemAction = () => ({
    type: ADD_ITEM,
});
```

### 3.4 ActionTypes

```js
export const ADD_ITEM="add_item";
```

### 3.5 component

```js
constructor(props) {
    super(props);
    this.state = store.getState();
    store.subscribe(this.handleStoreChange)
}

handleInputChange(e) {
    const action = getHandleInputChange(e.target.value);
    store.dispatch(action)
};

handleStoreChange() {
    this.setState(store.getState())
}
```

## 4 immutable

immutable是facebook研发的一个对象,是一个不可改变的对象

### 4.1 普通对象和immutable对象的转换

- 普通对象2immutable对象fromJS

```js
const defaultState=fromJS({
});
```

- immutable对象普通对象2toJS

```js
defaultState.toJS()
```

### 4.2 immutable对象取值

- 获取单层值get

```js
defaultState.get("key")
```

- 获取多层多个值

方法一:

```js
get("key1").get("key2")
```

方法二:

```js
getIn(["key1","key2"])
```

### 4.3 immutable对象设置新值

- 设置单个值

```js
state.set("key",value);
```

- 设置多个值

方法一:

```js
state.set("key1",value1).set("key2",value2);
```

方法二:

```js
state.merge({key1:value,key2:value2)
```





## 5 reduser拆分

### 5.1 combineReducers方法

combineReducers方法的主要功能是把不同的reducer统一到一个reducer中

```js
import headerReduser from "../common/header/store";
// import {combineReducers} from 'redux'
import {combineReducers} from 'redux-immutable'

export default combineReducers({
  header:headerReduser
})

```

### 5.2 调用引用合并后的reducer方法

- 正常方法

```js
focused:state.header.focused
```

- 子immutable方法

```js
focused:state.getIn(["header","focused"]),
```

- 父immutable方法

```js
mouseIn:state.header.get("mouseIn"),
```



## 6  provider和connection方法

provider和connect方法用于 父组件和子组件之间传递store的方法

### provider方法

如下所示在使用provider后会把store传递给Header组件

```js
import React, { Component } from 'react';
import {Provider} from 'react-redux'
import Header from './common/header'
import store from './store'

class App extends Component {
  render() {
    return (
      <Provider store={store}>
        <Header/>
      </Provider>
    );
  }
}

export default App;
```

### connect方法

connect方法用于获取父组件传递过来的值

```js
import {connect} from 'react-redux'
export default connect(mapStateToProps,mapDispatchToProps)(Header);
```

mapStateToProps方法

```js
const mapStateToProps=(state)=>({
  focused:state.getIn(["header","focused"]),
});

```

mapDispatchToProps方法

```js
const mapDispatchToProps=(dispatch)=>({
  handleInputFocus(){
    const action = actionCreators.createHeaderFocusedAction();
    dispatch(action)
  },
});
```





## 7 redux-thunck

redux-thunck是一个redux的中间件,用来处理发送异步请求

### 7.1 store

```js
import {createStore,applyMiddleware} from 'redux'

import reducer from "./reducer"

import thunk from 'redux-thunk'

const store = createStore(

    reducer,

    applyMiddleware(thunk),

);

export default store

```

在和redux-devtools结合使用时

```js
import {createStore,applyMiddleware,compose} from 'redux'
import reducer from "./reducer"
import thunk from 'redux-thunk'

const composeEnhancers = typeof window === 'object' && window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ ?
    window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__({}) : compose;

const enhancer = composeEnhancers(
    applyMiddleware(thunk),
);
const store = createStore(reducer, enhancer);

export default store
```



### 7.2 createAction

```js
export const initListAction = (data) => ({
    type: INIT_ACTION,
    data
});

export const getTodolist=()=>{
    return (dispatch)=>{
        axios.get("./api/list.json").then((res)=>{
            const data=res.data;
            const action=initListAction(data)
            dispatch(action)
        }).catch()
    }
};
```

### 7.3 ActionTypes

```js
export const INIT_ACTION="init_action";
```

### 7.4 reduser

```js
const defaultState={
    inputValue:"",
    list:[]
};
export default (state=defaultState,action)=> {
  const newState = JSON.parse(JSON.stringify(state));
  switch (action.type) {
    case "":
      return newState;
    default:
      return newState;
  }
}   
```

### 7.5 component

```js
componentDidMount(){
    const action=getTodolist();
    store.dispatch(action)
}
```



## 8 redux-saga

redux-saga是一个redux的中间件

### 8.1 安装

```
$ npm install --save redux-saga
```

### 8.2 store

在和redux-devtools结合使用时

```js
import {createStore,applyMiddleware,compose} from 'redux'
import reducer from "./reducer"
import createSagaMiddleware from 'redux-saga'
import TodoSagas from './sagas'

const composeEnhancers = typeof window === 'object' && window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ ? window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__({}) : compose;
const sagaMiddleware = createSagaMiddleware()
const enhancer = composeEnhancers(
    applyMiddleware(sagaMiddleware),
);
const store = createStore(reducer, enhancer);
sagaMiddleware.run(TodoSagas);

export default store
```

### 8.3 sagas

```js
import {takeEvery,put} from 'redux-saga/effects'
import {GET_INIT_ACTION} from './actionTypes'
import {initListAction} from "./actionCreators"
import axios from 'axios'

function* fetchUser() {
    const res=yield axios.get("./api/list.json");
    const action = initListAction(res.data);
    yield put(action)
}

function* TodoSagas() {
    yield takeEvery(GET_INIT_ACTION, fetchUser);
}

export default TodoSagas;
```