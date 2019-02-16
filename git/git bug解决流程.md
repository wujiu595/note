## git bug解决流程

基于master新建bug解决分支`wujiu/bugi-1357`

```shell
git checkout -b wujiu/bugi-1357
```

解决bug后提交在远程上新建分支

```shell
git push origin wujiu/bugi-1357：wujiu/bugi-1357
```

检查bug并且合并代码

```shell
git fetch origin wujiu/bugi-1357：wujiu/bugi-1357
git checkout master
git merge wujiu/bugi-1357
```

重新提交代码

```shell
git push origin master
```

删除远程分支

```shell
git push origin --delete wujiu/bugi-1357
```

