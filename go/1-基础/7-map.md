---
typora-copy-images-to: img
typora-root-url: img
---

# 7 map字典

字典在数学上的词汇是映射，将一个集合中的所有元素关联到另一个集合中的部分或全部元素，并且只能是一一映射或者多对一映射。

![1551884921179](/1551884921179.png)

## 7.1 字典的创建

map和slice一样都是引用类型，所以都是通过make方法创建

```go
package main
import  "fmt"
func main() {
	m:=make(map[string]int)
	fmt.Println(len(m),m)
}
--------------
0 map[]
```

### 7.1.1 创建并初始化

使用 make 函数创建的字典是空的，长度为零，内部没有任何元素。如果需要给字典提供初始化的元素，就需要使用另一种创建字典的方式。

```go
package main
import "fmt"
func main(){
	m:=map[string]int{
		"chinese":20,
		"english":30,
	}
	fmt.Println(m)
}
```

## 7.2 字典的读写

字典可以使用中括号来读写内部元素，使用 delete 函数来删除元素。

### 7.2.1读取元素

```go
package main
import "fmt"
func main(){
	m:=map[string]int{
		"chinese":20,
		"english":30,
	}
	fmt.Println(m["chinese"])
}
----------------------
20
```

### 7.2.2 删除元素

调用delete方法要传递两个参数

1. 想要操作的map
2. 想要删除的键

```go
package main
import "fmt"
func main(){
	m:=map[string]int{
		"chinese":20,
		"english":30,
	}
	fmt.Println("删除前",m)
	delete(m,"chinese")
	fmt.Println("删除后",m)
}
-------------------------
删除前 map[yu:20 ying:30]
删除后 map[ying:30]
```

### 7.2.3 新增和修改元素

使用`m[key]=value`方法可以向map中添加元素但如果key在map中存在则是修改元素的值

```go
package main
import "fmt"
func main(){
	m:=map[string]int{
		"chinese":20,
		"english":30,
	}
	fmt.Println("添加前",m)
	m["math"]=100
	fmt.Println("添加后",m)
	m["chinese"]=80
	fmt.Println("修改后",m)
}
----------------------------
添加前 map[yu:20 ying:30]
添加后 map[yu:20 ying:30 math:100]
修改后 map[math:100 yu:80 ying:30]
```

## 7.3 字典 key 不存在会怎样？

删除操作是，如果key不存在会怎么样，delete函数会静默处理。因为delete没有返回值我们无法知道delete方法是否真正的删除了元素。

读取时如果key值不存在也不会抛出异常，他会返回value类型对应的零值。

这个时候我们就要使用map的特殊语法。

```go
package main
import "fmt"
func main(){
	m:=map[string]int{
		"chinese":20,
		"english":30,
	}
	value,ok:=m["chinese"]
	fmt.Println("chinese在m中存在",value,ok)
	value,ok=m["math"]
	fmt.Println("math在m中不存在",value,ok)
}
-----------------------
chinese在m中存在 20 true
math在m中不存在 0 false
```

字典的下标读取可以返回两个值，使用第二个返回值都表示对应的 key 是否存在。

## 7.4 字典的遍历

字典的便利也有两种方法

### 7.4.1 方法一

```go
package main
import "fmt"
func main(){
	m:=map[string]int{
		"chinese":20,
		"english":30,
	}
	for k,v:=range m{
		fmt.Println(k,v)
	}
}
```

### 7.4.2 方法二

```go
package main
import "fmt"
func main(){
	m:=map[string]int{
		"chinese":20,
		"english":30,
	}
	for k:=range m{
		fmt.Println(k,m[k])
	}
}
```

### 7.4.3 获取key和value列表

go语言中没有提供 keys和values这样的方法，这就意味着，如果你想要获取key列表，就得自己循环一下

```go
package main

import "fmt"

func main(){
	m:=map[string]int{
		"chinese":20,
		"english":30,
	}
	keys:=make([]string,0,len(m))
	values:=make([]int,0,len(m))
	for k,v:=range m{
		keys=append(keys,k)
		values=append(values,v)
	}
	fmt.Println("keys=",keys)
	fmt.Println("values=",values)
}
--------------------------------
keys= [english chinese]
values= [30 20]

```

















