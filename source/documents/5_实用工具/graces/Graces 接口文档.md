# Graces 接口文档

## 一、链相关信息

### 添加链信息 /api/chain

方法：POST

#### Description

添加链信息进 graces 平台

#### Parameters

| Name     | Located in | Description | Required | Schema                           |
| -------- | ---------- | ----------- | -------- | -------------------------------- |
| chainDTO | body       | 链信息      | Yes      | [model.ChainDTO](graces_api_chaindto) |

#### Responses

| Code | Description | Schema                       |
| ---- | ----------- | ---------------------------- |
| 200  | OK          | [model.Result](graces_api_result) |
| 400  | Bad Request | [model.Result](graces_api_result) |

示例

```shell
curl -X POST "http://127.0.0.1:9999/api/chain" -H "accept: application/json" -H "Content-Type: application/json" -d "{ \"desc\": \"Venachain\", \"ip\": \"127.0.0.1\", \"name\": \"test\", \"p2p_port\": 6791, \"ws_port\": 26791}"
```

响应

```json
{
  "code": 200,
  "msg": "success",
  "data": null,
  "request": "[POST] /api/chain"
}
```
### 按条件查询链信息 /api/chains

方法：POST

#### Description

按条件查询链信息

#### Parameters

| Name      | Located in | Description    | Required | Schema                                                 |
| --------- | ---------- | -------------- | -------- | ------------------------------------------------------ |
| condition | body       | 链信息查询条件 | Yes      | [model.ChainQueryCondition](graces_api_chainquerycondition) |

#### Responses

| Code | Description | Schema                                |
| ---- | ----------- | ------------------------------------- |
| 200  | OK          | [model.Result](graces_api_result) & object |
| 400  | Bad Request | [model.Result](graces_api_result)          |

示例

```shell
curl -X POST "http://127.0.0.1:9999/api/chains" -H "accept: application/json" -H "Content-Type: application/json" -d "{}"
```

响应: 此处返回所有链的详细信息

```js
{
"code": 200,
  "msg": "success",
  "data": {
    "page_index": 1,
    "page_size": 10,
    "total": 2,
    "items": [
      {
        "id": "6141a06437bf88afa7924f9c",
        "name": "test",
...
}
```

### 通过 id 查询链信息 /api/chain/id/{id}

方法：GET

#### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| id   | path       | id          | Yes      | string |

#### Responses

| Code | Description | Schema                                |
| ---- | ----------- | ------------------------------------- |
| 200  | OK          | [model.Result](graces_api_result) & object |
| 400  | Bad Request | [model.Result](graces_api_result)          |

示例

```shell
curl -X GET "http://127.0.0.1:9999/api/chain/id/6141a06437bf88afa7924f9c" -H "accept: application/json"
```

响应

```js
// 返回链的详细信息
{
  "code": 200,
  "msg": "success",
  "data": {
    "id": "6141a06437bf88afa7924f9c",
    "name": "test",
    "username": "",
    "ip": "127.0.0.1",
    "p2p_port": 6791,
    "ws_port": 26791,
    "desc": "Venachain",
    ...
}
```

### 通过 name 查询链信息 /api/chain/name/{name}

#### GET


#### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| name | path       | name        | Yes      | string |

#### Responses

| Code | Description | Schema                                |
| ---- | ----------- | ------------------------------------- |
| 200  | OK          | [model.Result](graces_api_result) & object |
| 400  | Bad Request | [model.Result](graces_api_result)          |

示例

```shell
curl -X GET "http://127.0.0.1:9999/api/chain/name/test" -H "accept: application/json"
```

响应

```js
// 返回链的详细信息
{
  "code": 200,
  "msg": "success",
  "data": {
    "id": "6141a06437bf88afa7924f9c",
    "name": "test",
    "username": "",
    "ip": "127.0.0.1",
    "p2p_port": 6791,
    "ws_port": 26791,
    "desc": "Venachain",
    ...
}
```

### 开始链数据全量同步 /api/chain/fullsync/start/{chainid}

方法：GET

#### Description

开始全量同步链数据

#### Parameters

| Name    | Located in | Description | Required | Schema |
| ------- | ---------- | ----------- | -------- | ------ |
| chainid | path       | chainid     | Yes      | string |

#### Responses

| Code | Description | Schema                       |
| ---- | ----------- | ---------------------------- |
| 200  | OK          | [model.Result](graces_api_result) |
| 400  | Bad Request | [model.Result](graces_api_result) |

示例

```shell
curl -X GET "http://127.0.0.1:9999/api/chain/fullsync/start/6141a06437bf88afa7924f9c" -H "accept: application/json"
```

响应

```json
{
  "code": 200,
  "msg": "success",
  "data": "6141a06437bf88afa7924f9c",
  "request": "[GET] /api/chain/fullsync/start/6141a06437bf88afa7924f9c"
}
```
### 开始链数据增量同步 /api/chain/incrsync/start/{chainid}

方法：GET

#### Description

开始增量同步链数据

#### Parameters

| Name    | Located in | Description | Required | Schema |
| ------- | ---------- | ----------- | -------- | ------ |
| chainid | path       | chainid     | Yes      | string |

#### Responses

| Code | Description | Schema                       |
| ---- | ----------- | ---------------------------- |
| 200  | OK          | [model.Result](graces_api_result) |
| 400  | Bad Request | [model.Result](graces_api_result) |

示例

```shell
curl -X GET "http://127.0.0.1:9999/api/chain/incrsync/start/6141a06437bf88afa7924f9c" -H "accept: application/json"
```

响应

```json
{
  "code": 200,
  "msg": "success",
  "data": "6141a06437bf88afa7924f9c",
  "request": "[GET] /api/chain/incrsync/start/6141a06437bf88afa7924f9c"
}
```

### 链数据同步信息 /api/chain/sync/info/{chainid}

方法：GET

#### Description

查询链数据同步信息

#### Parameters

| Name    | Located in | Description | Required | Schema |
| ------- | ---------- | ----------- | -------- | ------ |
| chainid | path       | chainid     | Yes      | string |

#### Responses

| Code | Description | Schema                                |
| ---- | ----------- | ------------------------------------- |
| 200  | OK          | [model.Result](graces_api_result) & object |
| 400  | Bad Request | [model.Result](graces_api_result)          |

示例

```shell
curl -X GET "http://127.0.0.1:9999/api/chain/sync/info/614167f48a52a5322531a154" -H "accept: application/json"
```

响应

```js
// 返回区块、交易、cns、节点的同步情况，是否同步完成
{
  "code": 200,
  "msg": "success",
  "data": {
    "chain_id": "614167f48a52a5322531a154",
    "status": "success",
    "start_time": "2021-09-15 15:08:44",
    "estimate_complete_time": "2021-09-15 17:52:12",
    "err_msg": "",
    "NodeDataSyncInfoVO": {
    },
    "block_data_sync_info": {
    },
    "cns_data_sync_info_vo": {
    }
  },
}
```

### 获取当前链上交易、区块、合约的总量/api/chain/stats/{chainid}

方法：GET

#### Description

链数据查询

#### Parameters

| Name    | Located in | Description | Required | Schema |
| ------- | ---------- | ----------- | -------- | ------ |
| chainid | path       | chainid     | Yes      | string |

#### Responses

| Code | Description | Schema                       |
| ---- | ----------- | ---------------------------- |
| 200  | OK          | [model.Result](graces_api_result) |
| 400  | Bad Request | [model.Result](graces_api_result) |

示例

```shell
curl -X GET "http://127.0.0.1:9999/api/chain/stats/614167f48a52a5322531a154" -H "accept: application/json"
```

响应

```js
// 返回当前链区块的高度、交易数量、合约数量和节点的个数
{
  "code": 200,
  "msg": "success",
  "data": {
    "latest_height": 8,
    "total_tx": 11,
    "total_contract": 0,
    "total_node": 4
  },
  "request": "[GET] /api/chain/stats/614167f48a52a5322531a154"
}
```

### 获取当前账户的系统参数 /api/chain/getsystemconfig/{id}

方法：GET

#### Description

获取系统参数

#### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| id   | path       | chain ID    | Yes      | string |

#### Responses

| Code | Description | Schema                                |
| ---- | ----------- | ------------------------------------- |
| 200  | OK          | [model.Result](graces_api_result) & object |
| 400  | Bad Request | [model.Result](graces_api_result)          |

示例

```shell
curl -X GET "http://127.0.0.1:9999/api/chain/getsystemconfig/614167f48a52a5322531a154" -H "accept: application/json"
```

响应

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "chainID": "614167f48a52a5322531a154",
    "blockGasLimit": "10000000000",
    "txGasLimit": "1500000000",
    "isUseGas": "",
    "isApproveDeployedContract": "0",
    "isCheckDeployPermission": "0",
    "isProduceEmptyBlock": "0",
    "gasContractName": "0"
  },
  "request": "[GET] /api/chain/getsystemconfig/614167f48a52a5322531a154"
}
```

### 设置当前账户的系统参数 /api/chain/setsystemconfig

方法：POST

#### Description

设置系统参数

#### Parameters

| Name      | Located in | Description      | Required | Schema                                       |
| --------- | ---------- | ---------------- | -------- | -------------------------------------------- |
| condition | body       | 合约信息查询条件 | Yes      | [model.SystemConfigVO](graces_api_systemconfigvo) |

#### Responses

| Code | Description | Schema                       |
| ---- | ----------- | ---------------------------- |
| 200  | OK          | [model.Result](graces_api_result) |
| 400  | Bad Request | [model.Result](graces_api_result) |

示例

```shell
curl -X POST "http://127.0.0.1:9999/api/chain/setsystemconfig" -H "accept: application/json" -H "Content-Type: application/json" -d "{ \"chainID\": \"614167f48a52a5322531a154\", \"txGasLimit\": \"1999999999\"}"
```

响应

```json
{
  "code": 200,
  "msg": "success",
  "data": null,
  "request": "[POST] /api/chain/setsystemconfig"
}
```

### 为指定链部署智能合约 /api/chain/deploy/contract/:chainid

方法：POST

#### Description

部署智能合约

#### Parameters

| Name    | Located in | Description | Required | Schema |
| ------- | ---------- | ----------- | -------- | ------ |
| chainid | path       | chain ID    | Yes      | string |
| file    | path       |     file    | Yes      | string |
| file    | path       |     file    | Yes      | string |

#### Responses

| Code | Description | Schema                       |
| ---- | ----------- | ---------------------------- |
| 200  | OK          | [model.Result](graces_api_result) |
| 400  | Bad Request | [model.Result](graces_api_result) |

示例

```shell
curl -X POST 'http://127.0.0.1:9999/api/chain/deploy/contract/614586dae50dbd875a944f9f' -H "accept: application/json" -H 'Content-Type: multipart/form-data' -F 'file=@/Users/tmpatms/Desktop/wxblockchain/before 210714/tmp/contracts/appDemo/appDemo.cpp.abi.json' -F 'file=@/Users/tmpatms/Desktop/wxblockchain/before 210714/tmp/contracts/appDemo/appDemo.wasm'

```

响应

```json
{
  {
      "code":200,
      "msg":"success",
      "data":{
        "status":"Operation Succeeded",
        "contractAddress":"0x1ce958ca13b5d5ef1553f102490d171ed466f144",
        "blockNumber":5,
        "GasUsed":2579463,
        "From":"0xbfaab8846362f1998f770c74c98869a470b045d1",
        "To":"",
        "TxHash":"0xa271b12474c3f9e2402a0e935d0fb4f8bd5e0355e506cec6ef8585365b83fd25"
          
        },
      "request":"[POST] /api/chain/deploy/contract/614586dae50dbd875a944f9f"}
}
```

## 二、交易信息管理

### 获取7日内的交易总数 /api/chain/stats/tx/count/{chainid}

方法：GET

#### Description

链数据查询

#### Parameters

| Name    | Located in | Description | Required | Schema |
| ------- | ---------- | ----------- | -------- | ------ |
| chainid | path       | chainid     | Yes      | string |

#### Responses

| Code | Description | Schema                       |
| ---- | ----------- | ---------------------------- |
| 200  | OK          | [model.Result](graces_api_result) |
| 400  | Bad Request | [model.Result](graces_api_result) |

示例

```shell
curl -X GET "http://127.0.0.1:9999/api/chain/stats/tx/count/614167f48a52a5322531a154" -H "accept: application/json"
```

响应

```js
// 自请求日前7天的每日交易统计
{
  "code": 200,
  "msg": "success",
  "data": [
    {
      "date": "2021-09-14",
      "tx_amount": 11
    },
	...
  ],
  "request": "[GET] /api/chain/stats/tx/count/614167f48a52a5322531a154"
}
```

### 查询交易信息 /api/tx/hash

方法：POST

#### Description

通过 hash 查询交易信息

#### Parameters

| Name      | Located in | Description      | Required | Schema                                 |
| --------- | ---------- | ---------------- | -------- | -------------------------------------- |
| condition | body       | 交易信息查询条件 | Yes      | [model.TXByHashDTO](graces_api_txbyhashdto) |

#### Responses

| Code | Description | Schema                                |
| ---- | ----------- | ------------------------------------- |
| 200  | OK          | [model.Result](graces_api_result) & object |
| 400  | Bad Request | [model.Result](graces_api_result)          |

示例

```shell
curl -X POST "http://127.0.0.1:9999/api/tx/hash" -H "accept: application/json" -H "Content-Type: application/json" -d "{ \"chain_id\": \"614167f48a52a5322531a154\", \"hash\": \"string\"}"
```

响应

```
交易的详细信息
```

### 

### 查询交易信息 /api/tx/id/{id}

方法：GET

#### Description

通过 id 查询交易信息

#### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| id   | path       | id          | Yes      | string |

#### Responses

| Code | Description | Schema                                |
| ---- | ----------- | ------------------------------------- |
| 200  | OK          | [model.Result](graces_api_result) & object |
| 400  | Bad Request | [model.Result](graces_api_result)          |



### 查询交易信息 /api/txs

方法：POST

#### Description

按条件查询交易信息

#### Parameters

| Name      | Located in | Description      | Required | Schema                                           |
| --------- | ---------- | ---------------- | -------- | ------------------------------------------------ |
| condition | body       | 交易信息查询条件 | Yes      | [model.TXQueryCondition](graces_api_txquerycondition) |

#### Responses

| Code | Description | Schema                                |
| ---- | ----------- | ------------------------------------- |
| 200  | OK          | [model.Result](graces_api_result) & object |
| 400  | Bad Request | [model.Result](graces_api_result)          |

示例

```shell
curl -X POST "http://127.0.0.1:9999/api/txs" -H "accept: application/json" -H "Content-Type: application/json" -d "{}"
```

响应

```js
// 交易的详细信息
{
	...
    "items": [
      {
        "id": "6141a7f837bf88afa7925078",
        "chain_id": "6141a06437bf88afa7924f9c",
        "block_id": "6141a7f837bf88afa7925076",
        "hash": "0x7582e2a1c23de9eefc6c177fb660eff89f4e10f7062dea3d2bdff8e8e3cf004f",
        "height": 12,
        "timestamp": "2021-09-15 15:59:52",
        "from": "0x3FcaA0A86DFbbe105c7ed73Ca505c7a59c579667",
        "to": "0x1000000000000000000000000000000000000004",
        "gas_limit": 1999999999,
        "gas_price": 0,
        "nonce": "605110528694684",
        "input": "e08800000000000000028d73657454784761734c696d69748800000000773593ff",
        "value": 0,
        "receipt": {
				...
        },
        "is_to_contract": true,
        "detail": {
          "action": "InvokeContract",
          "contract": "paramManagerContract",
          "txtype": 2,
          "method": "setTxGasLimit",
          "params": [
            "no suitable type"
          ],
          "extra": null
        }
      },
}
```

### 查询属于合约调用的交易信息 /api/txs/contractcall

方法：POST

#### Description

按条件查询属于合约调用的交易信息

#### Parameters

| Name      | Located in | Description      | Required | Schema                                           |
| --------- | ---------- | ---------------- | -------- | ------------------------------------------------ |
| condition | body       | 交易信息查询条件 | Yes      | [model.TXQueryCondition](graces_api_txquerycondition) |

#### Responses

| Code | Description | Schema                                |
| ---- | ----------- | ------------------------------------- |
| 200  | OK          | [model.Result](graces_api_result) & object |
| 400  | Bad Request | [model.Result](graces_api_result)          |

示例

```shell
curl -X POST "http://127.0.0.1:9999/api/txs/contractcall" -H "accept: application/json" -H "Content-Type: application/json" -d "{}"
```

响应

```
// 交易的详细信息
{
  同上示例
}
```

## 三、区块信息管理

### 查询区块信息 /api/block/hash

方法：POST

#### Description

通过 hash 查询区块信息

#### Parameters

| Name      | Located in | Description      | Required | Schema                                       |
| --------- | ---------- | ---------------- | -------- | -------------------------------------------- |
| condition | body       | 区块信息查询条件 | Yes      | [model.BlockByHashDTO](graces_api_blockbyhashdto) |

#### Responses

| Code | Description | Schema                                |
| ---- | ----------- | ------------------------------------- |
| 200  | OK          | [model.Result](graces_api_result) & object |
| 400  | Bad Request | [model.Result](graces_api_result)          |

示例

```shell
curl -X POST "http://127.0.0.1:9999/api/block/hash" -H "accept: application/json" -H "Content-Type: application/json" -d "{ \"chain_id\": \"6141a06437bf88afa7924f9c\", \"hash\": \"0x4dbb54fae4bdca46769868b609f18c90866f38d52f34b9f542752b3ffc9ed4ce\"}"
```

响应

```js
// 区块的详细信息
{
    "items": [
      {
        "id": "6141a7f837bf88afa7925076",
        "chain_id": "6141a06437bf88afa7924f9c",
        "hash": "0x4dbb54fae4bdca46769868b609f18c90866f38d52f34b9f542752b3ffc9ed4ce",
        "height": 12,
        "timestamp": "2021-09-15 15:59:52",
        "tx_amount": 1,
        "proposer": "0x290282c54577e199B7652F2023E17985B48706a7",
        "gas_used": 102540,
        "gas_limit": 2000000000,
        "parent_hash": "0xe82870808623797d1feb70a668187e95fa3fd57313d1d51a32a27f11021d50dd",
				...
        "size": "1.08 kB",
        "head": {
        }
      }
 }
```

### 查询区块信息 /api/blocks

方法：POST

#### Description

按条件查询区块信息

#### Parameters

| Name      | Located in | Description      | Required | Schema                                                 |
| --------- | ---------- | ---------------- | -------- | ------------------------------------------------------ |
| condition | body       | 区块信息查询条件 | Yes      | [model.BlockQueryCondition](graces_api_blockquerycondition) |

#### Responses

| Code | Description | Schema                                |
| ---- | ----------- | ------------------------------------- |
| 200  | OK          | [model.Result](graces_api_result) & object |
| 400  | Bad Request | [model.Result](graces_api_result)          |

示例

```shell
curl -X POST "http://127.0.0.1:9999/api/blocks" -H "accept: application/json" -H "Content-Type: application/json" -d "{}"
```

响应

```js
// 区块的详细信息
{
    "items": [
      {
        "id": "6141a7f837bf88afa7925076",
        "chain_id": "6141a06437bf88afa7924f9c",
        "hash": "0x4dbb54fae4bdca46769868b609f18c90866f38d52f34b9f542752b3ffc9ed4ce",
        "height": 12,
        "timestamp": "2021-09-15 15:59:52",
        "tx_amount": 1,
        "proposer": "0x290282c54577e199B7652F2023E17985B48706a7",
        "gas_used": 102540,
        "gas_limit": 2000000000,
        "parent_hash": "0xe82870808623797d1feb70a668187e95fa3fd57313d1d51a32a27f11021d50dd",
				...
        "size": "1.08 kB",
        "head": {
        }
      }
 }
```

## 四、账户信息管理

### 展示节点的账户列表 /api/account/list

方法：POST

#### Description

展示指定链、指定节点的账户列表

#### Parameters

| Name | Located in | Description | Required | Schema                               |
| ---- | ---------- | ----------- | -------- | ------------------------------------ |
| dto  | body       | 账户展示DTO | Yes      | [model.AccountDTO](graces_api_accountdto) |

#### Responses

| Code | Description | Schema                                |
| ---- | ----------- | ------------------------------------- |
| 200  | OK          | [model.Result](graces_api_result) & object |
| 400  | Bad Request | [model.Result](graces_api_result)          |

示例

```shell
curl -X POST "http://127.0.0.1:9999/api/account/list" -H "accept: application/json" -H "Content-Type: application/json" -d "{ \"chain_id\": \"6141a06437bf88afa7924f9c\", \"node_id\": \"6141a0e04123af02da09cb0d\"}"
```

响应

```json
{
  "code": 200,
  "msg": "success",
  "data": [
    "0x3fcaa0a86dfbbe105c7ed73ca505c7a59c579667"
  ],
  "request": "[POST] /api/account/list"
}
```

### 锁定账户 /api/account/lock

方法：POST

#### Description

锁定指定链的指定节点上的指定账户

#### Parameters

| Name | Located in | Description | Required | Schema                                       |
| ---- | ---------- | ----------- | -------- | -------------------------------------------- |
| dto  | body       | 账户DTO     | Yes      | [model.LockAccountDTO](graces_api_lockaccountdto) |

#### Responses

| Code | Description | Schema                                |
| ---- | ----------- | ------------------------------------- |
| 200  | OK          | [model.Result](graces_api_result) & object |
| 400  | Bad Request | [model.Result](graces_api_result)          |

示例

```shell
curl -X POST "http://127.0.0.1:9999/api/account/lock" -H "accept: application/json" -H "Content-Type: application/json" -d "{ \"account\": \"0x3fcaa0a86dfbbe105c7ed73ca505c7a59c579667\", \"chain_id\": \"6141a06437bf88afa7924f9c\", \"node_id\": \"6141a0e04123af02da09cb0d\"}"
```

响应

```json
{
  "code": 200,
  "msg": "success",
  "data": true,
  "request": "[POST] /api/account/lock"
}
```

### 解锁账户 /api/account/unlock

方法：POST

#### Description

解锁指定链的指定节点上的指定账户

#### Parameters

| Name | Located in | Description | Required | Schema                                           |
| ---- | ---------- | ----------- | -------- | ------------------------------------------------ |
| dto  | body       | 解锁账户DTO | Yes      | [model.UnlockAccountDTO](graces_api_unlockaccountdto) |

#### Responses

| Code | Description | Schema                                |
| ---- | ----------- | ------------------------------------- |
| 200  | OK          | [model.Result](graces_api_result) & object |
| 400  | Bad Request | [model.Result](graces_api_result)          |

示例

```shell
curl -X POST "http://127.0.0.1:9999/api/account/unlock" -H "accept: application/json" -H "Content-Type: application/json" -d "{ \"account\": \"0x3fcaa0a86dfbbe105c7ed73ca505c7a59c579667\", \"chain_id\": \"6141a06437bf88afa7924f9c\", \"duration\": 30000, \"node_id\": \"6141a0e04123af02da09cb0d\", \"password\": \"0\"}"
```

响应

```json
{
  "code": 200,
  "msg": "success",
  "data": true,
  "request": "[POST] /api/account/unlock"
}
```

## 五、节点信息管理

### 查询节点信息 /api/node/id/{id}

方法：GET

#### Description

通过 id 查询节点信息

#### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| id   | path       | id          | Yes      | string |

#### Responses

| Code | Description | Schema                                |
| ---- | ----------- | ------------------------------------- |
| 200  | OK          | [model.Result](graces_api_result) & object |
| 400  | Bad Request | [model.Result](graces_api_result)          |

示例

```shell
curl -X GET "http://127.0.0.1:9999/api/node/id/6141a0e04123af02da09cb0d" -H "accept: application/json"
```

响应

```js
{
  "code": 200,
  "msg": "success",
  "data": {
    "id": "6141a0e04123af02da09cb0d",
    "chain_id": "6141a06437bf88afa7924f9c",
    "name": "0",
		....
    "desc": "",
    "internal_ip": "127.0.0.1",
    "external_ip": "127.0.0.1",
    "rpc_port": 6791,
    "p2p_port": 16791,
    "type": 1,
    "status": 1,
    "owner": "0x3fcaa0a86dfbbe105c7ed73ca505c7a59c579667",
    "is_alive": true,
    "blocknumber": 0
  },
}
```

### 查询节点信息 /api/nodes

方法：POST

#### Description

按条件查询节点信息

#### Parameters

| Name      | Located in | Description      | Required | Schema                                               |
| --------- | ---------- | ---------------- | -------- | ---------------------------------------------------- |
| condition | body       | 节点信息查询条件 | Yes      | [model.NodeQueryCondition](graces_api_nodequerycondition) |

#### Responses

| Code | Description | Schema                                |
| ---- | ----------- | ------------------------------------- |
| 200  | OK          | [model.Result](graces_api_result) & object |
| 400  | Bad Request | [model.Result](graces_api_result)          |

示例

```shell
curl -X POST "http://127.0.0.1:9999/api/nodes" -H "accept: application/json" -H "Content-Type: application/json" -d "{ \"chain_id\": \"6141a06437bf88afa7924f9c\" }"
```

响应

```
// 当前链上所有节点的信息
```

### 节点监控模块 /api/nodes/sync

方法：POST

#### Description

通过 节点的ip和port查询节点状态

#### Parameters

| Name      | Located in | Description    | Required | Schema                                 |
| --------- | ---------- | -------------- | -------- | -------------------------------------- |
| condition | body       | 节点的ip和port | Yes      | [model.NodeSyncReq](graces_api_nodesyncreq) |

#### Responses

| Code | Description | Schema                                       |
| ---- | ----------- | -------------------------------------------- |
| 200  | OK          | [model.SyncNodeResult](graces_api_syncnoderesult) |
| 400  | Bad Request | [model.Result](graces_api_result)                 |

示例

```shell
curl -X POST "http://127.0.0.1:9999/api/nodes/sync" -H "accept: application/json" -H "Content-Type: application/json" -d "{ \"ip\": \"127.0.0.1\", \"port\": 6791}"
```

响应

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "blocknumber": 12,
    "ismining": true,
    "gasprice": 0,
    "pendingtx": null,
    "pendingnumber": 0
  },
  "request": "[POST] /api/nodes/sync"
}
```

## 六、CNS 信息管理

### 查询CNS映射信息 /api/cns/{id}

方法：GET

#### Description

通过 id 查询CNS映射信息

#### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| id   | path       | id          | Yes      | string |

#### Responses

| Code | Description | Schema                                |
| ---- | ----------- | ------------------------------------- |
| 200  | OK          | [model.Result](graces_api_result) & object |
| 400  | Bad Request | [model.Result](graces_api_result)          |

示例

```shell
curl -X GET "http://127.0.0.1:9999/api/cns/6141b3c837bf88afa79250d2" -H "accept: application/json"
```

响应

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "id": "6141b3c837bf88afa79250d2",
    "chain_id": "6141a06437bf88afa7924f9c",
    "name": "test1",
    "version": "1.0.0.0",
    "address": "0x26527b41f4a5d9a1e0652c97fd629ced6f7a2263"
  },
  "request": "[GET] /api/cns/6141b3c837bf88afa79250d2"
}
```

### CNS映射信息版本重定向 /api/cns/redirect

方法：POST

#### Description

重定向该CNS映射信息的版本，默认情况下使用最新版本

#### Parameters

| Name | Located in | Description      | Required | Schema                                       |
| ---- | ---------- | ---------------- | -------- | -------------------------------------------- |
| dto  | body       | CNS重定向信息DTO | Yes      | [model.CNSRedirectDTO](graces_api_cnsredirectdto) |

#### Responses

| Code | Description | Schema                                |
| ---- | ----------- | ------------------------------------- |
| 200  | OK          | [model.Result](graces_api_result) & object |
| 400  | Bad Request | [model.Result](graces_api_result)          |

示例

```shell
curl -X POST "http://127.0.0.1:9999/api/cns/redirect" -H "accept: application/json" -H "Content-Type: application/json" -d "{ \"chain_id\": \"6141a06437bf88afa7924f9c\", \"name\": \"test1\", \"version\": \"1.0.0.0\"}"
```

响应

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "chain_id": "6141a06437bf88afa7924f9c",
    "status": "Operation Succeeded",
    "logs": [
      "Event [CNS] Notify: 0 [CNS] cns redirect succeed "
    ],
    "block_number": 18,
    "gas_used": 103000,
    "from": "0x3fcaa0a86dfbbe105c7ed73ca505c7a59c579667",
    "to": "0x0000000000000000000000000000000000000011",
    "tx_hash": "0xb77196438e182e34a5ef245fe9016a69a1828df399271f635e7536b943f36691",
    "err_msg": ""
  },
  "request": "[POST] /api/cns/redirect"
}
```

### 注册CNS映射信息 /api/cns/register

方法：POST

#### Description

把合约注册进合约命名系统（CNS）中

#### Parameters

| Name | Located in | Description        | Required | Schema                                       |
| ---- | ---------- | ------------------ | -------- | -------------------------------------------- |
| dto  | body       | CNS映射信息注册DTO | Yes      | [model.CNSRegisterDTO](graces_api_cnsregisterdto) |

#### Responses

| Code | Description | Schema                                |
| ---- | ----------- | ------------------------------------- |
| 200  | OK          | [model.Result](graces_api_result) & object |
| 400  | Bad Request | [model.Result](graces_api_result)          |

示例

```shell
curl -X POST "http://127.0.0.1:9999/api/cns/register" -H "accept: application/json" -H "Content-Type: application/json" -d "{ \"address\": \"0x26527b41f4a5d9a1e0652c97fd629ced6f7a2263\", \"chain_id\": \"6141a06437bf88afa7924f9c\", \"name\": \"test1\", \"version\": \"1.0.0.0\"}"
```

响应

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "chain_id": "6141a06437bf88afa7924f9c",
    "status": "Operation Succeeded",
    "logs": [
      "Event [CNS] Notify: 0 [CNS] cns register succeed "
    ],
    "block_number": 14,
    "gas_used": 105992,
    "from": "0x3fcaa0a86dfbbe105c7ed73ca505c7a59c579667",
    "to": "0x0000000000000000000000000000000000000011",
    "tx_hash": "0x19354e437eb134a65ccab20898720c0acd79b0a4514aac50d96fe2255a4a5ee3",
    "err_msg": ""
  },
  "request": "[POST] /api/cns/register"
}
```

### 查询CNS映射信息 /api/cnss

方法：POST

#### Description

按条件查询CNS映射信息

#### Parameters

| Name      | Located in | Description         | Required | Schema                                             |
| --------- | ---------- | ------------------- | -------- | -------------------------------------------------- |
| condition | body       | CNS映射信息查询条件 | Yes      | [model.CNSQueryCondition](graces_api_cnsquerycondition) |

#### Responses

| Code | Description | Schema                                |
| ---- | ----------- | ------------------------------------- |
| 200  | OK          | [model.Result](graces_api_result) & object |
| 400  | Bad Request | [model.Result](graces_api_result)          |

示例

```shell
curl -X POST "http://127.0.0.1:9999/api/cnss" -H "accept: application/json" -H "Content-Type: application/json" -d "{ \"chain_id\": \"6141a06437bf88afa7924f9c\"}"
```

响应

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "page_index": 1,
    "page_size": 10,
    "total": 1,
    "items": [
      {
        "id": "6141b3c837bf88afa79250d2",
        "chain_id": "6141a06437bf88afa7924f9c",
        "name": "test1",
        "version": "1.0.0.0",
        "address": "0x26527b41f4a5d9a1e0652c97fd629ced6f7a2263"
      }
    ],
  },
  "request": "[POST] /api/cnss"
}
```

## 七、合约信息管理

### 查询合约信息 /api/contract/address

方法：POST

#### Description

通过 合约地址 查询合约信息

#### Parameters

| Name      | Located in | Description      | Required | Schema                                                   |
| --------- | ---------- | ---------------- | -------- | -------------------------------------------------------- |
| condition | body       | 合约信息查询条件 | Yes      | [model.ContractByAddressDTO](graces_api_contractbyaddressdto) |

#### Responses

| Code | Description | Schema                                |
| ---- | ----------- | ------------------------------------- |
| 200  | OK          | [model.Result](graces_api_result) & object |
| 400  | Bad Request | [model.Result](graces_api_result)          |

示例

```shell
curl -X POST "http://127.0.0.1:9999/api/contract/address" -H "accept: application/json" -H "Content-Type: application/json" -d "{ \"chain_id\": \"614167f48a52a5322531a154\", \"contract_address\": \"0x26527B41f4A5D9a1E0652C97FD629CEd6F7A2263\"}"
```

响应

```js
// 合约内容
{
  "code": 200,
  "msg": "success",
  "data": {
    "chain_id": "614167f48a52a5322531a154",
    "address": "0x26527B41f4A5D9a1E0652C97FD629CEd6F7A2263",
    "cns": [
      {
        "id": "6141b4cc37bf88afa79250e2",
        "chain_id": "614167f48a52a5322531a154",
        "name": "test1",
        "version": "1.0.0.0",
        "address": "0x26527b41f4a5d9a1e0652c97fd629ced6f7a2263"
      }
    ],
    "creator": "0x3FcaA0A86DFbbe105c7ed73Ca505c7a59c579667",
    "tx_hash": "0x6f9dab48d4cae7a0a6e9e6480dfc460155e8cd040741a9d86512a4b51ebc07c8",
    "content": [
    ... 
}
```

### 关闭合约防火墙 /api/contract/closefirewall

方法：POST

#### Description

通过 合约地址 关闭合约防火墙

#### Parameters

| Name      | Located in | Description     | Required | Schema                           |
| --------- | ---------- | --------------- | -------- | -------------------------------- |
| condition | body       | 链id 和合约地址 | Yes      | [model.FireWall](graces_api_firewall) |

#### Responses

| Code | Description | Schema                       |
| ---- | ----------- | ---------------------------- |
| 200  | OK          | string                       |
| 400  | Bad Request | [model.Result](graces_api_result) |

示例

```shell
curl -X POST "http://127.0.0.1:9999/api/contract/closefirewall" -H "accept: application/json" -H "Content-Type: application/json" -d "{ \"chainid\": \"614167f48a52a5322531a154\", \"contractAddress\": \"0x26527b41f4a5d9a1e0652c97fd629ced6f7a2263\"}"
```

响应

```json
{
  "code": 200,
  "msg": "success",
  "data": "{\n\t\"status\": \"Operation Succeeded\",\n\t\"logs\": [\n\t\t\"Event Notify: 0 fw close success \"\n\t],\n\t\"blockNumber\": 19,\n\t\"GasUsed\": 35176,\n\t\"From\": \"0x3fcaa0a86dfbbe105c7ed73ca505c7a59c579667\",\n\t\"To\": \"0x1000000000000000000000000000000000000005\",\n\t\"TxHash\": \"0x115aa6666abb9998635ed4e1e3498a1b9ef14b957f5bc75df632880ad7bdfc28\"\n}",
  "request": "[POST] /api/contract/closefirewall"
}
```

### 开启合约防火墙 /api/contract/openfirewall

#### 方法：POST
#### Description

通过 合约地址 开启合约防火墙

#### Parameters

| Name      | Located in | Description     | Required | Schema                           |
| --------- | ---------- | --------------- | -------- | -------------------------------- |
| condition | body       | 链id 和合约地址 | Yes      | [model.FireWall](graces_api_firewall) |

#### Responses

| Code | Description | Schema                       |
| ---- | ----------- | ---------------------------- |
| 200  | OK          | string                       |
| 400  | Bad Request | [model.Result](graces_api_result) |

示例

```shell
curl -X POST "http://127.0.0.1:9999/api/contract/openfirewall" -H "accept: application/json" -H "Content-Type: application/json" -d "{ \"chainid\": \"614167f48a52a5322531a154\", \"contractAddress\": \"0x26527b41f4a5d9a1e0652c97fd629ced6f7a2263\"}"
```

响应

```json
{
  "code": 200,
  "msg": "success",
  "data": "{\n\t\"status\": \"Operation Succeeded\",\n\t\"logs\": [\n\t\t\"Event Notify: 0 fw start success \"\n\t],\n\t\"blockNumber\": 20,\n\t\"GasUsed\": 35108,\n\t\"From\": \"0x3fcaa0a86dfbbe105c7ed73ca505c7a59c579667\",\n\t\"To\": \"0x1000000000000000000000000000000000000005\",\n\t\"TxHash\": \"0x87b44f7a74ddeb1338fcc1283c592b801ea3012485da00fa16ceaa9d60323d44\"\n}",
  "request": "[POST] /api/contract/openfirewall"
}
```

### 查询合约信息 /api/contracts

#### 方法：POST
#### Description

按条件查询合约信息

#### Parameters

| Name      | Located in | Description      | Required | Schema                                                       |
| --------- | ---------- | ---------------- | -------- | ------------------------------------------------------------ |
| condition | body       | 合约信息查询条件 | Yes      | [model.ContractQueryCondition](graces_api_contractquerycondition) |

#### Responses

| Code | Description | Schema                                |
| ---- | ----------- | ------------------------------------- |
| 200  | OK          | [model.Result](graces_api_result) & object |
| 400  | Bad Request | [model.Result](graces_api_result)          |

示例

```shell
curl -X POST "http://127.0.0.1:9999/api/contracts" -H "accept: application/json" -H "Content-Type: application/json" -d "{}"
```

响应

```js
{
  "code": 200,
  "msg": "success",
  "data": {
    "page_index": 1,
    "page_size": 10,
    "total": 2,
    "items": [
      {
        "chain_id": "614167f48a52a5322531a154",
        "address": "0x26527B41f4A5D9a1E0652C97FD629CEd6F7A2263",
        "cns": [
          {
            "id": "6141b4cc37bf88afa79250e2",
            "chain_id": "614167f48a52a5322531a154",
            "name": "test1",
            "version": "1.0.0.0",
            "address": "0x26527b41f4a5d9a1e0652c97fd629ced6f7a2263"
          }
        ],
        "creator": "0x3FcaA0A86DFbbe105c7ed73Ca505c7a59c579667",
        "tx_hash": "0x6f9dab48d4cae7a0a6e9e6480dfc460155e8cd040741a9d86512a4b51ebc07c8",
        "content": "合约内容"
}
```

## 八、WebSocket 管理

### [非功能性接口] websocket 客户端向 websocket 服务端发送消息 /api/ws/clientsend

方法：POST

#### Description

让指定 id 和 group 的 WebSocket 客户端向它所连接的服务端发送信息

#### Parameters

| Name         | Located in | Description | Required | Schema                                   |
| ------------ | ---------- | ----------- | -------- | ---------------------------------------- |
| wsMessageDto | body       | 数据信息    | Yes      | [model.WSMessageDTO](graces_api_wsmessagedto) |

#### Responses

| Code | Description | Schema                       |
| ---- | ----------- | ---------------------------- |
| 200  | OK          | [model.Result](graces_api_result) |
| 400  | Bad Request | [model.Result](graces_api_result) |

### [非功能性接口] 拨号连接 /api/ws/dial

方法: POST

#### Description

作为 websocket 客户端向其他 websocket 服务端拨号建立连接

#### Parameters

| Name      | Located in | Description | Required | Schema                             |
| --------- | ---------- | ----------- | -------- | ---------------------------------- |
| wsDialDTO | body       | 拨号信息    | Yes      | [model.WSDialDTO](graces_api_wsdialdto) |

#### Responses

| Code | Description | Schema                                |
| ---- | ----------- | ------------------------------------- |
| 200  | OK          | [model.Result](graces_api_result) & object |
| 400  | Bad Request | [model.Result](graces_api_result)          |

### 单个 WebSocket 组信息 /api/ws/group/{group}

方法：GET

#### Description

通过 组名称 查询 WebSocket 中指定组的详细信息

#### Parameters

| Name  | Located in | Description | Required | Schema |
| ----- | ---------- | ----------- | -------- | ------ |
| group | path       | group       | Yes      | string |

#### Responses

| Code | Description | Schema                                |
| ---- | ----------- | ------------------------------------- |
| 200  | OK          | [model.Result](graces_api_result) & object |
| 400  | Bad Request | [model.Result](graces_api_result)          |

### 所有 WebSocket 组信息 /api/ws/groups

方法：GET

#### Description

查询 WebSocket 中所有组的详细信息

#### Responses

| Code | Description | Schema                                |
| ---- | ----------- | ------------------------------------- |
| 200  | OK          | [model.Result](graces_api_result) & object |
| 400  | Bad Request | [model.Result](graces_api_result)          |

### WebSocket 管理器信息 /api/ws/manager

方法：GET

#### Description

查询 WebSocket 管理器当前状态信息

#### Responses

| Code | Description | Schema                                |
| ---- | ----------- | ------------------------------------- |
| 200  | OK          | [model.Result](graces_api_result) & object |
| 400  | Bad Request | [model.Result](graces_api_result)          |

### [非功能性接口] 向单个 WebSocket 客户端发送信息 /api/ws/send

方法：POST

#### Description

向指定 id 和 group 的 WebSocket 客户端发送信息

#### Parameters

| Name         | Located in | Description | Required | Schema                                   |
| ------------ | ---------- | ----------- | -------- | ---------------------------------------- |
| wsMessageDto | body       | 数据信息    | Yes      | [model.WSMessageDTO](graces_api_wsmessagedto) |

#### Responses

| Code | Description | Schema                       |
| ---- | ----------- | ---------------------------- |
| 200  | OK          | [model.Result](graces_api_result) |
| 400  | Bad Request | [model.Result](graces_api_result) |

### [非功能性接口] 向所有 WebSocket 客户端广播信息 /api/ws/sendall

方法: POST

#### Description

向所有 WebSocket 客户端广播信息

#### Parameters

| Name                  | Located in | Description | Required | Schema                                                     |
| --------------------- | ---------- | ----------- | -------- | ---------------------------------------------------------- |
| wsBroadCastMessageDTO | body       | 数据信息    | Yes      | [model.WSBroadCastMessageDTO](graces_api_wsbroadcastmessagedto) |

#### Responses

| Code | Description | Schema                       |
| ---- | ----------- | ---------------------------- |
| 200  | OK          | [model.Result](graces_api_result) |
| 400  | Bad Request | [model.Result](graces_api_result) |

### [非功能性接口] 向一个组中的所有 WebSocket 客户端广播信息 /api/ws/sendgroup

方法：POST

#### Description

向指定 group 中的所有 WebSocket 客户端广播信息

#### Parameters

| Name              | Located in | Description | Required | Schema                                             |
| ----------------- | ---------- | ----------- | -------- | -------------------------------------------------- |
| wsGroupMessageDTO | body       | 数据信息    | Yes      | [model.WSGroupMessageDTO](graces_api_wsgroupmessagedto) |

#### Responses

| Code | Description | Schema                       |
| ---- | ----------- | ---------------------------- |
| 200  | OK          | [model.Result](graces_api_result) |
| 400  | Bad Request | [model.Result](graces_api_result) |

## 九、Models

(graces_api_accountdto)=
### model.AccountDTO

| Name     | Type   | Description | Required |
| -------- | ------ | ----------- |----------|
| chain_id | string | 链ID        | Yes      |
| node_id  | string | 节点ID      | Yes      |

### model.AccountVO

| Name     | Type   | Description | Required |
| -------- | ------ | ----------- | -------- |
| chain_id | string | 所属链ID    | No       |
| node_id  | string | 所属节点ID  | No       |
| account  | string | 账户地址    | No       |

(graces_api_blockheadvo)=
### model.BLockHeadVO

| Name              | Type    | Description                     | Required |
| ----------------- | ------- | ------------------------------- | -------- |
| parent_hash       | string  | 上一个区块的哈希                | No       |
| miner             | string  | 挖出该区块的矿工地址            | No       |
| state_root        | string  | merkle 状态树的根哈希           | No       |
| transactions_root | string  | merkle 交易树的根哈希           | No       |
| logs_bloom        | string  | LogsBloom                       | No       |
| receipts_root     | string  | merkle 收据树的根哈希           | No       |
| height            | integer | 区块高度                        | No       |
| gas_limit         | integer | 区块内部所有交易的 Gas 限制量   | No       |
| gas_used          | integer | 区块内部所有交易的 Gas 使用总量 | No       |
| timestamp         | string  | 区块生成时间                    | No       |
| extra_data        | string  | 区块的额外信息                  | No       |
| mix_hash          | string  | 混合哈希                        | No       |
| nonce             | integer | 区块 POW 随机数                 | No       |
| hash              | string  | 区块哈希                        | No       |

(graces_api_blockbyhashdto)=
### model.BlockByHashDTO

| Name     | Type   | Description | Required |
| -------- | ------ | ----------- | -------- |
| chain_id | string | 所属链ID    | No       |
| hash     | string | 区块哈希    | Yes      |

(graces_api_blockdatasyncinfovo)=
### model.BlockDataSyncInfoVO

| Name                   | Type    | Description                                                  | Required |
| ---------------------- | ------- | ------------------------------------------------------------ | -------- |
| block_sync_time_avg    | integer | 同步每个区块的平均耗时（单位：ms）                           | No       |
| current_height         | integer | 当前已经同步到的块高                                         | No       |
| err_msg                | string  | 错误信息                                                     | No       |
| estimate_complete_time | string  | 预计完成时间                                                 | No       |
| latest_height          | integer | 链上最新区块的块高                                           | No       |
| start_time             | string  | 开始时间                                                     | No       |
| status                 | string  | 数据同步状态：同步中（syncing）、同步出错（error）、同步成功（success） | No       |

(graces_api_blockquerycondition)=
### model.BlockQueryCondition

| Name       | Type    | Description                                         | Required |
| ---------- | ------- | --------------------------------------------------- | -------- |
| chain_id   | string  | 所属链ID                                            | No       |
| hash       | string  | 区块哈希                                            | No       |
| height     | integer | 区块高度                                            | No       |
| id         | string  | 主键ID                                              | No       |
| page_index | integer | 当前页数                                            | No       |
| page_size  | integer | 每页数据条数                                        | No       |
| proposer   | string  | 挖出该区块的矿工地址                                | No       |
| sort       | object  | 排序规则，k：字段名，v：排序规则，1位升序，-1位降序 | No       |
| time_end   | integer | 终止时间                                            | No       |
| time_start | integer | 起始时间                                            | No       |

### model.BlockVO

| Name        | Type                                   | Description                     | Required |
| ----------- | -------------------------------------- | ------------------------------- | -------- |
| chain_id    | string                                 | 所属链ID                        | No       |
| extra_data  | string                                 | 区块的额外信息                  | No       |
| gas_limit   | integer                                | 区块内部所有交易的 Gas 限制量   | No       |
| gas_used    | integer                                | 区块内部所有交易的 Gas 使用总量 | No       |
| hash        | string                                 | 区块哈希                        | No       |
| head        | [model.BLockHeadVO](graces_api_blockheadvo) | 区块头信息                      | No       |
| height      | integer                                | 区块高度                        | No       |
| id          | string                                 | 主键ID                          | No       |
| parent_hash | string                                 | 上一个区块的哈希                | No       |
| proposer    | string                                 | 挖出该区块的矿工地址            | No       |
| size        | string                                 | 区块大小                        | No       |
| timestamp   | string                                 | 区块生成时间                    | No       |
| tx_amount   | integer                                | 区块内部交易数量                | No       |

(graces_api_cnsdatasyncinfovo)=
### model.CNSDataSyncInfoVO

| Name                   | Type    | Description                                                  | Required |
| ---------------------- | ------- | ------------------------------------------------------------ | -------- |
| block_sync_time_avg    | integer | 同步每个 CNS合约映射信息 的平均耗时（单位：ms）              | No       |
| err_msg                | string  | 错误信息                                                     | No       |
| estimate_complete_time | string  | 预计完成时间                                                 | No       |
| index                  | integer | 当前已经同步到的下标                                         | No       |
| size                   | integer | 总的 CNS合约映射信息 数量                                    | No       |
| start_time             | string  | 开始时间                                                     | No       |
| status                 | string  | 数据同步状态：同步中（syncing）、同步出错（error）、同步成功（success） | No       |

(graces_api_cnsquerycondition)=
### model.CNSQueryCondition

| Name       | Type    | Description                                         | Required |
| ---------- | ------- | --------------------------------------------------- | -------- |
| address    | string  | 合约地址                                            | No       |
| chain_id   | string  | 所属链ID                                            | No       |
| id         | string  | 主键ID                                              | No       |
| name       | string  | 合约别名                                            | No       |
| page_index | integer | 当前页数                                            | No       |
| page_size  | integer | 每页数据条数                                        | No       |
| sort       | object  | 排序规则，k：字段名，v：排序规则，1位升序，-1位降序 | No       |
| version    | string  | 合约版本号                                          | No       |

(graces_api_cnsredirectdto)=
### model.CNSRedirectDTO

| Name     | Type   | Description | Required |
| -------- | ------ | ----------- | -------- |
| chain_id | string | 链ID        | Yes      |
| name     | string | 合约别名    | Yes      |
| version  | string | 合约版本号  | Yes      |

(graces_api_cnsregisterdto)=
### model.CNSRegisterDTO

| Name     | Type   | Description | Required |
| -------- | ------ | ----------- | -------- |
| address  | string | 合约地址    | Yes      |
| chain_id | string | 所属链ID    | Yes      |
| name     | string | 合约别名    | Yes      |
| version  | string | 合约版本号  | Yes      |

(graces_api_cnsvo)=
### model.CNSVO

| Name     | Type   | Description | Required |
| -------- | ------ | ----------- | -------- |
| address  | string | 合约地址    | No       |
| chain_id | string | 所属链ID    | No       |
| id       | string | CNS ID      | No       |
| name     | string | 合约别名    | No       |
| version  | string | 合约版本号  | No       |

(graces_api_chaindto)=
### model.ChainDTO

| Name         | Type    | Description                                   | Required |
| ------------ | ------- |-----------------------------------------------| -------- |
| chain_config | object  | 链第一个节点的配置信息。添加链信息时如对链不熟悉，不建议手动填写该字段，应该使用自动生成的 | No       |
| desc         | string  | 链的描述信息                                        | No       |
| ip           | string  | 链第一个节点的 IP 地址                                 | Yes      |
| name         | string  | 链名称                                           | Yes      |
| rpc_port     | integer | 链第一个节点的 rpc 端口号                               | Yes      |
| p2p_port     | integer | 链第一个节点的 p2p 端口号                               | Yes      |
| ws_port      | integer | 链第一个节点的 websocket 端口号                         | Yes      |

### model.ChainDataSyncInfoVO

| Name                   | Type                                                   | Description                                                  | Required |
| ---------------------- | ------------------------------------------------------ | ------------------------------------------------------------ | -------- |
| block_data_sync_info   | [model.BlockDataSyncInfoVO](graces_api_blockdatasyncinfovo) | 区块同步信息                                                 | No       |
| chain_id               | string                                                 | 链ID                                                         | No       |
| cns_data_sync_info_vo  | [model.CNSDataSyncInfoVO](graces_api_cnsdatasyncinfovo)     | CNS同步信息                                                  | No       |
| err_msg                | string                                                 | 错误信息                                                     | No       |
| estimate_complete_time | string                                                 | 预计完成时间                                                 | No       |
| nodeDataSyncInfoVO     | [model.NodeDataSyncInfoVO](graces_api_nodedatasyncinfovo)   | 节点同步信息                                                 | No       |
| start_time             | string                                                 | 开始时间                                                     | No       |
| status                 | string                                                 | 数据同步状态：同步中（syncing）、同步出错（error）、同步成功（success） | No       |

(graces_api_chainquerycondition)=
### model.ChainQueryCondition

| Name       | Type    | Description                                         | Required |
| ---------- | ------- | --------------------------------------------------- | -------- |
| id         | string  | 主键ID                                              | No       |
| ip         | string  | 链第一个节点的 IP 地址                              | No       |
| name       | string  | 链名称                                              | No       |
| rpc_port   | integer | 链第一个节点的 rpc 端口号                           | No       |
| p2p_port   | integer | 链第一个节点的 p2p 端口号                           | No       |
| ws_port    | integer | 链第一个节点的 websocket 端口号                     | No       |
| page_index | integer | 当前页数                                            | No       |
| page_size  | integer | 每页数据条数                                        | No       |
| sort       | object  | 排序规则，k：字段名，v：排序规则，1位升序，-1位降序 | No       |

### model.ChainVO

| Name         | Type    | Description                          | Required |
| ------------ | ------- | ------------------------------------ | -------- |
| chain_config | object  | 链的配置信息                         | No       |
| desc         | string  | 链的描述信息                         | No       |
| id           | string  | 主键ID                               | No       |
| ip           | string  | 链的IP地址                           | No       |
| name         | string  | 链名称                               | No       |
| rpc_port     | integer | 链第一个节点的 rpc 端口号            | No       |
| p2p_port     | integer | 链第一个节点的 p2p 端口号            | No       |
| ws_port      | integer | 链第一个节点的 websocket 端口号      | No       |
| username     | string  | 在服务端部署链时所需的服务端操作用户 | No       |
| update_time  | string  | 最后一次更新时间                     | No       |
| create_time  | string  | 创建时间                             | No       |
| delete_time  | string  | 删除时间                             | No       |

(graces_api_contractbyaddressdto)=
### model.ContractByAddressDTO

| Name             | Type   | Description | Required |
| ---------------- | ------ | ----------- | -------- |
| chain_id         | string | 所属链ID    | No       |
| contract_address | string | 合约地址    | Yes      |

### model.ContractCallResult

| Name         | Type       | Description                      | Required |
| ------------ | ---------- | -------------------------------- | -------- |
| block_number | integer    | 合约调用产生的交易所在的区块高度 | No       |
| chain_id     | string     | 所属链ID                         | No       |
| err_msg      | string     | 合约调用产生的错误信息           | No       |
| from         | string     | 合约调用产生的交易的发起人       | No       |
| gas_used     | integer    | 合约调用的 Gas 消耗              | No       |
| logs         | [ string ] | 合约调用日志                     | No       |
| status       | string     | 合约调用状态                     | No       |
| to           | string     | 合约调用产生的交易的交易目标     | No       |
| tx_hash      | string     | 合约调用产生的交易的交易哈希     | No       |

(graces_api_contractquerycondition)=
### model.ContractQueryCondition

| Name       | Type    | Description                                         | Required |
| ---------- | ------- | --------------------------------------------------- | -------- |
| address    | string  | 合约地址                                            | No       |
| chain_id   | string  | 所属链ID                                            | No       |
| creator    | string  | 合约创建人地址                                      | No       |
| id         | string  | 部署合约的交易ID                                    | No       |
| name       | string  | CNS名称                                             | No       |
| page_index | integer | 当前页数                                            | No       |
| page_size  | integer | 每页数据条数                                        | No       |
| sort       | object  | 排序规则，k：字段名，v：排序规则，1位升序，-1位降序 | No       |
| time_end   | integer | 终止时间                                            | No       |
| time_start | integer | 起始时间                                            | No       |
| tx_hash    | string  | 部署合约时的交易哈希                                | No       |

### model.ContractVO

| Name      | Type                           | Description          | Required |
| --------- | ------------------------------ | -------------------- | -------- |
| address   | string                         | 合约地址             | No       |
| chain_id  | string                         | 所属链ID             | No       |
| cns       | [ [model.CNSVO](graces_api_cnsvo) ] | CNS名称              | No       |
| content   | object                         | 合约内容             | No       |
| creator   | string                         | 合约创建人地址       | No       |
| timestamp | string                         | 部署时间             | No       |
| tx_hash   | string                         | 部署合约时的交易哈希 | No       |

(graces_api_firewall)=
### model.FireWall

| Name            | Type   | Description | Required |
| --------------- | ------ | ----------- |----------|
| chainid         | string | 所属链ID    | Yes      |
| contractAddress | string | 合约地址    | Yes      |

(graces_api_lockaccountdto)=
### model.LockAccountDTO

| Name     | Type   | Description | Required |
| -------- | ------ | ----------- |----------|
| account  | string | 账户        | Yes      |
| chain_id | string | 链ID        | Yes      |
| node_id  | string | 节点ID      | Yes      |

(graces_api_nodedatasyncinfovo)=
### model.NodeDataSyncInfoVO

| Name                   | Type    | Description                                                  | Required |
| ---------------------- | ------- | ------------------------------------------------------------ | -------- |
| err_msg                | string  | 错误信息                                                     | No       |
| estimate_complete_time | string  | 预计完成时间                                                 | No       |
| index                  | integer | 当前已经同步到的下标                                         | No       |
| size                   | integer | 总的 节点 数量                                               | No       |
| start_time             | string  | 开始时间                                                     | No       |
| status                 | string  | 数据同步状态：同步中（syncing）、同步出错（error）、同步成功（success） | No       |
| sync_time_avg          | integer | 同步每个节点的平均耗时（单位：ms）                           | No       |

(graces_api_nodequerycondition)=
### model.NodeQueryCondition

| Name        | Type    | Description                                         | Required |
| ----------- | ------- | --------------------------------------------------- | -------- |
| chain_id    | string  | 所属链ID                                            | No       |
| id          | string  | 主键ID                                              | No       |
| internal_ip | string  | 公网IP地址                                          | No       |
| name        | string  | 节点名称                                            | No       |
| page_index  | integer | 当前页数                                            | No       |
| page_size   | integer | 每页数据条数                                        | No       |
| sort        | object  | 排序规则，k：字段名，v：排序规则，1位升序，-1位降序 | No       |

(graces_api_nodesyncreq)=
### model.NodeSyncReq

| Name | Type    | Description  | Required |
| ---- | ------- | ------------ |----------|
| ip   | string  | 节点 IP 地址 | Yes      |
| port | integer | 节点端口号   | Yes      |

### model.NodeVO

| Name        | Type    | Description            | Required |
| ----------- | ------- | ---------------------- | -------- |
| blocknumber | integer | 节点当前最新区块的块高 | No       |
| chain_id    | string  | 所属链 id              | No       |
| desc        | string  | 节点描述               | No       |
| external_ip | string  | 节点公网 IP            | No       |
| id          | string  | 节点 id                | No       |
| internal_ip | string  | 节点内网 IP            | No       |
| is_alive    | boolean | 节点是否在线           | No       |
| name        | string  | 节点名称               | No       |
| owner       | string  | 节点部署人             | No       |
| p2p_port    | integer | 节点 P2P 端口号        | No       |
| public_key  | string  | 节点公钥               | No       |
| rpc_port    | integer | 节点 RPC 端口号        | No       |
| status      | integer | 节点状态               | No       |
| type        | integer | 节点类型               | No       |

### model.PageInfo

| Name          | Type    | Description    | Required |
| ------------- | ------- | -------------- | -------- |
| has_next_page | boolean | 是否存在下一页 | No       |
| has_pre_page  | boolean | 是否存在上一页 | No       |
| items         | object  | 当前页的数据   | No       |
| page_index    | integer | 当前页数       | No       |
| page_size     | integer | 每页数据条数   | No       |
| page_total    | integer | 总页数         | No       |
| total         | integer | 数据总量       | No       |

(graces_api_receiptvo)=
### model.ReceiptVO

| Name             | Type    | Description  | Required |
| ---------------- | ------- | ------------ | -------- |
| contract_address | string  | 合约地址     | No       |
| event            | string  | 事件信息     | No       |
| gas_used         | integer | Gas 使用量   | No       |
| status           | integer | 交易状态     | No       |
| status_name      | string  | 交易状态名称 | No       |

(graces_api_result)=
### model.Result

| Name    | Type    | Description     | Required |
| ------- | ------- | --------------- | -------- |
| code    | integer | HTTP 响应状态码 | No       |
| data    | object  | 响应数据        | No       |
| msg     | string  | 响应提示信息    | No       |
| request | string  | 请求资源路径    | No       |

(graces_api_syncnoderesult)=
### model.SyncNodeResult

| Name          | Type       | Description                         | Required |
| ------------- | ---------- | ----------------------------------- | -------- |
| blocknumber   | integer    | 节点当前最新区块的块高              | No       |
| gasprice      | integer    | Gas 价格                            | No       |
| ismining      | boolean    | 是否处于挖矿状态                    | No       |
| pendingnumber | integer    | 节点交易池 pending 交易的数量       | No       |
| pendingtx     | [ string ] | 节点交易池的 pending 交易的哈希列表 | No       |

(graces_api_systemconfigvo)=
### model.SystemConfigVO

| Name                      | Type   | Description                                                  | Required |
| ------------------------- | ------ | ------------------------------------------------------------ | -------- |
| blockGasLimit             | string | 设置区块 Gas 限制                                            | No       |
| chainID                   | string | 所属链ID                                                     | No       |
| gasContractName           | string | 设置交易所消耗的 Gas 由指定的合约名称来提供                  | No       |
| isApproveDeployedContract | string | 设置是否允许部署合约                                         | No       |
| isCheckDeployPermission   | string | 设置是否检查合约部署权限                                     | No       |
| isProduceEmptyBlock       | string | 设置是否允许出空块                                           | No       |
| isUseGas                  | string | 设置是否使用指定合约提供的 Gas，需与 gasContractName 共同使用才能起作用 | No       |
| txGasLimit                | string | 设置交易 Gas 限制                                            | No       |

(graces_api_txbyhashdto)=
### model.TXByHashDTO

| Name     | Type   | Description | Required |
| -------- | ------ | ----------- | -------- |
| chain_id | string | 所属链ID    | No       |
| hash     | string | 交易哈希    | Yes      |

(graces_api_txquerycondition)=
### model.TXQueryCondition

| Name             | Type    | Description                                         | Required |
| ---------------- | ------- | --------------------------------------------------- | -------- |
| block_id         | string  | 所属区块ID                                          | No       |
| chain_id         | string  | 所属链ID                                            | No       |
| contract_address | string  | 合约地址                                            | No       |
| hash             | string  | 交易哈希                                            | No       |
| height           | integer | 所属区块高度                                        | No       |
| id               | string  | 主键ID                                              | No       |
| page_index       | integer | 当前页数                                            | No       |
| page_size        | integer | 每页数据条数                                        | No       |
| participant_hash | string  | 交易参与人哈希                                      | No       |
| sort             | object  | 排序规则，k：字段名，v：排序规则，1位升序，-1位降序 | No       |
| status           | integer | 交易状态                                            | No       |
| time_end         | integer | 终止时间                                            | No       |
| time_start       | integer | 起始时间                                            | No       |

### model.TXVO

| Name           | Type                               | Description    | Required |
| -------------- | ---------------------------------- | -------------- | -------- |
| block_id       | string                             | 所属区块ID     | No       |
| chain_id       | string                             | 所属链ID       | No       |
| detail         | [model.TxDetail](graces_api_txdetail)   | 交易的详细信息 | No       |
| from           | string                             | 交易发起人地址 | No       |
| gas_limit      | integer                            | Gas 限制       | No       |
| gas_price      | integer                            | Gas 价格       | No       |
| hash           | string                             | 交易哈希       | No       |
| height         | integer                            | 所属区块高度   | No       |
| id             | string                             | 主键ID         | No       |
| input          | string                             | input 数据     | No       |
| is_to_contract | boolean                            | 是否是合约调用 | No       |
| nonce          | string                             | 随机数         | No       |
| receipt        | [model.ReceiptVO](graces_api_receiptvo) | 收据信息       | No       |
| timestamp      | string                             | 交易发生时间   | No       |
| to             | string                             | 交易目标地址   | No       |
| value          | integer                            | 交易数额       | No       |

(graces_api_txdetail)=
### model.TxDetail

| Name     | Type       | Description                                 | Required |
| -------- | ---------- | ------------------------------------------- | -------- |
| action   | string     | 交易执行的操作： 合约部署、合约调用或者转账 | No       |
| contract | string     | 显示合约相关的内容                          | No       |
| extra    | object     | 显示其它的信息                              | No       |
| method   | string     | 如果是合约调用，显示调用的合约方法          | No       |
| params   | [ object ] | 如果是合约调用，显示调用的合约参数          | No       |
| txtype   | integer    |                                             | No       |

(graces_api_unlockaccountdto)=
### model.UnlockAccountDTO

| Name     | Type    | Description                                              | Required |
| -------- | ------- | -------------------------------------------------------- |----------|
| account  | string  | 账户                                                     | Yes      |
| chain_id | string  | 链ID                                                     | Yes      |
| duration | integer | 解锁持续时间，单位：秒。如果为 0，则该值被默认设置为 300 | No       |
| node_id  | string  | 节点ID                                                   | Yes      |
| password | string  | 账户密码                                                 | Yes      |

(graces_api_wsbroadcastmessagedto)=
### model.WSBroadCastMessageDTO

| Name    | Type   | Description      | Required |
| ------- | ------ | ---------------- | -------- |
| message | string | 要发送的消息内容 | Yes      |

(graces_api_wsclientvo)=
### model.WSClientVO

| Name        | Type    | Description                    | Required |
| ----------- | ------- | ------------------------------ | -------- |
| group       | string  | websocket 该客户端所在的组     | No       |
| id          | string  | websocket 客户端连接 id        | No       |
| is_alive    | boolean | 连接是否存活                   | No       |
| is_dial     | boolean | 是否是 graces 的主动拨号的连接 | No       |
| local_addr  | string  | websocket 连接本地地址         | No       |
| path        | string  | websocket 请求服务端连接的路径 | No       |
| remote_addr | string  | websocket 连接远程地址         | No       |
| retry_cnt   | integer | 连接断线后已重试连接的次数     | No       |

(graces_api_wsdialdto)=
### model.WSDialDTO

| Name  | Type    | Description                       | Required |
| ----- | ------- | --------------------------------- | -------- |
| group | string  | 当前 websocket 连接被分配到的分组 | Yes      |
| ip    | string  | websocket 服务端 IP 地址          | Yes      |
| path  | string  | websocket 服务端请求路径          | No       |
| port  | integer | websocket 服务端端口号            | Yes      |

(graces_api_wsgroupmessagedto)=
### model.WSGroupMessageDTO

| Name    | Type   | Description                    | Required |
| ------- | ------ | ------------------------------ | -------- |
| group   | string | websocket 客户端连接所在的分组 | Yes      |
| message | string | 要发送的消息内容               | Yes      |

### model.WSGroupVO

| Name    | Type                                     | Description            | Required |
| ------- | ---------------------------------------- | ---------------------- | -------- |
| clients | [ [model.WSClientVO](graces_api_wsclientvo) ] | 该组所包含的客户端信息 | No       |
| name    | string                                   | websocket 组名称       | No       |

### model.WSManagerVO

| Name                        | Type    | Description                                          | Required |
| --------------------------- | ------- | ---------------------------------------------------- | -------- |
| chan_broad_cast_message_len | integer | websocket 向所有客户端广播消息时，消息缓冲队列的长度 | No       |
| chan_group_message_len      | integer | websocket 向组客户端广播消息时，消息缓冲队列的长度   | No       |
| chan_message_len            | integer | websocket 向单客户端发送消息时，消息缓冲队列的长度   | No       |
| chan_register_len           | integer | websocket 注册缓冲队列长度                           | No       |
| chan_unregister_len         | integer | websocket 注销缓冲队列长度                           | No       |
| client_len                  | integer | 当前 websocket 客户端分组数量                        | No       |
| group_len                   | integer | 当前 websocket 客户端分组数量                        | No       |

(graces_api_wsmessagedto)=
### model.WSMessageDTO

| Name    | Type   | Description                    | Required |
| ------- | ------ | ------------------------------ | -------- |
| group   | string | websocket 客户端连接所在的分组 | Yes      |
| id      | string | websocket 客户端连接 ID        | Yes      |
| message | string | 要发送的消息内容               | Yes      |