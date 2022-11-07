# Go SDK API 文档

```go
// Client 链 RPC 连接客户端
type Client struct {
	RpcClient *rpc.Client
	Key       *keystore.Key  //初始化后的key
	URL       *URL
}
```

在 `github.com/Venachain/client-sdk-go/client` 包下:

通用的Client 结构体包括RpcClient，可以继承 Venachian 中RpcClient 的方法，Passphrase 和 Key 为必须参数，URL 为要使用的 Venachian 的相关 IP 和 RPCPort。 其中提供了NewURL 函数来创建URL。

```go
func NewURL(ip string, port uint64) URL {...}
```

```go
type URL struct {
   IP      string
   RPCPort uint64
}
```

可以使用NewClient 方法初始化一个Client。
```go
func NewClient(ctx context.Context, url URL, keyfilePath string, passphrase string) (*Client, error) {...}
```

也可以通过传入key 来初始化Client。

```go
// 初始化keystore.Key
func NewKey(KeyfilePath, Passphrase string) (*keystore.Key, error){...}
```

```go
// 通过keystore.Key构建Client
func NewClientWithKey(ctx context.Context, url URL, key *keystore.Key) (*Client, error) {...}
```

## 使用方法: 同步获取交易回执

在 `github.com/Venachain/client-sdk-go/client` 包下:

由于初始化keystore.Key 时会消耗一部分比较大的内存，因此在使用时建议使用单例模式，通过定义一个全局的DefaultContractClient变量去调用Client 的方法。或者可以调用`NewKey(KeyfilePath, Passphrase string)` 将key定义为全局变量，然后使用`NewClientWithKey`去构建Client。具体的使用方法如下：

### 合约客户端 ContractClient

```go
// 合约客户端数据结构
type ContractClient struct {
	*Client  
	ContractContent *packet.ContractContent
	VmType          string
}
```
#### 1. 初始化合约客户端

```go
// 入参：
// url: 链的IP 和RPC 地址
// keyfilePath: keyfile.json 文件的路径
// passphrase: keyfile 文件对应的密码
// contract: 调用合约的abi文件或预编译合约地址
// vmType: 虚拟机类型
func NewContractClient(ctx context.Context, url URL, keyfilePath, passphrase, contract, vmType string)(*ContractClient, error) {...}
```

或者可以直接使用keystore.Key初始化合约客户端

```go
// 入参：
// url: 链的IP 和RPC 地址
// key: 用户的密钥，可以用NewKey(KeyfilePath, Passphrase string)函数构造
// contract: 调用合约的abi文件或预编译合约地址
// vmType: 虚拟机类型
func NewContractClientWithKey(ctx context.Context, url URL, key *keystore.Key, contract, vmType string) (*ContractClient, error){...}
```

#### **2. 定义默认的合约客户端**

```go
var DefaultContractClient *ContractClient
```

**初始化默认合约客户端示例**

使用NewContractClient()初始化合约客户端。

**示例：**

```go
// initContractClient 函数可放到main()函数中调用
// 调用成功后步骤1中的DefaultContractClient会被初始化
func initContractClient() {
	var err error
	contract := "0x0000000000000000000000000000000000000099" //存证合约
	keyfile := "/Users/cxh/go/src/github.com/Venachain/Venachain/release/linux/conf/keyfile.json"
	PassPhrase := "0"
	vm := "wasm"
	url := client.URL{
		IP:      "127.0.0.1",
		RPCPort: 6791,
	}
	DefaultContractClient, err = client.NewContractClient(context.Background(), url, keyfile, PassPhrase, contract, vm)
	if err != nil {
		log.Error("%s", err)
    return
	}
}
```

#### 3.1 调用合约 Execute

调用合约能够实现所有预编译合约合约调用的功能。

预编译合约abi 文件的路径为：`/client-sdk-go/precompiled/syscontracts`。可以根据abi文件中函数的名字和参数调用相应的预编译合约。相关的说明可参考 [**《链交互工具vcl》**](../../../5_实用工具/链交互工具vcl.rst) 查看各个预编译合约的功能。


使用`DefaultContractClient.Execute()` 调用合约。

```go
// 入参：
// funcName: 合约调用的函数名
// funcParams： 合约调用的参数
// contract：合约地址或cns名字
// 输出：上链的receipt结果
func Execute(ctx context.Context, funcName string, funcParams []string, contract string,sync bool) (interface{}, error){...}
```

**SaveEvidence 示例**

```go
func SaveEvidence(key string,value string) {
	funcname := "saveEvidence"
	funcparam := []string{}
	funcparam = append(funcparam, key)
	funcparam = append(funcparam, value)
	result, err := DefaultContractClient.Execute(context.Background(), funcname, funcparam, "0x0000000000000000000000000000000000000099",true)
	if err != nil {
		log.Error("%s", err)
		return
	}
	log.Info("result:%v", result)
	DefaultContractClient.Client.RpcClient.Close()
}
```

**GetEvidence 示例**

```go
func GetEvidence(key string) {
	funcname := "getEvidence"
	funcparam := []string{}
	funcparam = append(funcparam, key)
	result, err := DefaultContractClient.Execute(context.Background(), funcname, funcparam, "0x0000000000000000000000000000000000000099",true)
	if err != nil {
		log.Error("%s", err)
		return
	}
	log.Info("result:%v", result)
	DefaultContractClient.Client.RpcClient.Close()
}
```
**VerifyProofByRange 示例**

```go
func VerifyProofByRange(userid, proof, pid, rang string) {
	funcname := "verifyProofByRange"
	funcparam := []string{}
	funcparam = append(funcparam, userid)
	funcparam = append(funcparam, proof)
	funcparam = append(funcparam, pid)
	funcparam = append(funcparam, rang)
	result, err := DefaultContractClient.Execute(context.Background(), funcname, funcparam, "0x0000000000000000000000000000000000000100",true)
	if err != nil {
		log.Error("%s", err)
		return
	}
	log.Info("result:%v", result)
	DefaultContractClient.Client.RpcClient.Close()
}
```

**BpGetResult 示例**

```go
func VerifyProofByRange(pid string) {
	funcname := "getResult"
	funcparam := []string{}
	funcparam = append(funcparam, pid)
	result, err := DefaultContractClient.Execute(context.Background(), funcname, funcparam, "0x0000000000000000000000000000000000000100",true)
	if err != nil {
		log.Error("%s", err)
		return
	}
	log.Info("result:%v", result)
	DefaultContractClient.Client.RpcClient.Close()
}
```

#### 3.2 部署合约 Deploy

使用`DefaultContractClient.Deploy()` 调用合约。
```go
// 入参：
// abipath: 部署合约的abi 文件路径
// codepath： 部署合约的code 文件路径，比如wasm合约以.wasm结尾的文件的绝对路径
// consParams：solidity 合约的contractor参数
// sync：是否是同步获取receipt，如果是true，执行结束可得到交易的回执，如果false，执行结束可获取交易的哈希，之后再异步获取回执
// 输出：上链的receipt结果
func Deploy(ctx context.Context, abipath string, codepath string, consParams []string, sync bool) (interface{}, error){...}
```

部署合约需要使用codePath 和 ContractClient中的AbiPath (此时初始化AbiPath 时不能设置为空)。 consParams 为合约的某个参数。执行该测试函数后，成功部署了一个合约。得到返回的事件和合约地址：

```go
// 同步过去部署结果的示例
func TestContractClient_Deploy(t *testing.T) {
	codePath := "/Users/cxh/Downloads/example/example.wasm"
	abiPath := "/Users/cxh/Downloads/example/example.cpp.abi.json"
	var consParams []string
	result, err := DefaultContractClient.Deploy(context.Background(), abiPath, codePath, consParams, true)
	if err != nil {
		log.Error("error:%v", err)
	}
	log.Info("result:%v", result)
}
```

```js
// 部署合约成功的结果
{
	"status": "Operation Succeeded",
	"contractAddress": "0x35853e5643104cd96bd4590f5d4466c577786cfe",
	"logs": [
		"Event init: init success... "
	],
	"blockNumber": 198,
	"GasUsed": 1911684,
	"From": "0x3fcaa0a86dfbbe105c7ed73ca505c7a59c579667",
	"To": "",
	"TxHash": "0xacdc551f2539068eab227f112ebaeade286d75852aa27e63ecb53176489bee3f"
}
```

## 使用方法: 异步获取交易回执

### 合约客户端 ContractClient

#### 1. 初始化异步合约客户端

在`github.com/Venachain/client-sdk-go/client/asyn`包下.

异步获取receipt提供websocket 订阅区块头来为用户查询receipt的接口，sdk会在后台与链建立连接，订阅区块头的信息，再从区块hash中查找交易，再去查找交易的回执。

```go
// 异步获取回执的合约客户端
type AsynContractClient struct {
	RpcContractClient *rpcClient.ContractClient // 用于RPC连接的合约客户端
	WsClient          *WsClient                 //websocket 客户端
	Result            chan interface{}          // 获取receipt返回的结果
	Txhash            chan string               // 存储交易hash
}
```

```go
// 入参：
// ip: 链的ip地址
// rpcPort： 链的rpc端口号
// wsPort：链的websocket端口号
// keyfilePath：keyfile 文件的路径
// passphrase: keyfile 对应的密码
// contract: 要调用的合约的abi文件路径
// vmType：虚拟机的类型，wasm或者evm
// buffSize: 可处理回执的信道缓存大小，设为100即为一次可以监听100个回执
func NewAsynContractClient(ctx context.Context, ip string, rpcPort uint64, wsPort uint64, keyfilePath, passphrase, contract, vmType string, buffSize int) (*AsynContractClient, error) {...}
```

```go
// 也可以使用key来创建
func NewAsynContractClientWithKey(ctx context.Context, ip string, rpcPort uint64, wsPort uint64, key *keystore.Key, contract, vmType string, buffSize int) (*AsynContractClient, error){...}
```

#### 2.  定义默认的异步合约客户端

```go
var DefaultVenaContractClient *AsynContractClient
```

```go
// 初始化默认异步合约客户端的示例
func init() {
   var err error
   wsPort := uint64(26791)
   keyfile := "/Users/cxh/go/src/VenaChain/venachain/release/linux/conf/keyfile.json"
   PassPhrase := "0"
   key, _ := client.NewKey(keyfile, PassPhrase)
   DefaultVenaContractClient, err = asyn.NewAsynContractClientWithKey(context.Background(), "127.0.0.1", uint64(6791), wsPort, key, "0x0000000000000000000000000000000000000100", "wasm", 100)
   if err != nil {
      log.Error("err%v", err)
   }
}
```

#### 3.1 调用合约 Execute

```go
// 入参：
// funcName: 调用合约的函数名字
// funcParams: 调用合约的函数参数
// contract: 合约的地址
func ExecuteAsyncGetReceipt(ctx context.Context, funcName string, funcParams []string, contract string) error {...}
```

```go
func TestContractClient_bpAsynGetResult(t *testing.T) {
	funcname := "verifyProofByRange"
	funcparam := []string{}
	funcparam = append(funcparam, "cx1h")
	funcparam = append(funcparam,"bp param")
	funcparam = append(funcparam, "121")
	funcparam = append(funcparam, "test")
	go DefaultVenaContractClient.SubNewHeads()
	// 程序执行后关闭socket 消息
	defer DefaultVenaContractClient.WsClient.Socket.Close()
	// 处理receipt,获取结果,结果存储在AsynContractClient的result中
	go DefaultVenaContractClient.GetTxsReceipt()
	// receipt 结果放在result channel中，以下为输出result的示例
	go DefaultVenaContractClient.GetResultWithChan()
	err := DefaultVenaContractClient.ExecuteAsyncGetReceipt(context.Background(), funcname, funcparam, "0x0000000000000000000000000000000000000100")
	if err != nil {
		log.Error("error:%v", err)
	}
	time.Sleep(300 * time.Second)
}
```

#### 3.2 部署合约 Deploy

```go
// 入参：
// abipath: 合约abi 文件路径
// codepath: 合约code 文件路径
// consParams: solidity 合约的构造参数
func DeployAsyncGetReceipt(ctx context.Context, abipath string, codepath string, consParams []string) error{...}
```

```go
func TestContractClient_DeployAsyncGetReceipt(t *testing.T) {
   codePath := "/Users/cxh/Downloads/example/example.wasm"
   abiPath := "/Users/cxh/Downloads/example/example.cpp.abi.json"
   // 在wsClient 的Message 中存储收到的区块头，监听区块
   go DefaultVenaContractClient.SubNewHeads()
   // 程序执行后关闭socket 消息
   defer DefaultVenaContractClient.WsClient.Socket.Close()
   // 处理receipt,获取结果
   go DefaultVenaContractClient.GetTxsReceipt()
   // receipt 结果放在result channel中，以下为输出result的示例
   go DefaultVenaContractClient.GetResultWithChan()
   var consParams []string
   err := DefaultVenaContractClient.DeployAsyncGetReceipt(context.Background(), abiPath, codePath, consParams)
   if err != nil {
      log.Error("error:%v", err)
   }
   time.Sleep(time.Second)
   err = DefaultVenaContractClient.DeployAsyncGetReceipt(context.Background(), abiPath, codePath, consParams)
   if err != nil {
      log.Error("error:%v", err)
   }
   time.Sleep(300 * time.Second)
}
```

## 以太坊通用接口

```go
// 使用NewClient构建一个Client
func NewClient(ctx context.Context, url URL, keyfilePath string, passphrase string) (*Client, error) {...}
```

```go
// 入参：
// method：调用接口的方法
// args：调用接口的参数
func CallContext(ctx context.Context, method string, args ...interface{}) (json.RawMessage, error){...}
```

调用以太坊通用方法可参考[**以太坊RPC API手册**](http://cw.hubwiz.com/card/c/parity-rpc-api/)中的方法。例如以下使用方法：

**示例1:**

该方法表示锁定一个账户，函数名字为"personal_lockAccount"， 参数为账户地址，调用解锁账户返回的是一个bool 类型的参数，表示该账户的状态。

```go
// 使用NewClient()构建了一个Client后可调用Client.RpcClient.CallContext()
func Lock() (bool, error) {
	funcName := "personal_lockAccount"
	funcParams := "address"
	var res bool //需要根据rpc调用函数的返回结果的类型，传入json.Unmarshal 解析为结果。
	result, err := Client.RpcClient.CallContext(context.Background(), funcName, funcParams)
	if err != nil {
		return false, err
	}
	if err = json.Unmarshal(result, &res); err != nil {
		return false, err
	}
	return res, nil
}
```

**示例2:**

该方法表示解锁一个账户，函数名字为"personal_unlockAccount"， 参数为账户地址，调用解锁账户返回的是一个bool 类型的参数，表示该账户的状态。

```go
func UnLock() (bool, error) {
	funcName := "personal_unlockAccount"
	account := "0x85a4b8ad3a023fab30146fed114ea7cd6f8a4193"
  passphrase := "0"
  time := 0 // 0表示永久解锁
	var res bool 
	result, err := accountClient.RpcClient.CallContext(context.Background(), funcName, account, passphrase, time)
	if err != nil {
		return false, err
	}
	if err = json.Unmarshal(result, &res); err != nil {
		return false, err
	}
	return res, nil
}
```

## WebSocket 接口

Websocket 包连接了Client 前端和Venachain 的websocket 接口。为前端Client 提供订阅区块头和订阅事件的功能。

其相应的代码在`ws`中。

```go
// ws/type.go:21
// Client 单个 websocket 信息
type Client struct {
   Id, Group  string
   LocalAddr  string
   RemoteAddr string
   Path       string
   Socket     *websocket.Conn
   IsAlive    bool
   IsDial     bool
   RetryCnt   int64
   Message    chan []byte
}
```

`ws_subscriber.go `中的 `wsSubscriber` 负责向Venachain 发送订阅消息。`sub_msg_processor.go` 中的 `SubMsgProcessor` 负责向前端推送消息。

**使用示例：**

`example.go `中使用`gin`框架为后端实现写了一个例子。同时提供前端测试页 `ws_sub_test.html`。

```go
// 相关的ws/ws_subscriber.go:39
func NewWSSubscriber(ip string, port int64, group string) *WsSubscriber {
	return &WsSubscriber{
		wsManager: DefaultWebsocketManager,
		ip:        ip,
		port:      port,
		group:     group,
	}
}
```
使用方法，使用NewWSSubscriber方法定义一个全局DefaultWSSubscriber变量，后续调用DefaultWSSubscriber 

其中 Venachain-SDK-Go 提供了两种订阅功能：订阅区块头和订阅log。

```go
type Subscription interface {
	SubHeadForChain() error
	SubLogForChain(address, topic string) error
}
```

`SubLogForChain` 需要传递订阅合约的地址和订阅的topic。例如，如果要订阅防火墙打开的事件，可以传入以下参数：

```go
// ws/example.go:113
address := "0x1000000000000000000000000000000000000005"
topic := "0x8cd284134f0437457b5542cb3a7da283d0c38208c497c5b4b005df47719f98a1"
```

### 示例

`example.go `中使用`gin`框架为后端实现写了一个例子。同时提供前端测试页 `ws_sub_test.html`。

`"github.com/Venachain/client-sdk-go/ws" `提供了`InitWsSubscriber`函数初始化`DefaultWSSubscriber`调用相关的函数。

```go
// 入参：
// ip: 链的ip地址
// port: 链的ws 端口号
// group: 注册为对应的group
func InitWsSubscriber(ip string, port int64, group string){} 
```
**以下使用订阅区块头为例：**

外部 `main` 包中直接运行mian 函数。前端访问 `ws_sub_test.html`测试页 http://127.0.0.1:8888/api/ws/ws_sub_test.html。 

测试页的地址在: `client-sdk-go/ws/ws_sub_test.html` 中。

```go
// main.go:9
func main() {
	ws.InitWsSubscriber("127.0.0.1",26791,"venachain")
	gin.SetMode(gin.DebugMode)
	gracesRouter := ws.InitRouter()
	err := gracesRouter.Run("127.0.0.1:8888")
	if err != nil {
		logrus.Errorf("test start err: %v", err)
		return
	}
}

```

同时需要在下列函数中修改静态文件的位置

```go
func customRouter() {
   api := myRouter.Group("/api")
   {
      // websocket
      wsGroup := api.Group("/ws")
      {
         if gin.Mode() == gin.DebugMode {
           // ./ws/ws_sub_test.html需要修改为用户具体的网页文件位置
            wsGroup.StaticFile("/ws_sub_test.html", "./ws/ws_sub_test.html") 
         }
         wsGroup.GET("/log/:group", DefaultWebsocketManager.WsClientForLog)
         wsGroup.GET("/head/:group", DefaultWebsocketManager.WsClientForNewHeads)
      }
   }
}
```

可在 输入栏中输入 `ping` 查看当前的连接是否成功。如果返回 `pong`，则表示当前连接成功。此时在Venachain 中发送交易，该订阅的结果会返回到前端页面。

说明：

测试页中 `mygroup1` 为前端的group，可使用不同的group。

```javascript
var host = "ws://127.0.0.1:8888/api/ws/head/mygroup1"
```

如果不使用测试页，可以使用例如 http://coolaf.com/tool/chattest 这样的websocket 测试网页，与 `ws://127.0.0.1:8888/api/ws/head/mygroup1  `建立连接，也可查看到订阅的结果。



