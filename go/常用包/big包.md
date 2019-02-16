#  Big包

## big.int

### 初始化方法

初始化 big.int 返回Int的指针

```go
func NewInt(x int64) *Int
```



### set类方法

将某种类型的值转化写入到big.Int中

```go
var bigIntTemp big.Int
bigIntTemp.Set(`value`)
```
具体的类型为

- int64 
- uint64
- []byte
- string

```go
func (z *Int) SetInt64(x int64) *Int
func (z *Int) SetUint64(x uint64) *Int
func (z *Int) SetBytes(buf []byte) *Int
func (z *Int) SetString(s string, base int) (*Int, bool)
//如果基数为0，字符串的前缀决定实际的转换基数："0x"、"0X"表示十六进制；"0b"、"0B"表示二进制；"0"表示八进制；否则为十进制。
func (z *Int) SetBits(abs []Word) *Int
func (z *Int) SetBit(x *Int, i int, b uint) *Int
```



### 类型转化型方法

与set类方法相对可以将big.Int类型转化为对应的类型:

具体的类型为:

- int64 
- uint64
- []byte
- string

```go
func (x *Int) Int64() int64
func (x *Int) Uint64() uint64
func (x *Int) Bytes() ]byte
func (x *Int) String() string
func (x *Int) BitLen() int
func (x *Int) Bits() []Word
```



### 计算类的方法

#### 算数运算符

```go
func (z *Int) Add(x, y *Int) *Int
将z设为x + y并返回z。

func (z *Int) Sub(x, y *Int) *Int
将z设为x - y并返回z。

func (z *Int) Mul(x, y *Int) *Int
将z设为x * y并返回z。

func (z *Int) Div(x, y *Int) *Int
如果y != 0会将z设为x/y并返回z；如果y==0会panic。函数采用欧几里德除法（和Go不同），参见DivMod。

func (z *Int) Mod(x, y *Int) *Int
如果y != 0会将z设为x%y并返回z；如果y==0会panic。函数采用欧几里德除法（和Go不同），参见DivMod。

func (z *Int) DivMod(x, y, m *Int) (*Int, *Int)
如果y != 0将z设为x/y，将m设为x%y并返回(z, m)；如果y == 0会panic。采用欧几里德除法（和Go不同）


func (z *Int) Quo(x, y *Int) *Int
如果y != 0会将z设为x/y并返回z；如果y==0会panic。函数采用截断除法（和Go相同），参见QuoRem。

func (z *Int) Rem(x, y *Int) *Int
如果y != 0会将z设为x%y并返回z；如果y==0会panic。函数采用截断除法（和Go相同），参见QuoRem。

func (z *Int) QuoRem(x, y, r *Int) (*Int, *Int)
如果y != 0将z设为x/y，将r设为x%y并返回(z, r)；如果y == 0会panic。函数采用截断除法（和Go相同）

func (z *Int) Abs(x *Int) *Int
将z设为|x|并返回z。
          
func (z *Int) Neg(x *Int) *Int
将z设为-x并返回z。

```



#### 位运算符

```go
func (z *Int) Not(x *Int) *Int
将z设为^x并返回z（按位取反）。

func (z *Int) And(x, y *Int) *Int
将z设为x & y并返回z（按位且）。

func (z *Int) Or(x, y *Int) *Int
将z设为x | y并返回z（按位或）。

func (z *Int) Xor(x, y *Int) *Int
将z设为x ^ y并返回z（按位异或）。

func (z *Int) AndNot(x, y *Int) *Int
将z设为x & (^y)并返回z（按位减）。

func (z *Int) Lsh(x *Int, n uint) *Int
将z设为x << n并返回z（左位移运算）。

func (z *Int) Rsh(x *Int, n uint) *Int
将z设为x >> n并返回z（右位移运算）。
```

#### 其他常用方法

```go
func (z *Int) Rand(rnd *rand.Rand, n *Int) *Int
将z设为一个范围在[0, n)的伪随机值，并返回z。
func (x *Int) Cmp(y *Int) (r int)
比较x和y的大小。x<y时返回-1；x>y时返回+1；否则返回0。
```



一些nb的不能在nb的方法

```go
func (x *Int) ProbablyPrime(n int) bool
对x进行n次Miller-Rabin质数检测。如果方法返回真则x是质数的几率为1-(1/4)**n；否则x不是质数。

func (x *Int) Sign() int
返回x的正负号。x<0时返回-1；x>0时返回+1；否则返回0。

func (z *Int) Set(x *Int) *Int
将z设为x（生成一个拷贝）并返回z

func (z *Int) ModInverse(g, p *Int) *Int
将z设为g相对p的模逆（即z、g满足(z * g) % p == 1）。返回值z大于0小于p。

func (z *Int) Exp(x, y, m *Int) *Int
将z设为x**y mod |m|并返回z；如果y <= 0，返回1；如果m == nil 或 m == 0，z设为x**y。

func (z *Int) GCD(x, y, a, b *Int) *Int
将z设为a和b的最大公约数并返回z。a或b为nil时会panic；a和b都>0时设置z为最大公约数；如果任一个<=0方法就会设置z = x = y = 0。如果x和y都不是nil，会将x和y设置为满足a*x + b*y==z。

func (x *Int) Format(s fmt.State, ch rune)
Format方法实现了fmt.Formatter接口。本方法接受格式'b'（二进制）、'o'（八进制）、'd'（十进制）、'x'（小写十六进制）、'X'（大写十六进制）。

方法支持全套fmt包对整数类型的动作：包括用于符号控制的'+'、'-'、' '；用于十六进制和八进制前导0的'#'；"%#x"和"%#X"会设置前导的"0x"或"0X"；指定最小数字精度；输出字段宽度；空格或'0'的补位；左右对齐。

func (z *Int) Scan(s fmt.ScanState, ch rune) error
Scan实现了fmt.Scanner接口，将z设为读取的数字。方法可以接受接受格式'b'（二进制）、'o'（八进制）、'd'（十进制）、'x'（小写十六进制）、'X'（大写十六进制）。


func (x *Int) GobEncode() ([]byte, error)
本方法实现了gob.GobEncoder接口。

func (z *Int) GobDecode(buf []byte) error
本方法实现了gob.GobDecoder接口。

func (z *Int) MarshalJSON() ([]byte, error)
本方法实现了json.Marshaler接口。

func (z *Int) UnmarshalJSON(text []byte) error
本方法实现了json.Unmarshaler接口。

func (z *Int) MarshalText() (text []byte, err error)
本方法实现了encoding.TextMarshaler接口。

func (z *Int) UnmarshalText(text []byte) error
本方法实现了encoding.TextUnmarshaler接口。
```

