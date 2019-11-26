在《7-map》中我们使用两种两种方法进行了map的遍历，接下来我们将进一步探究一下两种方法的性能问题

main.go

```go
package main
import "fmt"

var m=map[string]int{
"chinese":20,
"english":30,
}

func Range1()  {
	for k,v:=range m{
		_,_=k,v
	}
}

func Range2()  {
	for k:=range m{
		_,_=k,m[k]
	}
}


func main(){
	Range1()
    Range2()
}
```

main_test.go

```go
package main
import "testing"
func BenchmarkTest1(b *testing.B)  {
	for i:=0;i<b.N;i++{
		Range1()
	}
}
func BenchmarkTest2(b *testing.B)  {
	for i:=0;i<b.N;i++{
		Range2()
	}
}
```

结果

```
[root@VM_0_14_centos test]# go test -bench=. -
goos: linux
goarch: amd64
pkg: test
BenchmarkTest1 	20000000	        68.2 ns/op
BenchmarkTest2 	20000000	        85.3 ns/op
PASS
ok  	test	3.233s
```







main.go

```go
package main

type people struct {
	name string
	age int
}

var slice = setSlice()

func setSlice()[]people  {
	slice:=make([]people,100)
	for i:=0;i<len(slice);i++{
		slice[i]=people{
			"wagnshuai",
				12,
		}
	}
	return slice
}

func Range1()  {
	n:=len(slice)
	for i:=0;i<n;i++{
		_,_=i,slice[i]
	}[root@VM_0_14_centos test]# go test -bench=. -
goos: linux
goarch: amd64
pkg: test
BenchmarkTest1 	20000000	        68.2 ns/op
BenchmarkTest2 	20000000	        85.3 ns/op
PASS
ok  	test	3.233s

}

func Range2()  {
	for i,v:=range slice{
		_,_=i,v
	}
}

func Range3()  {
	for i:=range slice{
		_,_=i,slice[i]
	}
}

func main()  {
	Range1()
	Range2()
	Range3()jieguo
}

```
main_test.go
```go
package main

import "testing"

func BenchmarkTest1(b *testing.B)  {
	for i:=0;i<b.N;i++{
		Range1()
	}
}

func BenchmarkTest2(b *testing.B)  {
	for i:=0;i<b.N;i++{
		Range2()
	}
}

func BenchmarkTest3(b *testing.B)  {
	for i:=0;i<b.N;i++{
		Range3()
	}
}

```
测试结果
```
[root@VM_0_14_centos test]# go test -bench=. -

goos: linux

goarch: amd64

pkg: test

BenchmarkTest1 	30000000	        44.9 ns/op

BenchmarkTest2 	30000000	        57.0 ns/op

BenchmarkTest3 	30000000	        58.9 ns/op

PASS

ok  	test	4.990s
```



