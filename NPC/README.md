# Contents  
- [借贷工厂协议](#借贷工厂协议_NPC-1)  


### 借贷工厂协议_NPC-1(#借贷工厂协议_NPC-1)
#### 协议属性
属性 | 类型 | 功能 | 描述
---|---|---|---
dataContract | NEST_ToLoanDataContract | 数据协议对象 | 协议内部逻辑中使用数据协议
mappingContract | IBMapping | 映射合约对象 | 合约内部逻辑中使用映射合约
loanTokenAddress | mapping(uint256 => address) | token类型映射 | 合约中支持的token类型
parameter | mapping(string => uint256) | 手续费比例参数映射 | 不同业务类型使用的手续费比例
mortgageRate | mapping(address => uint256) | token抵押率映射 | 不同token抵押率参数
priceCheck | NEST_PriceCheck | 借贷价格验证协议对象 | 协议内部逻辑中使用借贷价格验证协议
#### 协议方法
##### 初始化方法
> 1.初始化方法：constructor(address map) public

输入参数 | 类型 | 描述
---|---|---
map | address | 映射合约地址

部署合约，根据输入地址生成映射合约，查询映射合约返回数据合约地址，生成数据合约对象。初始化手续费参数。
##### 修改权限方法
> 1.修改映射合约地址：function changeMapping(address map) public onlyOwner

输入参数 | 类型 | 描述
---|---|---
map | address | 映射合约地址

修改映射合约地址，重新生成 mappingContract 对象，合约内部逻辑使用新的映射合约

> 2.添加、删除、修改token类型：function changeTokenAddress(uint256 num, address addr) public onlyOwner

输入参数 | 类型 | 描述
---|---|---
num | uint256 | token编号
addr | address | token地址

设置对应编号的token地址，设置为0x0000000000000000000000000000000000000000，视为删除该编号下的token

> 3.修改手续费参数：function changeParameter(string memory name, uint256 value) public onlyOwner

输入参数 | 类型 | 描述
---|---|---
name | string | 参数名称，ETH-ERC20:"borroweCommission",ERC20-ETH:"lenderCommission"。
value | uint256 | 参数值，borroweCommission：5，lenderCommission：10。千分制

设置对应业务的手续费比例
##### 借币人方法
> 1.部署合约：function createContract(uint256 borrowerAmount, uint256 borrowerId, uint256 lenderAmount, uint256 lenderId, uint256 limitdays,uint256 interestRate) public

输入参数 | 类型 | 描述
---|---|---
borrowerAmount | uint256 | 抵押资产数量
borrowerId | uint256 | 抵押资产id
lenderAmount | uint256 | 借币资产数量
lenderId | uint256 | 借币资产id
limitdays | uint256 | 借贷周期（天）
interestRate | uint256 | 借贷利率（万分之）

部署借贷合约，并设置参数

> 2.转入抵押资产：function transferIntoMortgaged(address contractAddress) public payable

输入参数 | 类型 | 描述
---|---|---
contractAddress | address | 借贷合约地址

执行转入抵押资产操作

> 3.还款：function sendRepayment(address contractAddress) public payable

输入参数 | 类型 | 描述
---|---|---
contractAddress | address | 借贷合约地址

执行还款操作

##### 投资人方法
> 1.投资：function investmentContracts(address contractAddress) public payable

输入参数 | 类型 | 描述
---|---|---
contractAddress | address | 借贷合约地址

执行投资操作

##### 外部方法（公开调用方法）
> 1.查询对应id的token地址：function checkToken(uint256 num) public view returns (address)

输入参数 | 类型 | 描述
---|---|---
num | uint256 | 查询 token 的 id

输出参数 | 类型 | 描述
---|---|---
--- | address | token 地址

查询对应id的token地址

> 2.查询手续费比例参数：function checkParameter(string memory name) public view returns(uint256)

输入参数 | 类型 | 描述
---|---|---
name | string | 查询参数的 key 

输出参数 | 类型 | 描述
---|---|---
--- | uint256 | 参数的值

查询手续费比例参数
