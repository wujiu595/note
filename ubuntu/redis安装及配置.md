# Redis

## 1 安装redis dserrver

```shell
sudo  apt-get install redis-server 
```

不推荐

```shell
service  redis status //查看状态
service  redis start //开启
service  redis stop //停止
service  redis restart //重启
```

推荐的方式

```shell
开启 sudo redis-server /etc/redis/redis.conf //使用制定的redis配置文件来启动
关闭 ps aux|grep redis
```

如果通过service启动的redis通过上诉推荐方式启动需要如下操作

```shell
service redis stop //停止服务

sudo su //进入超级管理员用户
cd /var/lib/redis
rm -r dump.rdb 

然后就可以通过 sudo redis-server /etc/redis/redis.conf 的形式开启redis
```





## 2 配置redis服务

### 2.1开启远程连接

找到/et/redis/redis.conf文件修改如下   ，注释掉  127.0.0.1   #bind 127.0.0.1，如果不需要远程连接redis则不需要这个操作

```mysql
vim /etc/redis/redis.conf

 #bind 127.0.0.1
```

### 2.2设置密码

找到/et/redis/redis.conf文件修改如下   ，添加  requirepass kingredis（密码设置为kingredis）

```mysql
vim /etc/redis/redis.conf
requirepass kingredis
```

## 3 测试redis服务

### 3.1 本地登录

```mysql
redis-cli
set a "sss"
get a
```

## 3.2 远程登录

在本地window打开一个客户端 ，cd到redis安装的目录，主要是要有redis-cli.exe的目录输入 redis-cli -h redis服务器IP -p redis服务端口号(默认6379)

```shell
redis-cli -h redis服务器IP -p redis服务端口号(默认6379)
```





















