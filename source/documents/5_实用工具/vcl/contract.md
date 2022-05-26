# 合约操作 contract

针对合约的相关操作

(vcl_contract_deploy)=
## 合约部署 contract deploy

**描述**

合约部署者将编写好的合约部署到链上。支持wasm虚拟机合约和evm虚拟机合约部署。

**参数**

-   必选参数:

``` console
<codeFile>:      合约编译后得到的二进制代码文件路径
```

-   可选参数:

``` console
--abi <file>:     合约abi文件路径，部署wasm合约必须提供，部署evm合约不需要提供
--vm value:       选择进行部署的合约（目前支持evm合约，wasm合约，默认为wasm合约）
```

**操作**

``` bash
## wasm合约
./vcl contract deploy "../../../cmd/vcl/test/test_case/wasm/contracta.wasm" --abi "../../../cmd/vcl/test/test_case/wasm/contracta.cpp.abi.json"  --keyfile ../conf/keyfile.json
## evm合约
./vcl contract deploy ../../../cmd/vcl/test/test_case/sol/storage_byzantium_065.bin --abi ../../../cmd/vcl/test/test_case/sol/storage_byzantium_065.abi -vm evm --keyfile ../conf/keyfile.json 
```

**输出结果**

-   成功

``` json
{
"status": "Operation Succeeded",
"contractAddress": "0x388d05bad3aab0fdd4a5256d4732c2129037cf19",
"blockNumber": 168,
"GasUsed": 1451477,
"From": "0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18",
"To": "",
"TxHash": ""
}
```

-   可能失败的原因，具体错误信息请参照代码
    -   rlp编码失败
    -   Http发送失败
        -   无返回结果
        -   发送出错
        -   发送成功，状态码不为200
    -   Rpc调用失败
        -   RPC JSON消息解析失败
        -   RPC调用失败:\<失败信息>
    -   交易回执查询失败
        -   查询超时
        -   交易执行失败，状态码为0x0

## 合约方法查询 contract methods

**描述**

根据合约的cns注册名或者地址查询合约方法。

**参数**

-   必选参数:

``` console
--abi <path>:      合约abi文件路径
```

**操作**

``` bash
./vcl contract methods --abi "../../../cmd/vcl/test/test_case/wasm/contracta.cpp.abi.json"
```

**输出结果**

``` console
# 查询结果
-------------------contract methods list------------------------
function: atransfer(from string,to string,asset int32)
function: atransfer1(from string,to string,asset int32) int32
function: atransfer2(from string,to string,asset int32) string
function: adcall(from string,to string,asset int32)
function: adcallInt64(from string,to string,asset int32) int32
function: adcallString(from string,to string,asset int32 string
```

(vcl_contract_execute)=
## 合约调用 contract execute

**描述**

调用并执行合约中的方法。支持wasm虚拟机合约和evm虚拟机合约方法的调用。仍然可以通过该命令实现系统合约的调用

**参数**

-   必选参数:

``` console
<contract>:           合约账户地址或合约cns注册名称
<function>:           被执行合约的具体方法，
--abi <file>:         合约abi文件路径。
```

-   可选参数:

``` console
--param value:        合约方法的入参，当有多个入参时，一个--param对应一个参数。格式:--param <value1> --param <value2>
--vm value:           选择进行执行的合约（目前支持evm合约，wasm合约，默认为wasm合约）
```

**操作**

``` bash
# 通过合约地址调用合约
## wasm合约（默认）
./vcl contract execute "0x2ee8d0545ebd20f9a992ff54cb0f21a153539206" "setName" --param wxbc  --abi "../../../cmd/vcl/test/test_case/wasm/contracta.cpp.abi.json" --keyfile ../conf/keyfile.json
## evm合约
./vcl contract execute ... ... --param --vm evm --keyfile ../conf/keyfile.json

# 通过合约名称调用合约（cns服务）
./vcl contract execute "test" "setName" --param wxbc --abi "../../../cmd/vcl/test/test_case/wasm/contracta.cpp.abi.json" --keyfile ../conf/keyfile.json
```

**输出结果**

``` bash
# 同步查询
result:Operation Succeeded
```

## 回执查询 contract receipt

**描述**

根据交易的哈希值查询交易回执。

**参数**

-   必选参数:

``` console
<tx hash>:      交易的哈希值
```

**操作**

``` bash
./vcl contract receipt 0x86d35fdd3bd67969ba71acba50076551ba8de31230b3bbfa8a536177c1610c23
```

**输出结果**

``` json
{
"blockHash": "0x308cd14101c4687b8966433f155e7272b8dbe6baa761c9b2d9e2aee225f39bad",
"blockNumber": "0xa8",
"contractAddress": "0x388d05bad3aab0fdd4a5256d4732c2129037cf19",
"cumulativeGasUsed": "0x1625d5",
"from": "0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18",
"gasUsed": "0x1625d5",
"root": "",
"to": "",
"transactionHash": "0x86d35fdd3bd67969ba71acba50076551ba8de31230b3bbfa8a536177c1610c23",
"transactionIndex": "0x0",
"logs": [],
"status": "0x1"
}
```
