# docker machine

## 查看docker machine

```shell
docker-machine version
```



```
docker-machine ls
```



```
docker-machine env my-machine
```



## 创建、启动、停止

```
docker-machine create my-machine
```



进入docker-machine

```
docker-machine ssh pg-master
```



启动docker-machine

```
docker-machine start
```



停止docker-machine

```
docker-machine stop
```



docker machine 默认密码

```
account：docker
password：tcuser
```

