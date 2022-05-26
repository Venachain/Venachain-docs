# personal

## personal_listAccounts

返回当前节点所管理的账户列表。

### 参数

无

### 返回值

-   `data array` : 账户地址数组

### 示例代码

-   请求：

``` sh
curl -H "Content-Type: application/json" -X POST -d '{"jsonrpc":"2.0","method":"personal_listAccounts","params":[],"id":1}' "http://127.0.0.1:6791"
```

-   响应：

``` json
{
    "jsonrpc":"2.0",
    "id":1,
    "result":[
        "0xb7cd4fc2e71de4ee912c9f29c6983df301df932b",
        "0x6f6e346eed4981a1b74b2689877123a49ef6da31"
    ]
}
```

## personal_listWallets

返回当前节点管理的钱包列表

### 参数

无

### 返回值

-   `object array` : 钱包数组
    - url `string` : 钱包路径url
    - status `string` : 钱包锁定状态
    - failure `string` : 失败信息
    - accounts `object array` : 钱包管理的账户数组
        - address `data` : 账户地址
        - url `string` : 账户资源路径url

### 示例代码

-   请求：

``` sh
curl -H "Content-Type: application/json" -X POST -d '{"jsonrpc":"2.0","method":"personal_listWallets","params":[],"id":1}' "http://127.0.0.1:6791"
```

-   响应：

``` json
{
    "jsonrpc":"2.0",
    "id":1,
    "result":[
        {
            "url":"keystore:///home/wujingwen/workspace/go/src/venachain/release/linux/data/node-0/keystore/UTC--2022-03-14T03-25-44.975579300Z--b7cd4fc2e71de4ee912c9f29c6983df301df932b",
            "status":"Unlocked",
            "accounts":[
                {
                    "address":"0xb7cd4fc2e71de4ee912c9f29c6983df301df932b",
                    "url":"keystore:///home/wujingwen/workspace/go/src/venachain/release/linux/data/node-0/keystore/UTC--2022-03-14T03-25-44.975579300Z--b7cd4fc2e71de4ee912c9f29c6983df301df932b"
                }
            ]
        },
        {
            "url":"keystore:///home/wujingwen/workspace/go/src/venachain/release/linux/data/node-0/keystore/UTC--2022-03-14T06-03-18.350538200Z--6f6e346eed4981a1b74b2689877123a49ef6da31",
            "status":"Locked",
            "accounts":[
                {
                    "address":"0x6f6e346eed4981a1b74b2689877123a49ef6da31",
                    "url":"keystore:///home/wujingwen/workspace/go/src/venachain/release/linux/data/node-0/keystore/UTC--2022-03-14T06-03-18.350538200Z--6f6e346eed4981a1b74b2689877123a49ef6da31"
                }
            ]
        }
    ]
}
```

## personal_openWallet

启动硬件钱包打开过程，建立USB连接并尝试通过提供的密码短语进行身份验证。

```{note}
该方法可能返回需要第二次打开的额外质询（例如Trezor针矩阵质询）。 
```

### 参数

-   `string` : 钱包资源路径url
-   `string` : 钱包密码

### 返回值

无

### 示例代码

-   请求：

``` sh
curl -H "Content-Type: application/json" -X POST -d '{"jsonrpc":"2.0","method":"personal_openWallet","params":["keystore:///home/wujingwen/workspace/go/src/venachain/release/linux/data/node-0/keystore/UTC--2022-03-14T03-25-44.975579300Z--b7cd4fc2e71de4ee912c9f29c6983df301df932b", "0"],"id":1}' "http://127.0.0.1:6791"
```

-   响应：

``` json
{
    "jsonrpc":"2.0",
    "id":1,
    "result":null
}
```

## personal_newAccount

创建一个新账户。

### 参数

-   `string` : 账户密码

### 返回值

-   `data` : 账户地址

### 示例代码

-   请求：

``` sh
## 密码为0
curl -H "Content-Type: application/json" -X POST -d '{"jsonrpc":"2.0","method":"personal_newAccount","params":["0"],"id":1}' "http://127.0.0.1:6791"
```

-   响应：

``` json
{
    "jsonrpc":"2.0",
    "id":1,
    "result":"0x18d6b1740e547e8037b9d1d11ca77938ff64ad4f"
}
```

## personal_unlockAccount

将使用给定密码解锁与给定地址关联的帐户，持续时间为指定的秒数。如果持续时间为零，则将使用默认值300秒。它返回帐户是否已解锁的指示。

### 参数

-   `data` : 账户地址
-   `string` : 密码
-   `int` : 解锁持续时间，单位:秒

### 返回值

-   `bool` : 账户是否已解锁

### 示例代码

-   请求：

``` sh
curl -H "Content-Type: application/json" -X POST -d '{"jsonrpc":"2.0","method":"personal_unlockAccount","params":["0xb7cd4fc2e71de4ee912c9f29c6983df301df932b","0", 500],"id":1}' "http://127.0.0.1:6791"
```

-   响应：

``` json
{
    "jsonrpc":"2.0",
    "id":1,
    "result":true
}
```

## personal_lockAccount

锁定给定地址关联的解锁账户。

### 参数

-   `data` : 账户地址

### 返回值

-   `bool` : 成功锁定返回 true，否则返回 false

### 示例代码

-   请求

``` sh
curl -H "Content-Type: application/json" -X POST -d '{"jsonrpc":"2.0","method":"personal_lockAccount","params":["0xb7cd4fc2e71de4ee912c9f29c6983df301df932b"],"id":1}' "http://127.0.0.1:6791"
```

-   响应

``` json
{
    "jsonrpc":"2.0",
    "id":1,
    "result":true
}
```

(rpc_methods_personal_sendTransaction)=
## personal_sendTransaction

**发起交易** 将使用给定的参数创建一笔交易，并尝试用交易发起人（from）的秘钥去对这个交易做签名，然后发起这笔交易，如果给定的密码无法解密这个秘钥则交易失败。

### 参数

- `object` : 交易参数对象
    - from `data` : 20字节，发送交易的源地址
    - to `data` : 20字节，交易的目标地址，当创建新合约时可选
    - gas `quantity` : 交易执行可用gas量，可选，默认值1500000000，未用gas将返还
    - gasPrice `quantity` : gas价格，可选
    - value `quantity` : 交易发送的金额，可选
    - nonce `quantity` : 随机数，可选，可以使用同一个nonce来实现挂起的交易的重写
    - input `data` : 交易数据 
- `string` : 账户密码

``` js
params: [
    {
        "from":"0xb7cd4fc2e71de4ee912c9f29c6983df301df932b",
        "to":"0x6f6e346eed4981a1b74b2689877123a49ef6da31",
        "gas":"0x76c0", //30400
        "gasPrice":"0x9184e72a000", //10000000000000
        "value":"0x0",  //0
        "nonce":"0x1630640264",
        "input":""
    },
    "0"
]
```

### 返回值

-   `data` : 交易哈希

### 示例代码

-   请求

``` sh
curl -H "Content-Type: application/json" -X POST -d '{"jsonrpc":"2.0","method":"personal_sendTransaction","params":[{"from":"0xb7cd4fc2e71de4ee912c9f29c6983df301df932b","to":"0x6f6e346eed4981a1b74b2689877123a49ef6da31","gas": "0x76c0", "gasPrice": "0x9184e72a000","value": "0x0", "nonce": "0x1630640264","input":""}, "0"],"id":1}' "http://127.0.0.1:6791"
```

-   响应

``` json
{
    "jsonrpc":"2.0",
    "id":1,
    "result":"0xff3b0a7ed8e673b2bf38fa75e5d2f5c741eee8ec88a295c471733be7f43b6bee"
}
```

## personal_signTransaction

**签名交易** 将使用给定的参数创建一笔交易，并尝试用交易发起人（from）的秘钥去对这个交易做签名，如果给定的密码无法解密这个秘钥则会签名失败。签名后的交易会以 RLP 数据返回，不会广播到其他节点。

### 参数

请参考 [personal_sendTransaction](rpc_methods_personal_sendTransaction)

### 返回值

-   `object` : 交易签名后的数据
    - raw `string` : 签名信息
    - tx `object` : 交易信息
        - txData `object` : 交易详细信息
            - nonce `quantity` : 账户随机数
            - gasPrice `quantity` : gas价格
            - gas `quantity` : gas限制
            - to `data` : 交易目标地址
            - value `quantity` : 交易金额
            - input `data` : 交易信息
            - hash `data` : 交易哈希
            - v、r、s `quantity` : 签名值

### 示例代码

-   请求

``` sh
curl -H "Content-Type: application/json" -X POST -d '{"jsonrpc":"2.0","method":"personal_signTransaction","params":[{"from":"0x297c91ba547850222e4419b359970032c52d2fdb","to":"0x297c91ba547850222e4419b359970032c52d2fdb","gas": "0x76c0", "gasPrice": "0x9184e72a000","value": "0x0", "nonce": "0x1630640266","input":""}, "0"],"id":1}' "http://127.0.0.1:6791"
```

-   响应

``` json
{
    "jsonrpc":"2.0",
    "id":1,
    "result":{
        "raw":"0xf86c8516306402668609184e72a0008276c094297c91ba547850222e4419b359970032c52d2fdb808082027ba056d1e661bc0bb1862836e6c583d0dbf45b460f38fe9994fb33d9aecd32f3348ea053be24dd3419796dbb0851ce7935448f3c93520fd4e9f0386bbea5dbe2ee8eb5",
        "tx":{
            "nonce":"0x1630640266",
            "gasPrice":"0x9184e72a000",
            "gas":"0x76c0",
            "to":"0x297c91ba547850222e4419b359970032c52d2fdb",
            "value":"0x0",
            "input":"0x",
            "v":"0x27b",
            "r":"0x56d1e661bc0bb1862836e6c583d0dbf45b460f38fe9994fb33d9aecd32f3348e",
            "s":"0x53be24dd3419796dbb0851ce7935448f3c93520fd4e9f0386bbea5dbe2ee8eb5",
            "hash":"0x7a82ea69e6d153410a063ba3bab6cbe7fe8bc2c4174b9da12a2b4a3dbd03dc79"
        }
    }
}
```

(rpc_methods_personal_sign)=
## personal_sign

为以下内容计算 Venachain 的 ECDSA 签名：

``` sh
keccack256("\x19Venachain Signed Message:\n" + len(message) + message))
```

```{note}
生成的签名符合secp256k1曲线R、S和V值，其中V值将为27或28，这是由于传统原因。用于计算签名的密钥使用给定密码解密。
```

### 参数

-   `data` : 要签名的数据
-   `data` : 账户地址
-   `string` : 账户密码

### 返回值

-   `data` : 签名后的数据

### 示例代码

-   请求

``` sh
curl -H "Content-Type: application/json" -X POST -d '{"jsonrpc":"2.0","method":"personal_sign","params":["0xf8b0bb723012a3741d8eb2edd2c5782eee588f755bfe2a1f6b66970d35531cf2","0xb7cd4fc2e71de4ee912c9f29c6983df301df932b","0"],"id":1}' "http://127.0.0.1:6791"
```

-   响应

``` json
{
    "jsonrpc":"2.0",
    "id":1,
    "result":"0x82be6dbc2b7a3fbb38d2495ff90de7ceeae3b163793a836d89a7740f00ce24921b213718ba579798be0bba73dfa7d46d1410c287af922b8fa0c22be7bc2bd5031c"
}
```

## personal_ecRecover

返回用于创建签名的帐户的地址。

```{note}
此功能与 [venachain_sign](rpc_methods_venachain_sign) 和 [personal_sign](rpc_methods_personal_sign) 兼容。因此，它恢复了以下地址：

	hash = keccak256("\x19Venachain Signed Message:\n"${message length}${message})
	addr = ecrecover(hash, signature)

```

```{note}
签名必须符合secp256k1曲线R、S和V值，其中，出于传统原因，V值必须为27或28。
```

### 参数

-   `data` : 被签名的原数据
-   `data` : 原数据被签名后的签名数据

### 返回值

-   `data` : 用于创建签名的帐户的地址

### 示例代码

-   请求

``` sh
curl -H "Content-Type: application/json" -X POST -d '{"jsonrpc":"2.0","method":"personal_ecRecover","params":["0xf8b0bb723012a3741d8eb2edd2c5782eee588f755bfe2a1f6b66970d35531cf2","0x82be6dbc2b7a3fbb38d2495ff90de7ceeae3b163793a836d89a7740f00ce24921b213718ba579798be0bba73dfa7d46d1410c287af922b8fa0c22be7bc2bd5031c"],"id":1}' "http://127.0.0.1:6791"
```

-   响应

``` json
{
    "jsonrpc":"2.0",
    "id":1,
    "result":"0xb7cd4fc2e71de4ee912c9f29c6983df301df932b"
}
```

## personal_signAndSendTransaction

签名并发起交易 

此方法其实是 [personal_sendTransaction](rpc_methods_personal_sendTransaction) 的别名

### 参数

请参考 [personal_sendTransaction](rpc_methods_personal_sendTransaction) 

### 返回值

请参考 [personal_sendTransaction](rpc_methods_personal_sendTransaction) 

### 示例代码

请参考 [personal_sendTransaction](rpc_methods_personal_sendTransaction) 

