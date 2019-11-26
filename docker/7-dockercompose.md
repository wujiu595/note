---
typora-copy-images-to: img
typora-root-url: img
---

## 7 Docker compose

Docker compose是一种docker容器的任务编排工具

官方地址：https://docs.docker.com/compose/

### 7.1 compose简介

任务编排介绍

场景：

​       我们在工作中为了完成业务目标，首先把业务拆分成多个子任务，然后对这些子任务进行顺序组合，当子任务按照方案执行完毕后，就完成了业务目标。

​       任务编排，就是对多个子任务执行顺序进行确定的过程。

常见的任务编排工具：

单机版：   docker compose

集群版：

Docker swarm               Docker

Mesos                 		Apache

Kubernetes（k8s）       Google

**docker compose是什么**

![1541810582833](/1552961534181.png)

**译文：**
compose是定义和运行多容器Docker应用程序的工具。通过编写，您可以使用YAML文件来配置应用程序的服务。然后，使用单个命令创建并启动配置中的所有服务。要了解更多有关组合的所有特性，请参见特性列表。

**docker compose的特点；**
本质：docker 工具

对象：应用服务

配置：YAML 格式配置文件

命令：简单

执行：定义和运行容器

**docker compose的配置文件**

docker-compose.yml

文件后缀是yml

文件内容遵循 ymal格式

### docker 和 Docker compose

| **Compose file format** | **Docker Engine release** |      | **Compose file format** | **Docker Engine release** |
| ----------------------- | ------------------------- | ---- | ----------------------- | ------------------------- |
| 3.4                     | 17.09.0+                  |      | 2.3                     | 17.06.0+                  |
| 7                       | 17.06.0+                  |      | 2.2                     | 1.13.0+                   |
| 3.2                     | 17.04.0+                  |      | 2.1                     | 1.12.0+                   |
| 3.1                     | 1.13.1+                   |      | 2.0                     | 1.10.0+                   |
| 3.0                     | 1.13.0+                   |      | 1.0                     | 1.9.1.+                   |

官方地址：https://docs.docker.com/compose/overview/

### 7.2 compose 快速入门

**docker compose 安装**

```shell
#安装依赖工具
sudo apt-get install python-pip -y
#安装编排工具
sudo pip install docker-compose
#查看编排工具版本
sudo docker-compose version
#查看命令帮助
docker-compose --help
```

**PIP 源问题**

```shell
#用pip安装依赖包时默认访问https://pypi.python.org/simple/，
#但是经常出现不稳定以及访问速度非常慢的情况，国内厂商提供的pipy镜像目前可用的有：

#在当前用户目录下创建.pip文件夹
mkdir ~/.pip
#然后在该目录下创建pip.conf文件填写：
[global]
trusted-host=mirrors.aliyun.com
index-url=http://mirrors.aliyun.com/pypi/simple/
```

**compose简单配置文件**

```shell
#创建compose文件夹
:~$ mkdir -p ./docker/compose
#进入到文件夹
:~$ cd ./docker/compose
#创建yml文件
:~$ vim docker-compose.yml
```

**docker-compose.yml 文件内容**

```yml
version: '2'
services:
  web1:
    image: nginx
    ports:
      - "9999:80"
    container_name: nginx-web1
  web2:
    image: nginx
    ports:
      - "8888:80"
    container_name: nginx-web2
```

**运行一个容器**

```shell
#后台启动：
docker-compose up -d
#注意：
    #如果不加-d，那么界面就会卡在前台
#查看运行效果
docker-compose ps
```

### 7.3 compose命令详解

**注意：**

所有命令尽量都在docker compose项目目录下面进行操作

项目目录：docker-compose.yml所在目录

**compose服务启动、关闭、查看**

```shell
#后台启动：
docker-compose up -d
#删除服务
docker-compose down
#查看正在运行的服务
docker-compose ps
```

**容器开启、关闭、删除**

```shell
#启动一个服务
docker-compose start <服务名>
#注意：
    #如果后面不加服务名，会停止所有的服务
#停止一个服务
docker-compose stop <服务名>
#注意：
    #如果后面不加服务名，会停止所有的服务
#删除服务
docker-compose rm
#注意：
    #这个docker-compose rm不会删除应用的网络和数据卷。工作中尽量不要用rm进行删除
```

**其他信息查看**

```shell
#查看运行的服务
docker-compose ps
#查看服务运行的日志
docker-compose logs -f
#注意：
    #加上-f 选项，可以持续跟踪服务产生的日志
#查看服务依赖的镜像
docke-compose  images
#进入服务容器
docker-compose exec <服务名> <执行命令>
#查看服务网络
docker network ls
```

### 7.4 compose文件详解

官方参考资料：
https://docs.docker.com/compose/overview/

**文件命名：**

后缀是 .yml

**YMAL介绍

YMAL文件格式：

```yml
李氏家族：posepose
  户主：
    姓名: 李雷
    性别: 男
    学校:
      - "小学"
      - "中学"
      - "大学"
    家属:
      - 夫人
  
  夫人：
    姓名: 韩梅梅
    性别: 女

```

**compose文件样例：**

```yml
version: '2'                        # compose 版本号
services:                           # 服务标识符
  web1:                             # 子服务命名
    image: nginx                    # 服务依赖镜像属性
    ports:                          # 服务端口属性
      - "9999:80"                   # 宿主机端口:容器端口
    container_name: nginx-web1      # 容器命名

```

**格式详解：**

```shell
compose版本号、服务标识符必须顶格写
属性名和属性值是以': '(冒号+空格) 隔开
层级使用'  '(两个空格)表示
服务属性使用' - '(空格空格-空格)来表示
```

**compose属性介绍**

```shell
#镜像：
    格式：
        image: 镜像名称:版本号
    举例：
        image: nginx:latest

#容器命名：
    格式：
        container_name: 自定义容器命名
    举例：
        container_name: nginx-web1

#数据卷：
    格式：
        volumes:
          - 宿主机文件:容器文件
    举例：
        volumes:
          - ./linshi.conf:/nihao/haha.sh

#端口：
    格式：
        ports:
          - "宿主机端口:容器端口"
    举例：
        ports:
          - "9999:80"

#镜像构建：
    格式：
        build: Dockerfile 的路径
    举例：
        build: .
        build: ./dockerfile_dir/
        build: /root/dockerfile_dir/
#镜像依赖：
    格式：
        depends_on:
          - 本镜像依赖于哪个服务
    举例：
        depends_on:
          - web1
```

### 7.5 go项目实践

**项目分析**

![1552961563168](/1552961563168.png)

**需求：**

自动部署一个集群，使用nginx代理两个go项目

**流程分析：**

1、go项目部署

2、nginx代理部署

3、docker 环境

4、docker compose任务编排

**技术点分析：**

1、go项目部署

go项目基础环境

go项目配置

2、nginx代理部署

nginx的配置文件

3、docker 环境

docker基础镜像

go镜像

nginx镜像

4、docker compose任务编排

4个任务：1个镜像构建任务、2个go任务、1个nginx任务

任务依赖关系：go任务执行依赖于nginx任务

镜像依赖：go镜像依赖于nginx镜像完毕

**实施方案：**

1、基础环境

1.1 compose基础目录

1.2 环境依赖文件：

sources.list、test.go、test1.go、test2.go、nginx-beego.conf

1.3 dockerfile文件

go项目环境的Dockerfile文件

2、任务编排文件

2.1 nginx任务

基于nginx镜像，增加一个nginx的代理配置即可

2.2 go基础镜像任务

基于ubuntu镜像，构建go基础环境镜像，参考3.2.4内容

该任务依赖于2.1 nginx任务

2.3 go项目任务

基于go基础镜像，增加测试文件即可

该任务依赖于2.2 go基础镜像任务

3、测试

3.1 集群测试

方案实施

1、基础环境

1.1 compose基础目录

创建compose基础目录

```shell
:~$ mkdir /docker/compose/
:~$ cd /docker/compose/
```

1.2 环境依赖文件：

**nginx配置文件**

```shell
创建nginx专用目录
:~$ mkdir nginx
:~$ cd nginx
创建nginx负载均衡配置nginx-beego.conf
:~$ vim nginx-beego.conf
```

**文件内容**

```shell
upstream beegos {  
#upstream模块
  server 192.168.8.14:10086;
  server 192.168.8.14:10087;
}
server {
  listen 80;
  #提供服务的端口 
  server_name _;
  #服务名称
  location / {
    proxy_pass http://beegos;
    #反选代理 upstream模块  beegos
    index index.html index.htm;
    #默认首页
    }
}
```

go**基础镜像依赖文件**

```shell
#创建go基础镜像目录：
:~$ mkdir go-base
:~$ cd go-base
#创建source.lise
:~$ vim sources.list
```

**文件内容**

```shell
deb http://mirrors.aliyun.com/ubuntu/ xenial main restricted
deb http://mirrors.aliyun.com/ubuntu/ xenial-updates main restricted
deb http://mirrors.aliyun.com/ubuntu/ xenial universe
deb http://mirrors.aliyun.com/ubuntu/ xenial-updates universe 
deb http://mirrors.aliyun.com/ubuntu/ xenial multiverse
deb http://mirrors.aliyun.com/ubuntu/ xenial-updates multiverse
deb http://mirrors.aliyun.com/ubuntu/ xenial-backports main restricted universe multiverse 
deb http://mirrors.aliyun.com/ubuntu/ xenial-security main restricted
deb http://mirrors.aliyun.com/ubuntu/ xenial-security universe
deb http://mirrors.aliyun.com/ubuntu/ xenial-security multiverse
```

**或者**

```shell
RUN sed -i 's/archive.ubuntu.com/mirrors.ustc.edu.cn/g' /etc/apt/sources.list
RUN sed -i 's/security.ubuntu.com/mirrors.ustc.edu.cn/g' /etc/apt/sources.list
```

**test.go配置文件**

```Go
package main
import (
	"github.com/astaxie/beego"
)

type MainController struct { 
	beego.Controller
}

func (this *MainController) Get(){ 
	this.Ctx.WriteString("hello world\n")
}

func main() {
	beego.Router("/", &MainController{})
	beego.Run()
}
```

go任务依赖文件：
**beego1/test.go配置文件**

```go
package main
import (
"github.com/astaxie/beego"
)

type MainController struct { 
beego.Controller
}

func (this *MainController) Get(){ 
this.Ctx.WriteString("<h1>hello beego1</h1>\n")
}

func main() {
beego.Router("/", &MainController{})
beego.Run()
}
```

**Beego2/test.go配置文件**

```Go
package main
import (
	"github.com/astaxie/beego"
)

type MainController struct { 
	beego.Controller
}

func (this *MainController) Get(){ 
	this.Ctx.WriteString("<h1>hello beego2</h1>\n")
}

func main() {
	beego.Router("/", &MainController{})
	beego.Run()
}
```

1.3 dockerfile文件

go项目环境的Dockerfile文件

创建Dockerfile文件

```
:~$ vim dockerfile

```

文件内容

```Dockerfile
# 构建一个基于ubuntu 的docker 定制镜像
# 基础镜像
FROM ubuntu
# 镜像作者
MAINTAINER panda kstwoak47@163.com
# 增加国内源
#COPY sources.list /etc/apt/

RUN sed -i 's/archive.ubuntu.com/mirrors.ustc.edu.cn/g' /etc/apt/sources.list
RUN sed -i 's/security.ubuntu.com/mirrors.ustc.edu.cn/g' /etc/apt/sources.list

# 执行命令
RUN apt-get update
RUN apt-get install gcc libc6-dev git lrzsz -y
#将go复制解压到容器中
ADD go1.10.linux-amd64.tar.gz  /usr/local/
# 定制环境变量
ENV GOROOT=/usr/local/go
ENV PATH=$PATH:/usr/local/go/bin
ENV GOPATH=/root/go
ENV PATH=$GOPATH/bin/:$PATH
# 下载项目
#RUN go get github.com/astaxie/beego
ADD astaxie.tar.xz  /root/go/src/github.com
# 增加文件
COPY test.go /root/go/src/myTest/
# 定制工作目录
WORKDIR /root/go/src/myTest/
# 对外端口
EXPOSE 8080
# 运行项目
ENTRYPOINT ["go","run","test.go"]

```

最终的文件目录结构

```shell
:~# tree /docker/compose/
/docker/compose/beego/
├── beego1
│   └── test.go
├── beego2
│   └── test.go
├── docker-compose.yml
├── go-base
│   ├── astaxie.tar.xz
│   ├── Dockerfile
│   ├── go1.10.linux-amd64.tar.gz
│   └── test.go
└── nginx
    └── nginx-beego.conf

```

2、任务编排文件

docker-compose.yml文件内容

```yml
version: '2'
services:
  web1:
    image: nginx
    ports:
      - "9999:80"
    volumes:
      - ./nginx/nginx-beego.conf:/etc/nginx/conf.d/default.conf
      #将配置文件映射到nginx的配置文件位置
    container_name: nginx-web1

  go-base:
    build: ./go-base/
    image: go-base:v0.1

  beego-web1:
    image: go-base:v0.1
    volumes:
      - ./beego1/test.go:/root/go/src/myTest/test.go
    ports:
      - "10086:8080"
    container_name: beego-web1
    depends_on:
    - go-base

  beego-web2:
    image: go-base:v0.1
    volumes:
      - ./beego2/test.go:/root/go/src/myTest/test.go
    ports:
      - "10087:8080"
    container_name: beego-web2
    depends_on:
      - go-base
```

3、 最后测试

```shell
#构建镜像
$docker-compose build
#启动任务
$docker-compose up -d
#查看效果
$docker-compose ps
#浏览器访问
192.168.110.5:9999
```



 


