---
typora-root-url: img
typora-copy-images-to: img
---

# redis

## 1. redis简介

### 1.1．redis介绍

redis是一种nosql(not only sql),不仅仅是sql关系型数据库,存储功能

### 1.2. redis 的数据类型

#### 值的类型分为五种：

- 字符串string
-  哈希hash
- 列表list
-  集合set
- 有序集合zset

###  1.3. 中文乱码解决

使用如下方式连接客户端

```shell
redis-cli --raw 
```



## 2. string

### 2.1．设置键值



#### 2.1.1. 设置键值

```redis
set key value
```

#### 2.1.2 设置键值及过期时间

```redis
setex key seconds value
```

#### 2.1.3 设置多个值

```redis
mset k1 v1 k2 v2
```



### 2.2. 查看键

#### 2.2.1查看一个键

```shell
get k1
```

#### 2.2.2查看多个键

```shell
mget k1 k2 k3
```



### 2.3. 追加值

```redis
append key value
```



## 3. 键命令

### 3.1.  查找键

#### 3.1.1. 查找制定的键

支持正则

```redis
keys pattern
```

举例：查找所有键

```shell
keys *
```

#### 3.1.2. 查看键是否存在

```shell
exists key
```

#### 3.1.3.  查看键的类型

```shell
type key
```

### 3.2. 删除键

```shell
del k1 k2
```

### 3.3. 键的过期时间

#### 3.3.1. 设置键的过期时间

```shell
expire key seconds
```

#### 3.3.2. 查看键的过期时间

```shell
ttl key
```



## 4. hash



### 4.1. 设置hash的值

#### 4.1.1.设置单个键的值

```shell
hset key field value
```

#### 4.1.2.设置多个键的值

```shell
hmset key field1 value1 field2 value2
```

### 4.2 查看hash



#### 4.2.1 获取指定键所有的属性

```shell
hkeys key
```

#### 4.2.2 获取指定键的一个属性

```shell
hget key field
```

#### 4.2.3 获取多个属性的值

```shell
hmget key field1 field2
```

#### 4.2.3 获取所有属性的值

```shell
hvals key
```

#### 4.2.4 获取一个hash有多少个属性

```shell
hlen key
```

### 4.2.5 获取键值对

```shell
hgetall key
```



### 4.3 删除

#### 4.3.1 删除整个hash键及值

```shell
del key
```

#### 4.3.2 删除hash的某个属性

```shell
hdel key field1 field2
```



## 5.list

- 列表的元素类型为string
- 按照插⼊顺序排序

### 5.1增加元素

#### 5.1.1 从左侧插入数据

```shell
lpush key value1 value2
```

#### 5.1.2 从右侧插入数据

```shell
rpush key value1 value2
```

#### 5.1.3 在指定位置的前后插入数据

```shell
lindex key before或after value1 value2
```

### 5.2获取元素

#### 5.2.1 获取指定范围内的元素

```shell
lrange key start stop
```

#### 5.2.2 获取所有的元素

```shell
lrange key 0 -1
```



### 5.3设置指定索引位置的元素值

```shell
lset key index value
```



### 5.4删除

删除指定元素

- 将列表中前count次出现的值为value的元素移除
- count > 0: 从头往尾移除
- count < 0: 从尾往头移除
- count = 0: 移除所有

```shell
lrem key count value
```



## 6.set

- ⽆序集合
- 元素为string类型
- 元素具有唯⼀性，不重复

说明：对于集合没有修改操作

### 6.1 增加元素

```shell
sadd key memeber1 member2
```



### 6.2 获取元素

```shell
smembers key
```



### 6.3 删除指定元素

```shell
srem key value
```



## 7. zset

- sorted set，有序集合
- 元素为string类型
- 元素具有唯⼀性，不重复
- 每个元素都会关联⼀个double类型的score，表示权重，通过权重将元素从⼩到⼤排序
- 说明：没有修改操作



### 7.1增加



```shell
zadd key score1 member1 score2 member2
```



### 7.2 获取

- 返回指定范围内的元素
- start、stop为元素的下标索引
- 索引从左侧开始，第⼀个元素为0
- 索引可以是负数，表示从尾部开始计数，如-1表示最后⼀个元素

#### 7.2.1 获取start和stop之间的元素

```shel
zrange key start stop
```

#### 7.2.2 获取所有元素

```shell
zrange key 0 -1
```

#### 7.2.3 返回score值在min和max之间的成员

```shell
zrangebyscore key min max
```

#### 7.2.4 返回成员member的score值

```shell
zscore key member
```



### 7.3删除

#### 7.3.1删除指定元素

```shell
zrem key member1 member2 ...
```

#### 7.3.2 删除权重在指定范围的元素

```shell
zremrangebyscore key min max
```













