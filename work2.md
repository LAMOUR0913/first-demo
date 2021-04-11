# 完善数字身份智能合约

```
pragma solidity ^0.6.0;
pragma experimental ABIEncoderV2;

contract PersonDID {
    struct Person {
        uint8 id;
        uint8 age;
        string name;
        string info;//个人经历 
    }
    modifier Onlyadmin(){//管理员 
        require(
           isPersonExsist[msg.sender] == true
            ,"You don't have right to do that!");
        _;
    }
    event AddPerson(uint8 id, uint8 age, string name, uint timestamp);
    event Addinfo(uint8 id,string info,uint timestamp); //个人经历
    event AddManagerExist(uint8 id,uint8 priviledge,uint timestamp);//权限 
    Person admin;
    Person[] persons;
    mapping(uint => mapping(uint8 => bool)) public Priviledge;//通过映射方式来实现权限标记
    mapping(address => Person) public PersonInfo;//映射个人信息 
    mapping(address=> bool) public isPersonExsist;
    mapping(address=>bool) public isManagerExist;//映射权限 
    

    constructor (address ip, uint8 id, string memory name, uint8 age) public {
        admin = Person(id, age, name,"");
        Person memory p = Person(id, age, name,"");
        persons.push(p);
        PersonInfo[msg.sender] = p;
        isPersonExsist[msg.sender] = true;
        isManagerExist[msg.sender]=false;
    }
    
    function getNumberOfPersons() view public returns (uint256) {
        return persons.length;
    }
    //管理员添加人员信息 
    function addPerson(uint8 id, uint8 age, string memory name,string memory info)Onlyadmin public returns (bool) {
        require(!((id == 0) || age == 0), "persons info can not be empty!!");
        require(!isPersonExsist[msg.sender], "person can not exsist !!");
        Person memory person = Person(id, age, name,"");
        persons.push(person);
        PersonInfo[msg.sender] = Person(id, age, name,"");
        isPersonExsist[msg.sender] = true;
        isManagerExist[msg.sender] = false;
        emit AddPerson(id, age, name, now);
        emit Addinfo(id,info,now);
        return true;
        
    }
    
    function setPersonAgeSto(address ip, uint8 age) public {
        Person storage p = PersonInfo[ip];
        p.age = age;
    }
    
    function setPersonAgeMem(address ip, uint8 age) public {
        Person memory p = PersonInfo[ip];
        p.age = age;
    }
    
    function getPersonAge(address ip) public view returns (uint8) {
        return PersonInfo[ip].age;
    }
    //公开给某用户查看
    function viewPower(uint8 ip,uint8 priviledge,uint8 count) public returns (bool){
        Priviledge[ip][priviledge]=true;
        count=count;
        emit AddManagerExist(ip,priviledge,now);
        return true;
        
    }
    //查看某用户的信息 
    function visitPower(uint8 ip,uint8 priviledge,uint8 count) public returns (bool){
        require(Priviledge[ip][priviledge],"You do not have permission to view the information of other users！！！");
        require(count!=0,"Your permission to view other users’ information has been used up！！！");
        count--;
    }


​    
}
```


