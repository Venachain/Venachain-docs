# 设置DAG

## 查看DAG是否开启

**参数**：

- `key`:	"IsUseDAG"

**操作**：

```bash
## 调用参数管理合约方式
./vcl contract execute 0x1000000000000000000000000000000000000004 getIntParam --param "IsUseDAG" --abi ../conf/contracts/paramManager.cpp.abi.json
```

**输出结果**：

```bash
result0:0
```

## 启用DAG

**参数**：

- `key`:      "IsUseDAG"
- `value`:    1或0，1表示开启，0表示关闭

**操作**：

```bash
## 调用参数管理合约方式
./vcl contract execute 0x1000000000000000000000000000000000000004 setIntParam --param "IsUseDAG" --param 1 --abi ../conf/contracts/paramManager.cpp.abi.json --keyfile ../conf/keyfile.json
```

**输出结果**：

```bash
result0:
{
  	"status": "Operation Succeeded",
  	"blockNumber": 12,
  	"GasUsed": 102824,
  	"From": "0x0df1787d9e9a55231c43d0f4e0c9005333376c9a",
  	"To": "0x1000000000000000000000000000000000000004",
  	"TxHash": "0xda9f469f36a34d2b9f23bfbdcd0ee18782bf448830c1c6c1d10273ccd6b1dc2f"
}
```