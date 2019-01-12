## Dom操作

Dom操作也叫做元素节点操作，它指的是改变html的标签结构，它有两种情况：
1、移动现有标签的位置
2、将新创建的标签插入到现有的标签中

**创建新标签**

```javascript
var $div = $('<div>'); //创建一个空的div
var $div2 = $('<div>这是一个div元素</div>');
```

**移动或者插入标签的方法** 
1、append()和appendTo()：在现存元素的内部，从后面放入元素

```javascript
var $span = $('<span>这是一个span元素</span>');
$('#div1').append($span);
......
<div id="div1"></div>
```

2、prepend()和prependTo()：在现存元素的内部，从前面放入元素

3、after()和insertAfter()：在现存元素的外部，从后面放入元素

4、before()和insertBefore()：在现存元素的外部，从前面放入元素

**删除标签**

```javascript
$('#div1').remove();
```

 