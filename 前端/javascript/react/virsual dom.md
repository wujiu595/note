---
typora-copy-images-to: img
typora-root-url: img
---

## 虚拟dom

### 什么是虚拟dom

js对象，用它来描述真实DOM

1.state 数据

2.JSX 模板

3.模板和数据结合，生成真实DOM，来显示

<div id='abc' ><span>hello world</span></div>

4.生成虚拟DOM（虚拟DOM是一个js对象，用它来描述真实DOM）

['div',{id:'abc'},[span',{},'hello world'] ]（损耗了性能）

5.state 发生变化

6.生成新的虚拟DOM（极大的提升）

['div',{id:'abc'},[span',{},'bye bye' ] ]

7.比较虚拟DOM和新的虚拟DOM的区别，找出区别是span中内容（极大的提升）

8.直接操作DOM，改变span中的内容

```javascript
<h1>To Do List</h1>
```

等价于

```javascript
{React.createElement("h1","","To Do List")}
```

jsx->createElement->虚拟DOM->真实DOM



优点：

1.性能提升

2.夸端应用得以实现react Nactive

### 虚拟dom的diff算法

同层比对:react 会依从根节点向下同层比较,下图的2变成4下面的所有都会被改变

![1550212284349](/1550212284349.png)

keys比较

![1550212508135](/1550212508135.png)

注意:在循环之中尽量不要使用 index，会使key不稳定，key值要保持稳定