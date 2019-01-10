## jquery绑定事件

给元素绑定click事件，可以用如下方法：

```javascript
$('#btn1').click(function(){

    // 内部的this指的是原生对象

    // 使用jquery对象用 $(this)

})
```

### **获取元素的索引值** 

有时候需要获得匹配元素相对于其同胞元素的索引位置，此时可以用index()方法获取

```javascript
var $li = $('.list li').eq(1);
alert($li.index()); // 弹出1
......
<ul class="list">
    <li>1</li>
    <li>2</li>
    <li>4</li>
    <li>5</li>
    <li>6</li>
</ul>
```

### 事件函数列表:

```javascript
blur() 元素失去焦点
focus() 元素获得焦点
change() 当表单元素的值发生改变时
click() 鼠标单击
mouseover() 鼠标进入（进入子元素也触发）
mouseout() 鼠标离开（离开子元素也触发）
mouseenter() 鼠标进入（进入子元素不触发）
mouseleave() 鼠标离开（离开子元素不触发）
ready() DOM加载完成
submit() 用户递交表单
```
### 元素显示隐藏

jquery将元素的显示和隐藏效果封装成了三个方法，通过这三个方法可以快速的制作元素的显示和隐藏效果

```
show()  // 将元素显示，相当使用了css方法  css({'display':'block'})
hide()  // 将元素显示，相当使用了css方法  css({'display':'none'})
toggle()  // 切换元素的显示和隐藏，在方法内部获取了元素的显示状态，然后反向设置元素的显示效果
```