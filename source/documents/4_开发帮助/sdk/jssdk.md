# JS-SDK

## 安装与使用

### **安装**

在packege.json依赖中加入：

```
npm install git+https://git-c.i.wxblockchain.com/vena/src/vena-sdk-js.git -s
```

或者**直接将 vena.bundle.js拷贝到项目中**（TypeScript 还需要拷贝 vena.bundle.d.ts，Flutter请使用bundle.js）。

### **使用**

**支持 TypeScript  ！！！**

可以使用HTTP和Websocket两种方式连接节点。使用bundle推荐动态import，并开启 g-zip。

```
// 通过npm install 方式安装,直接引用
import { Web3ForVena } from 'venachain-sdk'; 

// 使用bundle.js
import { Web3ForVena } from 'vena.bundle';
let web3 = new Web3ForVena('http://localhost:6791'); 

//或者使用import方法动态加载bundle.js,分割代码。防止打包后主文件过大。
let web3；
import('vena.bundle')
.then(({ Web3ForVena }) => web3 = new Web3ForVena('ws://localhost:26791'));
```

## API

### **sha3**( value:string ) : string

计算输入字符串的sha3。

```
web3.sha3('234');//"0xc1912fee45d61c87cc5ea59dae311904cd86b84fee17cc96966216f811ce6a79"
```

### getPublicKeyfromPrivate(privateKey: string): string 

根据私钥计算公钥。

```
web3.getPublicKeyfromPrivate('0x.....');
```

### getAddressfromPublicKey(publicKey: string): string 

根据公钥计算地址。

```
web3.getAddressfromPublicKey('0x.....');
```

### getAddressfromPrivateKey(privateKey: string): string 

根据私钥计算地址。

```
web3.getAddressfromPrivateKey('0x.....');
```

### getAccountFromPrivateKey(privateKey: string): Account 

根据私钥得到账户信息。

```
web3.getAccountFromPrivateKey('0x.....');
> 
{
    address: "0x.....",
    privateKey: "0x.....",
    signTransaction: function(tx){...},
    sign: function(data){...},
    encrypt: function(password){...}
}
```

### unlockNodeAccount(address: string, pwd: string, unlockDuration: number): Promise< any > 

解锁节点保存的账户。

```
web3.unlockNodeAccount("0x.....", "test password!", 600)
.then(console.log);
> true
```

### createNodeAccount(pwd: string): Promise< string > 

创建由节点控制的账户并返回账户地址

```
web3.createNodeAccount('pwd').then(console.log);
> '0x......'
```

### getNodeAccounts(): Promise< string[] > 

通过RPC接口`personal_listAccounts`得到由节点控制的账户列表. 

```
web3.getNodeAccounts().then(console.log);
> ["0x.....", "0x....."]
```

### createLocalAccount(entropy?: string): Account 

创建本地帐户。

```
web3.createLocalAccount();
>
{
    address: "0x.....",
    privateKey: "0x.....",
    signTransaction: function(tx){...},
    sign: function(data){...},
    encrypt: function(password){...}
}
```

### encryptAccount(privateKey: string, pwd: string): EncryptedKeystoreV3Json 

通过密码加密私钥，生成v3 keystore json。

```
web3.encryptAccount('0x4c0883a69102937d6231471b5dbb6204fe5129617082792ae468d01a3f362318', 'test!');
>
{
    version: 3,
    id: '04e9bcbb-96fa-497b-94d1-14df4cd20af6',
    address: '2c7536e3605d9c16a7a3d7b1898e529396a65c23',
    crypto: {
        ciphertext: 'a1c25da3ecde4e6a24f3697251dd15d6208520efc84ad97397e906e6df24d251',
        cipherparams: { iv: '2885df2b63f7ef247d753c82fa20038a' },
        cipher: 'aes-128-ctr',
        kdf: 'scrypt',
        kdfparams: {
            dklen: 32,
            salt: '4531b3c174cc3ff32a6a7a85d6761b410db674807b2d216d022318ceee50be10',
            n: 262144,
            r: 8,
            p: 1
        },
        mac: 'b8b010fff37f9ae5559a352a185e86f9b9c1d7f7a9f1bd4e82a5dd35468fc7f6'
    }
}
```

### decryptAccount(keystoreJsonV3: EncryptedKeystoreV3Json, pwd: string): Account 

根据密码解锁V3 keystore，并生成账户。

```
let keystore = {
    version: 3,
    id: '04e9bcbb-96fa-497b-94d1-14df4cd20af6',
    address: '2c7536e3605d9c16a7a3d7b1898e529396a65c23',
    crypto: {
        ciphertext: 'a1c25da3ecde4e6a24f3697251dd15d6208520efc84ad97397e906e6df24d251',
        cipherparams: { iv: '2885df2b63f7ef247d753c82fa20038a' },
        cipher: 'aes-128-ctr',
        kdf: 'scrypt',
        kdfparams: {
            dklen: 32,
            salt: '4531b3c174cc3ff32a6a7a85d6761b410db674807b2d216d022318ceee50be10',
            n: 262144,
            r: 8,
            p: 1
        },
        mac: 'b8b010fff37f9ae5559a352a185e86f9b9c1d7f7a9f1bd4e82a5dd35468fc7f6'
    }
};
web3.decryptAccount(keystore,'pwd');
>
{
    address: "0x.....",
    privateKey: "0x.....",
    signTransaction: function(tx){...},
    sign: function(data){...},
    encrypt: function(password){...}
}
```

### sendTransactionByNodeAccount(transactionObject: TransactionObject): Promise< any > 

使用节点账户发起交易，不需要本地签名。

```
let txObject = {
    from: '0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe',
    to: '0x11f4d0A3c12e86B4b5F39B213F7E19D048276DAe',
    value: '1000000000000000'
};
web3.sendTransactionByNodeAccount(txObject).then(console.log);
>
{
  "status": true,
  "transactionHash": "0x........",
  "transactionIndex": 0,
  "blockHash": "0x......",
  "blockNumber": 3,
  "contractAddress": "0x.....",
  "cumulativeGasUsed": 314159,
  "gasUsed": 30234,
  "logs": [{
         ...
     }, ...]
}
```

### signAndSendTransactionByLocalAccount(transactionObject: TransactionObject, privateKey: string): Promise< any > 

使用本地账户对交易进行签名并发起交易。

```
let txObject = {
    from: '0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe',
    to: '0x11f4d0A3c12e86B4b5F39B213F7E19D048276DAe',
    value: '1000000000000000'
};
web3.signAndSendTransactionByLocalAccount(txObject,'0x......')
.then(console.log);
>
{
  "status": true,
  "transactionHash": "0x........",
  "transactionIndex": 0,
  "blockHash": "0x......",
  "blockNumber": 3,
  "contractAddress": "0x.....",
  "cumulativeGasUsed": 314159,
  "gasUsed": 30234,
  "logs": [{
         ...
     }, ...]
}
```

### signTransaction(transactionObject: TransactionObject, privateKey: string): Promise< any > 

对交易进行签名。签名结果中的raw可以通过 sendSignedTransaction() 发起交易。

```
let txObject = {
    from: '0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe',
    to: '0x11f4d0A3c12e86B4b5F39B213F7E19D048276DAe',
    value: '1000000000000000'
};
web3.signTransaction(txObject,'0x......')
.then(console.log);
>
{
    messageHash: '0x...',
    v: '0x27b',
    r: '0x4e8813f557abf684d52aff2313247d2b7c8d49ce90a7e9b84a273f03eaaaa397',
    s: '0x684254ecb2d5ada441af3e9d7ed82fc81b0b473147fc4a8e8abedfdd37a697e2',
    rawTransaction: '0xf...',
    transactionHash: '0xd0b0610c97f642a9e333953937066c295f36e947c02abdbb85de5be2937712c4'
}
```

### getTransactionReceipt(transactionHash: string): Promise< any > 

根据交易哈希获取交易结果。

```
web3.getTransactionReceipt('0x.....')
.then(console.log);
>
{
  "status": true,
  "transactionHash": "0x.......",
  "transactionIndex": 0,
  "blockHash": "0x........",
  "blockNumber": 3,
  "contractAddress": "0x.......",
  "cumulativeGasUsed": 314159,
  "gasUsed": 30234,
  "logs": [{
         
     }, ...]
}
```

### getTransaction(transactionHash: string): Promise< any > 

根据交易哈希获取交易信息。

```
web3.getTransaction('0x.....')
.then(console.log);
>
{
    "hash": "0x9fc76417374aa880d4449a1f7f31ec597f00b1f6f3dd2d66f4c9c6c445836d8b",
    "nonce": 2,
    "blockHash": "0xef95f2f1ed3ca60b048b4bf67cde2195961e0bba6f70bcbea9a2c4e133e34b46",
    "blockNumber": 3,
    "transactionIndex": 0,
    "from": "0xa94f5374fce5edbc8e2a8697c15331677e6ebf0b",
    "to": "0x6295ee1b4f6dd65047762f924ecd367c17eabf8f",
    "value": '123450000000000000',
    "gas": 314159,
    "gasPrice": '2000000000000',
    "input": "0x57cb2fc4"
}
```

### sendSignedTransaction(rawTransaction: string): Promise< any > 

将已经签名的交易发送出去。

```
web3.sendSignedTransaction('0x.......').then(console.log);
>
{
  "status": true,
  "transactionHash": "0x.......",
  "transactionIndex": 0,
  "blockHash": "0x........",
  "blockNumber": 3,
  "contractAddress": "0x.......",
  "cumulativeGasUsed": 314159,
  "gasUsed": 30234,
  "logs": [{
         
     }, ...]
}
```

### buildContractInstance(address: string, abi: string): Promise< any > 

通过ABI构建合约实例。调用合约方法如果为const方法第一个参数为账户的地址，非const第一个参数为私钥。第二个参数为该方法需要的参数。

```
const contract = web3.buildContractInstance(contractAddress, abi);
contract.myfunction(privateKey,[param1,param2...])
.then(rec => web3.decodeInvokeContractLog(abi, rec.logs[0], true));

```

### callContract(contractAddress: string, from: string, ABI: string, funcName: string, params: Array< any >, isWasm: boolean): Promise< any > 

调用合约的const方法。wasm合约返回的结果可以通过 decodeCallContractData() 方法解析。

```
let contractAddress = '0x....';
let from = '0x....';
let ABI = '.....';
let funcName = 'getName(string)';
let params = ['ID00001'];

web3.callContract(contractAddress,from,ABI,funcName,params,true)
.then(console.log);
>"0x..............."
```

### decodeCallContractData(ABI: string, data: string, funcName): any 

解析 callContract() 调用wasm合约方法返回值。

```
let ABI = '.....';
let funcName = 'getName(string)';
web3.decodeCallContractData(ABI,data,funcName);// data由callContract()返回
>
{
  code:0,
  data:string
}
```

### invokeContract(from: string, ABI: string, funcName: string, params: Array< any >, contractAddress: string, isWasm: boolean, accountType: "local" | "node", privateKey?: string): Promise< any > 

发送交易到智能合约并执行其方法。wasm合约返回的交易结果中logs可以通过 decodeInvokeContractLog() 进行解析

```
let contractAddress = '0x....';
let from = '0x....';
let ABI = '.....';
let funcName = 'setName(string,string)';
let params = ['ID00001','Bob'];
let accountType = 'local';
let privateKey = '0x....';// accountType 为local时 需要私钥进行签名
web3.invokeContract(contractAddress,from,ABI,funcName,params,true,accountType,privateKey)
.then(console.log);
>
{
  "status": true,
  "transactionHash": "0x.......",
  "transactionIndex": 0,
  "blockHash": "0x........",
  "blockNumber": 3,
  "contractAddress": "0x.......",
  "cumulativeGasUsed": 314159,
  "gasUsed": 30234,
  "logs": [{
         
     }, ...]
} 
```

### decodeInvokeContractLog(ABI: string, log: any, isWasm: boolean): any 

解析 invokeContract() 发送交易调用合约产生的日志。

```
web3.decodeInvokeContractLog(ABI，receipt.logs[0]);
>['aaa',0,'ccc']
```

### encodeFunctionAndParams(ABI: string, funcName: string, paramsArr: Array< any >, isWasm: boolean): string 

对要调用的合约方法以及参数进行编码，得到的结果可以通过交易中的data字段进行发送，执行对应合约的方法。

```
let ABI = '.....';
let funcName = 'getName(string)';
let params = ['ID00001'];
web3.encodeCallParams(ABI,funcName,params,true);
>'0x.......'
```

### getContractAddressByCNSName(CNSName: string, from: string): Promise< string >  

通过CNS name 得到注册在CNS中的合约地址。

```
let CNSName = 'myContract';
let from = '0x.......'; // 部署合约的账户地址（？）
web3.getContractAddressByCNSName(CNSName,from);
>'0x.......'
```

### callContractByCNSName(CNSName: string, from: string, ABI: string, funcName: string, params: Array< any>, isWasm: boolean): Promise< any>  

通过CNS name调用合约的const方法。wasm合约返回的结果可以通过 decodeCallContractData() 方法解析。

```
let CNSName = 'myContract';
let from = '0x....';
let ABI = '.....';
let funcName = 'getName(string)';
let params = ['ID00001'];

web3.callContractByCNSName(CNSName,from,ABI,funcName,params,true)
.then(console.log);
>"..............."
```

### invokeContractByCNSName(CNSName: string, from: string, ABI: string, funcName: string, params: Array< any>, isWasm: boolean, accountType: "local" | "node", privateKey?: string): Promise< any>  

对要调用的合约方法以及参数进行编码，得到的结果可以通过交易中的data字段进行发送，执行对应合约的方法

```
let CNSName = 'myContract';
let from = '0x....';
let ABI = '.....';
let funcName = 'setName(string,string)';
let params = ['ID00001','Bob'];
let accountType = 'local';
let privateKey = '0x....';// accountType 为local时 需要私钥进行签名

web3.invokeContractByCNSName(CNSName,from,ABI,funcName,params,true,accountType,privateKey)
.then(console.log);
>
{
  "status": true,
  "transactionHash": "0x.......",
  "transactionIndex": 0,
  "blockHash": "0x........",
  "blockNumber": 3,
  "contractAddress": "0x.......",
  "cumulativeGasUsed": 314159,
  "gasUsed": 30234,
  "logs": [{
         
     }, ...]
} 
```

### encodeDeployWasmContractData(bin: any, abi: string): string

对要部署的合约wasm文件进行编码，得到的结果可以通过交易中的data字段进行发送来进行合约部署。

```
const abi = '[....]';
const privateKey = web3.createLocalAccount().privateKey;

if (window.FileReader) {
      const fr = new FileReader();
      fr.readAsArrayBuffer(file);//使用FileReader读取wasm文件的二进制码
      fr.onloadend = (e: any) => {
        web3.encodeDeployWasmContractData(e.target.result,abi)
        web3.signAndSendTransactionByLocalAccount(txObject,privateKey )
        .then(console.log);
      };
}
>
{
  "status": true,
  "transactionHash": "0x.......",
  "transactionIndex": 0,
  "blockHash": "0x........",
  "blockNumber": 3,
  "contractAddress": "0x.......",
  "cumulativeGasUsed": 314159,
  "gasUsed": 30234,
  "logs": [{
         
     }, ...]
} 
```

### deployWasmContract(bin: any, abi: string, privateKey: string): Promise< any >

部署wasm合约。
```
const abi = '[....]';
const privateKey = web3.createLocalAccount().privateKey;

if (window.FileReader) {
      const fr = new FileReader();
      fr.readAsArrayBuffer(file);//使用FileReader读取wasm文件的二进制码
      fr.onloadend = (e: any) => {
        web3.deployWasmContract(e.target.result,abi,privateKey)
        .then(console.log);
      };
}
>
{
  "status": true,
  "transactionHash": "0x.......",
  "transactionIndex": 0,
  "blockHash": "0x........",
  "blockNumber": 3,
  "contractAddress": "0x.......",
  "cumulativeGasUsed": 314159,
  "gasUsed": 30234,
  "logs": [{
         
     }, ...]
} 
```

### subscribe(type: string,option?:LogsOptions, callback?:Function): EventEmitter

订阅区块链的事件。

```
type:"logs" | "syncing" | "newBlockHeaders" | "pendingTransactions"
//type为"logs"时 option为必需；
```

```
let subscription = web3.subscribe('logs', {
    address: '0x123456..',
    topics: ['0x12345...']
}, function(error, result){
    if (!error)
        console.log(result);
})
.on("connected", function(subscriptionId){
    console.log(subscriptionId);
})
.on("data", function(log){
    console.log(log);
})
.on("changed", function(log){
});;

// unsubscribes the subscription
subscription.unsubscribe(function(error, success){
    if(success)
        console.log('Successfully unsubscribed!');
});
```

### clearSubscriptions(callback: (error: Error, result: boolean) => void): void 

清除订阅。

```
web3.clearSubscriptions();
```

## Angular使用相关

由于内部使用了web3.js，在使用npm方式安装的sdk时，如果使用的Ionic / Angular的版本> 5，则可能会遇到build错误。需要对Angular进行配置（ 参考 https://github.com/ChainSafe/web3.js#web3-and-angular ）。

1. 修改\node_modules\@angular-devkit\build-angular\src\webpack\configs\browser.js 最后的

    ```
    node:false
    ```

    改为

    ```
    node:{crypto: true,stream: true}
    ```

2. 修改packege.json，在末尾添加：

    ```
    "browser": {

        "http": true,

        "https": true,

        "net": true,

        "path": true,

        "stream": true,

        "tls": true,

        "os": true,

        "crypto": true

    }
    ```

3. 修改根目录下的polyfills.ts 添加以下代码：

    ```
    global.Buffer = global.Buffer || require('buffer').Buffer;
    ```

    即可在Angular中正常使用js-sdk。



