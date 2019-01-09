---
typora-root-url: img
typora-copy-images-to: img
---

# 搭建redis主从

a) ⼀个master可以拥有多个slave，⼀个slave⼜可以拥有多个slave，如此下去，形成了强⼤的多级服务器集群架构

b) master用来写数据，slave用来读数据，经统计：网站的读写比率是10:1

c) 通过主从配置可以实现读写分离

d) master和slave都是一个redis实例

## 搭建主从redis

### 主redis配置

1. 查看当前主机ip地址

```shell
ifconfig
```

2. 修改 etc/redis/redis.conf

```shell
sudo vim redis.conf

bind `本机ip`
```

3. 重启reids

查看redis占用的进程号

```shell
ps aux|grep redis
```

杀死进程

```shell
sudo kill -9 进程号
```

重新启动进程

```shell
sudo redis-server /etc/redis/redis.conf
```

## 从redis配置

1.复制配置文件mget a1 a2 a3mget a1 a2 a3

```shell
sudo cp redis.conf slave.conf
```

2.编辑slave.conf

```shell
bind `本机ip`
slaveof `本机ip` 6379
port 6378
```

3.开启redis服务

```shell
sudo redis-server slave.conf
```

4.查看主从关系

```shell
redis-cli -h `本机ip` info Replication
```



## 操作数据

1.进入主服务器

```shell
redis-cli -h 192.168.79.43 -p 6379
```

2.进入从服务器

```shell
redis-cli -h 192.168.79.43 -p 6379
```

3.在master上读数据

```shell
mset a1 11 a2 22 a3 33
```

4.在slave上读数据

```shell
mget a1 a2 a3
```
