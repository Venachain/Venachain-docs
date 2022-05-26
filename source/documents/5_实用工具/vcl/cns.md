# 合约命名系统 cns 
针对合约命名系统服务的相关操作。

## cns解析 cns resolve

**描述**

通过合约名称以及版本号（默认为"latest"）解析出对应的账户地址。一个合约名可以对应多个（在注册的）合约地址，通过版本号解析出对应的合约地址。

**参数**

-   必选参数:

``` bash
<name>:     合约在cns中注册的合约名称
```

-   可选参数:

``` bash
--version string:     合约在cns中注册的版本号，默认为"latest"
```

**操作**

``` bash
# 查询最新版本
./vcl cns resolve "test"  --keyfile ../conf/keyfile.json

# 查询指定版本
./vcl cns resolve "test" --version "1.0.0.0"  --keyfile ../conf/keyfile.json
```

**输出结果**

``` console
result: <address>
```

## cns注册 cns register

**描述**

将合约注册到cns平台中，注册后的合约不仅可以通过合约账户地址进行调用执行，还可以通过其对应的合约名称进行执行。

**参数**

-   必选参数:

``` bash
<name>:          在cns中注册的合约名称
<version>:       在cns中注册的版本号，（补充）。格式:"X.X.X.X"
<address>:       进行注册的合约的账户地址
```

**操作**

``` bash
./vcl cns register "test" "1.0.0.0" "0x2ee8d0545ebd20f9a992ff54cb0f21a153539206"  --keyfile ../conf/keyfile.json
```

**输出结果**

``` json
{
  "status": "Operation Succeeded",
  "logs": [
      "Event [CNS] Notify: 0 [CNS] cns register succeed "
  ],
  "blockNumber": 190,
  "GasUsed": 105856,
  "From": "0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18",
  "To": "0x0000000000000000000000000000000000000011",
  "TxHash": ""
}
```

## cns重定向 cns redirect

**描述**

指定cns名称对应的当前合约版本。

**参数**

-   必选参数:

``` bash
<name>:          在cns中注册的合约名称
<version>:       在cns中注册的版本号。格式:"X.X.X.X"
```

**操作**

``` bash
./vcl cns redirect "test" "1.0.0.1"  --keyfile ../conf/keyfile.json
```

**输出结果**

``` json
{
    "status": "Operation Succeeded",
    "logs": [
            "Event [CNS] Notify: 0 [CNS] cns redirect succeed "
    ],
    "blockNumber": 91,
    "GasUsed": 102932,
    "From": "0xd7ca86207614e5c7f10f910bd4e8b4bf7c07b12d",
    "To": "0x0000000000000000000000000000000000000011",
    "TxHash": "0xd3253b7a6e3814f71fcd26ea47fc0678f0d7e39aff2212469cab49626f76ef4d"
}
```

## cns信息查询 cns query

**描述**

根据查询键值以及辅助选项进行cns注册信息的筛选查询，返回所有匹配成功的数据对象。

**参数**

-   可选参数:

``` bash
--all:                             查询键，显示所有cns中所有注册的对象
--contract <name or address>:      查询键，通过合约账户地址或者合约名称进行查询
--addr <address>:                  查询键，通过用户账户地址进行查询，查询该用户注册在cns的合约
--pageNum:                         展示页面页码
--pageSize:                        展示页面大小（当pageSize为0时，则默认查询所有数据）
```

**操作**

``` bash
# 1 查询所有已注册的合约
./vcl cns query --all 
# 2 查询指定范围内已注册的合约
./vcl cns query --pageNum "0" --pageSize "10"
# 3 通过合约名称进行查询
./vcl cns query --contract "test" 
# 4 通过合约地址进行查询
./vcl cns query --contract "0x12a0de8326d814e1569d6a0e111be02b19741694"
# 5 通过注册者进行查询
./vcl cns query --addr "0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18"
```

**输出结果**

``` json
{
  "code":0,
  "msg":"success",
  "data":[{
      "name":"eeeee",
      "version":"0.0.0.1",
      "address":"0x12a0de8326d814e1569d6a0e111be02b19741694",
      "origin":"0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18",
      "create_time":1600758772
  },
  {
      "name":"tofu",
      "version":"0.0.0.1",
      "address":"0x9185686d2a1fc1bbadaba646d7323f597fae0073",
      "origin":"0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18",
      "create_time":1600761759
  },
  {
      "name":"test",
      "version":"0.0.0.2",
      "address":"0x12a0de8326d814e1569d6a0e111be02b19741694",
      "origin":"0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18",
      "create_time":1600918255
  },
  {
      "name":"test",
      "version":"0.0.0.3",
      "address":"0xdb907806b906cfaa9049e5774e03263c6ff203e8",
      "origin":"0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18",
      "create_time":1601350402
  },
  {
      "name":"damn",
      "version":"0.0.0.1",
      "address":"0xe3471eace6b0eca6150d3a41051d8c7212c35da7",
      "origin":"0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18",
      "create_time":1601364209
  },
  {
      "name":"ljj",
      "version":"1.0.0.0",
      "address":"0x388d05bad3aab0fdd4a5256d4732c2129037cf19",
      "origin":"0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18",
      "create_time":1602234874
  }]
}
```

## cns状态查询 cns state

**描述**

通过查询键查询一个合约在cns平台中的注册状态，注册状态分为已注册（true）和未注册（false）。

**参数**

-   必选参数:

``` bash
<contract>:         查询键，根据合约账户地址或合约账户名称进行查询
```

**操作**

``` bash
# 查询合约地址是否注册
./vcl cns state "0x2ee8d0545ebd20f9a992ff54cb0f21a153539206" --keyfile ../conf/keyfile.json
# 查询合约名称是否被注册
./vcl cns state "test" --keyfile ../conf/keyfile.json
```

**输出结果**

``` console
# 已注册
result: the contract is registered in CNS
# 未注册
result: the contract is not registered in CNS
```
