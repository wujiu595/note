## package ioutil

`import "io/ioutil"`

### func ReadAll

```
func ReadAll(r io.Reader) ([]byte, error)
```

ReadAll从r读取数据直到EOF或遇到error，返回读取的数据和遇到的错误。成功的调用返回的err为nil而非EOF。因为本函数定义为读取r直到EOF，它不会将读取返回的EOF视为应报告的错误。

### func ReadFile

```
func ReadFile(filename string) ([]byte, error)
```

ReadFile 从filename指定的文件中读取数据并返回文件的内容。成功的调用返回的err为nil而非EOF。因为本函数定义为读取整个文件，它不会将读取返回的EOF视为应报告的错误。

### func WriteFile

```
func WriteFile(filename string, data []byte, perm os.FileMode) error
```

函数向filename指定的文件中写入数据。如果文件不存在将按给出的权限创建文件，否则在写入数据之前清空文件。

### func ReadDir

```
func ReadDir(dirname string) ([]os.FileInfo, error)
```

返回dirname指定的目录的目录信息的有序列表。

### func TempDir

```
func TempDir(dir, prefix string) (name string, err error)
```

在dir目录里创建一个新的、使用prfix作为前缀的临时文件夹，并返回文件夹的路径。如果dir是空字符串，TempDir使用默认用于临时文件的目录（参见os.TempDir函数）。 不同程序同时调用该函数会创建不同的临时目录，调用本函数的程序有责任在不需要临时文件夹时摧毁它。

### func TempFile

```
func TempFile(dir, prefix string) (f *os.File, err error)
```

在dir目录下创建一个新的、使用prefix为前缀的临时文件，以读写模式打开该文件并返回os.File指针。如果dir是空字符串，TempFile使用默认用于临时文件的目录（参见os.TempDir函数）。不同程序同时调用该函数会创建不同的临时文件，调用本函数的程序有责任在不需要临时文件时摧毁它。