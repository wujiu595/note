# MySQL数据操作

## 1 插入数据

### 1.1 插入常规数据

一般形式：字段的顺序和值的顺序必须一致

```mysql
insert into `表名`（`字段`） values(`值`);
```

如果值和字段一一对应可以省略字段名写成如下形式

```mysql
insert into `表名` values(`值`);
```

### 1.2 插入自增数据(atuo )

再插入自增数据时可以使用null代替

```mysql
insert into `表名`(id) values(null);
```



### 1.3 插入默认值

再向有默认值的字段添加数据时可以使用 default

```mysql
insert into `表名`(name) values(default);
```

也可以指定为所有没有默认值的字段添加数据

```mysql
insert into `表名`(name) values();
```



### 1.4 一次插入多条数据

```mysql
insert into `表名` values(`值`),(`值`),(`值`)..........;
```



### 1.5 insert..set 添加数据

```mysql
insert into stu set `字段`=`值`.........
```



### 1.6 insert…select语句

```mysql
insert into 表1 select 字段1,字段2,.....from 表2
```





## 2.删除数据



### 2.1 删除指定的数据

```mysql
delete from `表名` where `字段`=`值`
```

### 2.2 清空表

逐条删除数据

```mysql
delete from `表名`
```

删除表并新建一张表

```mysql
truncate table `表名`
```



## 3.修改数据

```mysql
update `表名` set `字段名` = `值`
```



## 4.查询数据

### 4.1 总览

select查询语句的一般形式如下：

```mysql
select 选项 from 表名 where 条件 [group by  字段名][having 条件] [order by 字段][limit 参数]
```

### 4.2 选项

选项本质上是一个表达式

#### 4.2.1 字段名

```mysql
`字段名`，`字段名`，`字段名`#字段名 用逗号隔开
* #所有字段

select id,name from table
```

#### 4.2.2.表达式

```mysql
`字段名`+`字段名`

select id+name from table
```

#### 4.2.3 函数

```mysql
select sum(id) from table
```



### 4.3 from子句

from后面连接的是表名可以是一个表或者多个表用逗号隔开

```mysql
select * from `表名`,`表名`
```



### 4.4 where子句

where条件用来过滤选择的内容

#### 4.4.1 算数表达式

=,>,<,>=,<=

```mysql
select * from `表名`  where `字段名`=`值`
```

#### 4.4.2 in|not in

表示在....中或不在...中

in

```mysql
select * from `表名`  where `字段名` in (`值`，`值`)
```

not

```mysql
select * from `表名`  where `字段名` not in (`值`，`值`)
```

#### 4.4.3 between and|not between and

表示在...和...之间

between and

```mysql
select * from `表名`  where `字段名` between `值` and`值`
```

not between and

```mysql
select * from `表名`  where `字段名` not between `值` and`值`
```

#### 4.4.4 **is** **null** | is not null

是否为空

is  null

```mysql
select * from `表名`  where `字段名` is null
```

is not null

```mysql
select * from `表名`  where `字段名` is not null
```

#### 4.4.5 模糊查询

占位符

1）%,0个或多个字符

2）_，一个字符

like

```mysql
select * from `表名` where `字段名` like `%???%`
```

```mysql
select * from `表名` where `字段名` like `?_?`
```



### 4.5 分组查询  （group by）

1）按照字段中内容的不同进行分组，有多少个不同的值，就分多少个组

2）一般配合聚合函数使用

3）在不使用聚合函数的情况下一般返回不同的值的第一行

4）多个聚合函数在group中不能嵌套使用

```mysql
select * from `表名` group by `字段名`
```



### 4.6 having 子句

having字句的作用和where字句类似但是可以使用自定自定义的字段



### 4.7 limit子句

限制执行sql语句输出的行数

```mysql
select * from `表名` limit 0,1# 如果从第0行开始可以简写成 limit 1
```

















