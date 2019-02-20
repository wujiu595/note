```shell
npm install react-transition-group
```

## Transition

底层的实现

# cssTransiton



```html
import { CSSTransition } from 'react-transition-group';

<CSSTransition
     //什么时候增加动画          
     in={this.state.show}          
     timeout={1000}
     classNames='fade'
     appear={true}          
>
    
</CSSTransition>    
```

### fade-enter, fade-enter-active, fade-enter-done

入场动画的第一时刻:fade-enter

入场动画的第二个时刻到入场动画执行完的时候:fade-enter-active

入场动画执行完成之后:fade-enter-done

### fade-exit, fade-exit-active, fade-exit-done

出场动画的第一时刻:fade-enter

出场动画的第二个时刻到入场动画执行完的时候:fade-enter-active

出场动画执行完成之后:fade-enter-done

```js
fade-enter, fade-enter-active, fade-enter-done, fade-exit, fade-exit-active, fade-exit-done, fade-appear, and fade-appear-activ
```

### fade-appear,fade-appear-activ

首次加载页面时



### 钩子函数

```js
onEnter onEntering onEntered onExit onExiting onExited
```



## TranstionGroup

```html
import { CSSTransition,TranstionGroup } from 'react-transition-group';
<TranstionGroup>
	<CSSTransition
	     //什么时候增加动画          
	     in={this.state.show}          
	     timeout={1000}
	     classNames='fade'
	     appear={true}          
	> 
	</CSSTransition>
</TranstionGroup>
```







