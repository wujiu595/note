# go语言连接redis及操作

## 1. go连接redis

go语言连接reids需要先导入第三方包redisgo

```go
go get -u -v github.com/gomodule/redigo/redis
```

连接redis

```go
package main
import github.com/gomodule/redigo/redis
func main(){
    conn,err:=redis.Dial("tcp","127.0.0.1:6379")
}
```



## 2.go操作redis

连接上redis后会返回一个conn对象,我们可以调用他的Do方法完成redis操作

```go
conn.Do("set","key","value")
```



## 3.回复助手函数

Bool，Int，Bytes，map，String，Strings和Values函数将回复转换为特定类型的值。为了方便地包含对连接Do和Receive方法的调用，这些函数采用了类型为error的第二个参数。如果错误是非nil，则辅助函数返回错误。如果错误为nil，则该函数将回复转换为指定的类型：

```够]
redis.Ints(redis.Do("MGET", "key1", "key2"))
```



## 4.san函数

```go
func Scan(src [] interface {},dest ... interface {})([] interface {},error)
```

Scan函数从src复制到dest指向的值。

```go
var value int
var value string

reply, err := redis.Values(c.Do("MGET", "key1", "key2"))
if err != nil {
    //处理错误代码
}
if _, err := redis.Scan(reply, &value1, &value2); err != nil {
    // 处理错误代码
}
```

## 5.序列化与反序列化

序列化(字节化)

```go
var articleTypes articleTypes []models.ArticleType
var buffer bytes.Buffer
//定义一个编码器
enc := gob.NewEncoder(&buffer)
//把articleTypes编码
enc.Encode(&articleTypes)
//设置到redis种
conn.Do("set","articleTypes",buffer.Bytes())

```

反序列化（反字节化）

```go
var articleTypes articleTypes []models.ArticleType
resp ,err:= redis.Bytes(conn.Do("get","articleTypes"))
//定义一个解码器
dec :=gob.NewDecoder(bytes.NewReader(resp))
//解码
dec.Decode(&articleTypes)
```