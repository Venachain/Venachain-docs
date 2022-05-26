# 用户操作 account

针对账户地址的相关操作。

## 用户注册 account add

**描述**

对拥有的账户进行用户注册（User），审核通过的账户地址及其个人信息将被记录在用户平台。

**参数**

-   必选参数:

``` bash
<account>:       用户账户地址
<name>:          用户名
```

-   可选参数:

``` bash
--tel:                      用户电话信息
--email:                    用户邮箱信息
--organization string:      用户所属机构
```

**操作**

``` bash
./vcl account add "0xb239401ecf8427f17c6de134d6a6bddd3100251f" "Alice" --phone "13111111111" --email "alice@wx.bc.com" --organization wxbc --keyfile ../conf/keyfile.json
```

**输出结果**

``` json
{
  "status": "Operation Succeeded",
  "logs": [
      "Event addUser: 0 Success "
  ],
  "blockNumber": 227,
  "GasUsed": 113404,
  "From": "0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18",
  "To": "0x1000000000000000000000000000000000000001",
  "TxHash": ""
}
```

## 用户信息更新 account update

**描述**

更新用户的电话、邮箱等相关信息，普通用户（无角色/无权限用户无法修改用户的信息，仅管理员账户可操作。

**参数**

-   必选参数:

``` bash
<address>:     （选择进行更新的）用户账户地址
```

-   可选参数:

``` bash
--phone <number>:         用户电话信息（更新）
--email string:           用户邮箱信息（更新）
--organization string:    用户所属机构（更新）
```

**操作**

``` bash
# optional flags:
## 修改用户电话
./vcl account update "0xb239401ecf8427f17c6de134d6a6bddd3100251f" --phone "13241231233" --keyfile ../conf/keyfile.json

## 修改用户邮箱
./vcl account update "0xb239401ecf8427f17c6de134d6a6bddd3100251f" --email "123@qq.com" --keyfile ../conf/keyfile.json

## 修改用户所属机构
./vcl account update "0xb239401ecf8427f17c6de134d6a6bddd3100251f" --organization "wxbc" --keyfile ../conf/keyfile.json
```

**输出结果**

``` json
{
  "status": "Operation Succeeded",
  "logs": [
      "Event updateUserDescInfo: 0 Success "
  ],
  "blockNumber": 228,
  "GasUsed": 110548,
  "From": "0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18",
  "To": "0x1000000000000000000000000000000000000001",
  "TxHash": ""
}
```

## 用户信息查询 account query

**描述**

根据查询键值以及辅助选项进行信息的筛选查询，返回所有匹配成功的数据对象

**参数**

-   可选参数: 用户信息查询，用于用户信息更新。

``` bash
--user:            查询键，通过用户账户地址或账户名称进行查询（返回结果唯一）
--all:             查询全部用户
```

**操作**

用户信息和用户角色信息分别来自不同系统合约的存储中，重构后我们把用户信息与角色信息在内部进行关联后再反馈给用户。

``` bash
# 1 通过用户账户地址查询用户信息
./vcl account query --user "0xb239401ecf8427f17c6de134d6a6bddd3100251f" --keyfile ../conf/keyfile.json

# 2 通过用户账户名查询用户信息
./vcl account query --user "Alice" --keyfile ../conf/keyfile.json
```

**输出结果**

``` json
{
  "address":"0xb239401ecf8427f17c6de134d6a6bddd3100251f",
  "authorizer":"0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18",
  "name":"Alice"
}
```
