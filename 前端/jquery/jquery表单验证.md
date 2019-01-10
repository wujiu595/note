# jquery表单验证

### jquery正则使用

- 使用一对 ''/''声明一个正则表达式对象
- 使用test()方法验证正则,返回bool值

```javascript
 var ret = /^[\w-]+(\.[\w-]+)*@[\w-]+(\.[\w-]+)+$/;
 if(ret.test(str)){
     alert('ok');
 else{
      alert('wrong');

 }
```

### jquery验证表单内容

- 表单内的内容都是通过 $("元素").val()获取值

```javascript
$(".div").val()
```

- 获取后验证数据

```javascript
var value = $(".div").val()
if `验证方法`(value){
    `处理逻辑`
}
```

- 展示处理内容

可以在内容位置输入内容框,通过隐藏和显示内容输出验证信息

```javascript
$pwd.next().html('密码不能为空！').show();
```



### 完整逻辑梳理

```javascript
//定义错误内容 默认为有错
var phoneError = true
var passWordError = true
//校验telError和passWord
$(.phone).blur(function(){
 	   testPhone()
}).click(function(){
    $(.telError).next().hide()
})

function testPhone(){
    var phone=$(.phone).val()
    ret = /^1[34578]\d{9}$/
    if (val==""){
        phoneError = true
        $(.telError).next().html("手机号码不能为空").show()
        return
    }else if !ret.test(phone){
        phoneError = true
         $(.telError).next().html("手机号格式不正确").show()
        return
    }else{
        phoneError = false
        return
    }
}

$(.passWord).blur(function(){
 	   testPassWord()
}).click(function(){
    $(.telError).next().hide()
})

function testPassWord(){
    var passWord=$(.passWord).val()
    var pattern = /^.*(?=.{6,16})(?=.*\d)(?=.*[A-Z]{2,})(?=.*[a-z]{2,})(?=.*[!@#$%^&*?\(\)]).*$/;
    if !pattern.test(){
        passWordError = true
        $(.telError).next().html("密码格式不正确").show()
        return
    }else{
        passWordError = false
        return
    }
}

$("form").submit(function(){
    if passWordError||phoneError{
        alert("")
        return false
    }
    return true
})

```



