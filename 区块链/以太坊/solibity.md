# solidity

contract 声明合约

function a() view returns(uint,uint){

}普通方法声明

条件

if(count<256){

}

循环

for(;i<257;i++){

​	count++;

}

mapping(address=>uint256) public personToMoney

构造函数

func Inbox() payable{

}

合约的余额

return address(this).balance                                                                                                   



全局变量

​                              msg.sender 发送交易的账户

1.合约的部署人

2.也是合约函数的调用者



总结:msg.sender它就是一个全局变量,它存储的是当前这笔交易的调用者

```js
pragma solidity ^0.4.24;


contract test{

	string public message;
	address public manager;
	address public caller;
	function Inbox() public{
		caller=msg.sender;      //合约的创建者
	}
	
    function setMessage(string newMessage) public{
        manager= msg.sender;
        message=newMessage;
    }
    
    function getMessage() public view returns(string){
    	return  message;
    }

}
```

如果抛出异常,会回滚msg.sender

msg.value

每一笔交易都可以写到一笔钱给合约

```js
pragma solidity ^0.4.24;

contract test{
    mapping(address=>uint256) public personToMoney;
    function invest() public payable{
        if (msg.value!=10 ether){
            throw;
        }
        personToMoney[msg.sender]=msg.value
    }
    function getBanlance()public{
        return address(this).balance
    }
}
```



错误处理

1.throw

2.

require(msg.sender==ower)



事件



```js
pragma solidity ^0.4.24;

contract test{
    event Deposit(
        address _from,
        uint _value
        );
    function deposit()payable public{
        emit Deposit(msg.sender,msg.value);
    }    
}
```



访问函数



