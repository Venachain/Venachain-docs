# 角色权限操作 role

针对角色权限的相关操作

## 设置超级管理员权限 role setSuperAdmin

```{warning}
只能设置一次
```

**描述**

链部署后可以调用该方法将当前账户进行超级管理员权限设置。

**操作**

``` bash
./vcl role setSuperAdmin  --keyfile ../conf/keyfile.json
```

**输出结果**

``` json
{
  "status": "Operation Succeeded",
  "logs": [
      "Event setSuperAdmin: Set SuperAdmin Succeed "
  ],
  "blockNumber": 2,
  "GasUsed": 102184,
  "From": "0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18",
  "To": "0x1000000000000000000000000000000000000001",
  "TxHash": ""
}
```

## 转移超级管理员权限 role transferSuperAdmin

**描述**

转移超级管理员权限（调用者需为当前的超级管理员）。

**参数**

-   必选参数:

``` bash
<address>: 转移后的超级管理员地址
```

**操作**

``` bash
./vcl role transferSuperAdmin "0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18"  --keyfile ../conf/keyfile.json
```

**输出结果**

``` json
{
  "status": "Operation Succeeded",
  "logs": [
      "Event setSuperAdmin: Set SuperAdmin Succeed "
  ],
  "blockNumber": 2,
  "GasUsed": 102184,
  "From": "0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18",
  "To": "0x1000000000000000000000000000000000000001",
  "TxHash": ""
}
```

## 角色添加 role addXXX

**描述**

为某个账户地址添加指定角色的权限。

**参数**

-   必选参数:

``` bash
<address>: 被赋予角色权限的账户地址
```

**操作**

``` bash
#链管理员
./vcl role addChainAdmin 0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18  --keyfile ../conf/keyfile.json
#群组管理员
./vcl role addGroupAdmin 0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18  --keyfile ../conf/keyfile.json
#节点管理员
./vcl role addNodeAdmin 0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18  --keyfile ../conf/keyfile.json
#合约管理员
./vcl role addContractAdmin 0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18  --keyfile ../conf/keyfile.json
#普通合约部署者
./vcl role addContractDeployer 0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18  --keyfile ../conf/keyfile.json
```

**输出结果**

``` json
{
  "status": "Operation Succeeded",
  "logs": [
      "Event addGroupAdminByAddress: 0 Success "
  ],
  "blockNumber": 197,
  "GasUsed": 105788,
  "From": "0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18",
  "To": "0x1000000000000000000000000000000000000001",
  "TxHash": ""
}
```

## 角色删除 role delXXX

**描述**

为某个账户地址删除指定角色的权限。

**参数**

-   必选参数:

``` bash
<address>: 被赋予角色权限的账户地址
```

**操作**

``` bash
#链管理员
./vcl role delChainAdmin 0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18  --keyfile ../conf/keyfile.json
#群组管理员
./vcl role delGroupAdmin 0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18  --keyfile ../conf/keyfile.json
#节点管理员
./vcl role delNodeAdmin 0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18  --keyfile ../conf/keyfile.json
#合约管理员
./vcl role delContractAdmin 0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18  --keyfile ../conf/keyfile.json
#普通合约部署者
./vcl role delContractDeployer 0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18  --keyfile ../conf/keyfile.json
```

**输出结果**

``` json
{
  "status": "Operation Succeeded",
  "logs": [
      "Event delGroupAdminByAddress: 0 Success "
  ],
  "blockNumber": 198,
  "GasUsed": 105788,
  "From": "0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18",
  "To": "0x1000000000000000000000000000000000000001",
  "TxHash": ""
}
```

## 获取权限地址列表 role getAddrListOfRole

**描述**

获取权限地址列表。

**参数**

-   必选参数:

``` bash
<role>: 角色可以且只能为"SUPER_ADMIN", "CHAIN_ADMIN", "GROUP_ADMIN", "NODE_ADMIN", "CONTRACT_ADMIN", "CONTRACT_DEPLOYER"其中之一
```

**操作**

``` bash
#以SUPER_ADMIN为例
./vcl role getAddrListOfRole "SUPER_ADMIN"  --keyfile ../conf/keyfile.json
```

**输出结果**

``` console
# 以SUPER_ADMIN为例
["0x10ad2ec4831a1f89ec870a3224fead87cdb75931"]
```

## 权限检查 role hasRole

**描述**

检查某账户地址是否拥有指定用户权限。

**参数**

-   必选参数:

``` bash
<address>: 待检查账户地址
<role>: 角色可以且只能为"SUPER_ADMIN", "CHAIN_ADMIN", "GROUP_ADMIN", "NODE_ADMIN", "CONTRACT_ADMIN", "CONTRACT_DEPLOYER"其中之一
```

**操作**

``` bash
#以SUPER_ADMIN为例
./vcl role hasRole 0x10ad2ec4831a1f89ec870a3224fead87cdb75931 SUPER_ADMIN  --keyfile ../conf/keyfile.json
```

**输出结果**

``` console
# 以SUPER_ADMIN为例
# 有权限
result: 1
# 无权限
result: 0
```

## 权限获取 role getRoles

**描述**

获取某账户地址用户权限。

**参数**

-   必选参数:

``` bash
<address>: 待检查账户地址
```

**操作**

``` bash
#以SUPER_ADMIN为例
./vcl role getRoles 0x10ad2ec4831a1f89ec870a3224fead87cdb75931  --keyfile ../conf/keyfile.json
```

**输出结果**

``` console
["SUPER_ADMIN"]
```
