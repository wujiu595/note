## 搭建集群reids

### 为什么要有集群

1. 服务器可能因为代码原因，人为原因，或者自然灾害等造成服务器损坏。数据服务就挂掉了
2. 大公司都会有很多的服务器(华东地区、华南地区、华中地区、华北地区、西北地区、西南地区、东北地区、台港澳地区机房)



### 什么是集群

集群是一组相互独立的、通过高速网络互联的计算机，它们构成了一个组，并以单一系统的模式加以管理。一个客户与集群相互作用时，集群像是一个独立的服务器。集群配置是用于提高可用性和可缩放性。



### redis集群

软件层面：只有一台电脑，在这台电脑上启动了多台redis服务

硬件层面：存在多台实体电脑,每台电脑都启动了一个redis或者多个redis服务



**参考阅读**

Redis搭建集群<http://www.cnblogs.com/wuxl360/p/5920330.html>

go语言redis-cluster开源客户端<https://github.com/gitstliu/go-redis-cluster>



### **配置机器1**

- 在演示中，192.168.110.37为当前ubuntu机器的ip
- 在192.168.110.37上进⼊Desktop⽬录，创建conf⽬录
- 在conf⽬录下创建⽂件7000.conf，编辑内容如下

```shell
port 7000
bind 192.168.110.37
daemonize yes
pidfile 7000.pid
cluster-enabled yes
cluster-config-file 7000_node.conf
cluster-node-timeout 15000
appendonly yes
```

- 在conf⽬录下创建⽂件7001.conf，编辑内容如下

```shell
port 7001
bind 192.168.110.37
daemonize yes
pidfile 7001.pid
cluster-enabled yes
cluster-config-file 7001_node.conf
cluster-node-timeout 15000
appendonly yes
```

- 在conf⽬录下创建⽂件7002.conf，编辑内容如下

```shell
port 7002
bind 192.168.110.37
daemonize yes
pidfile 7002.pid
cluster-enabled yes
cluster-config-file 7002_node.conf
cluster-node-timeout 15000
appendonly yes
```



总结：这三个文件的配置区别只有port、pidfile、cluster-config-file三项

使用配置文件启动redis服务

redis-server 7000.conf

redis-server 7001.conf

redis-server 7002.conf

### **配置机器2**

- 在演示中，192.168.110.38为当前ubuntu机器的ip
- 在192.168.110.38上进⼊Desktop⽬录，创建conf⽬录
- 在conf⽬录下创建⽂件7003.conf，编辑内容如下

```shell
port 7003
bind 192.168.110.38
daemonize yes
pidfile 7003.pid
cluster-enabled yes
cluster-config-file 7003_node.conf
cluster-node-timeout 15000
appendonly yes
```

- 在conf⽬录下创建⽂件7004.conf，编辑内容如下

```shell
port 7004
bind 192.168.110.38
daemonize yes
pidfile 7004.pid
cluster-enabled yes
cluster-config-file 7004_node.conf
cluster-node-timeout 15000
appendonly yes
```

- 在conf⽬录下创建⽂件7005.conf，编辑内容如下

```shell
port 7005
bind 192.168.110.38
daemonize yes
pidfile 7005.pid
cluster-enabled yes
cluster-config-file 7005_node.conf
cluster-node-timeout 15000
appendonly yes
```

总结：这三个文件的配置区别只有port、pidfile、cluster-config-file三项

使用配置文件启动redis服务

redis-server 7003.conf

redis-server 7004.conf

redis-server 7005.conf

### **创建集群**

- redis的安装包中包含了redis-trib.rb，⽤于创建集群  //ruby
- 接下来的操作在192.168.110.37机器上进⾏
- 将命令复制，这样可以在任何⽬录下调⽤此命令

```shell
sudo cp /usr/share/doc/redis-tools/examples/redis-trib.rb /usr/local/bin/
```

- 安装ruby环境，因为redis-trib.rb是⽤ruby开发的

```shell
sudo apt-get install ruby
```

- 运⾏如下命令创建集群

```shell
redis-trib.rb create --replicas 1 192.168.110.37:7000 192.168.110.37:7001 192.168.110.37:7002 192.168.110.38:7003 192.168.110.38:7004 192.168.110.38:7005
```

- 执⾏上⾯这个指令在某些机器上可能会报错,主要原因是由于安装的 ruby 不是最 新版本 

天朝的防⽕墙导致⽆法下载最新版本,所以需要设置 gem 的源

解决办法如下：

//先查看⾃⼰的 gem 源是什么地址

```shell
gem source -l  //如果是https://rubygems.org/ 就需要更换
```

// 更换指令为

```shell
gem sources --add https://gems.ruby-china.com --remove https://rubygems.org/
```

// 通过 gem 安装 redis 的相关依赖

```shell
sudo gem install redis
```

// 然后重新执⾏指令

```shell
redis-trib.rb create --replicas 1 192.168.110.37:7000 192.168.110.37:7001 192.168.110.37:7002 192.168.110.38:7003 192.168.110.38:7004 192.168.110.38:7005
```

- 当前搭建的主服务器为7000、7001、7003，对应的从服务器是7005、7004、7002
- 在192.168.110.37机器上连接7002，加参数-c表示连接到集群

```shell
redis-cli -h 192.168.110.37 -c -p 7002
```

- ⾃动跳到了7003服务器，并写⼊数据成功

- 在7003可以获取数据，如果写入数据又重定向到7001(负载均衡)

   注意点:Redis 集群会把数据存在⼀个 master 节点，然后在这个 master 和其对应的salve 之间进⾏数据同步。当读取数据时，也根据⼀致性哈希算法到对应的cluster, _ := redis.NewCluster(	&redis.Options{		StartNodes: []string{"192.168.110.37:7000", "192.168.110.37:7001", "192.168.110.37:7002","192.168.110.38:7003","192.168.110.38:7004","192.168.110.38:7005"},		ConnTimeout: 50 * time.Millisecond,		ReadTimeout: 50 * time.Millisecond,		WriteTimeout: 50 * time.Millisecond,		KeepAlive: 16,		AliveTime: 60 * time.Second,	})cluster.Do("set","name","itheima")

   


  	name,_ := redis.String(cluster.Do("get","name"))
  	
  	beego.Info(name)
  	
  	this.Ctx.WriteString("集群创建成功")

-  master 节 点获取数据。只有当⼀个master 挂掉之后，才会启动⼀个对应的 salve 节点，充 当 master

-  需要注意的是：必须要3个或以上的主节点，否则在创建集群时会失败，并且当存 活的主节点数⼩于总节点数的⼀半时，整个集群就⽆法提供服务了

### **go语言redis-cluster开源客户端**

安装：

```shell
go get github.com/gitstliu/go-redis-cluster
```

**示例代码**

```go
func (this*ClusterController)Get(){
	cluster, _ := redis.NewCluster(
		&redis.Options{
			StartNodes: []string{"192.168.110.37:7000", "192.168.110.37:7001", "192.168.110.37:7002","192.168.110.38:7003","192.168.110.38:7004","192.168.110.38:7005"},
		 	ConnTimeout: 50 * time.Millisecond,
			ReadTimeout: 50 * time.Millisecond,
			WriteTimeout: 50 * time.Millisecond,
			KeepAlive: 16,
			AliveTime: 60 * time.Second,
		})
	cluster.Do("set","name","itheima")
	name,_ := redis.String(cluster.Do("get","name"))
	beego.Info(name)
	this.Ctx.WriteString("集群创建成功")
}
```