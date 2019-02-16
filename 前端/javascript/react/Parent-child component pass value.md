# Parent-child component pass value

## 父组件向子组件传值

父组件通过在引用的子组件中写入自定义属性生效

```js
<子组件  自定义属性={值}>
```

值的类型

1. 字符
2. 函数

父组件

```js
import React, {Component} from 'react';
import Test from "./Test";

class App extends Component {
    render(){
        return (
            <Test content={"hello world"}/>
        )
    }
}

export default App;
```

子组件(Test)

```js
import React,{Component} from 'react'

class Test extends Component{
    render(){
        return (<div>{this.props.content}</div>)
    }
}

export default Test
```



## 子组件向父组件传值

子组件通过调用父组件传递的函数改变父组件的值

- 父组件

```js
import React, {Component} from 'react';
import Test from "./Test";

class App extends Component {
    constructor(props){
        super(props);
        //将方法绑定this指针,防止地址this指向混乱,在构造方法中声明可以提升性能
        this.HandleAddNum=this.HandleAddNum.bind(this);
        this.state={
            num:0
        }
    }
    render(){
        return (
            //将 HandleAddNum 作为自定义属性传递
            <Test 
            content={"hello world"} 
            num={this.state.num} 
            addFunc={this.HandleAddNum}/>
        )
    }PropTypes&DefaultPropTypes

用来限制父组件对子组件

    
    HandleAddNum(){
       this.setState((prevState)=>({
           num:prevState.num+1
       }))
    }
}


export default App;
```

- 子组件

```js
import React,{Component,Fragment} from 'react'

class Test extends Component{
    render(){
        return (
            <Fragment>
                <div>{this.props.num}</div>
                {this.props.content}
            	//点击按钮触发事件调用父组件的方法改变
                <div><button onClick={this.props.addFunc}>add</button></div>
            </Fragment>
        )
    }
}

export default Test
```

## PropTypes&DefaultPropTypes

### 安装

使用npm 安装第三方包 prop-types

```shell
npm install prop-types
```



### PropTypes

预设父组件的传值类型,并检测父组件对子组件的传值提出警告(warning)

- 导入包

```js
import PropTypes from 'prop-types'
```

- 形式

```
PropTypes.`数据类型`.`约束`
```

- 示例

```js
Test.propTypes={
    test:PropTypes.string.isRequired,
};
```

- 类型

```js
optionalArray: PropTypes.array, //数组
optionalBool: PropTypes.bool,//布尔型
optionalFunc: PropTypes.func,//函数
optionalNumber: PropTypes.number,//数值
optionalObject: PropTypes.object,//对象
optionalString: PropTypes.string,//字符串
optionalSymbol: PropTypes.symbol,??
optionalNode: PropTypes.node, //anything
optionalElement: PropTypes.element,//React Component
optionalMessage: PropTypes.instanceOf(Message),//类的实例
optionalEnum: PropTypes.oneOf(['News', 'Photos']),//News或者Photos
optionalUnion: PropTypes.oneOfType([
    PropTypes.string,
    PropTypes.number,
    PropTypes.instanceOf(Message)
  ]),//string或者number或者类的实例
optionalArrayOf: PropTypes.arrayOf(PropTypes.number),//某一类型的数组
optionalObjectOf: PropTypes.objectOf(PropTypes.number),//属性值是某一类型的对象
optionalObjectWithShape: PropTypes.shape({
    color: PropTypes.string,
    fontSize: PropTypes.number
  }),//呈现某种结构的对象
requiredFunc: PropTypes.func.isRequired,//不能为空
requiredAny: PropTypes.any.isRequired,//任何类型    
customProp: function(props, propName, componentName) {
  if (!/matchme/.test(props[propName])) {
    return new Error(
      'Invalid prop `' + propName + '` supplied to' +
      ' `' + componentName + '`. Validation failed.'
    );
   }
 },//自定以函数
customArrayProp: PropTypes.arrayOf(function(propValue, key, componentName, location, propFullName) {
    if (!/matchme/.test(propValue[key])) {
      return new Error(
        'Invalid prop `' + propFullName + '` supplied to' +
        ' `' + componentName + '`. Validation failed.'
      );
    }
  })
};//自定义函数搭配 arrayof和oneof一起使用
```

### DefaultPropTypes

设置某个值的默认值,尽量不和PropTypes 约束相同的字段

```
Test.defaultProps={
    test:'hello world!!'
};
```





















