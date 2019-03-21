#  orm增删该查 CRUD

orm简单的crud操作包括：insert、read、update、delete 4个方法，第一个参数都是操作的对象的指针，第二个参数是一个不定参数用来查询的匹配属性。当给对象的id赋值并进行操作是可以省略第二个参数。

## 1. Read(查询读取)

read的返回值为err常用的err如下

```go
var user models.User
user.name = "selen"
o:=orm.NewOrm()
err:=o.read(&user，"name")//ErrNoRows 查询不到信息 //ErrMissPk 找不到主键
```

## 2 . ReadOrCreate(读取或者创建)

尝试从数据库读取数据，不存在的话就新建一个,ReadOrCreate方法会返回三个参数，第一个是是否插入的数据，

第二个是对象的Id值，第三个参数是err

```mysql
var user models.Usedelete(删除)r
user.name = "selen"
o:=orm.NewOrm()
id,err:=o.ReadOrCreate(&user，"name")
```

## 3. Insert(插入)

insert的第一个返回值为自增键的id，第二个键为err

```mysql
var user models.User
user.Name = "selen"
o:=orm.NewOrm()
id,err:=o.Insert(&user)
```

## 4. InsertMulti(插入多条数据)

InsertMulti的第一个参数是插入的条目数，第二个参数是对象的切片，返回插入的数量和错误

```go
var users = []models.User{
    {Name:"selen1"},
    {Name:"selen2"},
    {Name:"selen3"},
    {Name:"selen4"},
}
o:=orm.NewOrm()
num,err:=o.InsertMulti(len(user),users)

```

## 5.  Update(更新数据)

```mysql
var user models.User
user.Id = 1
if o.Read(&user)==nil{
	user.name = "bob"
	num,err:=o.Update
}
```

## 6. delete(删除)

```go
var user models.User
user.Id = 1
num,err:=o.delete(&user)
```




