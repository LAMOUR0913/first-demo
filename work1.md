## 1.完善solidity代码与生成的go代码

```
pragma solidity ^0.6.0;
contract UserManager {
    

    address payable public owner;
    
    mapping(uint8 => string) accounts;    //id -> password    通过 ID查密码 
    mapping(uint8 => address) ips;       //IP address       通过 IP查地址 
    
    event Login(uint8 id, uint time);
    event Register(uint8 id,string password);
    event SetPassword(string password,uint time);
    
    constructor () public {
        owner = msg.sender;
    }
    
    function login(uint8 id, string memory password) public  returns (bool) {//登录 
        require(ips[id] == msg.sender);
        if (equal(accounts[id],password)) {
            emit Login(id, now);
            return true; 
        }
        return false;
    }
    
    function getIP(uint8 id) public view returns (address) {
        require(ips[id] != address(0));
        return ips[id];
    }
    
    function register(uint8 id,string memory password) public returns (bool) {//注册  1.用户是否存在   2.密码设定  
        if(equal(accounts[id],"")){
        accounts[id]=password;
        emit Register(id,password);
        return true;
    }
        return false;
    }
    
    function setPassword(uint8 id,string memory oldpassword,string memory password) public returns (bool) {//修改密码  
    // if(equal(accounts[id],password)){
    //     accounts[id]=password;
    //             emit SetPassword(password,now);
    //     return true;
    // }
    //     return false;
    if((equal(accounts[id],oldpassword))&&(equal(accounts[id],""))){
        //1.判断用户是否存在  2.判断密码是否匹配
             accounts[id]=password;
             //3.修改密码 
             emit SetPassword(password,now);
        return true;
        }
    return false;
    }
     function equal(string memory a,string memory b)  public returns (bool){//比较 
        require(keccak256(abi.encodePacked(a))==keccak256(abi.encodePacked(b)));
        
    }

  }
```



#### 心得：

学到了solidity中比较的方式及特有的编写风格

掌握了各种类型之间的转换

