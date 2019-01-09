# beego orm自动建表

## 1 注册数据库

```go
func init()  {
	//连接数据库
	orm.RegisterDataBase("default","mysql","root:wujiu59@tcp(127.0.0.1:3306)/test?charset=utf8")
	//注册表
	orm.RegisterModel(new(User),new(Article))
	//新建表
	orm.RunSyncdb("default",true,true)
}
```



## 2 表字段映射



### 2.1 结构体属性映射

 结构体属性映射到对应的表中生成数据库对应类型的字段



#### 2.1.1整数类型

| 数据库类型(有符号) | beego数据类型（有符号） | 数据库类型(无符号) | beego数据类型（无符号） |
| :----------------: | :---------------------: | ------------------ | ----------------------- |
|      tinyint       |        byte/int8        | tinyint            | uint8                   |
|      smallint      |          int16          | smallint           | uint16                  |
|      integer       |     int/int32/rune      | integer            | uint/uint32             |
|       bigint       |          int64          | bigint             | uint64                  |



#### 2.1.2浮点数

|  数据库类型   |              beego数据类型              |
| :-----------: | :-------------------------------------: |
|    double     |             float32/float64             |
| decimal(10,4) | float64 ``orm:“digits(10);decimal(4)”`` |



#### 2.1.3 字符型

|  数据库类型  |           beego数据类型           |
| :----------: | :-------------------------------: |
| varchar（x） |      string `orm:"size(x)"`       |
|   char(x)    | string `orm:"type(char);size(x)"` |
|   longtext   |     string `orm:"type(text)"`     |



#### 2.1.4 时间类型

| 数据库类型 |           beego数据类型           |
| :--------: | :-------------------------------: |
|  datetime  | time.Time `orm："type(datetime)"` |
|    date    |   time.Time `orm："type(date)"`   |



### 2.2 数据库约束设置



#### 2.2.1一般形式

beego使用在属性后添加``并填写内容的形式设置表字段约束不同的约束用；号隔开

```go
`orm：“pk；auto”`
```



#### 2.2.2  约束的映射关系

|     数据库     |              beego              |
| :------------: | :-----------------------------: |
|      null      | `orm:"null"`(beego默认not null) |
|     unique     |         `orm:"unique"`          |
|       pk       |           `orm:"pk"`            |
|    default     |       `orm:"default(1)"`        |
|     index      |          `orm:"index"`          |
| atuo_increment |          `orm:"auto"`           |

#### 2.2.3 on_delete

on_delete是beego用来处理rel关系删除时，处理关系字段。

```go
cascade `orm:"on_delete(cascade)"`    级联删除(默认值)
set_null  `orm:"on_delete(set_null)"` 设置为 NULL，需要设置 null = true
set_default `orm:"on_delete(set_default)"`  设置为默认值，需要设置 default值
do_nothing `orm:"on_delete(do_nothing)"`    什么也不做，忽略
```



#### 2.2.4 多字段索引和多字段唯一键

```go
// 多字段索引
func (u *User) TableIndex() [][]string {
    return [][]string{
        []string{"Id", "Name"},
    }
}

// 多字段唯一键
func (u *User) TableUnique() [][]string {
    return [][]string{
        []string{"Name", "Email"},
    }
}
```



### 2.3 auto_now / auto_now_add

```go
Created time.Time `orm:"auto_now_add;type(datetime)"`
Updated time.Time `orm:"auto_now;type(datetime)"`
```

- auto_now 每次 model 保存时都会对时间自动更新
- auto_now_add 第一次保存时才设置时间
- 对于批量的 update 此设置是不生效的 



### 2.4 表引擎设置

```go
// 设置引擎为 INNODB
func (u *User) TableEngine() string {
    return "INNODB"
}
```



## 3 表关系

这里我们使用 文章、文章类型、和用户举例

### 3.1 一对一关系

```go
rel(one)
reverse(on)
```



### 3.2 一对多关系

一种类型可以有多个文章，所以文章类型和文章是一对多的关系

```go
`外键`  rel(fk);

`主键`  reverse(many)；
```

实例

```go
文章
type Article struct {
	Id int `orm:"pk;auto"`
	ArticleType *ArticleType `orm:"rel(fk)"` //1对多关系多的
	Users []*User `orm:"reverse(many)"`//多对多(可互换)
}
//需要创建一对多的类型表以及多对多的用户与文章关系表
type ArticleType struct {
	Id int
	Article []*Article `orm:"reverse(many)"`
}
```





### 3.3 多对多关系

```go
 rel(m2m);
 reverse(many)
```

一个用户可以阅读多篇文章，一个文章可以被多个用户阅读，所以文章和用户是多对多的关系

```go
type Article struct {
	Id int `orm:"pk;auto"`
	ArticleType *ArticleType `orm:"rel(fk)"` //1对多关系多的
	Users []*User `orm:"reverse(many)"`//多对多(可互换)
}

type User struct {
	Id int
	Articles []*Article `orm:"rel(m2m)"`//多对多(可互换)
}
```

本质上，设置多对多的两张表。orm会自动新建第三张表用来建立两个关系的连接

```mysql
+------------+------------+------+-----+---------+----------------+
| Field      | Type       | Null | Key | Default | Extra          |
+------------+------------+------+-----+---------+----------------+
| id         | bigint(20) | NO   | PRI | NULL    | auto_increment |
| user_id    | int(11)    | NO   |     | NULL    |                |
| article_id | int(11)    | NO   |     | NULL    |                |
+------------+------------+------+-----+---------+----------------+
```











