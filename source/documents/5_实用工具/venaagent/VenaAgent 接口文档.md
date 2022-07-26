# VenaAgent 接口文档

## 修订记录

| 版本 | 更新内容 | 更新人 | 更新日期 |
| --- | --- | --- | --- |
| 0.1 | 新建 | 庄建涵、毛智凯 | 2022/5/16 |
| 0.2 | 增加背景描述、上链方式等信息 | 毛智凯 | 2022/5/31 |
| 0.3 | 补全每个接口出现的错误码 | 庄建涵 | 2022/6/8 |
| 0.4 | 合约接口文档调整 | 毛智凯 | 2022/6/8 |
| 0.5 | 交易发送，from 改为非必填 | 毛智凯 | 2022/6/20 |
| 0.6 | 所有接口对链相关过滤，关键字均为 chainName | 毛智凯 | 2022/6/20 |
| 0.7 | 新版本功能接口调整 | 毛智凯 | 2022/7/20 |

## 背景及功能

VenaAgent 是万向联盟链代理服务器，旨在帮助用户通过 VenaAgent 完成链上交易，而不需要花成本去维护链上节点连接。

在 V0.0.1 版本中，核心主要提供链上相关信息查询（包括 区块信息、交易信息）以及以同步或异步方式实现不同交易数据上链功能，起着代理交易上链的作用。

在 V0.1.0 版本中，加入多链管理模块、节点信息查询、订阅交易回执消息、密钥托管功能。

## 交易上链方式

1. 上链数据已签名

    1.1. 针对已签名的数据，上链时，可以调用【签名交易】接口，完成数据上链

    1.2. 该方式要求用户使用自己的私钥对交易数据进行签名

2. 上链数据未签名

    2.1. 针对未签名的数据，上链时，可以调用【发送交易】接口，指定交易发起方、接收方、交易值及交易负载信息等相关数据

    2.2. 代理服务器会使用自身的私钥对交易数据进行签名，并推送上链

3. 合约调用方式

    3.1. 需要先上传用户的合约的 ABI 文件（调用【合约 ABI 文件上传】接口）（需指定合约所属链、合约地址、合约类型及合约描述等信息）

    3.2. 合约调用时，指定合约所属链、合约地址、要调用的方法及方法参数，代理服务器会根据合约链信息和合约地址定位合约的 ABI 文件及合约类型，使用代理服务器自身的私钥对合约交易进行签名，并推送上链

## 链代理服务器使用说明

## 接口列表

### 1. 交易发送（同步）

**请求URL**

- /api/tx/sendTx

**请求方式：**

- POST

**请求参数：**

| 参数名 | 必选 | 类型 | 说明 |
| --- | --- | --- | --- |
| chainName | 否 | string | 链标识，未填，则使用链代理服务器默认配置链 |
| from | 否 | string | 交易发起方，可不填，默认使用代理服务器私钥地址 |
| to | 否 | string | 交易接收方地址，当创建新合约时可选 |
| gas | 否 | uint64 | 进行交易所愿支付的 gas 数量 |
| gasPrice | 否 | uint64 | 每 gas 单价 |
| value | 否 | uint64 | 交易值 |
| data | 否 | string | 交易负载数据 |

**返回参数说明：**

| 参数名 | 类型 | 说明 |
| --- | --- | --- |
| code | int | 200:成功 500:失败 10011：输入参数无效 10013：类型转换失败 12011：链操作失败 13011：获取链客户端失败 13012：无相关链信息配置 22011：等待交易发送超时 22012：发送交易失败 22013：交易等待处理中 22014：交易签名失败 23013： 账户需要解锁 10009：发起方私钥未上传 详细错误信息见msg |
| msg | string | 错误信息 |
| data | object | 根据交易发送是否成功，data 交易回执（常见类型中的 txReceipt，如果异常，回执中仅有 txHash) |
| request | string | 访问路径 |

例子：

request：

```shell
curl -X 'POST' 'http://127.0.0.1:9999/api/tx/sendTx' -H 'accept: application/json' -H 'Content-Type: application/json' -d '{ "chainName": "string","data": "string","from": "string", "gas": 0,"gasPrice": 0,"to": "string","value": 0}'
```

response：
```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "blockHash": "0x6eaae5ff9b79a4522feef2bc8489ef05fb4eab6c736c2e17185f6eea17b8fef0",
    "blockNumber": 43,
    "contractAddress": "",
    "cumulativeGasUsed": 49504,
    "from": "0x0866bdc67af4f546968c9c16a52b4b9a80b21c60",
    "gasUsed": 24752,
    "root": "",
    "to": "0xb306ca6cd684dcff93fd81315e0d298b0d3f9d79",
    "txHash": "0x689ceae1c2f1fed09b3e44b868946da6ae53e117c420f6c5155b78f2e9811505",
    "txIndex": 0,
    "status": 1,
    "logs": [
      {
        "address": "0xb306ca6cd684dcff93fd81315e0d298b0d3f9d79",
        "topics": [
          "0x7e3809921fe10ea80a140db7d9f125d80b8653f054cd9bd46aa689d5f91d8134"
        ],
        "data": "0x000000000000000000000000000000000000000000000000000000000000006000000000000000000000000000000000000000000000000000000000000000a0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000057261697365000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000001be6938de4bd9ce5a4b1e8b4a5efbc9ae6b2a1e69c89e69d83e999900000000000"
      }
    ]
  },
  "request": "[POST] /api/tx/sendTx"
}
```

### 2. 交易发送（异步）

**请求URL**

- /api/tx/sendTxAsync

**请求方式：**

- POST

**请求参数：**

| 参数名 | 必选 | 类型 | 说明 |
| --- | --- | --- | --- |
| chainName | 否 | string | 链标识，未填，则使用链代理服务器默认配置链 |
| from | 否 | string | 交易发起方，可不填，默认使用代理服务器私钥地址 |
| to | 否 | string | 交易接收方地址，当创建新合约时可选 |
| gas | 否 | uint64 | 进行交易所愿支付的 gas 数量 |
| gasPrice | 否 | uint64 | 每 gas 单价 |
| value | 否 | uint64 | 交易值 |
| data | 否 | string | 交易负载数据 |

**返回参数说明：**

| 参数名 | 类型 | 说明 |
| --- | --- | --- |
| code | int | 200:成功 500:失败 10011：输入参数无效 12011：链操作失败 13011：获取链客户端失败 13012：无相关链信息配置 22012：发送交易失败 22014：交易签名失败 23013： 账户需要解锁 10009：发起方私钥未上传 详细错误信息见msg |
| msg | string | 错误信息 |
| data | string | 交易哈希 |
| request | string | 访问路径 |

例子：

request：
```shell
curl -X 'POST' 'http://127.0.0.1:9999/api/tx/sendTxAsync' -H 'accept: application/json' -H 'Content-Type: application/json' -d '{ "chainName": "string","data": "string","from": "string", "gas": 0,"gasPrice": 0,"to": "string","value": 0}'
```

response：
```json
{
  "code": 200,
  "msg": "success",
  "data": "0x139a38d18e4024af5f4effc2c36f71e34f612591a05f3e39b337c68431afc661",
  "request": "[POST] /api/tx/sendTxAsync"
}
```

### 3. 签名交易发送（同步）

**请求URL：**

- /api/tx/sendSignedTx

**请求方式：**

- POST

**请求参数：**

| 参数名 | 必选 | 类型 | 说明 |
| --- | --- | --- | --- |
| chainName | 否 | string | 链标识，未填，则使用链代理服务器默认配置链 |
| signedTx | 是 | string | 交易签名内容 |

**返回参数说明：**

| 参数名 | 类型 | 说明 |
| --- | --- | --- |
| code | int | 200:成功 500:失败 10011：输入参数无效 10013：类型转换失败 12011：从链上查询签名交易失败 || 从链上查询交易失败 || 从链上查询对应交易回执失败 || 链上获取到的交易回执反序列化解析失败 13011：获取链客户端失败 13012：无相关链信息配置 22011：等待交易发送超时 22012：发送交易失败 22013：交易等待处理中 详细错误信息见msg |
| msg | string | 错误信息 |
| data | object | 根据交易发送是否成功，data 交易回执（常见类型中的 txReceipt，如果异常，回执中仅有 txHash) |
| request | string | 访问路径 |

例子：

request：
```shell
curl -X 'POST' 'http://127.0.0.1:9999/api/tx/sendSignedTx' -H 'accept: application/json' -H 'Content-Type: application/json' -d '{ "chainName": "string","signedTx": "string"}'
```

response：

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "blockHash": "0x6eaae5ff9b79a4522feef2bc8489ef05fb4eab6c736c2e17185f6eea17b8fef0",
    "blockNumber": 43,
    "contractAddress": "",
    "cumulativeGasUsed": 49504,
    "from": "0x0866bdc67af4f546968c9c16a52b4b9a80b21c60",
    "gasUsed": 24752,
    "root": "",
    "to": "0xb306ca6cd684dcff93fd81315e0d298b0d3f9d79",
    "txHash": "0x689ceae1c2f1fed09b3e44b868946da6ae53e117c420f6c5155b78f2e9811505",
    "txIndex": 0,
    "status": 1,
    "logs": [
      {
        "address": "0xb306ca6cd684dcff93fd81315e0d298b0d3f9d79",
        "topics": [
          "0x7e3809921fe10ea80a140db7d9f125d80b8653f054cd9bd46aa689d5f91d8134"
        ],
        "data": "0x000000000000000000000000000000000000000000000000000000000000006000000000000000000000000000000000000000000000000000000000000000a0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000057261697365000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000001be6938de4bd9ce5a4b1e8b4a5efbc9ae6b2a1e69c89e69d83e999900000000000"
      }
    ]
  },
  "request": "[POST] /api/tx/sendTx"
}
```

### 4. 签名交易发送（异步）

**请求URL：**

- /api/tx/sendSignedTxAsync

**请求方式：**

- POST

**请求参数：**

| 参数名 | 必选 | 类型 | 说明 |
| --- | --- | --- | --- |
| chainName | 否 | string | 链标识，未填，则使用链代理服务器默认配置链 |
| signedTx | 是 | string | 交易签名内容 |

**返回参数说明：**

| 参数名 | 类型 | 说明 |
| --- | --- | --- |
| code | int | 200:成功 500:失败 10011：输入参数无效 12011：从链上查询签名交易失败 13011：获取链客户端失败 13012：无相关链信息配置 22012：发送交易失败 详细错误信息见msg |
| msg | string | 错误信息 |
| data | string | 交易哈希 |
| request | string | 访问路径 |

例子：

request：

```shell
curl -X 'POST' 'http://127.0.0.1:9999/api/tx/sendSignedTxAsync' -H 'accept: application/json' -H 'Content-Type: application/json' -d '{ "chainName": "string","signedTx": "string"}'
```

response：

```json
{
  "code": 200,
  "msg": "success",
  "data": "0x139a38d18e4024af5f4effc2c36f71e34f612591a05f3e39b337c68431afc661",
  "request": "[POST] /api/tx/sendTxAsync"
}
```

### 5. 合约 Abi 文件上传

**请求URL：**

- /api/contract/uploadAbiFile

**请求方式：**

- POST

**请求参数：**

| 参数名 | 必选 | 类型 | 说明 |
| --- | --- | --- | --- |
| chainName | 否 | string | 链标识，未填，则使用链代理服务器默认配置链 |
| address | 是 | string | 合约地址 |
| type | 是 | int | 合约类型, 1 wasm, 2 evm |
| desc | 否 | string | 描述 |
| abiFile | 是 | file | 合约接口文件 |

**返回参数说明：**

| 参数名 | 类型 | 说明 |
| --- | --- | --- |
| code | int | 200:成功 500:失败 10006：文件读取失败 10007：文件创建失败 10008：文件删除失败 10011：输入参数无效 22021：合约 Abi 无效 详细错误信息见msg |
| msg | string | 错误信息 |
| data | bool | true 代表上传成功；false 代表上传失败 |
| request | string | 访问路径 |

例子：

request：

```shell
curl -X 'POST' 'http://127.0.0.1:9999/api/contract/uploadAbiFile' -H 'accept: application/json' -H 'Content-Type: multipart/form-data' -F 'file=@did_identity.cpp.abi.json;type=application/json' -F 'address=0xdrqewfsdafsdafweqr' -F 'type=1'
```

response：

```json
{
  "code": 200,
  "msg": "success",
  "data": true,
  "request": "[POST] /api/contract/uploadAbiFile"
}
```

### 6. 合约调用（同步）

**请求URL：**

- /api/contract/execute

**请求方式：**

- POST

**请求参数：**

| 参数名 | 必选 | 类型 | 说明 |
| --- | --- | --- | --- |
| chainName | 否 | string | 链标识，未填，则使用链代理服务器默认配置链 |
| caller | 否 | string | 合约发起方，交易上链时会使用发起方私钥（使用前需要先上传私钥并解锁）对交易进行签名，未填，则使用代理服务器的私钥对交易进行签名 |
| address | 是 | string | 合约地址 |
| func | 是 | string | 调用的合约方法 |
| funcParams | 否 | string[] | 调用合约方法参数 |

**返回参数说明：**

| 参数名 | 类型 | 说明 |
| --- | --- | --- |
| code | int | 200:成功 500:失败 10005：找不到全路径文件 10011：输入参数无效 10013：类型转换失败 11011：找不到合约记录 12011：从链上查询签名交易失败 || 从链上查询交易失败 || 从链上查询对应交易回执失败 || 链上获取到的交易回执反序列化解析失败 13011：获取链客户端失败 13012：无相关链信息配置 22011：等待交易发送超时 22012：发送交易失败 22013：交易等待处理中 22014：交易签名失败 23013： 账户需要解锁 10009：发起方私钥未上传 详细错误信息见msg |
| msg | string | 错误信息 |
| data | object | 返回结果 |
| callResult | string | 针对查询类方法，返回查询结果 |
| txReceipt | object | 根据交易发送是否成功，data 交易回执（常见类型中的 txReceipt，如果异常，回执中仅有 txHash)，如果调用查询类方法，则该值为空字符串 |
| parsedLogs | string[] | 解析后的日志内容 |
| request | string | 访问路径 |

例子：

request：

```shell
curl -X 'POST' 'https://127.0.0.1:9999/api/contract/execute' -H 'accept: application/json' -H 'Content-Type: application/json' -d '{"address": "0x0000000000000000000000000000000000000099","caller": "0x2a4fa8c987d7b1f73c1b363b98dc85ee0156e88f","chainName": "","func": "saveEvidence","funcParams": ["test","testval"]}'
```

response：

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "queryResult": "",
    "txReceipt": {
      "blockHash": "0x8a52445274238b8a25856f01dc93992383bca8a62bd826c9f9f7b78b81a8e56d",
      "blockNumber": 19,
      "contractAddress": "0x0000000000000000000000000000000000000000",
      "cumulativeGasUsed": 103000,
      "from": "0x2a4fa8c987d7b1f73c1b363b98dc85ee0156e88f",
      "gasUsed": 103000,
      "root": "0x",
      "to": "0x0000000000000000000000000000000000000099",
      "txHash": "0x786eb3c1ec748c2f200eb9014578004b03d027c77fcd166faeeca9b9bfe40abc",
      "status": 1,
      "logs": [
        {
          "address": "0x0000000000000000000000000000000000000099",
          "topics": [
            "0x8cd284134f0437457b5542cb3a7da283d0c38208c497c5b4b005df47719f98a1"
          ],
          "data": "0xd78095736176652065766964656e63652073756363657373"
        }
      ]
    },
    "parsedLogs": [
      "Event Notify: 0 save evidence success "
    ]
  },
  "request": "[POST] /api/contract/execute"
}
```

### 7. 合约调用（异步）

**请求URL：**

- /api/contract/executeAsync

**请求方式：**

- POST

**请求参数：**

| 参数名 | 必选 | 类型 | 说明 |
| --- | --- | --- | --- |
| chainName | 否 | string | 链标识，未填，则使用链代理服务器默认配置链 |
| address | 是 | string | 合约地址 |
| func | 是 | string | 调用的合约方法 |
| funcParams | 否 | string[] | 调用合约方法参数 |

**返回参数说明：**

| 参数名 | 类型 | 说明 |
| --- | --- | --- |
| code | int | 200:成功 500:失败 10005：找不到全路径文件 10011：输入参数无效 11011：找不到合约abi文件 12011：从链上查询签名交易失败 13011：获取链客户端失败 13012：无相关链信息配置 22012：发送交易失败 22014：交易签名失败 23013： 账户需要解锁 10009：发起方私钥未上传 详细错误信息见msg |
| msg | string | 错误信息 |
| data | object | 返回结果 |
| callResult | string | 针对查询类方法，返回查询结果 |
| txHash | string | 交易哈希值（如果调用查询类方法，则该值为空字符串） |
| request | string | 访问路径 |

例子：

request：

```shell
curl -X 'POST' 'http://127.0.0.1:9999/api/contract/executeAsync' -H 'accept: application/json' -H 'Content-Type: application/json' -d '{"address": "string","chainName": "string","func": "string","funcParams": ["string"]}'
```

response：

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "queryResult": "",
    "txHash": "0xa1d2d5a15dc0f242eb6327ef4b09d040ca43c69f2e21f4d0bdefa4b08c636670"
  },
  "request": "[POST] /api/contract/executeAsync"
}
```

### 8. 获取合约信息

**请求URL：**

- /api/contract/{chainName}

**请求方式：**

- GET

**请求参数：无**

**返回参数说明：**

| 参数名 | 类型 | 说明 |
| --- | --- | --- |
| code | int | 200:成功 500:失败 11011：数据库查询失败 详细错误信息见msg |
| msg | string | 错误信息 |
| data | object | 返回结果 |
| chainName | string | 链标识 |
| address | string | 合约地址 |
| desc | string | 合约描述信息 |
| type | int | 合约类型, 1 wasm, 2 evm |
| modifiedTime | int64 | 修改时间 |
| request | string | 访问路径 |

**例子：**

**request：**

```shell
curl -X 'GET' 'https://127.0.0.1:9999/api/contract/{chainName}' -H 'accept: application/json'
```

**response ：**

```json
{
  "code": 200,
  "msg": "success",
  "data": [
    {
      "chainName": "vena",
      "address": "0xa60fc186d48e95d6d024d212b4d507c5a35d3e31",
      "desc": "",
      "type": 2,
      "modifiedTime": 1658224953
    }
  ],
  "request": "[GET] /api/contract/vena"
}
```

### 9. 设置系统配置

**请求URL：**

- /api/systemConfig

**请求方式：**

- POST

**请求参数：**

请求参数是一个字典集合，为方便通用处理，k、v 均为字符串包装，v 如果为数值类型，需要使用字符串包起来。目前提供的 key 及相应值类型如下：

| 参数名 | 类型 | 说明 |
| --- | --- | --- |
| MaxSyncSendTxDuration | int | 最大等待发送交易时长, 单位为 毫秒 |
| NodeConnTimeout | int | 节点连接超时时长, 单位为 毫秒 |
| NetFailRetryCount | int | 发送交易重试次数 |

**返回参数说明：**

| 参数名 | 类型 | 说明 |
| --- | --- | --- |
| code | int | 200:成功 500:失败 10011：输入参数无效 详细错误信息见msg |
| msg | string | 错误信息 |
| data | bool | 交易哈希 |
| request | string | 访问路径 |

例子：

request：

```shell
curl -X 'POST' 'http://127.0.0.1:9999/api/systemConfig' -H 'accept: application/json' -H 'Content-Type: application/json' -d '{"MaxSyncSendTxDuration": "15000","NodeConnTimeout": "2000"}'
```

response：

```json
{
  "code": 200,
  "msg": "success",
  "data": true,
  "request": "[POST] /api/systemConfig"
}
```

### 10. 获取系统配置

**请求URL：**

- /api/systemConfig

**请求方式：**

- GET

**请求参数：无**

**返回参数说明：**

| 参数名 | 类型 | 说明 |
| --- | --- | --- |
| code | int | 200:成功 500:失败 详细错误信息见msg |
| msg | string | 错误信息 |
| data | map | key 为配置项，value 为配置内容，用字符串包装 |
| request | string | 访问路径 |

例子：

request：

```shell
curl -X 'GET' 'http://127.0.0.1:9999/api/systemConfig' -H 'accept: application/json'
```

response：

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "MaxWaitSyncSendTxTime": "2000",
    "NetFailRetryCount": "12",
    "NodeConnTimeout": "3000"
  },
  "request": "[GET] /api/systemConfig"
}
```

### 11. 通过区块号查询区块

**请求URL：**

- /api/block/getByNumber

**请求方式：**

- GET

**请求参数：**

| 参数名 | 必选 | 类型 | 说明 |
| --- | --- | --- | --- |
| chainName | 否 | string | 链标识，未填，则使用链代理服务器默认配置链 |
| number | 否 | uint64 | 区块号（默认值：latest） |

**返回参数说明**

| 参数名 | 类型 | 说明 |
| --- | --- | --- |
| code | int | 200:成功 500:失败 10012：用户输入的区块号解析失败 10013：类型转换失败 12011：从链上查询对应区块失败 13011：获取链客户端失败 13012：无相关链信息配置 详细错误信息见msg |
| msg | string | 错误信息 |
| data | object | 区块信息 |
| request | string | 访问路径 |

例子：

request：

```shell
curl -X 'GET' 'http://127.0.0.1:9999/api/block/getByNumber' -H 'accept: application/json'
```

response：

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "parentHash": "0xb790117713ed347d7c952e8eb2edf08e3cb6e9738520737721cefb4775bb149f",
    "miner": "0x6f6d8c96cae849ea570b258af9040ac69976762a",
    "stateRoot": "0x94a29850f9a8e7f21711b157ce27eea132828f02efab55bcb686fbb4c9a8c181",
    "transactionsRoot": "0x808cc6d8f0e4a34b3c8b1c08cd7b320f42639081a523a1abc2a5826abc1346cc",
    "receiptsRoot": "0x8805a863fc9f0815dedb85a045c6566b14ceebd5214c5dd283192ec78f298796",
    "logsBloom": "0x00000000000400000000000000000000000000000000000000000040000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000804000000000000000020000000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000040000000000000000000002000000000000000000000000000000000000000000000000000000000040000000000000000000000000000000000000000000000000000000000200000000000000000000008000000000000008000000000000",
    "number": 13,
    "gasLimit": 10000000000,
    "gasUsed": 68104,
    "timestamp": 1652432620703,
    "extraData": "0xdd830100018976656e61636861696e88676f312e31372e39856c696e75780000f90164f85494338bf58dc9e4b4031a47db2250432342b3a7c0019433b2d409c65c77b6339dbe978226f6a78e00d165946f6d8c96cae849ea570b258af9040ac69976762a94bdbff898d0987d210a0357bcb3dbc3fafa9cb340b841cdb5ef9472038fd9d2b8a7b1f91f61231e3fd48b41863dbeb12667b47822df0b22928022d85f2c21d59d9d839d32f44446ee244920107ed260402e48a851944d01f8c9b841ebbea5cd4e6511a984578c1d7aeb0c39b7647a85ef520ad4218cd3af591e1c08592689b98af5c26854de50826bb8122afad0489b0b6beec9aeb5e84056428d2201b841a7cac22e1c0c6f9b963768107ddb38e5436b3c1a41090fb3a11f54f8037e05f3094f3d57cde1f498ceb891c7804a2f1d28ff53cae5ac81b4430aa0f3a5d31e5000b84134b6cbdfe56aa9d28300d23fba3b2eb495be2c3bbd14fd38bbf46e37c364c58b2819fc97e48067f04d8f70b954bfd50fb9ffeb215a12abae0781b7c57af2d6ef01",
    "mixHash": "0x63746963616c2062797a616e74696e65206661756c7420746f6c6572616e6365",
    "nonce": "0x026d73381b93004da1e09914e8cdd54e1017596967c7cbfea3f4ea4f65ba13ba3ad6e80195998236f4c7c0f34ee58708a8c1296891dbe603b72bc6d43996a5e89ee0cb86cb298efa83dbbbb00f0c45198c",
    "hash": "0xb270fc7b5923baf293ed9dc81f527366196870f2c9c9c82759b565c1cc2e67ba",
    "transactions": [
      "0x6b0806113e285d1ef70459dbc3ba4a867b070a1b17b44e03e5695432e8e61581"
    ]
  },
  "request": "[GET] /api/block/getByNumber"
}
```

### 12. 通过哈希查询区块

**请求URL：**

- /api/block/getByHash

**请求方式：**

- GET

**请求参数：**

| 参数名 | 必选 | 类型 | 说明 |
| --- | --- | --- | --- |
| chainName | 否 | string | 链标识，未填，则使用链代理服务器默认配置链 |
| hash | 是 | string | 区块哈希 |

**返回参数说明**

| 参数名 | 类型 | 说明 |
| --- | --- | --- |
| code | int | 200:成功 500:失败 10013：类型转换失败 12011：从链上查询对应区块失败 13011：获取链客户端失败 13012：无相关链信息配置 详细错误信息见msg |
| msg | string | 错误信息 |
| data | object | 区块信息 |
| request | string | 访问路径 |

例子：

request：

```shell
curl -X 'GET' 'http://127.0.0.1:9999/api/block/getByHash?hash=0xb270fc7b5923baf293ed9dc81f527366196870f2c9c9c82759b565c1cc2e67ba' -H 'accept: application/json'
```

response：

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "parentHash": "0xb790117713ed347d7c952e8eb2edf08e3cb6e9738520737721cefb4775bb149f",
    "miner": "0x6f6d8c96cae849ea570b258af9040ac69976762a",
    "stateRoot": "0x94a29850f9a8e7f21711b157ce27eea132828f02efab55bcb686fbb4c9a8c181",
    "transactionsRoot": "0x808cc6d8f0e4a34b3c8b1c08cd7b320f42639081a523a1abc2a5826abc1346cc",
    "receiptsRoot": "0x8805a863fc9f0815dedb85a045c6566b14ceebd5214c5dd283192ec78f298796",
    "logsBloom": "0x00000000000400000000000000000000000000000000000000000040000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000804000000000000000020000000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000040000000000000000000002000000000000000000000000000000000000000000000000000000000040000000000000000000000000000000000000000000000000000000000200000000000000000000008000000000000008000000000000",
    "number": 13,
    "gasLimit": 10000000000,
    "gasUsed": 68104,
    "timestamp": 1652432620703,
    "extraData": "0xdd830100018976656e61636861696e88676f312e31372e39856c696e75780000f90164f85494338bf58dc9e4b4031a47db2250432342b3a7c0019433b2d409c65c77b6339dbe978226f6a78e00d165946f6d8c96cae849ea570b258af9040ac69976762a94bdbff898d0987d210a0357bcb3dbc3fafa9cb340b841cdb5ef9472038fd9d2b8a7b1f91f61231e3fd48b41863dbeb12667b47822df0b22928022d85f2c21d59d9d839d32f44446ee244920107ed260402e48a851944d01f8c9b841ebbea5cd4e6511a984578c1d7aeb0c39b7647a85ef520ad4218cd3af591e1c08592689b98af5c26854de50826bb8122afad0489b0b6beec9aeb5e84056428d2201b841a7cac22e1c0c6f9b963768107ddb38e5436b3c1a41090fb3a11f54f8037e05f3094f3d57cde1f498ceb891c7804a2f1d28ff53cae5ac81b4430aa0f3a5d31e5000b84134b6cbdfe56aa9d28300d23fba3b2eb495be2c3bbd14fd38bbf46e37c364c58b2819fc97e48067f04d8f70b954bfd50fb9ffeb215a12abae0781b7c57af2d6ef01",
    "mixHash": "0x63746963616c2062797a616e74696e65206661756c7420746f6c6572616e6365",
    "nonce": "0x026d73381b93004da1e09914e8cdd54e1017596967c7cbfea3f4ea4f65ba13ba3ad6e80195998236f4c7c0f34ee58708a8c1296891dbe603b72bc6d43996a5e89ee0cb86cb298efa83dbbbb00f0c45198c",
    "hash": "0xb270fc7b5923baf293ed9dc81f527366196870f2c9c9c82759b565c1cc2e67ba",
    "transactions": [
      "0x6b0806113e285d1ef70459dbc3ba4a867b070a1b17b44e03e5695432e8e61581"
    ]
  },
  "request": "[GET] /api/block/getByHash?hash=0xb270fc7b5923baf293ed9dc81f527366196870f2c9c9c82759b565c1cc2e67ba"
}
```

### 13.交易查询

**请求URL：**

- /api/tx/getByHash

**请求方式：**

- GET

**请求参数：**

| 参数名 | 必选 | 类型 | 说明 |
| --- | --- | --- | --- |
| chainName | 否 | string | 链标识，未填，则使用链代理服务器默认配置链 |
| hash | 是 | string | 交易哈希 |

**返回参数说明**

| 参数名 | 类型 | 说明 |
| --- | --- | --- |
| code | int | 200:成功 500:失败 10012：链上获取到的交易反序列化解析失败 10013：类型转换失败 12011：从链上查询对应交易失败 13011：获取链客户端失败 13012：无相关链信息配置 详细错误信息见msg |
| msg | string | 错误信息 |
| data | object | 交易信息 |
| request | string | 访问路径 |

**例子：**

request：

```shell
curl -X 'GET' 'http://127.0.0.1:9999/api/tx/getByHash?hash=0x6b0806113e285d1ef70459dbc3ba4a867b070a1b17b44e03e5695432e8e61581' -H 'accept: application/json'
```

response：

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "blockHash": "0xb270fc7b5923baf293ed9dc81f527366196870f2c9c9c82759b565c1cc2e67ba",
    "blockNumber": 13,
    "from": "0x48b90138e09fa69bb7f3aa932073cda541ed15ae",
    "to": "0xb302f974dc0eedaef8dc043ae1650eddf05e94b7",
    "gas": 0,
    "gasPrice": 0,
    "hash": "0x6b0806113e285d1ef70459dbc3ba4a867b070a1b17b44e03e5695432e8e61581",
    "input": "0x004f480300000000000000000000000000000000000000000000000000000000000000c8",
    "value": 0,
    "nonce": "0x312e83f982e31137",
    "transactionIndex": 0
  },
  "request": "[GET] /api/tx/getByHash?hash=0x6b0806113e285d1ef70459dbc3ba4a867b070a1b17b44e03e5695432e8e61581"
}
```

### 14. 交易回执查询

**请求URL：**

- /api/txReceipt/getByHash

**请求方式：**

- GET

**请求参数：**

| 参数名 | 必选 | 类型 | 说明 |
| --- | --- | --- | --- |
| chainName | 否 | string | 链标识，未填，则使用链代理服务器默认配置链 |
| hash | 是 | string | 交易哈希 |

**返回参数说明**

| 参数名 | 类型 | 说明 |
| --- | --- | --- |
| code | int | 200:成功 500:失败 10012：链上获取到的交易反序列化解析失败 10013：类型转换失败 12011：链操作失败 13011：获取链客户端失败 13012：无相关链信息配置 22013：交易在交易池中,暂时没有回执 详细错误信息见msg |
| msg | string | 错误信息 |
| data | txReceipt | 交易回执信息 |
| request | string | 访问路径 |

**例子：**

request：

```shell
curl -X 'GET' 'http://127.0.0.1:9999/api/txReceipt/getByHash?hash=0x6b0806113e285d1ef70459dbc3ba4a867b070a1b17b44e03e5695432e8e61581' -H 'accept: application/json'
```

response：

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "blockHash": "0xb270fc7b5923baf293ed9dc81f527366196870f2c9c9c82759b565c1cc2e67ba",
    "blockNumber": 13,
    "contractAddress": "",
    "cumulativeGasUsed": 68104,
    "from": "0x48b90138e09fa69bb7f3aa932073cda541ed15ae",
    "gasUsed": 68104,
    "root": "",
    "to": "0xb302f974dc0eedaef8dc043ae1650eddf05e94b7",
    "transactionHash": "0x6b0806113e285d1ef70459dbc3ba4a867b070a1b17b44e03e5695432e8e61581",
    "transactionIndex": 0,
    "status": 1,
    "logs": [
      {
        "address": "0xb302f974dc0eedaef8dc043ae1650eddf05e94b7",
        "topics": [
          "0x7e3809921fe10ea80a140db7d9f125d80b8653f054cd9bd46aa689d5f91d8134"
        ],
        "data": "0x000000000000000000000000000000000000000000000000000000000000006000000000000000000000000000000000000000000000000000000000000000a0000000000000000000000000000000000000000000000000000000000000000100000000000000000000000000000000000000000000000000000000000000057261697365000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000ce6938de4bd9ce68890e58a9f0000000000000000000000000000000000000000"
      },
      {
        "address": "0xb302f974dc0eedaef8dc043ae1650eddf05e94b7",
        "topics": [
          "0x6b82d188802bfeb5f310a21136f9e9f46f3348eaa4f1a30d6a16900650fc0e0a",
          "0x00000000000000000000000048b90138e09fa69bb7f3aa932073cda541ed15ae"
        ],
        "data": "0x00000000000000000000000000000000000000000000000000000000000000c8"
      }
    ]
  },
  "request": "[GET] /api/txReceipt/getByHash?hash=0x6b0806113e285d1ef70459dbc3ba4a867b070a1b17b44e03e5695432e8e61581"
}
```

### 15. 获取所有链信息

**请求URL：**

- /api/chains

**请求方式：**

- GET

**请求参数：无**

**返回参数说明**

| 参数名 | 类型 | 说明 |
| --- | --- | --- |
| code | int | 200:成功 500:失败 10013：类型转换失败 11011：数据库查询异常 详细错误信息见msg |
| msg | string | 错误信息 |
| data | chain[] | 一组链信息 |
| request | string | 访问路径 |

**例子：**

request：

```shell
curl -X 'GET' 'https://127.0.0.1:9999/api/chains' -H 'accept: application/json'
```

response：

```json
{
  "code": 200,
  "msg": "success",
  "data": [
    {
      "name": "vena",
      "desc": "default",
      "nodeEndpoint": [
        {
          "ip": "127.0.0.1",
          "rpcPort": 6791
        }
      ],
      "nodeWSPortMap": {
        "0": 26792,
        "1": 26793,
        "2": 26793,
        "3": 26793
      },
      "nodes": [
        {
          "chainName": "vena",
          "name": "0",
          "desc": "",
          "type": 1,
          "publicKey": "3a0f514b25238437db4240bca295bf92864f8409932b06803b16bb3f8659dd1a07c20e616bf9f1596b88af0e88936641c1297bc9eec964e5db9d2405e6c0bcbc",
          "externalIP": "127.0.0.1",
          "internalIP": "127.0.0.1",
          "rpcPort": 6791,
          "p2pPort": 16791,
          "wsPort": 26791,
          "status": 1,
          "owner": "",
          "modifiedTime": 1658216426
        }
      ],
      "modifiedTime": 1658210066
    }
  ],
  "request": "[GET] /api/chains"
}
```

### 16. 获取指定链信息

**请求URL：**

- /api/chain/{name}

**请求方式：**

- GET

**请求参数：无**

**返回参数说明**

| 参数名 | 类型 | 说明 |
| --- | --- | --- |
| code | int | 200:成功 500:失败 10013：类型转换失败 11011：数据库查询异常 详细错误信息见msg |
| msg | string | 错误信息 |
| data | chain | 链信息 |
| request | string | 访问路径 |

**例子：**

request：

```shell
curl -X 'GET' 'https://127.0.0.1:9999/api/chain/vena' -H 'accept: application/json'
```

response：

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "name": "vena",
    "desc": "default",
    "nodeEndpoint": [
      {
        "ip": "127.0.0.1",
        "rpcPort": 6791
      }
    ],
    "nodeWSPortMap": {
      "0": 26792,
      "1": 26793,
      "2": 26793,
      "3": 26793
    },
    "nodes": [
      {
        "chainName": "vena",
        "name": "0",
        "desc": "",
        "type": 1,
        "publicKey": "3a0f514b25238437db4240bca295bf92864f8409932b06803b16bb3f8659dd1a07c20e616bf9f1596b88af0e88936641c1297bc9eec964e5db9d2405e6c0bcbc",
        "externalIP": "127.0.0.1",
        "internalIP": "127.0.0.1",
        "rpcPort": 6791,
        "p2pPort": 16791,
        "wsPort": 26791,
        "status": 1,
        "owner": "",
        "modifiedTime": 1658216426
      }
    ],
    "modifiedTime": 1658210066
  },
  "request": "[GET] /api/chain/vena"
}
```

### 17. 更新链信息

**请求URL：**

- /api/chain/

**请求方式：**

- POST

**请求参数：**

| 参数名 | 必选 | 类型 | 说明 |
| --- | --- | --- | --- |
| name | 是 | string | 链标识 |
| nodeEndpoints | 是 | string | 链节点网络地址串，配置多个节点网络信息，可更好的确保连接到链，形如为 "ip1:port1,ip2:port2..." |
| nodeWSPortMap | 否 | map[string]uint64 | key 为节点名，value 为节点 websocket 端口（用于兼容低版本无法获取节点websocket 端口的情况，使用 及后续版本的链，可不填） |
| desc | 否 | string | 链信息描述 |

**返回参数说明**

| 参数名 | 类型 | 说明 |
| --- | --- | --- |
| code | int | 200:成功 500:失败 10011：参数无效 10013：类型转换失败 11011：数据库查询异常 12011：从链上获取所有节点信息失败 详细错误信息见msg |
| msg | string | 错误信息 |
| data | bool | 是否更新成功 |
| request | string | 访问路径 |

**例子：**

request：

```shell
curl -X 'POST' 'https://127.0.0.1:9999/api/chain/' -H 'accept: application/json' -H 'Content-Type: application/json' -d '{"name": "vena","desc": "default","nodeEndpoints": "127.0.0.1:6791, 127.0.0.1:6792","nodeWSPortMap": {"0": 26792,"1": 26793,"2": 26793,"3": 26793}}'
```

response：

```json
{
  "code": 200,
  "msg": "success",
  "data": true,
  "request": "[POST] /api/chain/"
}
```

### 18. 删除链信息

**请求URL：**

- /api/chain/delete/{name}

**请求方式：**

- POST

**请求参数：无**

**返回参数说明**

| 参数名 | 类型 | 说明 |
| --- | --- | --- |
| code | int | 200:成功 500:失败 11011：数据库操作异常 详细错误信息见msg |
| msg | string | 错误信息 |
| data | bool | 是否删除成功 |
| request | string | 访问路径 |

**例子：**

request：

```shell
curl -X 'POST' 'https://127.0.0.1:9999/api/chain/delete/testChain' -H 'accept: application/json'
```

response：

```json
{
  "code": 200,
  "msg": "success",
  "data": true,
  "request": "[POST] /api/chain/delete/testChain"
}
```

### 19. 获取用户地址列表

**请求URL：**

- /api/keyTrust/accountList

**请求方式：**

- GET

**请求参数：无**

**返回参数说明**

| 参数名 | 类型 | 说明 |
| --- | --- | --- |
| code | int | 200:成功 500:失败 11011：数据库操作异常 详细错误信息见msg |
| msg | string | 错误信息 |
| data | string[] | 用户地址清单 ( 已上传用户私钥的用户 ) |
| request | string | 访问路径 |

**例子：**

request：

```shell
curl -X 'GET' 'https://127.0.0.1:9999/api/keyTrust/accountList' -H 'accept: application/json'
```

response：

```json
{
  "code": 200,
  "msg": "success",
  "data": [
    "0x2a4fa8c987d7b1f73c1b363b98dc85ee0156e88f"
  ],
  "request": "[GET] /api/keyTrust/accountList"
}
```

### 20. 上传用户私钥

**请求URL：**

- /api/keyTrust/submit

**请求方式：**

- POST

**请求参数：**

| 参数名 | 必选 | 类型 | 说明 |
| --- | --- | --- | --- |
| password | 否 | string | 用于解锁账号的口令，可不填，默认为 &quot;0&quot; |
| privateKey | 是 | string | 用户私钥（ 64 位十六进制字符串） |

**返回参数说明**

| 参数名 | 类型 | 说明 |
| --- | --- | --- |
| code | int | 200:成功 500:失败 10007：文件创建失败 11011：数据库操作异常 23012：私钥无效 23014：口令加密失败 详细错误信息见msg |
| msg | string | 错误信息 |
| data | bool | 是否上传成功 |
| request | string | 访问路径 |

**例子：**

request：

```shell
curl -X 'POST''https://127.0.0.1:9999/api/keyTrust/submit' -H 'accept: application/json' -H 'Content-Type: application/json' -d '{"password": "0","privateKey": "3a7401bba9177ea3541fb4f9d27fd1c6a9e218ea7212fbae2ef7112dc003b7a5"}'
```

response：

```json
{
  "code": 200,
  "msg": "success",
  "data": true,
  "request": "[POST] /api/keyTrust/submit"
}
```

### 21. 上传 keyFile 文件

**请求URL：**

- /api/keyTrust/uploadKeyFile

**请求方式：**

- POST

**请求参数：**

| 参数名 | 必选 | 类型 | 说明 |
| --- | --- | --- | --- |
| file | 是 | file | keyfile 文件 |

**返回参数说明**

| 参数名 | 类型 | 说明 |
| --- | --- | --- |
| code | int | 200:成功 500:失败 10007：文件创建失败 11011：数据库操作异常 23011：keyfile 文件解析失败 23012：私钥无效 23014：口令加密失败 详细错误信息见msg |
| msg | string | 错误信息 |
| data | bool | 是否上传成功 |
| request | string | 访问路径 |

**例子：**

request：

```shell
curl -X 'POST' 'https://127.0.0.1:9999/api/keyTrust/uploadKeyFile' -H 'accept: application/json' -H 'Content-Type: multipart/form-data' -F 'file=@keyfile.json;type=application/json'
```

response：

```json
{
  "code": 200,
  "msg": "success",
  "data": true,
  "request": "[POST] /api/keyTrust/uploadKeyFile"
}
```

### 22. 解锁用户私钥

**请求URL：**

- /api/keyTrust/unlock

**请求方式：**

- POST

**请求参数：**

| 参数名 | 必选 | 类型 | 说明 |
| --- | --- | --- | --- |
| address | 是 | string | 用户地址 |
| duration | 否 | int | 解锁时长，单位秒，不填默认 300 秒，设置为 -1 则永久解锁 |
| password | 否 | string | 私钥解锁口令，不填默认为 &quot;0&quot; |

**返回参数说明**

| 参数名 | 类型 | 说明 |
| --- | --- | --- |
| code | int | 200:成功 500:失败 10009：未找到上传的目标私钥 11011：数据库操作失败 11013：数据库事务操作失败 23015：类型转换失败 23014：口令加密失败 详细错误信息见msg |
| msg | string | 错误信息 |
| data | bool | 是否解锁成功 |
| request | string | 访问路径 |

**例子：**

request：

```shell
curl -X 'POST' 'https://127.0.0.1:9999/api/keyTrust/unlock' -H 'accept: application/json' -H 'Content-Type: application/json' -d '{"address": "0x2a4fa8c987d7b1f73c1b363b98dc85ee0156e88f","duration": -1,"password": "0"}'
```

response：

```json
{
  "code": 200,
  "msg": "success",
  "data": true,
  "request": "[POST] /api/keyTrust/unlock"
}
```

## 常见类型

### 链信息（ chain ）

| 参数名 | 类型 | 说明 |
| --- | --- | --- |
| name | string | 链标识 |
| nodeEndpoint | object[] | 上传的节点网络信息 |
| ip | string | 节点网络地址 |
| rpcPort | uint64 | 节点 rpc 端口号 |
| nodeWSPortMap | map[string]uint64 | 提交的节点 websocket 端口，key 为节点名，value 为 websocket 端口 |
| nodes | node[] | 动态获取到的节点信息 |
| modifiedTime | string | 链信息修改时间 |

### 节点信息（ node ）

| 参数名 | 类型 | 说明 |
| --- | --- | --- |
| chainName | string | 链标识 |
| name | string | 节点名字 |
| desc | string | 节点描述 |
| type | int | 节点类型，1:共识节点；2:观察者节点 |
| publicKey | string | 节点公钥 |
| externalIP | string | 外网地址 |
| internalIP | string | 内网地址 |
| rpcPort | uint64 | 节点的 RPC 端口号 |
| p2pPort | uint64 | 节点的 P2P 端口号 |
| wsPort | uint64 | 节点的 ws 端口号 |
| status | int | 节点状态，1:正常；2：删除 |
| owner | string | 申请者地址 |
| modifiedTime | int64 | 节点信息更新时间 |

### 区块（ block ）

| 参数名 | 类型 | 说明 |
| --- | --- | --- |
| hash | string | 区块哈希 |
| number | uint64 | 区块号 |
| parentHash | string | 父区块的哈希 |
| mixHash | string | 混合哈希 |
| logsBloom | string | 区块日志的bloom过滤器哈希 |
| stateRoot | string | 区块中状态树的根节点哈希 |
| transactionsRoot | string | 区块中的交易树根节点哈希 |
| receiptsRoot | string | 区块中交易收据树的根节点哈希 |
| miner | string | 挖出该区块的矿工地址 |
| gasLimit | uint64 | 本区块允许的最大gas用量 |
| gasUsed | uint64 | 本区块中所有交易使用的总gas用量 |
| timestamp | uint64 | 区块时间戳 |
| extraData | string | 区块额外数据 |
| nonce | string | 随机数 |
| transactions | string[] | 关联的交易 |

### 交易（ tx ）

| 字段名 | 类型 | 说明 |
| --- | --- | --- |
| blockHash | string | 区块哈希 |
| blockNumber | uint64 | 区块号 |
| from | string | 交易发送方地址 |
| to | string | 交易接收方地址，对于合约创建交易，该值为空 |
| gas | uint64 | 发送方提供的gas可用量 |
| gasPrice | uint64 | 发送方提供的gas价格 |
| hash | string | 交易哈希 |
| txIndex | uint64 | 交易索引 |
| input | string | 交易数据 |
| value | uint64 | 交易数额 |
| nonce | string | 随机数 |

### 交易回执（ txReceipt ）

| 参数名 | 类型 | 说明 |
| --- | --- | --- |
| blockHash | string | 区块哈希 |
| blockNumber | uint64 | 区块号 |
| contractAddress | string | 合约地址 |
| cumulativeGasUsed | uint64 | 区块消耗的gas总量 |
| from | string | 交易发送方地址 |
| to | string | 交易接收方地址，对于合约创建交易，该值为空 |
| gasUsed | uint64 | 本次交易消耗的gas量 |
| txHash | string | 交易哈希 |
| txIndex | uint64 | 交易索引 |
| logs | object[] | 交易执行产生的事件日志 |
| address | string | 事件触发地址 |
| topics | string[] | 订阅事件哈希 |
| data | string | 编码后的日志内容 |
| status | uint64 | 交易执行状态，1 代表成功，0 代表失败,与 root 两者取一 |
| root | string | 后交易状态根，与status两者取一 |

## 返回码

| 返回码（code） | 说明（msg） |
| --- | --- |
| 200 | 成功 |
| 500 | 失败 |
| 10001 | 通用错误码 |
| 10002 | 空指针错误 |
| 10003 | 不支持 |
| 10004 | 未知错误 |
| 10005 | 文件找不到 |
| 10006 | 文件读取失败 |
| 10007 | 文件创建失败 |
| 10008 | 文件删除失败 |
| 10009 | 记录未找到 |
| 19999 | 服务内部错误 |
| 10011 | 参数无效 |
| 10012 | 解析失败 |
| 10013 | 类型转换失败 |
| 11011 | 数据库错误 |
| 11012 | 数据库连接超时 |
| 11013 | 数据库事务操作失败 |
| 11021 | 对象标识符无效 |
| 11022 | 过滤条件无效 |
| 12011 | 链上错误 |
| 12012 | 链连接超时 |
| 12013 | 链网络连接失败 |
| 13011 | 获取链客户端失败 |
| 13012 | 无相关链信息配置 |
| 22011 | 等待交易发送超时 |
| 22012 | 发送交易失败 |
| 22013 | 交易等待处理中 |
| 22014 | 交易签名失败 |
| 22021 | 合约 Abi 无效 |
| 22022 | 合约参数解析失败 |
| 23011 | 解析 keyfile 失败 |
| 23012 | 私钥无效 |
| 23013 | 账户需要解锁 |
| 23014 | 口令加密失败 |
| 23015 | 解析私钥失败 |

**附录**

**链代理服务器使用说明**

[链代理服务器使用说明](https://git-c.i.wxblockchain.com/vena/src/venaagent/blob/integration.0.1.0.0.0/README.md)

**RPC**  **接口**

[通信协议定制路径](https://git-c.i.wxblockchain.com/vena/src/venaagent/tree/integration.0.0.1.0.0/rpc/message)

**私钥托管方案**

[密钥托管方案](https://tczezm50za.feishu.cn/docx/doxcn2G6vj9S9EX7H7UcNemnYnh)
