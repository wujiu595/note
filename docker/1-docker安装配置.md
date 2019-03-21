---
typora-copy-images-to: img
typora-root-url: img
---

# 1 docker环境准备

## 1.1 docker的安装

```shell
#安装基本软件
$ sudo apt-get update
$ sudo apt-get install apt-transport-https ca-certificates curl software-properties-common lrzsz -y
#使用官方推荐源{不推荐}#
$ sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
#使用阿里云的源{推荐}（ubuntu）
$ sudo curl -fsSL https://mirrors.aliyun.csudo apt-get install docker-ceom/docker-ce/linux/ubuntu/gpg | sudo apt-key add -
$ sudo add-apt-repository "deb [arch=amd64] https://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable"
#{推荐}（deepin）
$ sudo curl -fsSL https://mirrors.ustc.edu.cn/docker-ce/linux/debian/gpg | sudo apt-key add -
$ sudo add-apt-repository "deb [arch=amd64] https://mirrors.ustc.edu.cn/docker-ce/linux/debian  wheezy stable"

sudo apt-get install docker-ce
#软件源升级
$ sudo apt-get update

#安装docker
$ sudo apt-get install docker-ce -y

#注：
#可以指定版本安装docker：
$ sudo apt-get install docker-ce=<VERSION> -y
  
#查看支持的docker版本
$ sudo apt-cache madison docker-ce
#测试docker
docker version
```

## 1.2 docker服务管理

```shell
#基本格式
systemctl [参数] docker
#参数详解：
	start         开启服务
    stop          关闭
    restart       重启
    status        状态
```

## 1.3 docker的删除

### 1.3.1 删除docker

```shell
$  sudo apt-get purge docker-ce -y
$  sudo rm -rf /etc/docker
$  sudo rm -rf /var/lib/docker/
```

### 1.3.2 目录介绍

```shell
/etc/docker/                #docker的认证目录
/var/lib/docker/            #docker的应用目录
```

## 1.4 docker加速器

在国内使用docker的官方镜像源，会因为网络的原因，造成无法下载，或者一直处于超时。所以我们使用 daocloud的方法进行加速配置。
加速器文档链接：http://guide.daocloud.io/dcs/daocloud-9153151.html

### 1.4.1 方法:

访问 https://dashboard.daocloud.io 网站，登录 daocloud 账户

![1551517640992](/1551517640992.png)

点击右上角的 加速器

![1551517660871](/1551517660871.png)



## 1.5 docker常见问题解决方法

### 1.5.1 背景

​	因为使用的是sudo安装docker，所以会导致一个问题。以普通用户登录的状况下，在使用docker images时必须添加sudo，那么如何让docker免sudo依然可用呢？

### 1.5.2 理清问题

​	当以普通用户身份去使用docker命令时，出现以下错误：

```shell
Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Post http://%2Fvar%2Frun%2Fdocker.sock/v1.35/images/create?fromSrc=-&message=&repo=ubuntu-16.04&tag=: dial unix /var/run/docker.sock: connect: permission denied
```

​	可以看都，最后告知我们时权限的问题。那么在linux文件权限有三个数据左右drwxrwxrwx，其中第一为d代表该文件是一个文件夹前三位、中三位、后三位分别代表这属主权限、属组权限、其他人权限。

![1551517737790](/1551517737790.png)

上图是报错文件的权限展示，可以看到其属主为root，权限为rw，可读可写；其属组为docker，权限为rw，可读可写。如果要当前用户可直接读取该文件，那么我们就为docker.sock 添加一个其他用户可读写权限 或者添加1个用户组就可以了

### 1.5.3 方法：一劳永逸

```shell
#如果还没有 docker group 就添加一个：
$sudo groupadd docker
#将用户加入该 group 内。然后退出并重新登录就生效啦。
$sudo gpasswd -a ${USER} docker
#重启 docker 服务
$systemctl restart docker
#切换当前会话到新 group 或者重启 X 会话
$newgrp - docker
#注意:最后一步是必须的，否则因为 groups 命令获取到的是缓存的组信息，刚添加的组信息未能生效，
#所以 docker images 执行时同样有错。
```







