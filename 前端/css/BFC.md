## 一、常见定位方案

在讲 BFC 之前，我们先来了解一下常见的定位方案，定位方案是控制元素的布局，有三种常见方案:

- 普通流 (normal flow)

> 在普通流中，元素按照其在 HTML 中的先后位置至上而下布局，在这个过程中，行内元素水平排列，直到当行被占满然后换行，块级元素则会被渲染为完整的一个新行，除非另外指定，否则所有元素默认都是普通流定位，也可以说，普通流中元素的位置由该元素在 HTML 文档中的位置决定。

- 浮动 (float)

> 在浮动布局中，元素首先按照普通流的位置出现，然后根据浮动的方向尽可能的向左边或右边偏移，其效果与印刷排版中的文本环绕相似。

- 绝对定位 (absolute positioning)

> 在绝对定位布局中，元素会整体脱离普通流，因此绝对定位元素不会对其兄弟元素造成影响，而元素具体的位置由绝对定位的坐标决定。

## 二、BFC 概念

BFC 即 Block Formatting Contexts (块级格式化上下文)，它属于上述定位方案的普通流。W3C CSS2.1 规范中的一个概念.

具有 BFC 特性的元素可以看作是隔离了的独立容器，容器里面的元素不会在布局上影响到外面的元素，并且 BFC 具有普通容器所没有的一些特性。

通俗一点来讲，可以把 BFC 理解为一个封闭的大箱子，箱子内部的元素无论如何翻江倒海，都不会影响到外部。

## 规则

1. 内部的Box会在垂直方向，一个接一个地放置。
2. Box垂直方向的距离由margin决定。属于同一个BFC的两个相邻Box的margin会发生重叠
3. 每个元素的margin box的左边， 与包含块border box的左边相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如此。
4. BFC的区域不会与float box重叠。
5. BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。反之也如此。
6. 计算BFC的高度时，浮动元素也参与计算

## 三、触发 BFC

- body 根元素
- 浮动元素：float 除 none 以外的值
- 绝对定位元素：position (absolute、fixed)
- display 为 inline-block、table-cells、flex
- overflow 除了 visible 以外的值 (hidden、auto、scroll)

#### 1. 自适应两栏布局

代码：

```html
<style>
    body {
        width: 300px;
        position: relative;
    }
    .aside {
        width: 100px;
        height: 150px;
        float: left;
        background: #f66;
    }
    .main {
        height: 200px;
        background: #fcc;
    }
</style>
<body>
    <div class="aside"></div>
    <div class="main"></div>
</body>
```



　　页面：
　　![此处输入图片的描述](http://p1.qhimg.com/d/inn/4055c62a/4dca44a927d4c1ffc30e3ae5f53a0b79.png)

　　根据`BFC`布局规则第3条：

> 每个元素的margin box的左边， 与包含块border box的左边相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如此。

　　因此，虽然存在浮动的元素aslide，但main的左边依然会与包含块的左边相接触。

　　根据`BFC`布局规则第四条：

> `BFC`的区域不会与`float box`重叠。

　　我们可以通过通过触发main生成`BFC`， 来实现自适应两栏布局。

```css
.main {
    overflow: hidden;
}
```

　　当触发main生成`BFC`后，这个新的`BFC`不会与浮动的aside重叠。因此会根据包含块的宽度，和aside的宽度，自动变窄。效果如下：

　　*![此处输入图片的描述](http://p6.qhimg.com/t01077886a9706cb26b.png)*

#### 2. 清除内部浮动

代码：

```html
<style>
    .par {
        border: 5px solid #fcc;
        width: 300px;
    }
 
    .child {
        border: 5px solid #f66;
        width:100px;
        height: 100px;
        float: left;
    }
</style>
<body>
    <div class="par">
        <div class="child"></div>
        <div class="child"></div>
    </di
```

页面：
　　![此处输入图片的描述](http://p1.qhimg.com/t016035b58195e7909a.png)

　　根据`BFC`布局规则第六条：

> 计算`BFC`的高度时，浮动元素也参与计算

为达到清除内部浮动，我们可以触发par生成`BFC`，那么par在计算高度时，par内部的浮动元素child也会参与计算。

代码：

```css
.par {
    overflow: hidden;
}
```

效果如下：
　　![此处输入图片的描述](http://p2.qhimg.com/t016bbbe5236ef1ffd5.png) ￼

#### 3. 防止垂直 margin 重叠

代码：

```html
<style>
    p {
        color: #f55;
        background: #fcc;
        width: 200px;
        line-height: 100px;
        text-align:center;
        margin: 100px;
    }
</style>
<body>
    <p>Haha</p>
    <p>Hehe</p>
</body
```

页面：
　　![此处输入图片的描述](http://p5.qhimg.com/t01b47b8b7d153c07cc.png)

　　两个p之间的距离为100px，发送了margin重叠。
　　根据BFC布局规则第二条：

> `Box`垂直方向的距离由margin决定。属于同一个`BFC`的两个相邻`Box`的margin会发生重叠

　　我们可以在p外面包裹一层容器，并触发该容器生成一个`BFC`。那么两个P便不属于同一个`BFC`，就不会发生margin重叠了。
代码：

```js
<style>
    .wrap {
        overflow: hidden;
    }
    p {
        color: #f55;
        background: #fcc;
        width: 200px;
        line-height: 100px;
        text-align:center;
        margin: 100px;
    }
</style>
<body>
    <p>Haha</p>
    <div class="wrap">
        <p>Hehe</p>
    </div>
</body>
```

效果如下:
　　![此处输入图片的描述](http://p3.qhimg.com/t0118d1d2badbb00521.png)这时候，两个盒子边距就变成了 200px 