# 节点操作 node

针对节点的相关操作

```{note}
  - `--type string` :    节点类型，1为共识节点，2位观察者节点，3为轻节点。初次加入链网络的节点都为观察节点，观察节点可以由管理员设置为共识节点。
  - `--status string` :  节点状态，1为有效状态，2为无效（删除）状态
  - 初始化时都是固定常数值，可以省略
```

## 节点添加 node add

**描述**

将节点添加到Venachain网络中。没有被管理员添加到节点列表的节点无法参与Venachain网络中节点的区块同步，共识等等。第一次被添加的节点类型都为观察者节点。观察者节点后续可以由管理员修改成为共识节点参与共识。

```{note}
-   如果节点列表中已有同名且状态为有效的节点，则注册失败。
-   如果节点列表中已有同公钥的节点（无论节点状态），则注册失败。
```

**参数**

-   必选参数:

``` bash
<name>:            节点名称
<publicKey>:       节点公钥，用于节点间安全通信。节点的公私钥对可由venakey工具产生。
<externalIP>:      节点外网IP
<internalIP>:      节点内网IP
```

-   可选参数:

``` bash
--rpcPort int<num>:      用于rpc远程调用的网络端口，默认端口6791
--p2pPort int<num>:      用于p2p通信的网络端口，默认端口16791
--desc string:           节点描述
--delayNum <num>:        共识节点延迟设置的区块高度，默认实时设置
```

**操作**

``` bash
./vcl node add "test" "feffe2938d427088f5fcce94a9245760b92c468d3ca25ab5ef2b1cdccf0ed911963b74ca2dffef20ef135966e34ebcc905d1f12c1df09f05974a617cf8afe8e8" "127.0.0.1" "127.0.0.1" --keyfile ../conf/keyfile.json
```

**输出结果**

``` json
{
  "status": "Operation Succeeded",
  "logs": [
      "Event Notify: 0 add node success. node:{\"name\":\"test\",\"owner\":\"\",\"desc\":\"\",\"type\":0,\"status\":1,\"externalIP\":\"127.0.0.1\",\"internalIP\":\"127.0.0.1\",\"publicKey\":\"feffe2938d427088f5fcce94a9245760b92c468d3ca25ab5ef2b1cdccf0ed911963b74ca2dffef20ef135966e34ebcc905d1f12c1df09f05974a617cf8afe8e8\",\"rpcPort\":6791,\"p2pPort\":16791} "
  ],
  "blockNumber": 234,
  "GasUsed": 118776,
  "From": "0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18",
  "To": "0x1000000000000000000000000000000000000002",
  "TxHash": ""
}
```

## 节点删除 node delete

**描述**

将节点从节点列表中删除。在下一次peers更新后，被删除的节点会被Venachain网络中的其他节点断开连接。

**参数**

-   必选参数:

``` bash
<name>:        节点名称
```

**操作**

``` bash
./vcl node delete "test" --keyfile ../conf/keyfile.json
```

```{note}
-   不存在用户直接修改status的情况。确保status只能从1-\>2。
-   状态修改后，节点的完整信息依旧可以通过query命令查询到
```

**输出结果**

``` json
{
  "status": "Operation Succeeded",
  "logs": [
      "Event Notify: 0 update node success. info:{\"status\":2} "
  ],
  "blockNumber": 235,
  "GasUsed": 102932,
  "From": "0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18",
  "To": "0x1000000000000000000000000000000000000002",
  "TxHash": ""
} 
```

## 节点信息查询 node query

**描述**

通过查询键对节点信息进行查询，返回匹配成功的数据对象。

**参数**

-   可选参数:

``` bash
--all                 查询键，查询所有节点(包含已被删除的节点)
--name string:        查询键，通过节点名称进行查询（返回结果可能不唯一）
--status string:      查询键，通过节点状态进行查询。valid(1)为有效状态，invalid(2)为无效（删除）状态
--type string:        查询键，通过节点类型进行查询。consensus(1)为共识节点，observer(2)为共识节点，lightnode(3)为轻节点
--publicKey string:   查询键，通过节点公钥进行查询（返回结果唯一）
```

**操作**

``` bash
## 返回网络中所有节点
./vcl node query --all --keyfile ../conf/keyfile.json
## 根据查询键进行搜索
./vcl node query --name "test" --keyfile ../conf/keyfile.json

./vcl node query --status "valid" --keyfile ../conf/keyfile.json

./vcl node query --type "consensus" --keyfile ../conf/keyfile.json

./vcl node query -publicKey feffe2938d427088f5fcce94a9245760b92c468d3ca25ab5ef2b1cdccf0ed911963b74ca2dffef20ef135966e34ebcc905d1f12c1df09f05974a617cf8afe8e8 --keyfile ../conf/keyfile.json 
## 组合查询
./vcl node query --status "valid" --name "root" --keyfile ../conf/keyfile.json
```

**输出结果**

读操作

``` console
result: %s
```

示例

``` js
{
  "code":0,
  "msg":"success",
  "data":[{
    "name": ...,
    "owner": ...,
    "desc": ...,
    "type": ...,
    "publickey": ...,
    "externalIP": ...,
    "internalIP": ...,
    "rpcPort": ...,
    "p2pPort": ...,
    "status": ...,
    "delynum": `omitempty`
    }
  ]
}
// 无 approver 字段
```

## 节点统计 node stat

**描述**

通过查询键对节点信息进行查询，对匹配成功的数据对象进行统计，返回统计值。

**参数**

-   可选参数:

``` bash
--status string:    查询键，通过节点状态进行统计。"valid"为有效状态(1)，"invalid"为无效（删除）状态(2)
--type string:      查询键，通过节点类型进行统计。consensus(1)为共识节点，observer(2)为共识节点，lightnode(3)为轻节点
```

**操作**

``` bash
# 指定公钥对应的节点数目
./vcl  node stat --status "valid" --keyfile ../conf/keyfile.json
```

**输出结果**

``` console
# 读操作
* result: <num>
```

## 节点更新 node update

**描述**

-   更新节点的 `desc` 、 `delayNum` 与 `type` 字段中的信息。无法更新权限同级及其以上角色节的信息。
-   状态无效的节点依旧可以更新相应信息(bug?)

**参数**

-   必选参数:

``` bash
<name>:            节点名称
```

-   可选参数:

``` bash
--desc string:     节点描述
--type string:     节点类型，consensus(1)为共识节点，observer(2)为共识节点，lightnode(3)为轻节点
--delay <num>:     共识节点延迟设置的区块高度，默认实时设置
```

**操作**

``` bash
# 更新节点type信息
./vcl  node update "test" --type "consensus" --keyfile ../conf/keyfile.json
# 更新节点desc信息
./vcl  node update "test" --desc "this is a description" --keyfile ../conf/keyfile.json
  # 更新节点delayNum信息
./vcl  node update "test" --delay 10 --keyfile ../conf/keyfile.json
```

**输出结果**

``` console
# 同步查询
result: NodeManager update key: type
```
