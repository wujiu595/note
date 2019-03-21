## 3.1 Dockerfile

### 3.1.1 Dockerfile简介

**什么是Dockerfile**
​	Dockerfile类似于我们学习过的脚本，将我们在上面学到的docker镜像，使用自动化的方式实现出来。

**Dockerfile的作用**
​	1、找一个镜像：    ubuntu 

​	2、创建一个容器：    docker run    ubuntu

​	3、进入容器：    docker exec -it 容器 命令

​	4、操作：    各种应用配置....

​	5、构造新镜像：    docker commit

**Dockerfile 使用准则**
​	1、大： 首字母必须大写D

​	2、空： 尽量将Dockerfile放在空目录中。

​	3、单： 每个容器尽量只有一个功能。

​	4、少： 执行的命令越少越好。

**Dockerfile 分为四部分:**

​	基础镜像信息             从哪来？

​	维护者信息               我是谁？

​	镜像操作指令             怎么干？

​	容器启动时执行指令        嗨！！！

**Dockerfile文件内容：**

​	首行注释信息

​	指令(大写) 参数

**Dockerfile使用命令：**

```shell
#构建镜像命令格式：
docker build -t [镜像名]:[版本号][Dockerfile所在目录]
#构建样例：
docker build -t nginx:v0.2 /opt/dockerfile/nginx/
#参数详解：
	-t        					指定构建后的镜像信息，
	/opt/dockerfile/nginx/      则代表Dockerfile存放位置，如果是当前目录，则用 .(点)表示
```

### 3.1.2 Dockerfile快速入门

​	接下来我们快速的使用Dockerfile来基于ubuntu创建一个定制化的镜像：nginx。

```shell
#创建Dockerfile专用目录
:~$ mkdir ./docker/images/nginx -p
:~$ cd docker/images/nginx/
#创建Dockerfile文件
:~/docker/images/nginx$ vim Dockerfile
```

**dockerfile内容**

```Dockerfile
# 构建一个基于ubuntu的docker定制镜像
# 基础镜像
FROM ubuntu

# 镜像作者
MAINTAINER panda kstwoak47@163.com

# 执行命令
RUN mkdir hello
RUN mkdir world
RUN sed -i 's/archive.ubuntu.com/mirrors.ustc.edu.cn/g' /etc/apt/sources.list
RUN sed -i 's/security.ubuntu.com/mirrors.ustc.edu.cn/g' /etc/apt/sources.list
RUN apt-get update  
RUN apt-get install nginx -y

# 对外端口
EXPOSE 80
```

**进行构建操作**

```shell
#构建镜像
:~/docker/images/nginx$ docker build -t ubuntu-nginx:v0.1 .
#查看新生成镜像
:~/docker/images/nginx$ docker images
REPOSITORY    TAG     IMAGE ID      CREATED        SIZE
ubuntu-nginx  v0.1    a853de1b8be4  9 seconds ago  208MB
nginx         latest  e548f1a579cf  6 days ago     109MB
ubuntu        latest  0458a4468cbc  4 weeks ago    112MB
#查看构建历史
:~/docker/images/nginx$ docker history a853de1b8be4
IMAGE         CREATED            CREATED BY                         SIZE   COMMENT
#镜像			创建时间             依赖命令                            大小    评论
a853de1b8be4  41 seconds ago     /bin/sh -c #(nop)  EXPOSE 80                  0B      
925825b680fd  42 seconds ago     /bin/sh -c apt-get install nginx -y          56.5MB  
4c57d6c99603  About a minute ago /bin/sh -c apt-get update                     40MB    
b6d030a0d123  About a minute ago /bin/sh -c sed -i's/security.ubuntu.com/mir… 2.77kB  
3357bf8069ca  About a minute ago /bin/sh -c sed -i's/archive.ubuntu.com/mirr… 2.77kB  
7bfb90c1e20d  About a minute ago /bin/sh -c mkdir world                         0B      
972d6ab76d01  About a minute ago /bin/sh -c mkdir hello                         0B      
a76394bfad01  About a minute ago /bin/sh -c #(nop) MAINTAINER panda kstwoak4…  0B    
#注意：
因为容器没有启动命令，所以肯定访问不了

```

**优化刚刚的Dockerfile文件**

```Dockerfile
# 构建一个基于ubuntu的docker定制镜像
# 基础镜像
FROM ubuntu

# 镜像作者
MAINTAINER panda kstwoak47@163.com

# 执行命令
RUN mkdir hello
RUN mkdir world
RUN sed -i 's/archive.ubuntu.com/mirrors.ustc.edu.cn/g' /etc/apt/sources.list
RUN sed -i 's/security.ubuntu.com/mirrors.ustc.edu.cn/g' /etc/apt/sources.list
RUN apt-get update  
RUN apt-get install nginx -y

# 对外端口
EXPOSE 80
```

  **运行修改好的Dockerfile进行构建**

```shell
:~/docker/images/nginx$ docker build -t  ubuntu-nginx:v0.2 .
:~/docker/images/nginx$ docker history ubuntu-nginx:v0.2
IMAGE         CREATED         CREATED BY                                     SIZE    COMMENT
eaba9bd1c4ac  3 minutes ago   /bin/sh -c #(nop)  EXPOSE 80                   0B      
ed08d6e29eb1  3 minutes ago   /bin/sh -c apt-get update && apt-get install…  96.5MB  
eef6238ec5bd  6 minutes ago   /bin/sh -c sed -i 's/archive.ubuntu.com/mirr…  2.77kB  
58f755a1b29c  6 minutes ago   /bin/sh -c mkdir hello &&  mkdir world         0B      
a76394bfad01  25 minutes ago  /bin/sh -c #(nop)  MAINTAINER panda kstwoak4…  0B    
#对比两个镜像的大小
:~/docker/images/nginx$ docker images 
REPOSITORY    TAG   IMAGE ID      CREATED         SIZE
ubuntu-nginx  v0.2  eaba9bd1c4ac  7 seconds ago   208MB
ubuntu-nginx  v0.1  a853de1b8be4  21 minutes ago  208MB
#深度对比连个镜像的大小
:~/docker/images/nginx$ docker inspect a853de1b8be4
        "Size": 208237435,
        "VirtualSize": 208237435,
:~/docker/images/nginx$ docker inspect eaba9bd1c4ac
        "Size": 208234662,
        "VirtualSize": 208234662,
```

**Dockerfile构建过程：**
​	从基础镜像1运行一个容器A

​	遇到一条Dockerfile指令，都对容器A做一次修改操作

​	执行完毕一条命令，提交生成一个新镜像2

​	再基于新的镜像2运行一个容器B

​	遇到一条Dockerfile指令，都对容器B做一次修改操作

​	执行完毕一条命令，提交生成一个新镜像3

​	…

**构建过程镜像介绍

​       构建过程中，创建了很多镜像，这些中间镜像，我们可以直接使用来启动容器，通过查看容器效果，从侧面能看到我们每次构建的效果。提供了镜像调试的能力

### 3.1.3 基础指令详解

#### **FROM**

```shell
FROM
#格式：
    FROM <image>
    FROM <image>:<tag>
#解释：
    #FROM 是 Dockerfile 里的第一条而且只能是除了首行注释之外的第一条指令
    #可以有多个FROM语句，来创建多个image
    #FROM 后面是有效的镜像名称，如果该镜像没有在你的本地仓库，那么就会从远程仓库Pull取，如果远程也没有，就报错失败
    #下面所有的 系统可执行指令 在 FROM 的镜像中执行。

 
```

#### **MAINTAINER**

```shell
MAINTAINER
#格式：
    MAINTAINER <name>
#解释：
    #指定该dockerfile文件的维护者信息。类似我们在docker commit 时候使用-a参数指定的信息
```

#### **RUN**

```shell
RUN
#格式：
    RUN <command>                                   (shell模式)
    RUN["executable", "param1", "param2"]            (exec 模式)
#解释：
    #表示当前镜像构建时候运行的命令，如果有确认输入的话，一定要在命令中添加 -y
    #如果命令较长，那么可以在命令结尾使用 \ 来换行
    #生产中，推荐使用上面数组的格式
#注释：
    #shell模式：类似于  /bin/bash -c command
    #举例： RUN echo hello
    #exec模式：类似于 RUN["/bin/bash", "-c", "command"]
    #举例： RUN["echo", "hello"]
```

#### **EXPOSE**

```shell
EXPOSE
#格式：
    EXPOSE <port> [<port>...]
#解释：
    设置Docker容器对外暴露的端口号，Docker为了安全，不会自动对外打开端口，如果需要外部提供访问，
    还需要启动容器时增加-p或者-P参数对容器的端口进行分配。
```

### 3.1.4 运行时指令详解

#### **CMD**

```shell
CMD
#格式：
    CMD ["executable","param1","param2"]         (exec 模式)推荐
    CMD command param1 param2                     (shell模式)
    CMD ["param1","param2"]          提供给ENTRYPOINT的默认参数；
#解释： 
    #CMD指定容器启动时默认执行的命令
    #每个Dockerfile只能有一条CMD命令，如果指定了多条，只有最后一条会被执行
    #如果你在启动容器的时候使用docker run 指定的运行命令，那么会覆盖CMD命令。
    #举例： CMD ["/usr/sbin/nginx","-g","daemon off；"]
    "/usr/sbin/nginx"   nginx命令
    "-g"				设置配置文件外的全局指令
    "daemon off；"		后台守护程序开启方式 关闭
#CMD指令实践:
    #修改Dockerfile文件内容：
    #在上一个Dockerfile文件内容基础上，末尾增加下面一句话：
    CMD ["/usr/sbin/nginx","-g","daemon off;"]
    #构建镜像
    :~/docker/images/nginx$ docker build  -t ubuntu-nginx:v0.3  .
    #根据镜像创建容器,创建时候，不添加执行命令
    :~/docker/images/nginx$ docker run --name nginx-1 -itd ubuntu-nginx:v0.3
    #根据镜像创建容器,创建时候，添加执行命令/bin/bash
    :~/docker/images/nginx$ docker run  --name nginx-2 -itd ubuntu-nginx:v0.3 /bin/bash
    docker ps
    
    #发现两个容器的命令行是不一样的
    itcast@itcast-virtual-machine:~/docker/images/nginx$ docker ps -a
    CONTAINER ID  IMAGE             COMMAND                 CREATED           NAMES
    921d00c3689f  ubuntu-nginx:v0.3 "/bin/bash"             5 seconds ago    nginx-2
    e6c39be8e696  ubuntu-nginx:v0.3 "/usr/sbin/nginx -g …"  14 seconds ago   nginx-1
    
```

#### **ENTRYPOINT**

```shell
ENTRYPOINT
#格式：
    ENTRYPOINT ["executable", "param1","param2"] (exec 模式)
    ENTRYPOINT command param1 param2 (shell 模式)
#解释：
    #和CMD 类似都是配置容器启动后执行的命令，并且不会被docker run 提供的参数覆盖。
    #每个Dockerfile 中只能有一个ENTRYPOINT，当指定多个时，只有最后一个起效。
    #生产中我们可以同时使用ENTRYPOINT 和CMD，
    #想要在docker run 时被覆盖，可以使用"docker run --entrypoint"
#ENTRYPOINT指令实践：
    #修改Dockerfile文件内容：
    #在上一个Dockerfile 文件内容基础上，修改末尾的CMD 为ENTRYPOINT：
    ENTRYPOINT ["/usr/sbin/nginx","-g","daemon off;"]
    
    #构建镜像
    :~/docker/images/nginx$ docker build -t ubuntu-nginx:v0.4 .
    
    #根据镜像创建容器,创建时候，不添加执行命令
    :~/docker/images/nginx$ docker run  --name nginx-3 -itd ubuntu-nginx:v0.4
    
    #根据镜像创建容器,创建时候，添加执行命令/bin/bash
    :~/docker/images/nginx$ docker run  --name nginx-4 -itd ubuntu-nginx:v0.4 /bin/bash
    
    #查看ENTRYPOINT是否被覆盖
    :~/docker/images/nginx$ docker ps -a
    CONTAINER ID  IMAGE               COMMAND                  CREATED              NAMES
    e7a2f0d0924e  ubuntu-nginx:v0.4   "/usr/sbin/nginx -g …"   59 seconds ago       nginx-4
    c92b2505e28e  ubuntu-nginx:v0.4   "/usr/sbin/nginx -g …"   About a minute ago   nginx-3
    
    #根据镜像创建容器,创建时候，使用--entrypoint参数，添加执行命令/bin/bash
    docker run  --entrypoint "/bin/bash" --name nginx-5 -itd ubuntu-nginx:v0.4
    
    #查看ENTRYPOINT是否被覆盖
    :~/docker/images/nginx$ docker ps 
    CONTAINER ID  IMAGE               COMMAND                  CREATED              NAMES
    6c54726b2d96  ubuntu-nginx:v0.4   "/bin/bash"              3 seconds ago        nginx-5

```

#### **CMD ENTRYPOINT 综合使用实践**

```shell
#修改Dockerfile文件内容：

# 在上一个Dockerfile文件内容基础上，修改末尾的ENTRYPOINT

    :~/docker/images/nginx$ vim Dockerfile
    ENTRYPOINT ["/usr/sbin/nginx"]
    CMD ["-g"]
    #构建镜像
    :~/docker/images/nginx$ docker build -t ubuntu-nginx:v0.5 .
    #根据镜像创建容器,创建时候，不添加执行命令
    :~/docker/images/nginx$ docker run --name nginx-6 -d ubuntu-nginx:v0.5
    
    #查看效果
   :~/docker/images/nginx$ docker ps -a 
    CONTAINER ID   IMAGE               COMMAND                  CREATED         NAMES
    e28875d281eb   ubuntu-nginx:v0.5   "/usr/sbin/nginx -g"     9 seconds ago   nginx-6
#根据镜像创建容器,创建时候，不添加执行命令，覆盖cmd的参数 -g "daemon off;"
:~/docker/images/nginx$ docker run  --name nginx-7 -d ubuntu-nginx:v0.5 -g "daemon off;"

#查看效果
itcast@itcast-virtual-machine:~/docker/images/nginx$ docker ps -a 
CONTAINER ID    IMAGE               COMMAND                  CREATED         NAMES
e5addad86ef5    ubuntu-nginx:v0.5   "/usr/sbin/nginx -g …"   5 seconds ago   nginx-7
#注释：
#任何docker run设置的命令参数或者CMD指令的命令，都将作为ENTRYPOINT 指令的命令参数，追加到ENTRYPOINT指令之后
```

### 3.1.5 文件编辑指令详解

#### **ADD**

```shell
#ADD
#格式：
    ADD <src>... <dest>
    ADD ["<src>",... "<dest>"]
#解释：
    #将指定的<src> 文件复制到容器文件系统中的<dest>
    #src 指的是宿主机，dest 指的是容器
    #所有拷贝到container 中的文件和文件夹权限为0755,uid 和gid 为0
    #如果文件是可识别的压缩格式，则docker 会帮忙解压缩
    #注意：
    #1、如果源路径是个文件，且目标路径是以/ 结尾， 则docker 会把目标路径当作一个目录，会把源文件拷贝到该目录下;
#如果目标路径不存在，则会自动创建目标路径。


    #2、如果源路径是个文件，且目标路径是不是以/ 结尾，则docker 会把目标路径当作一个文件。
    #如果目标路径不存在，会以目标路径为名创建一个文件，内容同源文件；
    #如果目标文件是个存在的文件，会用源文件覆盖它，当然只是内容覆盖，文件名还是目标文件名。
#如果目标文件实际是个存在的目录，则会源文件拷贝到该目录下。注意，这种情况下，最好显示的以/ 结尾，以避免混淆。


    #3、如果源路径是个目录，且目标路径不存在，则docker 会自动以目标路径创建一个目录，把源路径目录下的文件拷贝进来。
#如果目标路径是个已经存在的目录，则docker 会把源路径目录下的文件拷贝到该目录下。


    #4、如果源文件是个压缩文件，则docker 会自动帮解压到指定的容器目录中。
    
#ADD实践：
    #拷贝普通文件
    :~/docker/images/nginx$ vim Dockerfile 
    
    #Dockerfile文件内容
    
    # 构建一个基于ubuntu的docker定制镜像
    # 基础镜像
    FROM ubuntu
    # 镜像作者
    MAINTAINER panda kstwoak47@163.com
    # 执行命令
    ADD ["sources.list","/etc/apt/sources.list"]
    RUN apt-get clean
    RUN apt-get update
    RUN apt-get install nginx -y
    # 对外端口
    EXPOSE 80
 
    #构建镜像
    :~/docker/images/nginx$ docker build -t ubuntu-nginx:v0.6 .
    
    #根据镜像创建容器,创建时候，不添加执行命令进入容器查看效果
    docker run  --name nginx-8 -it ubuntu-nginx:v0.6
    
    #拷贝压缩文件
    tar zcvf this.tar.gz ./*
    #Dockerfile文件内容
    ...
    # 执行命令
    ...
    # 增加文件
    ADD ["linshi.tar.gz","/nihao/"]
    ...
    #构建镜像
    :~/docker/images/nginx$ docker build -t ubuntu-nginx:v0.7 .
    #根据镜像创建容器,创建时候，不添加执行命令进入容器查看效果
    docker run --name nginx-9 -it ubuntu-nginx:v0.7
    :~/docker/images/nginx$ docker run --name nginx-9 -it ubuntu-nginx:v0.7
```

#### COPY

```shell
#COPY
    #格式：
    COPY <src>... <dest>
    COPY ["<src>",... "<dest>"]
    #解释：
    #COPY 指令和ADD 指令功能和使用方式类似。只是COPY 指令不会做自动解压工作。
    #单纯复制文件场景，Docker 推荐使用COPY
#COPY实践
    #修改Dockerfile文件内容:
    # 构建一个基于ubuntu的docker定制镜像
    # 基础镜像
    FROM ubuntu
    # 镜像作者
    MAINTAINER panda kstwoak47@163.com
    # 执行命令
ADD ["sources.list","/etc/apt/sources.list"]
RUN apt-get clean
RUN apt-get update
RUN apt-get install nginx -y
COPY ["index.html","/var/www/html/"]
# 对外端口
EXPOSE 80
#运行时默认命令 
ENTRYPOINT ["/usr/sbin/nginx","-g","daemon off;"]
index.html 文件内容：
<h1>hello world </h1>
<h1>hello docker </h1>
<h1>hello nginx</h1>
 
#构建镜像
:~/docker/images/nginx$ docker build -t ubuntu-nginx:v0.8 .
#根据镜像创建容器,创建时候，不添加执行命令
:~/docker/images/nginx$ docker run  --name nginx-10 -itd ubuntu-nginx:v0.8
#查看nginx-10信息
:~/docker/images/nginx$docker inspect  nginx-10
#浏览器访问nginx查看效果    
```

**VOLUME**

```shell
#VOLUME
    #格式：
    VOLUME ["/data"]
    #解释：
    #VOLUME 指令可以在镜像中创建挂载点，这样只要通过该镜像创建的容器都有了挂载点
    #通过VOLUME 指令创建的挂载点，无法指定主机上对应的目录，是自动生成的。
    #举例：
    VOLUME ["/var/lib/tomcat7/webapps/"]
```

**VOLUME实践**

```shell
#VOLUME实践
#修改Dockerfile文件内容：
#将COPY替换成为VOLUME
:~/docker/images/nginx$vim Dockerfile
VOLUME ["/helloworld/"]
...
#构建镜像
:~/docker/images/nginx$docker build -t ubuntu-nginx:v0.9 .
#创建数据卷容器
:~/docker/images/nginx$docker run -itd --name nginx-11 ubuntu-nginx:v0.9
#查看镜像信息
:~/docker/images/nginx$docker inspect nginx-11
#验证操作
:~/docker/images/nginx$docker run -itd --name vc-nginx-1 --volumes-from nginx-11 nginx
:~/docker/images/nginx$docker run -itd --name vc-nginx-2 --volumes-from nginx-11 nginx
#进入容器1
:~/docker/images/nginx$docker exec -it vc-nginx-1 /bin/bash
:/# echo 'nihao itcast' > helloworld/nihao.txt
#进入容器2
:~/docker/images/nginx$ docker exec -it vc-nginx-2 /bin/bash
:/# cat helloworld/nihao.txt
```

### 3.1.6 环境指令详解

#### ENV

```shell
#ENV
    #格式：
    ENV <key> <value>                    （一次设置一个环节变量）
    ENV <key>=<value> ...                （一次设置一个或多个环节变量）
#解释：
#设置环境变量，可以在RUN 之前使用，然后RUN 命令时调用，容器启动时这些环境变量都会被指定
```

**ENV实践**

```shell
#ENV实践：
    #命令行创建ENV的容器
    :~$ docker run -e NIHAO="helloworld" -itd --name ubuntu-111 ubuntu /bin/bash
    #进入容器ubuntu-111
    :~$ docker exec -it ubuntu-111 /bin/bash
    :/# echo $NIHAO
    
    #修改Dockerfile文件内容：
    #在上一个Dockerfile 文件内容基础上，在RUN 下面增加一个ENV
    ENV NIHAO=helloworld
    ...
    #构建镜像
    docker build -t ubuntu-nginx:v0.10 .
    
    #根据镜像创建容器,创建时候，不添加执行命令
    docker run  --name nginx-12 -itd ubuntu-nginx:v0.10
    docker exec -it nginx-12 /bin/bash
    echo $NIHAO
```

#### WORKDIR

```shell
#WORKDIR
    #格式：
    WORKDIR /path/to/workdir (shell 模式)
    #解释：
    #切换目录，为后续的RUN、CMD、ENTRYPOINT 指令配置工作目录。相当于cd
    #可以多次切换(相当于cd 命令)，
    #也可以使用多个WORKDIR 指令，后续命令如果参数是相对路径，则会基于之前命令指定的路径。例如
    #举例：
    WORKDIR /a
    WORKDIR b
    WORKDIR c
    RUN pwd
    #则最终路径为/a/b/c
```

**WORKDIR实践**

```shell
#WORKDIR实践：
    #修改Dockerfile文件内容：
    # 在上一个Dockerfile 文件内容基础上，在RUN 下面增加一个WORKDIR
    WORKDIR /nihao/itcast/
    RUN ["touch","itcast1.txt"]
    WORKDIR /nihao
    RUN ["touch","itcast2.txt"]
    WORKDIR itcast
    RUN ["touch","itcast3.txt"]
    ...
    #构建镜像
    :~/docker/images/nginx$ docker build -t ubuntu-nginx:v0.11 .
    #根据镜像创建容器,创建时候，不添加执行命令
    docker run  --name nginx-13 -itd ubuntu-nginx:v0.11
    #进入镜像
    docker exec -it nginx-13 /bin/bash
```

**USER与ARG**

```shell
#USER
    #格式：
    USER daemon
    #解释：
    #指定运行容器时的用户名和UID，后续的RUN 指令也会使用这里指定的用户。
    #如果不输入任何信息，表示默认使用root 用户

#ARG
    #格式：
    ARG <name>[=<default value>]
    #解释：
    #ARG 指定了一个变量在docker build 的时候使用，可以使用--build-arg <varname>=<value>来指定参数的值，不过
    #如果构建的时候不指定就会报错。

```

### 3.1.7 触发器指令详解

触发器指令

```shell
ONBUILD
#格式：
    ONBUILD [command]
#解释：
    #当一个镜像A被作为其他镜像B的基础镜像时，这个触发器才会被执行，
    #新镜像B在构建的时候，会插入触发器中的指令。
    #使用场景对于版本控制和方便传输，适用于其他用户。

```

触发器实践

```shell
#编辑Dockerfile
:~/docker/images/nginx$ vim Dockerfile 
#内容如下：
# 构建一个基于ubuntu的docker定制镜像
# 基础镜像
FROM ubuntu
# 镜像作者
MAINTAINER panda kstwoak47@163.com
# 执行命令
ADD ["sources.list","/etc/apt/sources.list"] 
RUN apt-get clean
RUN apt-get update 
RUN apt-get install nginx -y 
#触发器
ONBUILD COPY ["index.html","/var/www/html/"]
# 对外端口
EXPOSE 80
#运行时默认命令 
ENTRYPOINT ["/usr/sbin/nginx","-g","daemon off;"]
 
#构建镜像
:~/docker/images/nginx$ docker build -t ubuntu-nginx:v0.12 .
 
#根据镜像创建容器,
:~/docker/images/nginx$docker run -p 80 --name nginx-14 -itd ubuntu-nginx:v0.12
:~/docker/images/nginx$docker ps
#查看镜像信息
:~/docker/images/nginx$ docker inspect ubuntu-nginx:v0.12  
#访问容器页面，是否被更改
:~/docker/images/nginx$ docker inspect nginx-14

```



```shell
#构建子镜像Dockerfile文件
FROM ubuntu-nginx:v0.12
MAINTAINER panda kstwoak47@163.com
EXPOSE 80
ENTRYPOINT ["/usr/sbin/nginx","-g","daemon off;"]
 
#构建子镜像
:~/docker/images/nginx$ docker build -t ubuntu-nginx:v0.13 .
#根据镜像创建容器,
docker run -p 80 --name nginx-15 -itd ubuntu-nginx:v0.13
#查看镜像信息
:~/docker/images/nginx$ docker inspect ubuntu-nginx:v0.13
docker ps
#访问容器页面，是否被更改

```

### 3.1.8 Dockerfile构建缓存

我们第一次构建很慢，之后的构建都会很快，因为他们用到了构建的缓存。

```shell
#取消缓存：
docker build --no-cache -t [镜像名]:[镜像版本][Dockerfile位置] 
```

## 3.2 Dockerfile构建go环境

接下来我们就来做一个工作实践，搭建一个go环境,然后尝试使用Dockerfile的方式，构造一个镜像。

### 3.2.1 项目描述

beego官方网站：https://beego.me/

我们借助于beego的简介，部署一个go项目，然后运行起来。

### 3.2.2 手工部署go语言环境

**需求：**

基于docker镜像，手工部署go项目环境

**方案分析：**

1、docker环境部署

2、go环境部署

3、go项目部署

4、测试

技术关键点：

1、docker环境部署

使用docker镜像启动一个容器即可

2、go环境部署

go软件的依赖环境

go软件的基本环境配置

3、go项目部署

beego框架的下载

项目文件配置

启动go项目

4、测试

宿主机测试

解决方案：

1、docker环境配置

1.1 获取docker镜像

1.2 启动docker容器

2、go环境部署

2.1 基础环境配置

2.2 go环境配置

3、go项目部署

3.1 获取beego代码

3.2 项目文件配置

3.3 项目启动

4、测试

4.1 宿主机测试



**实施方案：**

```shell
#1、docker环境配置
#1.1 获取docker镜像
#获取一个ubuntu的模板文件
cat ubuntu-16.04-x86_64.tar.gz | docker import - ubuntu-nimi
#1.2 启动docker容器
#启动容器，容器名称叫go-test
docker run -itd --name go-test ubuntu-nimi
#进入容器
docker exec -it go-test /bin/bash
#2、go环境部署
#2.1 基础环境配置
#配置国内源
vim /etc/apt/sources.list
#文件内容如下
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
#如果由于网络环境原因不能进行软件源更新可以使用如下内容
sudo sed -i 's/cn.archive.ubuntu.com/mirrors.ustc.edu.cn/g' /etc/apt/sources.list 

#更新软件源，安装基本软件
apt-get update
apt-get install gcc libc6-dev git vim lrzsz -y
#2.2 go环境配置
#安装go语言软件
//apt-get install golang -y
由于软件源问题改使用新版本go
将go1.10.linux-amd64.tar.gz拷贝到容器中进行解压
tar -C /usr/local  -zxf go1.10.linux-amd64.tar.gz

#配置go基本环境变量
export GOROOT=/usr/local/go                
export PATH=$PATH:/usr/local/go/bin   
export GOPATH=/root/go
export PATH=$GOPATH/bin/:$PATH
#3、go项目部署
#3.1 获取beego代码
#下载项目beego
go get github.com/astaxie/beego
#3.2 项目文件配置
#创建项目目录
mkdir /root/go/src/myTest
cd /root/go/src/myTest
#编辑go项目测试文件test.go
package main
import (
"github.com/astaxie/beego"
)
type MainController struct {
beego.Controller
}
func (this *MainController) Get() {
this.Ctx.WriteString("hello world\n")
}
func main() {
beego.Router("/", &MainController{})
beego.Run()
}
#3.3 项目启动
#运行该文件
go run test.go
#可以看到：
#这个go项目运行起来后，开放的端口是8080
#4、测试
#4.1宿主机测试
#查看容器的ip地址
docker inspect go-test
#浏览器查看效果：
curl 172.17.0.2:8080
```

### 3.2.3 Dockerfile案例分析

环境分析：

1、软件源文件，使用国外源，速度太慢，所以我们可以自己使用国内的软件源。

因为我们在手工部署的时候，使用的是官方(国外)的源，所以为了部署快一点呢，我使用国内的阿里云的源。
源内容：

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

 由于阿里云的源出现问题所有更换为中科大的源 详情见3.1.1

2、软件安装，涉及到了各种软件

3、涉及到了go的环境变量设置

4、软件运行涉及到了软件的运行目录

5、项目访问，涉及到端口

关键点分析：

1、增加文件，使用 ADD 或者 COPY 指令

2、安装软件，使用 RUN 指令

3、环境变量，使用 ENV 指令

4、命令运行，使用 WORKDIR 指令

5、项目端口，使用 EXPOSE 指令

定制方案：

1、基于ubuntu基础镜像进行操作

2、增加国内源文件

3、安装环境基本软件

4、定制命令工作目录

5、执行项目

6、开放端口

### 3.2.4 Dockerfile实践

**创建目录**

```
#创建目录
#进入标准目录
mkdir /docker/images/beego
cd /docker/images/beego

```

**Dockerfile内容**

```shell
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

RUN go get github.com/astaxie/beego

# 增加文件

COPY test.go /root/go/src/myTest/

# 定制工作目录

WORKDIR /root/go/src/myTest/

# 对外端口

EXPOSE 8080

# 运行项目

ENTRYPOINT ["go","run","test.go"]

#把sources.list和test.go文件放到这个目录中
#构建镜像
docker build -t go-test:v0.1 .
#运行镜像
docker run -p 8080:8080 -itd go-test:v0.1
#访问镜像，查看效果
```