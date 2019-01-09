# ubuntu下安装mysql

通过apt获取mysql server and cli

```shell
sudo apt-get install mysql-server
sudo apt-get install mysql-client
sudo apt-get install libmysqlclient-dev
```

设置mysql初始密码:

### 1.停止mysql的运行，并设置mysql跳过密码访问

```shell
sudo service mysql stop
sudo /usr/bin/mysqld_safe --skip-grant-tables --skip-networking &
修改mysql.cnf 在 [mysqld]下添加skip-grant-tables
```

### 2.设置mysql初始密码

```shell
> use mysql;

> update user set authentication_string=PASSWORD("这里输入你要改的密码") where User='root'; 
update user set authentication_string=PASSWORD("wujiu59") where User='root';#更改密码
> update user set plugin="mysql_native_password"; #如果没这一行可能也会报一个错误，因此需要运行这一行

> flush privileges; #更新所有操作权限
> quit;
```

### 3.设置访问权限

```mysql

```

