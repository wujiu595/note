## jquery选择器

**jquery用法思想一** 
选择某个网页元素，然后对它进行某种操作

**jquery选择器** 
jquery选择器可以快速地选择元素，选择规则和css样式相同，使用length属性判断是否选择成功。

```javascript
$('#myId') //选择id为myId的网页元素
$('.myClass') // 选择class为myClass的元素
$('li') //选择所有的li元素
$('#ul1 li span') //选择id为为ul1元素下的所有li下的span元素
$('input[name=first]') // 选择name属性等于first的input元素
```

**对选择集进行过滤**

```javascript
$('div').has('p'); // 选择包含p元素的div元素
$('div').not('.myClass'); //选择class不等于myClass的div元素
$('div').eq(5); //选择第6个div元素
```

**选择集转移**

```javascript
$('#box').prev(); //选择id是box的元素前面紧挨的同辈元素
$('#box').prevAll(); //选择id是box的元素之前所有的同辈元素
$('#box').next(); //选择id是box的元素后面紧挨的同辈元素
$('#box').nextAll(); //选择id是box的元素后面所有的同辈元素
$('#box').parent(); //选择id是box的元素的父元素
$('#box').children(); //选择id是box的元素的所有子元素
$('#box').siblings(); //选择id是box的元素的同级元素
$('#box').find('.myClass'); //选择id是box的元素内的class等于myClass的元素
```

**判断是否选择到了元素** 
jquery有容错机制，即使没有找到元素，也不会出错，可以用length属性来判断是否找到了元素,length等于0，就是没选择到元素，length大于0，就是选择到了元素。

```javascript
var $div1 = $('#div1');
var $div2 = $('#div2');
alert($div1.length); // 弹出1
alert($div2.length); // 弹出0
......
<div id="div1">这是一个div</div>
```