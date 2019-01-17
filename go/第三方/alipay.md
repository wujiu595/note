**支付宝支付**

# 1. **支付宝开放平台登录**

使用已有的支付宝账号即可

<https://open.alipay.com/platform/home.htm>

 

# 2.  **关于沙箱环境（开发模拟环境）**

![img](file:////tmp/wps-wujiu/ksohtml/wpstRnGUt.jpg) 

<https://docs.open.alipay.com/200/105311>

 

登录后，在页面顶部可以选择进入沙箱环境设置页面

![img](file:////tmp/wps-wujiu/ksohtml/wpss7bTKi.jpg) 

# 3. **支付宝开发者文档**

<https://openhome.alipay.com/developmentDocument.htm>

电脑网站支付

<https://docs.open.alipay.com/270>

 

# 4. **电脑网站支付流程**

![img](file:////tmp/wps-wujiu/ksohtml/wpsHrVdB7.png) 

![img](file:////tmp/wps-wujiu/ksohtml/wps6nAQrW.jpg) 

 

# 5. **使用go工具包**

https://github.com/smartwalle/alipay

**安装**

go get github.com/smartwalle/alipay

**生成密秘钥文件**

openssl

OpenSSL> genrsa -out app_private_key.pem   2048  # 私钥

OpenSSL> rsa -in app_private_key.pem -pubout -out app_public_key.pem # 导出公钥

OpenSSL> exit

 

cat app_publict_key.pem 查看公钥的内容

![img](file:////tmp/wps-wujiu/ksohtml/wpstTjyiL.jpg) 

将-----BEGIN PUBLIC KEY-----和-----END PUBLIC KEY-----中间的内容保存在支付宝的用户配置中（沙箱或者正式）

<https://openhome.alipay.com/platform/appDaily.htm?tab=info>

 

![img](file:////tmp/wps-wujiu/ksohtml/wpsLH5i9z.jpg) 

![img](file:////tmp/wps-wujiu/ksohtml/wpssMP5Zo.jpg) 

![img](file:////tmp/wps-wujiu/ksohtml/wpsbRDUQd.jpg) 

 

 

下载支付宝的公钥文件

![img](file:////tmp/wps-wujiu/ksohtml/wpsTBNLH2.jpg) 

![img](file:////tmp/wps-wujiu/ksohtml/wpsx5SEyR.jpg) 

支付代码：

var privateKey = "MIIEogIBAAKCAQEAuVkA0yyrqrWay/pjywY6ev6/+IvutzEuIckAQDXxVlU349ED" +

​	"9Oqfi3gxbDYBmXg8bvUleSvuXFSuRagaEFp5XuvaS4Lh0D9ReWwbSgnAlChWFRde"+

​	"o+zDl/nDmStmd5pmmlvJgi04+p2orNWBnsADYi25Jq1Hr006zz2k9GZzj8W6GMFx"+

​	"URFKYrXPaexlkM/4oY3Bdnjf4dzyduLGGhLb7JhsCwHdKY7LGnqI/3ipK5HCvUoi"+

​	"O/0QwN3SgV7u23DU8E+ewK0Yd6ncTB408Pw9kp5RXK3QBPN/P861+/aF9pZP7iZG" +

​	"lZBAvq892VDkPTv7CxRPe3VDyYLiT31rqNgJ9wIDAQABAoIBAEp7WbmC2flfwTT3"+

​	"CeMsPZPvi3V1xhgXy1IIU/F5u+HVnQAPnmdtYW/KfRHfIgVqK97q5IQtAzxXSzDy"+

​	"vIaV1PAwFykBD31/9F2288Bs6tae3vjay01Ud8U6uT90EVk+0rx05iVJxvvvHzlV" +

​	"EyGYR8PMW/sO5x+rCVb+jqsoAIhlXbDcocpyPZpdO07n6mIVhkFkLFElM+2awoAD" +

​	"1fjtpSGUa/fAoUtBHxcrnSgS7byo4xsszX2RadtEQ4DDVw3NsS+9ClFLJAAa5HHC" +

​	"Dj1Wq7jHxSKqMu9cEXBoZfZpTcOnQdlT9QGAwjuYOl0IA1Yq6jqP2myTSJspR/M0" +

​	"oSqa5wECgYEA2ri2ZbVdiErlh1GiLpLVhND2mxsKGjJZr9+8GckUwZbSKj/qX5G4" +

​	"LQaODwFQgZlc+mh0JYdi1XpXwXTfBuzmjS8ckHhTEdwl/4GZXjXTQlmoA5TW4UwR" +

​	"ARZXNYUS6NKDMycLwLOXzMcYcrr+/ENqEakrlfTgndZ4aAhQttbVqBsCgYEA2PAe" +

​	"JkpSUa3LGBL4eNbJOWyjxRmhHTiL0CwV+S3O109ODqlgCNTINVOWHbqASGRCyEq6" +

​	"OAQ+ZcRdVH/V1liQguFpPM1GiIUhNpBJvOtTghIzmjMBVl3pBGrZRHEkRetCB08i" +

​	"eJwVCLAZiPe5y6/1/YxFNzo9CQlDXvBsRmcLO1UCgYAbke8D8RGiLXazUPn1jvK1" +

​	"NDXxpT3nwXMGtVgbk/o20NEbHEVp2I9ztYDQqWTBgVh0BBin5mHx8OMA8r9uOwxw" +

​	"vYCHQXOPK8XqaCax9mzzzyNbmDZh3dnC3lMN3wFcMbTyDLjxiHHZETumsqWTnNfQ" +

​	"9BcXZu+tVayFyI6MDZaPAwKBgAnCI1kNls5dxvj6QXsODlcq7+L52Cl8Va2zjfl7" +

​	"egZtZtF6BPvgtnDPpb0ImqSm/eoMknvalQP25UvbxD60FSwN/7Hgef/CHVBbBTYe" +

​	if err != nil {

​	"T89WCkQxbyn3Z3fvZn6RqFQM1ReHE8HhI4EUitGCczUaYTJakPJ/CCfT5tfqcLRq" +

​	"BjNRAoGAeO6/oTQaEaerBOM3HfJp1TV55As11xY+yZtzGhsGcywvBg7EZYWCm1z0" +

​	"cw3oLsaVbFlXLLJjcjsLT63Llh3+Ua9m1J1XRPV2SIxCSRQHF1xeyaCE5d/VzOgF" +

​	"GBNcPN71at6g+0S6p119eBNtVl6d43R+WjjPPfwheieeS1KyIec="

 

​	var appId = "2016092200569649"

​	var aliPublicKey = "MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAtPLfffeuLcVVBAZmiQuA7BtFGv7GKG6mWP7P+r9/koOTsICX6PObhGZwSR1BYtJhgcdimRI3UBBxyR3P4Ay7egpcconLuyxqZYNfohfVRL48MfIyS7cHDdNkjz2r70gOLfjYwchM6ttkzftME0k4QLJf/Y+qbSCiWvZ+9YRFmHo9Iq8juKDbnYkYmhoq7LDUxwVh7k9JeYW20kTIJecfNutCWGOcAC01jFymbNglrne8cUWet+qgY2WhGwEK1+2r1lWu+0azsNPPF3i3vVPAH1F2yxz6njhU26zO7A6+sB5Ff4DiULh3UAH9yID6LKJNBVJTpKobwidhFqk3ip5UqQIDAQAB"

 

​	var client = alipay.New(appId, aliPublicKey, privateKey, false)

 

​	//alipay.trade.page.pay

​	var p = alipay.AliPayTradePagePay{}

​	p.NotifyURL = "http://192.168.110.81:8080/user/payOk"

​	p.ReturnURL = "http://192.168.110.81:8080/user/payOk"

​	p.Subject = "天天生鲜"

​	p.OutTradeNo = "1234567811"

​	p.TotalAmount = "1000.00"

​	p.ProductCode = "FAST_INSTANT_TRADE_PAY"

 

​	var url, err = client.TradePagePay(p)

​	if err != nil {

​		fmt.Println(err)

​	}

 

​	var payURL = url.String()

 

​	this.Redirect(payURL,302)

 

支付成功处理：

 