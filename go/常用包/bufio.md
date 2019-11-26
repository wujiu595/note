## buffio

有缓冲的读取

#### func NewScanner

```go
func NewScanner(r io.Reader) *Scanner
```

NewScanner创建并返回一个从r读取数据的Scanner，默认的分割函数是ScanLines。

### NewScanner成员方法

#### func (*Scanner) Scan

```go
func (s *Scanner) Scan() bool
```

Scan方法获取当前位置的token（该token可以通过Bytes或Text方法获得），并让Scanner的扫描位置移动到下一个token。当扫描因为抵达输入流结尾或者遇到错误而停止时，本方法会返回false。在Scan方法返回false后，Err方法将返回扫描时遇到的任何错误；除非是io.EOF，此时Err会返回nil。

#### func (*Scanner) Bytes

```go
func (s *Scanner) Bytes() []byte
```

Bytes方法返回最近一次Scan调用生成的token。底层数组指向的数据可能会被下一次Scan的调用重写。

#### func (*Scanner) Text

```go
func (s *Scanner) Text() string
```

Bytes方法返回最近一次Scan调用生成的token，会申请创建一个字符串保存token并返回该字符串。

#### func (*Scanner) Err

```go
func (s *Scanner) Err() error
```

Err返回Scanner遇到的第一个非EOF的错误。

#### func (*Scanner) Split

```go
func (s *Scanner) Split(split SplitFunc)
```



### Split方法

Split设置该Scanner的分割函数。本方法必须在Scan之前调用。

#### func ScanBytes

```go
func ScanBytes(data []byte, atEOF bool) (advance int, token []byte, err error)
```

ScanBytes是用于Scanner类型的分割函数（符合SplitFunc），本函数会将每个字节作为一个token返回。

#### func ScanRunes

```go
func ScanRunes(data []byte, atEOF bool) (advance int, token []byte, err error)
```

ScanRunes是用于Scanner类型的分割函数（符合SplitFunc），本函数会将每个utf-8编码的unicode码值作为一个token返回。本函数返回的rune序列和range一个字符串的输出rune序列相同。错误的utf-8编码会翻译为U+FFFD = "\xef\xbf\xbd"，但只会消耗一个字节。调用者无法区分正确编码的rune和错误编码的rune。

#### func ScanWords

```go
func ScanWords(data []byte, atEOF bool) (advance int, token []byte, err error)
```

ScanRunes是用于Scanner类型的分割函数（符合SplitFunc），本函数会将空白（参见unicode.IsSpace）分隔的片段（去掉前后空白后）作为一个token返回。本函数永远不会返回空字符串。

#### func ScanLines

```go
func ScanLines(data []byte, atEOF bool) (advance int, token []byte, err error)
```

ScanRunes是用于Scanner类型的分割函数（符合SplitFunc），本函数会将每一行文本去掉末尾的换行标记作为一个token返回。返回的行可以是空字符串。换行标记为一个可选的回车后跟一个必选的换行符。最后一行即使没有换行符也会作为一个token返回。