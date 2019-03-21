# go、chanel、select

## go

​	go关键字是启动goroutine的唯一途径，一个go语句就代表了一个函数或一个方法并发的执行。go后面的表达式会重新放在一个goroutine中运行。go后面的方法可以有返回值但是即使有返回值也会被丢弃。



```go
import "fmt"

func main(){
    go fmt.Println("hello world!!")
}
```

上面代码大部分时候都不会打印出结果。go 函数会被封装到一个G中至于什么时候执行就要看调度器了。

## chanel

