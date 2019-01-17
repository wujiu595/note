# Gite本地版本管理

linus做的git



最开始是做版本控制的



分布式



## 1. 安装git

```shell
sudo  apt-get install git
```



## 2 .初始化在当前选中目录



### 2.1 初始化git仓库

```shell
wujiu@wujiu-PC:~$ git init
已初始化空的 Git 仓库于 /home/wujiu/.git/
```

### 2.2 初始化账号密码

下面是本地仓库的账号密码设置

```shell
  git config --global user.email "you@example.com"
  git config --global user.name "Your Name"
```


## 3. 提交版本git stash

### 3.1 把add.go添加到缓存

```shell
git add.go
```


### 3.2 把add.go提交到版本库

```shell
git commit -m "备注信息"
```



### 3.3 查看git的状态和log信息

```shell
git status  
git log
```

SHELL注意：重复提交版本会进行版本校验



## 4.回退版本

### 4.1 回退到上一个版本

```shell
git reset --hard HEAD^
```



### 4.2 回退到指定版本

查看所有的版本号记录

```shell
git reflog
```

通过制定的版本号回退版本

```shell
git reset --hard `版本号`
```


### 4.3 回退多个版本

回退到前面多个版本

```shell
git reset --hard HEAD~`回退多少个版本`
```



## 5. 丢弃改变

SHELLSHELLSHELL

### 5.1 在工作去丢弃更改

```shell
git checkout -- test2.go
```



### 5.2 在缓存区丢弃更改

首先丢弃缓存区的更改

```shell
git reset HEAD test.go
```

然后丢弃工作区的更改

```shell
git checkout -- test2.go
```



### 5.3 删除文件

```shell
rm -r test2.go
```

回退删除的文件

```shell
git checkout -- test2.go
```



删除文件

```shell
git rm test2.gp
```



```shell
git commit 
```



### 5.4 两者之间的比较

```SHELL
git diff HEAD HEAD~1
```





# 分支

```shell
git branch 查看分支
git branch `分支名` 新建分支
git branch -d `分支名` 新建分支
```



```shell
git checkout master 选择分支

git checkout -b newDev 选择并新建该分支
```



```shell
git merge dev
```



## 暂时存储工作状态



git stash  git stash list

git stash pop







