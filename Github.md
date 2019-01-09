# github远程仓库



1.在github上新建项目



2.在vim  .gitconfig

```shell
[user]
	email = 676316526@qq.com
	name = WangShuai
```



3.使用如下命令生成ssh秘钥

```shell
ssh-keygen -t rsa -C "676316526@qq.com"
```



4. cd.ssh文件夹

```shell
公钥为id_rsa.pub
私钥为id_rsa
```

查看公钥内容并copy内容

```shell
cat id_rsa.pub
```



5.回到浏览器，填写标题，粘贴公钥





6.复制git地址克隆

```shell
git clone `地址`
```



7.copy本地代码到clone的目录下 

git add

git commit



git push origin `分支名称`

git branch --set-upstream-to=origin/ttsx ttsx



git pull orgin 分支名称



## 5. **工作使用git**

**项目经理：**

(1) 项目经理搭建项目的框架。

(2) 搭建完项目框架之后，项目经理把项目框架代码放到服务器。

**普通员工：**

(1) 在自己的电脑上，生成ssh公钥，然后把公钥给项目经理，项目经理把它添加的服务器上面。

(2) 项目经理会给每个组员的项目代码的地址，组员把代码下载到自己的电脑上。

(3) 创建本地的分支dev,在dev分支中进行每天的开发。

(4) 每一个员工开发完自己的代码之后，都需要将代码发布远程的dev分支上。

 

Master:用户保存发布的项目代码。V1.0,V2.0

Dev:保存开发过程中的代码。