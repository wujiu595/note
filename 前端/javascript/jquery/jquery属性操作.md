## jquery属性操作

1、html() 取出或设置html内容

```javascript
// 取出html内容

var $htm = $('#div1').html();

// 设置html内容

$('#div1').html('<span>添加文字</span>');
```

2、prop() 取出或设置某个属性的值

```javascript
// 取出图片的地址

var $src = $('#img1').prop('src');

// 设置图片的地址和alt属性

$('#img1').prop({src: "test.jpg", alt: "Test Image" });
```

3、attr()获取自定义属性

```javascript
var $src = $('#img1').attr('goodsId');
$('#img1').attr({src: "test.jpg", alt: "Test Image" });
```

3、text()取出html的文字内容

```javascript
var $src = $('#html').text();
$('#html').text("nihao");
```

4、val()取出表单的值

```javascript
var $src = $('#input').val();
$('#html').val("nihao");
```





