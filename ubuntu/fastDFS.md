# fastDFS

服务端两个角色: 

Tracker:管理集群，tracker 也可以实现集群。每个 tracker 节点地位平等。收集 Storage 集群的状态。 

Storage:实际保存文件 



## fastdfs安装及配置

#### １．在　～目录下新建　fastfds 文件夹

```shell
mkdir /home/name/fds
```



#### ２．安装FastDFS依赖包

1. 解压缩libfastcommon-master.zip
2. 进入到libfastcommon-master的目录中
3. 执行**./make.sh**
4. 执行**sudo ./make.sh install**



#### ３．安装FastDFS

1. 解压缩fastdfs-master.zip
2. 进入到 fastdfs-master目录中
3. 执行 **./make.sh**
4. 执行 **sudo ./make.sh install**



#### ４．配置跟踪服务器tracker

1.

```shell
sudo cp /etc/fdfs/tracker.conf.sample /etc/fdfs/tracker.conf
```

2.在fastfds文件种新建　 tracker文件夹

```shell
mkdir /home/name/fds/tracker
```

3.编辑/etc/fdfs/tracker.conf配置文件

```shell
sudo vim /etc/fdfs/tracker.conf
```

修改 

```shell
base_path=/home/itcast/fastdfs/tracker
```



#### ５．配置存储服务器storage

1.

```shell
sudo cp /etc/fdfs/storage.conf.sample /etc/fdfs/storage.conf
```

2.在fastfds文件种新建　 tracker文件夹

```shell
mkdir /home/name/fds/storage
```

3.编辑/etc/fdfs/storage.conf配置文件

```shell
sudo vim /etc/fdfs/storage.conf
```

修改内容

```shell
base_path=/home/itcast/fastdfs/storage
store_path0=/home/itcast/fastdfs/storage
tracker_server=本机ip:22122
```



#### ６．启动tracker和storage

进入到/etc/fdfs/下面执行以下两条指令

```mysql
sudo  fdfs_trackerd  /etc/fdfs/tracker.conf
sudo fdfs_storaged  /etc/fdfs/storage.conf
```



#### ７．测试是否安装成功

1.编辑/etc/fdfs/client.conf配置文件

```shell
sudo cp /etc/fdfs/client.conf.sample /etc/fdfs/client.conf
```

2.编辑/etc/fdfs/client.conf配置文件

```shell
sudo vim /etc/fdfs/client.conf
```

修改：

```shell
base_path=/home/itcast/fastdfs/tracke1. r
tracker_server=自己ubuntu虚拟机的ip地址:22122
```

3.上传文件测试

```shell
sudo fdfs_upload_file /etc/fdfs/client.conf ｀文件名｀
```

返回数据

```shell
group1/M00/00/00/rBIK6VcaP0aARXXvAAHrUgHEviQ394.jpg
```



#### ８．安装fastdfs-nginx-module

- 解压缩 nginx-1.8.1.tar.gz
- 解压缩 fastdfs-nginx-module-master.zip
- 进入nginx-1.8.1目录中
- 执行

```shell
sudo ./configure  --prefix=/usr/local/nginx/ --add-module=fastdfs-nginx-module-master解压后的目录的绝对路径/src
```

注意：**这时候会报一个错，说没有PCRE库**

- 安装libpcre3

```apt
sudo apt-get install libpcre3 libpcre3-dev 
```

- 此时运行还会出现一个错误

```shell
vim /home/wujiu/fastDFS/nginx-1.8.1/objs/Makefile
```

- 去掉:

```mysql
-Werror
```

- 执行安装命令

```shell
sudo make
sudo make install 
```

- sudo cp fastdfs-nginx-module-master解压后的目录中src下mod_fastdfs.conf   /etc/fdfs/mod_fastdfs.conf

- sudo vim /etc/fdfs/mod_fastdfs.conf

修改内容：

```shell
connect_timeout=10　
tracker_server=自己ubuntu虚拟机的ip地址:22122
url_have_group_name=true
store_path0=/home/python/fastdfs/storage
```

- sudo cp 解压缩的fastdfs-master目录中的conf中的http.conf  /etc/fdfs/http.conf

- sudo cp 解压缩的fastdfs-master目录中conf的mime.types /etc/fdfs/mime.types

- sudo vim /usr/local/nginx/conf/nginx.conf

在http部分中添加配置信息如下：

```shell
server {
            listen       8888;
            server_name  localhost;
            location ~/group[0-9]/ {
                ngx_fastdfs_module;
            }
            error_page   500 502 503 504  /50x.html;
            location = /50x.html {
            root   html;
            }
        }

```

- 启动nginx

  sudo  /usr/local/nginx/sbin/nginx

## 启动服务

```shell
sudo  fdfs_trackerd  /etc/fdfs/tracker.conf
sudo fdfs_storaged  /etc/fdfs/storage.conf
sudo  /usr/local/nginx/sbin/nginx
```





## go连接fastdfs

- 先导包，把我们下载的包导入

```go
import "github.com/weilaihui/fdfs_client"
```


- 导包之后,我们需要指定配置文件生成客户端对象

```go
client,_:=fdfs_client.NewFdfsClient("/etc/fdfs/client.conf")
```

- 接着我们就可以通过client对象执行文件上传，上传有两种方法，一种是通过文件名，一种是通过字节流

1. 通过文件名上传**UploadByFilename **,参数是文件名（必须通过文件名能找到要上传的文件），返回值是fastDFS定义的一个结构体，包含组名和文件ID两部分内容

```go
fdfsresponse,err := client.UploadByFilename("flieName")
```
2. 通过字节流上传**UploadByBuffer**,参数是字节数组和文件后缀，返回值和通过文件名上传一样。

```go
fdfsresponse,err := client.UploadByBuffer(fileBuffer,ext)
```












































