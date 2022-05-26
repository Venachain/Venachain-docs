# VenaProof 接口文档

| **时间**   | **修改人** | **修改事项** | **存证平台版本** | **文档版本** |
| ---------- | ---------- | ------------ | ---------------- | ------------ |
| 2022.05.11 | 吴经文     | 初稿         | 0.0.1            | 1.0          |

## 一、存证相关

### 存证查询 /api/evidence/id/:id

方法：GET

#### Description

根据存证ID查询存证详细信息

#### Parameters

| Name       | Located in | Description | Required | Schema |
| ---------- | ---------- | ----------- | -------- | ------ |
| evidenceID | path       | 存证ID      | Yes      | string |

#### Responses

| Code | Description | Schema                                                       |
| ---- | ----------- | ------------------------------------------------------------ |
| 200  | OK          | [model.HttpResult](venaproof_api_httpresult) (Data: [mode.EvidenceDetailVO](venaproof_api_evidencedetailvo)) |
| 400  | Bad Request | [model.HttpResult](venaproof_api_httpresult)                         |

示例

```bash
curl -X GET "http://127.0.0.1:17017/api/evidence/id/test" -H "accept: application/json" -H "Content-Type: application/json" -d "{}"
```

响应

```json
{
    "code":200,
    "data":{
        "evidenceID":"test",
        "time":"2022-05-13 14:17:08",
        "evidenceHash":"0x0000000000000000000000000000000000000000000000000000000074657374",
        "evidenceValue":"test",
        "blockNumber":31,
        "blockHash":"0x4985b4808ccf4a50f96810b968f4616cb2f74d8c07842ed96679a1d32584032b",
        "txHash":"0xd3f5bc8027f11db3a22139eb6ed2ed8415b37cdd3dd0dabff436580e9b5afe13"
    },
    "msg":"success",
    "request":"[GET] /api/evidence/id/test"
}
```

### 存证存储 /api/evidence/save

#### Description

将给定的存证内容上链存储

#### Parameters

| Name            | Located in | Description | Required | Schema                                         |
| --------------- | ---------- | ----------- | -------- | ---------------------------------------------- |
| SaveEvidenceDTO | body       | 存证内容    | Yes      | [model.SaveEvidenceDTO](venaproof_api_saveevidencedto) |

#### Responses

| Code | Description | Schema                                                       |
| ---- | ----------- | ------------------------------------------------------------ |
| 200  | OK          | [model.HttpResult](venaproof_api_httpresult) (Data: [mode.EvidenceDetailVO](venaproof_api_evidencedetailvo)) |
| 400  | Bad Request | [model.HttpResult](venaproof_api_httpresult)                         |

示例

```bash
curl -X POST "http://127.0.0.1:17017/api/evidence/save" -H "accept: application/json" -H "Content-Type: application/json" -d "{\"evidenceID\": \"\", \"evidenceValue\": \"20200513\"}"
```

响应

```json
{
    "code":200,
    "data":{
        "evidenceID":"0x726f6f663132372e302e302e3132303230203520313331363532343233363537",
        "time":"2022-05-13 14:34:19",
        "evidenceHash":"0x0000000000000000000000000000000000000000000000000032303230353133",
        "evidenceValue":"20200513",
        "blockNumber":32,
        "blockHash":"0x22d3dd3adf288c0c1608d45226f2b508d64cff870ece0ee012652bcf03f09b6f",
        "txHash":"0x7b3d3a17e182f581376af28661358b34ec6f42163c9ba289ec2c13cb016741ad"
    },
    "msg":"success",
    "request":"[POST] /api/evidence/save"
}
```

## 二、Models

(venaproof_api_saveevidencedto)=
### model.SaveEvidenceDTO

| Name          | Type   | Description | Required |
| ------------- | ------ | ----------- | -------- |
| EvidenceID    | string | 存证ID      | No       |
| EvidenceValue | string | 存证内容    | Yes      |

(venaproof_api_httpresult)=
### model.HttpResult

| Name    | Type        | Description    | Required |
| ------- | ----------- | -------------- | -------- |
| Code    | int         | HTTP响应状态码 | No       |
| Data    | interface{} | 返回数据       | No       |
| Msg     | string      | 提示信息       | No       |
| Request | string      | 请求资源路径   | No       |

(venaproof_api_evidencedetailvo)=
### mode.EvidenceDetailVO

| Name          | Type   | Description                           | Required |
| ------------- | ------ | ------------------------------------- | -------- |
| EvidenceID    | string | 存证ID，存证合约中存储的存证key       | No       |
| FormatTime    | string | 格式化的存证时间，yyyy-MM-dd HH:mm:ss | No       |
| EvidenceHash  | string | 存证内容哈希                          | No       |
| EvidenceValue | string | 存证内容，存证合约中存储的存证value   | No       |
| BlockNumber   | uint64 | 存证交易所在区块的高度                | No       |
| BlockHash     | string | 存证交易所在区块的哈希                | No       |
| TxHash        | string | 存证交易的哈希                        | No       |