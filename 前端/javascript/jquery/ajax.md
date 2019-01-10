## ajax2

ajax一个前后台配合的技术，它可以让javascript发送http请求，与后台通信，获取数据和信息。ajax技术的原理是实例化xmlhttp对象，使用此对象与后台通信。jquery将它封装成了一个函数$.ajax()，我们可以直接用这个函数来执行ajax请求。



### $.ajax使用方法

```javascript
$.ajax({
    url: `url`,
    type: `method`,
    dataType: `json`,
    data:`data`
    success:function(data){
    },
    error:function(){
    }
});
```



常见参数:

- url               请求地址

- type            请求方法

- dataType    请求数据格式

- data             数据

- success        请求成功的回调函数

- error             请求失败的回调函数

- async            请求失败的回调函数

  

### 推荐写法

使用'done()'的方法用来处理成功的请求回调,"fail()"请求数据失败的回调函数

```javascript
$.ajax({
    url: `url`,
    type: `method`,
    dataType: `json`,
    data:`data`
})
.done(function(data) {
})
.fail(function() {
});
```



