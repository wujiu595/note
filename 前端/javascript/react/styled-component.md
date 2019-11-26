#                                                                               Styled-component

### 安装

```shell
npm install styled-components --save
```

### 基本使用

```js
import styled  from 'styled-components'

export const Title=styled.h1`
     background:red
`
```

### 对具有某class的组件定义新样式

```js
export const TipTitleItem = styled.div`
    color: #969696;
     &.left{
      float: left;
    }
    &.right{
      float: right;
    }
`;
```

### 对某组件下的class定义新样式

```js
export const NavWrapper=styled.div`
	 font-size: 10px;
    .iconfont{
      font-size: 20px;
    }
`;
```

### 设置组件的属性

```js
export const Input=styled.input.attrs({placeholder:"搜索"})`

`;
```

### 伪类样式

```js
export const Input=styled.input`
    &::placeholder{
      color: #aaa;
    }
`;
```

