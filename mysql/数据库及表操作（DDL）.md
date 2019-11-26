# MySQL 数据库及表操作



## 1 数据库操作

### 1.1 备注

```mysql
#单行备注
-- 单行备注
/*
块备注
*/

```
### 1.2 创建数据库

```mysql
create database [if not exists] `数据库名`  [charset = `字符集`]
```



### 1.3 删除数据库

```mysql
alter database [if exists] `数据库名`  charset = `字符集`；
```



### 1.4 查看数据库

查看数据库有两种方法

1.查看所有的数据库

```mysql
show databases； 
```

2.查看单个数据库及创建语句

```mysql
show database `数据库名`；
```



### 1.5 使用数据库

```mysql
use `数据库名`；
```



## 2 表的操作



### 2.1 创建表

```mysql
create table `表名`(
	`字段名` `数据类型` [约束] comment '这是学生的id',
)[engine=存储引擎] [default][charset=字符编码] 
/*
null|not null：是否为空
Default: 默认值
Auto_increment: 自动增长
Primary key: 主键
Comment: 备注
Engine:存储引擎，不同存储引擎表示不同的数据存储方式
Charset：设置表的字符编码
*/
```

```mysql
create table stu(
	id int primary key comment '这是学生的id',
	name varchar(5)
);
```



### 2.2  删除表

```mysql
drop table [if exists] `表名`;
```



### 2.3 修改表

#### 2.3.1 添加或删除字段

1.在末尾添加字段

```mysql
alter table `表名` add `字段`;
```

2.在某字段之后添加字段

```mysql
alter table `表名` add `字段` after `字段`;
```

3.在开始添加字段

```mysql
alter table `表名` add `字段` first;
```

4.删除指定字段

```mysql
 alter table `表名` drop `字段`;
```



#### 2.3.2 修改字段

1.modify 可以修改字段的属性,除了字段名都可以修改

```mysql
alter table `表名` modify `字段` `数据类型` `修改的属性`;
```

2.change 可以修改字段属性和字段名

```mysql
alter table `表名` change `老的字段` `新的字段名` `数据类型` `修改的属性`;
```



### 2.4 查看表

1.查看所有的表

```mysql
show tables;
```

2.显示一个表及创建语句

```mysql
show table `表名` [/G]
```

3.对其显示一个表

```mysql
desc `表名`
```













