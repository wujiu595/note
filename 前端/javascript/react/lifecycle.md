# React Lifecycle

## function of LifeCycle

### 什么是声明周期函数

在某一时刻组建会自动执行的函数

![1550214151244](/tmp/1550214151244.png)

## 生命周期函数

### Mounting(挂载)

此阶段的声明周期函数只会执行一次

- componentWillMount() 

  组件开始挂载之前

- render()

  挂载时

- componentDidMount()

  挂载后

### Updation(更新)

props 或者state发生变化时

组件更新之前，会被自动的执行

- shouldConponentUpdate() bool 组件需要被更行吗？
- componentWillUpate() 组件被更新之前
- render（）
- componentDidUpate()组件更新完成之后

​     ajax方法一般写在这个方法中

- componentWillReceiveProps()  子组件从父组件接收参数时

备注:

componentWillReceiveProps() 

1. 当一个组建从父组建接受参数，

2. 如果这个组建第一次存在父组件中，不会执行
3. 如果这个组建已经存在父组件中，会执行

### Unmounting(卸载)

- componentWillUnmount()把一个组件从一个组件中剔除的时候会被执行。
- reder()