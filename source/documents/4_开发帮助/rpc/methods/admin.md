# admin

## admin_peers

在协议粒度上返回当前节点所维护的已知 peer 的所有信息。

### 参数

无

### 返回值

-   `object array` : 对等网络的网络节点信息（PeerInfo）数组

    - id `string` : 节点的唯一id
    -   name `string` : 节点名称，包括客户端类型、版本、操作系统、自定义数据 
    -   caps `string array` : 该节点发布的协议  
    -   network `object` : 网络信息  
        - localAddress `string` : 本地IP地址
        - remoteAddress `string` : 远程IP地址
        - inbound `bool` : 节点是否是主动接入的
        - trusted `bool` : 是否是可信节点
        - static `bool` : 是否是静态节点 bootnodes
        - consensus: `bool` : 是否是共识节点       
    -   protocols `map[string]object` : 节点协议特定的元数据字段 
        - version `int` : 协议版本号
        - number `int` : 节点最新区块的块高
        - head `data` : 节点最新区块的哈希

### 示例代码

-   请求：

``` sh
curl -H "Content-Type: application/json" -X POST -d '{"jsonrpc":"2.0","method":"admin_peers","params":[],"id":1}' "http://127.0.0.1:6791"
```

-   响应：

``` json
{
    "jsonrpc":"2.0",
    "id":1,
    "result":[
        {
            "id":"2b860b6eece87328a2f183ab520875437b3b138ecefc06b9fc7750bc382d03d6a4d9f3d1331b0726600724bd18b777a9c9dfc9d07b652cbed2da1bdde066f703",
            "name":"Venachain/venachain/v1.0.1-stable-74f96652/linux-amd64/go1.16.7",
            "caps":[
                "vena/1"
            ],
            "network":{
                "localAddress":"127.0.0.1:16791",
                "remoteAddress":"127.0.0.1:56908",
                "inbound":true,
                "trusted":false,
                "static":false,
                "consensus":false
            },
            "protocols":{
                "vena":{
                    "version":1,
                    "number":0,
                    "head":"0x97cd651449fdc57aae5640a53f4ea112fcd1ab636bbdc5d511ff048e1b4a762a"
                }
            }
        },
        {
            "id":"2ef456b0da676a750db55c7d3eb84c547466b7e1179c6a685a89bc7051487a1ba6a23a6fe2cc2fb38dd7a90810702427f482f023e5b9d04b61694ef9383459f5",
            "name":"Venachain/venachain/v1.0.1-stable-74f96652/linux-amd64/go1.16.7",
            "caps":[
                "vena/1"
            ],
            "network":{
                "localAddress":"127.0.0.1:16791",
                "remoteAddress":"127.0.0.1:56898",
                "inbound":true,
                "trusted":false,
                "static":false,
                "consensus":false
            },
            "protocols":{
                "vena":{
                    "version":1,
                    "number":0,
                    "head":"0x97cd651449fdc57aae5640a53f4ea112fcd1ab636bbdc5d511ff048e1b4a762a"
                }
            }
        },
        {
            "id":"4956f5120a054a523c628749cedabdab6165bfa27799db09bd687b014ad16d69ac9eb073dfb7a7461575897181069c597be6b6a14b46be56349675c364a2bf06",
            "name":"Venachain/venachain/v1.0.1-stable-74f96652/linux-amd64/go1.16.7",
            "caps":[
                "vena/1"
            ],
            "network":{
                "localAddress":"127.0.0.1:16791",
                "remoteAddress":"127.0.0.1:56888",
                "inbound":true,
                "trusted":false,
                "static":false,
                "consensus":false
            },
            "protocols":{
                "vena":{
                    "version":1,
                    "number":8,
                    "head":"0xf99a8db5dd135b50aa16cd084418e046cd4c6b5439233d99b5bec69a58a22aa1"
                }
            }
        }
    ]
}
```

## admin_nodeInfo

在协议粒度上返回当前节点的所有信息。

### 参数

无

### 返回值

-   `object` : 主机节点信息（NodeInfo)
    - id `string` : 节点的唯一id（也是加密密钥）
    - name `string` : 节点的名称，包括客户端类型、版本、操作系统、自定义数据 
    - enode `string` : 用于从远程 peer 添加此 peer 的URL 
    - ip `string` : 节点的 IP 地址
    - ports `object` : 节点端口信息
        - discovery `int` : 节点发现协议监听的 UDP 端口号
        - listener `int` : RLPx 监听的 TCP 端口号
    - listenAddr `string` 监听地址
    - protocols `map[string]object` : 节点协议特定的元数据字段
        - network `int` : 网络状态
        - genesis `data` : genesis哈希
        - config `object` : 协议配置信息
            - chainId `int` : 链id
            - istanbul `object` : 共识配置信息
                - timeout `int` : 超时时间，单位:毫秒
                - period `int` : 共识周期，单位:秒
                - firstValidatorNode `string` : 第一个共识节点信息 
            - interpreter `string` : VM编译器支持配置，all表示支持 wasm 虚拟机和 solidity 虚拟机
        - head `data` : 节点最新区块的哈希

### 示例代码

-   请求：

``` sh
curl -H "Content-Type: application/json" -X POST -d '{"jsonrpc":"2.0","method":"admin_nodeInfo","params":[],"id":1}' "http://127.0.0.1:6791"
```

-   响应：

``` json
{
    "jsonrpc":"2.0",
    "id":1,
    "result":{
        "id":"3dc8055a53544d0cd899bcf5c7c2a9c44a4cddb4aa368b84b25867d78a4197a7d1479974987fcb79635046b75e3c88a916e2bd1373e18410d47758ec347ec359",
        "name":"Venachain/venachain/v1.0.1-stable-74f96652/linux-amd64/go1.16.7",
        "enode":"enode://3dc8055a53544d0cd899bcf5c7c2a9c44a4cddb4aa368b84b25867d78a4197a7d1479974987fcb79635046b75e3c88a916e2bd1373e18410d47758ec347ec359@[::]:16791?discport=0",
        "ip":"::",
        "ports":{
            "discovery":0,
            "listener":16791
        },
        "listenAddr":"[::]:16791",
        "protocols":{
            "vena":{
                "network":1,
                "genesis":"0x97cd651449fdc57aae5640a53f4ea112fcd1ab636bbdc5d511ff048e1b4a762a",
                "config":{
                    "chainId":300,
                    "istanbul":{
                        "timeout":10000,
                        "period":1,
                        "firstValidatorNode":"enode://3dc8055a53544d0cd899bcf5c7c2a9c44a4cddb4aa368b84b25867d78a4197a7d1479974987fcb79635046b75e3c88a916e2bd1373e18410d47758ec347ec359@127.0.0.1:16791"
                    },
                    "interpreter":"all"
                },
                "head":"0xaace1302862c6d1f2472cb056e990c2ac9b9f51153fff3f2466f514c253938ae"
            }
        }
    }
}
```

## admin_datadir

返回当前节点正在使用的数据目录。

### 参数

无

### 返回值

-   `string` : 目录字符串

### 示例代码

-   请求：

``` sh
curl -H "Content-Type: application/json" -X POST -d '{"jsonrpc":"2.0","method":"admin_datadir","params":[],"id":1}' "http://127.0.0.1:6791"
```

-   响应：

``` json
{
    "jsonrpc":"2.0",
    "id":1,
    "result":"/home/wujingwen/workspace/go/src/venachain/release/linux/data/node-0"
}
```
