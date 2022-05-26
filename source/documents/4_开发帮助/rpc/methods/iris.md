# iris

(rpc_methods_iris_getSnapshot)=
## iris_getSnapshot

返回给定块上的状态快照。

### 参数

-   number `quatity | TAG` : 整数块编号，或字符串 "earliest" 、 "latest" 或 "pending"

``` js
params: [
   '0x9', // 9
]
```

### 返回值

-   `object` : 状态快照，是在给定时间点进行授权投票的状态信息
    - number `int` : 创建该快照的区块的块高
    - hash `data` : 创建该快照的区块的哈希
    - votes `object array` 按时间顺序排列的参与投票的共识节点名单
        - validator `data` : 投票的共识节点
        - block `int` : 被投票的区块编号（区块高度）
        - address `data` : 正在投票更改其授权的帐户
        - authorize `bool` : 投票帐户是否授权或取消授权
    - tally `map[string]object` : 一种简单的计票方法，用于保持当前的票数
        - authorize `bool` : 投票是否授权或取消授权
        - votes `int` : 到目前为止希望通过该提案的票数  
    - validators `string array` : 此时的共识节点集合
    - policy `int` : 提议者选举的规则

### 示例代码

-   请求：

``` sh
curl -H "Content-Type: application/json" -X POST -d '{"jsonrpc":"2.0","method":"iris_getSnapshot","params":["0x9"],"id":1}' "http://127.0.0.1:6791"
```

-   响应：

``` json
{
    "jsonrpc":"2.0",
    "id":1,
    "result":{
        "number":9,
        "hash":"0xaace1302862c6d1f2472cb056e990c2ac9b9f51153fff3f2466f514c253938ae",
        "votes":[

        ],
        "tally":{

        },
        "validators":[
            "0x1ba074f05c7baee70847c5b5fb7f6a444b8c3c23",
            "0x682edc3215de9faa02746d908b4f13a0c0974eba",
            "0x6e5c7b3b69c467eea589d872691eea2841e82761",
            "0x81bbbb41ca72850b259e33c24190738d09c1eb43"
        ],
        "policy":0
    }
}
```

## iris_getSnapshotAtHash

返回给定哈希的块上的状态快照。

### 参数

-   `data` : 区块的哈希

``` js
params: [
   "0xaace1302862c6d1f2472cb056e990c2ac9b9f51153fff3f2466f514c253938ae",
]
```

### 返回值

-   `object` : 状态快照，是在给定时间点进行授权投票的状态信息。

详情请参照 [iris_getSnapshot](rpc_methods_iris_getSnapshot)

### 示例代码

-   请求：

``` sh
curl -H "Content-Type: application/json" -X POST -d '{"jsonrpc":"2.0","method":"iris_getSnapshotAtHash","params":["0xaace1302862c6d1f2472cb056e990c2ac9b9f51153fff3f2466f514c253938ae"],"id":1}' "http://127.0.0.1:6791"
```

-   响应：

``` json
{
    "jsonrpc":"2.0",
    "id":1,
    "result":{
        "number":9,
        "hash":"0xaace1302862c6d1f2472cb056e990c2ac9b9f51153fff3f2466f514c253938ae",
        "votes":[

        ],
        "tally":{

        },
        "validators":[
            "0x1ba074f05c7baee70847c5b5fb7f6a444b8c3c23",
            "0x682edc3215de9faa02746d908b4f13a0c0974eba",
            "0x6e5c7b3b69c467eea589d872691eea2841e82761",
            "0x81bbbb41ca72850b259e33c24190738d09c1eb43"
        ],
        "policy":0
    }
}
```

## iris_getValidators

返回参与指定区块的出块过程投票的共识节点列表。此处的投票指的是某个共识节点是否通过对某一区块的出块认证。

### 参数

-   `quantity` : 指定区块的块高

### 返回值

-   `data array` : 参与该区块出块的投票过程的共识节点地址数组

### 示例代码

-   请求:

``` sh
curl -H "Content-Type: application/json" -X POST -d '{"jsonrpc":"2.0","method":"iris_getValidators","params":["0x9"],"id":1}' "http://127.0.0.1:6791"
```

-   响应：

``` json
{
    "jsonrpc":"2.0",
    "id":1,
    "result":[
        "0x1ba074f05c7baee70847c5b5fb7f6a444b8c3c23",
        "0x682edc3215de9faa02746d908b4f13a0c0974eba",
        "0x6e5c7b3b69c467eea589d872691eea2841e82761",
        "0x81bbbb41ca72850b259e33c24190738d09c1eb43"
    ]
}
```

## iris_getValidatorsAtHash

返回参与指定区块的出块过程投票的共识节点列表。此处的投票指的是某个共识节点是否通过对某一区块的出块认证。

### 参数

-   `data` : 指定区块的哈希

### 返回值

-   `data array` : 参与该区块出块的投票过程的共识节点地址数组

### 示例代码

-   请求:

``` sh
curl -H "Content-Type: application/json" -X POST -d '{"jsonrpc":"2.0","method":"iris_getValidatorsAtHash","params":["0xaace1302862c6d1f2472cb056e990c2ac9b9f51153fff3f2466f514c253938ae"],"id":1}' "http://127.0.0.1:6791"
```

-   响应：

``` json
{
    "jsonrpc":"2.0",
    "id":1,
    "result":[
        "0x1ba074f05c7baee70847c5b5fb7f6a444b8c3c23",
        "0x682edc3215de9faa02746d908b4f13a0c0974eba",
        "0x6e5c7b3b69c467eea589d872691eea2841e82761",
        "0x81bbbb41ca72850b259e33c24190738d09c1eb43"
    ]
}
```

## iris_candidates

以指定区块的块高为划分点，返回所有区块高度小于等于该块高的 **正常共识节点** 列表。如果指定的区块不是链上的最新区块（latestBlock），则返回所有的 **正常共识节点** 列表。

### 参数

-   `quantity` : 指定区块的块高

### 返回值

-   `data array` : 参与生成指定区块的共识节点列表

### 示例代码

-   请求：

``` sh
curl -H "Content-Type: application/json" -X POST -d '{"jsonrpc":"2.0","method":"iris_candidates","params":["0x9"],"id":1}' "http://127.0.0.1:6791"
```

-   响应：

``` json
{
    "jsonrpc":"2.0",
    "id":1,
    "result":[
        "0x81bbbb41ca72850b259e33c24190738d09c1eb43",
        "0x682edc3215de9faa02746d908b4f13a0c0974eba",
        "0x1ba074f05c7baee70847c5b5fb7f6a444b8c3c23",
        "0x6e5c7b3b69c467eea589d872691eea2841e82761"
    ]
}
```
