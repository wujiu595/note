# golang 环境配置

## 下载

ubuntu下使用apt-get 可快速安装go

```shell
sudo apt-get install golang
```

## 环境变量设置

打开配置文件

```shell
vim /etc/profile
```

在文件末尾添加

```shell
export GOROOT=/usr/lib/go-1.8  /*go语言对应版本*/
export PATH="$PATH:$GOROOT/bin" 
export GOPATH=$HOME/go 
export PATH="$PATH:$GOPATH/bin"
```

保存更改

```shell
source /etc/profile
```

