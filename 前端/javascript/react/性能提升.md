## 性能提升

### constructor中 bind this指针

尽量在构造函数中绑定函数的指针

### setState 结合多次的改变成一次改变，异步



### 虚拟dom 同层比对



### key比对

在循环之中尽量不要使用 index，会使key不稳定，key值要保持稳定

### shouldComponent()

一般情况下,只要父组件更新子组件也会更新,可以使用shouldComponentUpdate声明周期函数阻止组件更新节省性能.

```js
shouldComponentUpdate(nextProps,nextState){
    if(nextProps.content!==this.props.content){
        return true
    }
    return false
}
```

