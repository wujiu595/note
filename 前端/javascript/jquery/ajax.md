## ajax

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

  ​        

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





### ajax jsonp



type，async，url，dataType，jsonp，jsonpCallback，success，error

```html

<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml" >
<head>
     <title>Untitled Page</title>
      <script type="text/javascript" src=jquery.min.js"></script>
      <script type="text/javascript">
     jQuery(document).ready(function(){ 
        $.ajax({
             type: "get",
             async: false,
             url: "http://flightQuery.com/jsonp/flightResult.aspx?code=CA1998",
             dataType: "jsonp",
             jsonp: "callback",//传递给请求处理程序或页面的，用以获得jsonp回调函数名的参数名(一般默认为:callback)
             jsonpCallback:"flightHandler",//自定义的jsonp回调函数名称，默认为jQuery自动生成的随机函数名，也可以写"?"，jQuery会自动为你处理数据
             success: function(json){
                 alert('您查询到航班信息：票价： ' + json.price + ' 元，余票： ' + json.tickets + ' 张。');
             },
             error: function(){
                 alert('fail');
             }
         });
     });
     </script>
     </head>
  <body>
  </body>

```

