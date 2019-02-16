# redux

## 简介

react是一个视图层框架,需要使用redux 数据层框架来管理数据

在不使用数据框架管理数据是如下图:5在与3传递数据   5->2->1->3

![1550218572526](/tmp/1550218572526.png)



在使用数据框架管理数据后:5->store->3

![1550218762108](/tmp/1550218762108.png)

所以我们需要使用redux进行数据的管理,redux的原理如下图

![1550217602525](/tmp/1550217602525.png)



## 安装

```shell
npm install redux
```

## 详解

### store

创建store

```js
import {createStore} from 'redux'
import reducer from "./reducer"

const store = createStore(
    reducer,
);

export default store
```

### reduser

```js
const defaultState={
    inputValue:"",
    list:[]
};
switch (action.type) {
    case ADD_ITEM:
        newState.list=[...newState.list,newState.inputValue];
        newState.inputValue="";
        return newState;
}
```

### createAction

```js
import {ADD_ITEM} from "./actionTypes";

export const getAddItemAction = () => ({
    type: ADD_ITEM,
});
```

### ActionTypes

```js
export const ADD_ITEM="add_item";
```

### component

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

## redux-thunck

redux-thunck是一个redux的中间件,用来处理发送异步请求

### store

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



### createAction

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

### ActionTypes

```js
export const INIT_ACTION="init_action";
```

### reduser

```js
const defaultState={
    inputValue:"",
    list:[]
};
switch (action.type) {
	case INIT_ACTION:
         newState.list=action.data;
         return newState;
}      
```

### component

```js
componentDidMount(){
    const action=getTodolist();
    store.dispatch(action)
}
```



## redux-saga

redux-saga是一个redux的中间件

### 安装

```
$ npm install --save redux-saga
```

### store

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

### sagas

```
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