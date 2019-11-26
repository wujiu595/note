---
typora-copy-images-to: img
typora-root-url: img
---

# 2 images管理

## 2.1 镜像简介

Docker镜像是什么？
​	镜像是一个Docker的可执行文件，其中包括运行应用程序所需的所有代码内容、依赖库、环境变量和配置文件等。
	通过镜像可以创建一个或多个容器。

### 2.1.1帮助命令

当不知道如何使用某一命令时可以使用如下命令获取具体的作用和参数

```shell
docker `commond` --help
```



## 2.2 搜索、查看、获取、重命名、删除

### 2.2.1 搜索

```shell
docker search `image`
#NAME          镜像名称                     
#DESCRIPTION   描述                                  
#STARS         下载数量      
#OFFICIAL      是否是官方出品      
#AUTOMATED     自动的运行
```

### 2.2.2 查看

#### 查看所有的镜像信息

```shell
docker images
#REPOSITORY    镜像名称    
#TAG           版本      
#IMAGE ID      镜像id      
#CREATED       创建时间      
#SIZE          大小
```

#### 查看镜像历史

```shell
docker history [镜像名称]:[镜像版本]
docker history [镜像ID]

#IMAGE：编号           
#CREATED：创建的  
#CREATED BY ：基于那些命令创建的                                 
#SIZE：大小
#COMMENT：评论
```

#### 镜像详细信息

```shell
docker inspect [OPTIONS] NAME|ID [NAME|ID...]

#example
docker inspect nginx
```



### 2.2.3获取镜像

```shell
docker pull `image`
#获取的镜像在哪里？
#/var/lib/docker 目录下
#由于权限的原因我们需要切换root用户
#那我们首先要重设置root用户的密码：
:~$ sudo passwd root
#这样就可以设置root用户的密码了。
#之后就可以自由的切换到root用户了
:~$ su
#输入root用户的密码即可。
```

### 2.2.4镜像重命名

```shell
docker tag `oldname`：`version` `newname`：`version`
#oldname的version如果是latest可以省略
#newname不能含有大写字母
```

### 2.2.4删除镜像

```shell
1.docker rmi [命令参数][镜像ID]
2.docker rmi [命令参数][镜像名称]:[镜像版本]
#命令演示：
$docker rmi 3fa822599e10
$docker rmi mysql:latest
#注意：
重命名后的镜像只能使用2方法删除
#命令参数(OPTIONS)：	
	-f, --force      		强制删除
```

## 2.3镜像的导入、导出

### 2.3.1导出

```shell
docker save [OPTIONS] IMAGE [IMAGE...]

#Options:
  -o, --output string   Write to a file, instead of STDOUT

#example：
docker save -o /images/nginx.img nginx
```

### 2.3.2导入

```shell
docker load [OPTIONS]
#Options:
  -i, --input string   Read from tar archive file, instead of STDIN
  -q, --quiet          Suppress the load output

#example
docker load -i ~/docker-images/nginx.img
```

![1551577002102](/1551577002102.png)



















