# React JSX

React 使用 JSX 来替代常规的 JavaScript。

JSX 是一个看起来很像 XML 的 JavaScript 语法扩展。

## 优点

- JSX 执行更快，因为它在编译为 JavaScript 代码后进行了优化。
- 它是类型安全的，在编译过程中就能发现错误。
- 使用 JSX 编写模板更加简单快速。

## 语法

- 组件首字母必须大写

## 定义组件的两种方式

函数式

```js
import React from 'react'

export const Test=(props)=>{
    return(
    	<div>
        	hello world!!
        <div/>
    )
}
```

一般用于UI组件(傻瓜组件)

类式

```js
import React.{Compontent} from 'react'

class Test extends Component {
    render(){
        return{
        	<div>
        		hello world!!
        	<div/> 
        }
    }
}

export default Test
```

## 条件渲染

React的条件渲染是通过定义一个字段,根据该字段的值返回不同的组件进行条件渲染

```js
function UserGreeting(props) {
  return <h1>欢迎回来!</h1>;
}

function GuestGreeting(props) {
  return <h1>请先注册。</h1>;
}
```



```js
function Greeting(props) {
  const isLoggedIn = props.isLoggedIn;
  if (isLoggedIn) {
    return <UserGreeting />;
  }
  return <GuestGreeting />;
}
 
ReactDOM.render(
  <Greeting isLoggedIn={false} />,
  document.getElementById('example')
);
```

## React 列表 & Keys

我们可以使用 [JavaScript 的 map() 方法](http://www.runoob.com/jsref/jsref-map.html)来创建列表。

```js
class Test extends Component{
    consturctor(props){
        super(props);
        this.state={
            values:[1,2,3,4,5,6]
        }
    }t
    render(){
    	return(
        	<ul>
            {this,state.values.map((index,item)=>{
             	return <li key="item">{item}<li>
            })}
            <ul/>
        )
	}
}

export default Test
```

值得注意的是:循环后的内容必须有一个key的属性,并且不要使用变化的量,否则会影响程序的性能

## dangerouslySetInnerHTML

```html
<div dangerouslySetInnerHTML={{__html:this.props.content}}>
    
</div>
```



## jsx和html语法上的区别

### jsx 事件处理

React 元素的事件处理和 DOM 元素类似。但是有一点语法上的不同:

- React 事件绑定属性的命名采用驼峰式写法，而不是小写。

```js
<div onClick={func}><div/>
```

### jsx class

在jsx中使用className 代替class使用

```html
<div className={}> <div/>
```

### jsx for

jsx中使用htmlFor代替for

```js
<label htmlFor="inputArea">请输入:<label/>
<input id="inputArea"/>    
```

### jsx 在dom中写样式

```shell
<div style={{"margin-top":"10px"}}><div/>
```

