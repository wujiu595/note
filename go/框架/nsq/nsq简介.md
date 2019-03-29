---
typora-copy-images-to: ../img
typora-root-url: ../img
---

# nsq组件

nsqd
单个nsqd实例可以用来树立多个数据流。

topic
数据流也叫topic， 一个topic可以有多个channel。

channel
每一个channel都接收该topic下面的所有消息，然后发送给消费者。channel会均匀的发送消息给消费者，不重复消费。

![](/f1434dc8-6029-11e3-8a66-18ca4ea10aca-1553736013807.gif)

Topics and channels 都没有优先级，topic在两种情况下会被使用到，在发送数据到一该topic时候首次使用，或者有消费者订阅该topic下的某个channel时候。channel在有消费者订阅了该channle的时候使用。 


nsqlookupd
这是一个辅助应用，启动目录服务保存着nsqd的地址，consumer可以通过nsqlookupd查询到它消费的channel、topic对应的nsqd服务。consumer不需要了解producer的信息。

nsqadmin
提供了一个webUI，我们可以查询到topics/channels/consumers这些信息，便于监管。

nsq消息传递确保
nsq可以确保发送过来的消息至少被一个消费者消费；为了确保消息消费，重复消费是不可避免的。nsq通过以下来确保消息消费

客户端表示它们可以接收消息
NSQ发送消息，暂时将消息存在本地
客户端回复FIN 或者 REQ 表示成功或者重发。如果客户端未能及时发送，则NSQ将重复发送消息给该客户端。
nsq的安装使用

```shell

```

启动

>在一个shell中：nsqlookupd 
>
>另一个shell中：nsqd –lookupd-tcp-address=127.0.0.1:4160 
>
>第三个shell中：nsqadmin –lookupd-http-address=127.0.0.1:4161

发送接收

>curl -d ‘hello world 1’ ‘http://127.0.0.1:4151/pub?topic=test’ //发送消息 
>nsq_to_file –topic=test –output-dir=/tmp –lookupd-http-address=127.0.0.1:4161

![1553735538612](/1553735538612.png)

值得注意的是：nsq_to_file并不会精确的告诉topic产生的位置，它从nsqlookupd获取信息，不会丢掉消息。

nsq go生产者和消费者
go get github.com/nsqio/go-nsq

发送端

```go
package main

import (

        "github.com/nsqio/go-nsq"

       )

var producer *nsq.Producer

func main() {

    nsqd := "127.0.0.1:4150"

    producer, err := nsq.NewProducer(nsqd, nsq.NewConfig())

    producer.Publish("test", []byte("nihao"))  //topic， message

    if err != nil {

        panic(err)

    }

}
```

消费端

```go
package main

import (
 "fmt"
 "sync"
 "github.com/nsqio/go-nsq"
)

type NSQHandler struct {

}

func (this *NSQHandler) HandleMessage(msg *nsq.Message) error {
    fmt.Println("receive", msg.NSQDAddress, "message:", string(msg.Body))
    return nil
}

func testNSQ() {
    waiter := sync.WaitGroup{}
    waiter.Add(1)
go func() {
    defer waiter.Done()
    config:=nsq.NewConfig()
    config.MaxInFlight=9
//建立多个连接
for i := 0; i<5; i++ {
    consumer, err := nsq.NewConsumer("test", "zj", config)  //topic， channel， config
    if nil != err {
        fmt.Println("err", err)
        return
    }
    consumer.AddHandler(&NSQHandler{})
    err = consumer.ConnectToNSQD("127.0.0.1:4150")
    if nil != err {
        fmt.Println("err", err)
        return
    }
}
    select{}

}()

waiter.Wait()
}

func main() {
        testNSQ();
}
```
![1553735793500](/1553735826722.png)

nsq性能介绍
Kafka自身服务与消息的生产和消费都依赖与Zookeeper，使用Scala语言开发。因为其消息的消费使用客户端Pull方式，消息可以被多个客户端消费，理论上消息会重复，但是不会丢失（除非消息过期）。因此比较常用的场景是作为日志传输的消息平台。

nsq默认是不保存消息，消息传递时候通常是无序的，可以保留信息去check时间戳，nsq更适合处理数据量大但是彼此间没有顺序关系的消息。

![1553735846966](/1553735846966.png)

