# react路由

## 安装

```shell
npm install react-router-dom --save
```

## 引用

```js
import {BrowseRouter,Route} from "react-router-dom"
```

## 应用

- 只能有一个BrowseRouter
- 所有的Route必须定义在一个组件当中

```js
import React, { Component } from 'react';
import {Provider} from 'react-redux'
import {BrowserRouter,Route} from "react-router-dom"
import {Container} from './styled'
import Header from './common/header'
import store from './store'
import './rest.css'
import './static/icon.css'

class App extends Component {
  render() {
    return (
      <Provider store={store}>
        <Container>
          <BrowserRouter>
            <div>
              <Route path="/" exact render={()=><div>index</div>}></Route>
              <Route path="/detail" exact render={()=><div>detail</div>}></Route>
            </div>
          </BrowserRouter>
        </Container>
        <Header/>
      </Provider>
    );
  }
}

export default App;
```

## link

在使用如下内容时会整页刷新页面

```html
<a href=""></a>
```

react的推荐使用 link的连接方式,避免重新请求页面浪费性能,

```html
<Link to=""></Link>
```

注意:拥有link组件的元素必须在BrowseRouter中

## 获取路由参数

### 动态路由

设置

```js
<Link to={"/"+"1"}></Link>
```

获取

```js
this.props.math. params.id
```

### 参数路由

设置

```html
<Link to={"/?id="+"1"}></Link>
```

获取

```js
this.props.location.search
```

## 重定向

```js
Redirect("/")
```



## withRouter

使组件拥有获取app.js的功能

```
withRouter(Download)
```













