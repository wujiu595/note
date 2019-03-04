---
typora-copy-images-to: img
typora-root-url: img
---

# 3 docker 容器管理

- docker容器技术指Docker是一个由GO语言写的程序运行的“容器”（Linux containers， LXCs）
- containers的中文解释是集装箱。
- Docker则实现了一种应用程序级别的隔离，它改变我们基本的开发、操作单元，由直接操作虚拟主机（VM）,转换到操作程序运行的“容器”上来。

## 3.1简介

#### 容器是什么？

​	容器（Container）：容器是一种轻量级、可移植、并将应用程序进行的打包的技术，使应用程序可以在几乎任何地方以相同的方式运行

- Docker将镜像文件运行起来后，产生的对象就是容器。容器相当于是镜像运行起来的一个实例。

- 容器具备一定的生命周期。
- 另外，可以借助docker ps命令查看运行的容器，如同在linux上利用ps命令查看运行着的进程那样。

		我们就可以理解容器就是被封装起来的进程操作,只不过现在的进程可以简单也可以复杂,复杂的话可以运行1个操作系统.简单的话可以运行1个回显字符串.

#### 容器与虚拟机的相同点

- 容器和虚拟机一样，都会对物理硬件资源进行共享使用。

- 容器和虚拟机的生命周期比较相似（创建、运行、暂停、关闭等等）。

- 容器中或虚拟机中都可以安装各种应用，如redis、mysql、nginx等。也就是说，在容器中的操作，如同在一个虚拟机(操作系统)中操作一样。

- 同虚拟机一样，容器创建后，会存储在宿主机上：linux上位于/var/lib/docker/containers下

#### 容器与虚拟机的不同点

**注意**：容器并不是虚拟机，但它们有很多相似的地方

- 虚拟机的创建、启动和关闭都是基于一个完整的操作系统。一个虚拟机就是一个完整的操作系统。而容器直接运行在宿主机的内核上，其本质上以一系列进程的结合。

- 容器是轻量级的，虚拟机是重量级的。

- 首先容器不需要额外的资源来管理，虚拟机额外更多的性能消耗；

其次创建、启动或关闭容器，如同创建、启动或者关闭进程那么轻松，而创建、启动、关闭一个操作系统就没那么方便了。也因此，意味着在给定的硬件上能运行更多数量的容器，甚至可以直接把Docker运行在虚拟机上。

## 3.2 docker的查看、创建、启动

### 3.2.1 查看容器

```shell
 xxxxxxxxxx docker ps [OPTIONS]#options-a, --all             Show all containers (default shows just running#fieldCONTAINER ID        进程idIMAGE               镜像名COMMAND             使用什么命令创建的CREATED             创建时间STATUS              状态PORTS               运行的端口NAMES               容器名#exampledocker ps    查看运行中的容器docker ps -a 查看所有的容器包括未运行的
```

### 3.2.2 创建容器

```shell
#Usage:	
docker create [OPTIONS] IMAGE [COMMAND] [ARG...]
#OPTIONS
	-t, --tty           		分配一个伪TTY，也就是分配虚拟终端
    -i, --interactive    	    即使没有连接，也要保持STDIN打开
        --name          		为容器起名，如果没有指定将会随机产生一个名称
#COMMAND\ARG:
	COMMAND 表示容器启动后，需要在容器中执行的命令，如ps、ls 等命令
	ARG 表示执行 COMMAND 时需要提供的一些参数，如ps 命令的 aux、ls命令的-a等等        
#example
docker create --name nginx testnginx
docker create -it --name nginx nginx /bin/bash
```

### 3.2.3 启动容器

启动容器有三种方式

- 方法一

启动待启动或已关闭容器

```shell
#Usage:	
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
#options
	-a, --attach		将当前shell的 STDOUT/STDERR 连接到容器上
	-i, --interactive		将当前shell的 STDIN连接到容器上	
#example
docker start nginx
docker start -a nginx
```

- 方法二

基于镜像新建一个容器并启动

```shell
#Usage：
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
#options：
	-t, --tty           	分配一个伪TTY，也就是分配虚拟终端
    -i, --interactive    	即使没有连接，也要保持STDIN打开
        --name          	为容器起名，如果没有指定将会随机产生一个名称
	-d, --detach			在后台运行容器并打印出容器ID
	--rm					当容器退出运行后，自动删除容器

#example
#启动一个镜像输出内容并删除容器
$docker run -it --name mynginx nginx /bin/bash
#注意：
docker run 其实 是两个命令的集合体 docker create + docker start
```

- 方法三

守护进程方式启动docker

更多的时候，需要让Docker容器在后台以守护形式运行。此时可以通过添加-d参数来实现

```shell
#Usage：
docker run -d [image_name] [COMMAND] [ARG...]
#守护进程方式启动容器:
$ docker run -d nginx

nohup docker run -d nginx &
```

## 3.3 暂停、取消暂停、重启、关闭、终止

### 3.3.1 暂停

```shell
#Usage：
docker pause CONTAINER [CONTAINER...]
#demo
docker pause friendly_newton
```

### 3.3.2 取消暂停

```shell
#Usage：
docker unpause CONTAINER [CONTAINER...]
#demo
docker unpause friendly_newton
```

### 3.3.3 重新启动

```shell
#Usage：
docker restart [容器名称]或[容器ID]
#options
	 -t, --time int   		重启前，等待的时间，单位秒(默认 10s) 
#demo
docker restart friendly_newton
```

### 3.3.4 关闭

在生产中，我们会以为临时情况，要关闭某些容器，我们使用 stop 命令来关闭某个容器

```shell
#Usage:	
docker stop [OPTIONS] CONTAINER [CONTAINER...]
#Options:
  -t, --time int   Seconds to wait for stop before killing it (default 10)
#demo
docker stop friendly_newton
```

### 3.3.5 终止

强制并立即关闭一个或多个处于暂停状态或者运行状态的容器

```shell
#Usage:	
docker kill [OPTIONS] CONTAINER [CONTAINER...]
#demo
docker kill 
```



## 3.4 删除容器

删除容器有三种方法：

### 3.4.1 方法一

```shell
#Usage：
$ docker rm [容器名称]或[容器ID]   
#demo:
$ docker rm 1a5f6a0c9443
#attention
不能删除一个正在运行的容器
```

### 3.4.2 方法二

```shell
#Usage：
  docker rm -f [容器名称]或[容器ID]
#demo
$ docker rm -f 8005c40a1d16
```

### 3.4.3 方法三

```shell
#Usage：
$ docker rm -f $(docker ps -a -q)
#按照执行顺序$（）， 获取到现在容器的id然后进行删除
```



## 3.5 进入、退出

### 3.5.1 进入

```shell
#Usage：
Usage:	docker exec [OPTIONS] CONTAINER COMMAND [ARG...]
#demo：
$ docker exec -it d74fff341687 /bin/bash
```

### 3.5.2 退出

```shell
#方法一：
exit
#方法二：
Ctrl + D
```



## 3.6 基于容器创造镜像

```shell
#Usage：
docker export [容器id] > 模板文件名.tar
#创建镜像:
$ docker export ae63ab299a84 > nginx.tar
#导入镜像:
$ cat nginx.tar | docker import - panda-test
```

| 导出（export）导入（import）与保存（save）加载（load）的恩怨情仇 |
| ------------------------------------------------------------ |
| import与load的区别：<br/>import可以重新指定镜像的名字，docker load不可以 |
| export 与 保存 save 的区别：<br/>1、export导出的镜像文件大小，小于 save保存的镜像。<br/>2、export 导出（import导入）是根据容器拿到的镜像，再导入时会丢失镜像所有的历史。 |

## 3.7  日志、信息、端口、重命名

#### 3.7.1 查看容器运行日志

```shell
#Usage：
docker logs [容器id]
#demo：
$ docker logs 7c5a24a68f96
```

#### 3.7.2 查看容器详细信息

```shell
#Usage：
docker inspect [容器id]
#demo：
查看容器全部信息:
$ docker inspect 930f29ccdf8a
查看容器网络信息:
$ docker inspect --format='{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' 930f29ccdf8a
```

#### 查看容器端口信息

```shell
#Usage：
docker port [容器id]
#demo：
$ docker port 930f29ccdf8a
#没有效果没有和宿主机关联
```

#### 容器重命名

```shell
#Usage：    
	docker rename [容器id]或[容器名称] [容器新名称]
#demo：
$ docker rename 930f29ccdf8a u1
```

![1551594977955](/1551594977955.png)











