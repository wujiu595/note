---
stypora-copy-images-to: img
typora-root-url: img
---

# 数字签名（rsa+ecc）

## 1.rsa数字签名

1. 保证数据来源（公钥解私钥）

2. 保证数据不被篡改（私钥只有自己持有）

3. 消息的发送者无法否认

   ![1547864278321](/1547864278321.png)

### 签名：

#### - 主要方法：

```go
import "crypto/rsa"
func SignPKCS1v15(rand io.Reader, priv *PrivateKey, hash crypto.Hash, hashed []byte) ([]byte, error) 
/*
参数：
1.随机数生成器
2.私钥
3.hash算法  crypto.SHA256
4.签名文件的Hash值
*/
```

#### - 代码：

```go
func rsaSignData(src []byte,file string) []byte  {
   //将文件做sha256hash 
   hasher:=sha256.New()
   _,err:=hasher.Write(src)
   if err!=nil{
      panic(err)
   }
   hash:=hasher.Sum(nil)
   //从文件中读取私钥 
   buffer,err:=ioutil.ReadFile(file)
   if err!=nil{
      panic(err)
   }
   block,_:=pem.Decode(buffer)
   berText:=block.Bytes
   privateKey,err:=x509.ParsePKCS1PrivateKey(berText)
   if err!=nil{
      panic(err)
   }
   //使用私钥签名
   signature,err:=rsa.SignPKCS1v15(rand.Reader,privateKey,crypto.SHA256,hash[:])
   if err!=nil{
      panic(err)
   }
   return signature
}
```

### 验证：

#### - 主要方法

```go
import "crypto/rsa"
func VerifyPKCS1v15(pub *PublicKey, hash crypto.Hash, hashed []byte, sig []byte) error
/*
参数：
1.公钥
2.hash算法  crypto.SHA256
3.签名文件的Hash值
4.签名
*/
```

#### - 代码

```go
func verSignData(src,sig []byte,file string) bool  {
    //将文件做sha256hash 
   hasher:=sha256.New()
   _,err:=hasher.Write(src)
   if err!=nil{
      panic(err)
   }
   hash:=hasher.Sum(nil)
   //从文件中读取公钥
   buffer,err:=ioutil.ReadFile(file)
   if err!=nil{
      panic(err)
   }
   block,_:=pem.Decode(buffer)
   berText:=block.Bytes
   publicKey,err:=x509.ParsePKCS1PublicKey(berText)
   if err!=nil{
      panic(err)
   }
   //使用公钥校验签名
   err=rsa.VerifyPKCS1v15(publicKey,crypto.SHA256,hash[:],sig)
   if err!=nil{
      return false
   }
   return true
}
```

### 完整代码：

```go
package main

import (
   "crypto"
   "crypto/sha256"
   "crypto/rand"
   "crypto/rsa"
   "crypto/x509"
   "encoding/pem"
   "fmt"
   "io/ioutil"
)
//签名
func rsaSignData(src []byte,file string) []byte  {
   hasher:=sha256.New()
   _,err:=hasher.Write(src)
   if err!=nil{
      panic(err)
   }
   hash:=hasher.Sum(nil)
   buffer,err:=ioutil.ReadFile(file)
   if err!=nil{
      panic(err)
   }
   block,_:=pem.Decode(buffer)
   berText:=block.Bytes
   privateKey,err:=x509.ParsePKCS1PrivateKey(berText)
   if err!=nil{
      panic(err)
   }
   signature,err:=rsa.SignPKCS1v15(rand.Reader,privateKey,crypto.SHA256,hash[:])
   if err!=nil{
      panic(err)
   }
   return signature
}
//校验
func verSignData(src,sig []byte,file string) bool  {
   hasher:=sha256.New()
   _,err:=hasher.Write(src)
   if err!=nil{
      panic(err)
   }
   hash:=hasher.Sum(nil)
   buffer,err:=ioutil.ReadFile(file)
   if err!=nil{
      panic(err)
   }
   block,_:=pem.Decode(buffer)
   berText:=block.Bytes
   publicKey,err:=x509.ParsePKCS1PublicKey(berText)
   if err!=nil{
      panic(err)
   }
   err=rsa.VerifyPKCS1v15(publicKey,crypto.SHA256,hash[:],sig)
   if err!=nil{
      return false
   }
   return true
}

func main() {
   src:=[]byte("日照香炉生紫烟")
   sig:=rsaSignData(src,"rsa_private_key.pem")
   fmt.Printf("签名数据:%x\n",src)
   fmt.Printf("签名后数据:%x\n",sig)
   verifyStatus:=verSignData(src,sig,"rsa_public_key.pem")
   fmt.Printf("校验结果：%t",verifyStatus)
}
```

## 2.ecc数字签名

