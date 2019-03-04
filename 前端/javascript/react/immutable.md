## 1 immutable

​	mmutable Data 就是一旦创建，就不能再被更改的数据。对 Immutable 对象的任何修改或添加删除操作都会返回一个新的 Immutable 对象。Immutable 实现的原理是 Persistent Data Structure（持久化数据结构），也就是使用旧数据创建新数据时，要保证旧数据同时可用且不变。同时为了避免 deepCopy 把所有节点都复制一遍带来的性能损耗，Immutable 使用了 Structural Sharing（结构共享），即如果对象树中一个节点发生变化，只修改这个节点和受它影响的父节点，其它节点则进行共享。

​	Facebook 工程师 Lee Byron 花费 3 年时间打造，与 React 同期出现，但没有被默认放到 React 工具集里（React 提供了简化的 Helper）。它内部实现了一套完整的 Persistent Data Structure，还有很多易用的数据类型。像 `Collection`、`List`、`Map`、`Set`、`Record`、`Seq`。有非常全面的`map`、`filter`、`groupBy`、`reduce``find`函数式操作方法。同时 API 也尽量与 Object 或 Array 类似。

其中有 3 种最重要的数据结构说明一下：（Java 程序员应该最熟悉了）

- Map：键值对集合，对应于 Object，ES6 也有专门的 Map 对象
- List：有序可重复的列表，对应于 Array
- Set：无序且不可重复的列表

### 1.1 普通对象和immutable对象的转换

- 普通对象2immutable对象fromJS

```js
const defaultState=fromJS({
});
```

- immutable对象普通对象2toJS

```js
defaultState.toJS()
```

### 1.2 immutable对象取值

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

### 1.3 immutable对象设置新值

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