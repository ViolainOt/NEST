- [借贷工厂协议_NPC-1](#借贷工厂协议_NPC-1)
- [矿池存储协议_NPC-2](#矿池存储协议_NPC-2)


### 借贷工厂协议_NPC-1
***

#### 协议属性
属性 | 类型 | 功能 | 描述
---|---|---|---
dataContract | NEST_ToLoanDataContract | 数据协议对象 | 协议内部逻辑中使用数据协议
mappingContract | IBMapping | 映射合约对象 | 合约内部逻辑中使用映射合约
loanTokenAddress | mapping(uint256 => address) | token类型映射 | 合约中支持的token类型
parameter | mapping(string => uint256) | 手续费比例参数映射 | 不同业务类型使用的手续费比例
mortgageRate | mapping(address => uint256) | token抵押率映射 | 不同token抵押率参数
priceCheck | NEST_PriceCheck | 借贷价格验证协议对象 | 协议内部逻辑中使用借贷价格验证协议
***

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
***

### 矿池存储协议_NPC-2
***

#### 协议属性
属性 | 类型 | 功能 | 描述
---|---|---|---
mappingContract | IBMapping | 保存映射合约对象 | 映射合约
nestContract | ERC20 | 保存NEST ERC20对象 | NEST Token
***

#### 协议方法
##### 初始化方法
> 1.初始化方法：constructor (address map) public

输入参数 | 类型 | 描述
---|---|---
map | address | 映射合约地址

初始化映射合约，从映射合约中获取 NEST token 地址，并实例化 NEST Token

##### 修改权限方法
> 1.修改映射合约：function changeMapping(address map) public onlyOwner

输入参数 | 类型 | 描述
---|---|---
map | address | 映射合约地址

重新初始化映射合约，从映射合约中获取 NEST token 地址，并实例化 NEST Token
***



- [Mortgage Loan Factory Protocol_NPC-1](#Mortgage-Loan-Factory-Protocol_NPC-1)

### Mortgage Loan Factory Protocol_NPC-1
***

#### Protocol Attributes
Attribute | Type | Function | Description
---|---|---|---
dataContract | NEST_ToLoanDataContract | Data protocol object | Data protocol used in the internal logic of the protocol
mappingContract | IBMapping | Mapping contract object | Mapping contract used in the internal logic of the contract
loanTokenAddress | mapping(uint256 => address) | Token type mapping | Token type that supported in the contract
parameter | mapping(string => uint256) | Handling fee proportion parameter mapping | Proportion of fees for different types of business
mortgageRate | mapping(address => uint256) | Token mortgage rate mapping | Mortgage rate of different token mortgage
priceCheck | NEST_PriceCheck | Loan price verification protocol object  | Loan price verification protocol used in the internal logic of the protocol
***

 

#### Protocol Methods
##### Initialization Methods
> 1. Initialization method: constructor(address map) public

Input Parameter | Type | Description
---|---|---
map | address | mapping contract address

Deploying contract, generate contract according to the input address, check mapping contract returns data
contract address, generate data contract object. Initialize handling fee parameter.

##### Modify Permission Methods
> 1. Modify mapping contract address: function changeMapping(address map) public onlyOwner

Input Parameter | Type | Description
---|---|---
map | address | mapping contract address

Modify mapping contract address, regenerate new mappingContract object, use new mapping contract in the
internal logic of the contract.

> 2. Add, delete, modify token type: function changeTokenAddress(uint256 num, address addr) public onlyOwner

Input Parameter | Type | Description
---|---|---
num | uint256 | token number
addr | address | token address

Setting corresponded number of token address, setting as 0x0000000000000000000000000000000000000000, consider deleting the token under the number.

> 3. Modify handling fee parameter: function changeParameter(string memory name, uint256 value) public onlyOwner

Input Parameter | Type | Description
---|---|---
name | string | parameter names，ETH- ERC20:"borroweCommission", ERC20- ETH:"lenderCommission"。
value | uint256 | parameter values，borroweCommission: 5, lenderCommission: 10, thousand points system

Setting handling fee proportion of corresponded business.

##### Borrower Methods
> 1.Deploy contract: function createContract(uint256 borrowerAmount, uint256 borrowerId, uint256 lenderAmount, uint256 lenderId, uint256 limitdays,uint256 interestRate) public

Input Parameter | Type | Description
---|---|---
borrowerAmount | uint256 | amount of mortgage assets
borrowerId | uint256 | id of mortgage assets
lenderAmount | uint256 | amount of lending assets
lenderId | uint256 | id of lending assets
limitdays | uint256 | loan cycle (day)
interestRate | uint256 | loan rate (per 10 thousand)

Deploying loan contract，and setting parameters.

> 2. Transfer in mortgage asset: function transferIntoMortgaged(address contractAddress) public payable

Input Parameter | Type | Description
---|---|---
contractAddress | address | loan contract address

Execute transfer in mortgage asset operation.

> 3. Repayment: function sendRepayment(address contractAddress) public payable

Input Parameter | Type | Description
---|---|---
contractAddress | address | loan contract address

Execute repayment operation.

##### Investor Methods
> 1. Investment: function investmentContracts(address contractAddress) public payable

Input parameter | Type | Description
---|---|---
contractAddress | address | loan contract address

Execute investment operation.

##### External Methods (public configuration methods)
> 1. Check responded id of token address: function checkToken(uint256 num) public view returns (address)

Input Parameter | Type | Description
---|---|---
num | uint256 | check id of token

Output Parameter | Type | Description
---|---|---
--- | address | token address

Check responded id of token address.

> 2. Check handling fee proportion parameter: function checkParameter(string memory name) public view returns(uint256)

Input Parameter | Input Parameter | Description
---|---|---
name | string | key of check parameter

Output Parameter | Type | Description
---|---|---
--- | uint256 | value of parameter

Check handling fee proportion parameter.
***

