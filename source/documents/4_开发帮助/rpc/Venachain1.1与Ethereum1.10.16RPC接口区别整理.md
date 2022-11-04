# Venachain1.1与Ethereum1.10.16RPC接口区别整理

| Namespace | Venachain | Ethereum | 备注                                            |
| --------- | --------- | -------- | ----------------------------------------------- |
| web3      | ✔         | ✔        |                                                 |
| net       | ✔         | ✔        |                                                 |
| eth       | ✔         | ✔        |                                                 |
| venachain | ✔         |          | Venachain的venachain rpc接口与其eth rpc接口一致 |
| admin     | ✔         | ✔        |                                                 |
| personal  | ✔         | ✔        |                                                 |
| txpool    | ✔         | ✔        |                                                 |
| iris      | ✔         |          | Venachain使用的是Iris共识算法                   |
| clique    |           | ✔        | Ethereum使用的是Clique共识算法                  |
| miner     |           | ✔        |                                                 |
| debug     |           | ✔        |                                                 |

Venachain RPC接口文档: [**RPC接口方法文档**](./methods.rst)

Venachain项目：<https://github.com/Venachain/Venachain/releases/tag/v1.1.2>

Ethereum项目：<https://github.com/ethereum/go-ethereum/releases/tag/v1.10.16>

## web3

| RPC                | Venachain | Ethereum |
| ------------------ | --------- | -------- |
| web3_clientVersion | ✔         | ✔        |
| web3_sha3          | ✔         | ✔        |

具体接口文档请见：[**web3接口文档**](./methods/web3.md)

```{note}
接口一致
```

## net

| RPC           | Venachain | Ethereum |
| ------------- | --------- | -------- |
| net_listening | ✔         | ✔        |
| net_peerCount | ✔         | ✔        |
| net_version   | ✔         | ✔        |

具体接口文档请见：[**net接口文档**](./methods/net.md)

```{note}
接口一致
```

## eth

| RPC                                        | Venachain | Ethereum |
| ------------------------------------------ | --------- | -------- |
| eth_gasPrice                               | ✔         | ✔        |
| eth_maxPriorityFeePerGas                   |           | ✔        |
| eth_feeHistory                             |           | ✔        |
| eth_protocolVersion                        | ✔         |          |
| eth_syncing                                | ✔         | ✔        |
| eth_chainId                                | ✔         | ✔        |
| eth_blockNumber                            | ✔         | ✔        |
| eth_getBalance                             | ✔         | ✔        |
| eth_getAccountBaseInfo                     | ✔         |          |
| eth_getProof                               |           | ✔        |
| eth_getHeaderByNumber                      |           | ✔        |
| eth_getHeaderByHash                        |           | ✔        |
| eth_getBlockByNumber                       | ✔         | ✔        |
| eth_getBlockByHash                         | ✔         | ✔        |
| eth_getUncleByBlockNumberAndIndex          |           | ✔        |
| eth_getUncleByBlockHashAndIndex            |           | ✔        |
| eth_getUncleCountByBlockNumber             |           | ✔        |
| eth_getUncleCountByBlockHash               |           | ✔        |
| eth_getCode                                | ✔         | ✔        |
| eth_getStorageAt                           | ✔         | ✔        |
| eth_call                                   | ✔         | ✔        |
| eth_estimateGas                            | ✔         | ✔        |
| eth_createAccessList                       |           | ✔        |
| eth_getBlockTransactionCountByNumber       | ✔         | ✔        |
| eth_getBlockTransactionCountByHash         | ✔         | ✔        |
| eth_getTransactionByBlockNumberAndIndex    | ✔         | ✔        |
| eth_getTransactionByBlockHashAndIndex      | ✔         | ✔        |
| eth_getRawTransactionByBlockNumberAndIndex | ✔         | ✔        |
| eth_getRawTransactionByBlockHashAndIndex   | ✔         | ✔        |
| eth_getTransactionCount                    | ✔         | ✔        |
| eth_getTransactionByHash                   | ✔         | ✔        |
| eth_getRawTransactionByHash                | ✔         | ✔        |
| eth_getTransactionReceipt                  | ✔         | ✔        |
| eth_sendTransaction                        | ✔         | ✔        |
| eth_fillTransaction                        |           | ✔        |
| eth_sendRawTransaction                     | ✔         | ✔        |
| eth_sign                                   | ✔         | ✔        |
| eth_signTransaction                        | ✔         | ✔        |
| eth_pendingTransactions                    | ✔         | ✔        |
| eth_pendingTransactionsLength              | ✔         |          |
| eth_resend                                 | ✔         | ✔        |
| eth_accounts                               | ✔         | ✔        |
| eth_newPendingTransactionFilter            | ✔         | ✔        |
| eth_newPendingTransactions                 |           | ✔        |
| eth_newBlockFilter                         | ✔         | ✔        |
| eth_newHeads                               |           | ✔        |
| eth_logs                                   |           | ✔        |
| eth_newFilter                              | ✔         | ✔        |
| eth_getLogs                                | ✔         | ✔        |
| eth_uninstallFilter                        | ✔         | ✔        |
| eth_getFilterLogs                          | ✔         | ✔        |
| eth_getFilterChanges                       | ✔         | ✔        |
| eth_etherbase                              |           | ✔        |
| eth_coinbase                               |           | ✔        |
| eth_hashrate                               |           | ✔        |
| eth_mining                                 |           | ✔        |
| eth_getWork                                |           | ✔        |
| eth_submitWork                             |           | ✔        |
| eth_submitHashrate                         |           | ✔        |
| eth_getHashrate                            |           | ✔        |

具体接口文档请见：[**eth接口文档**](./methods/eth.md)

### 接口区别

#### eth_syncing

返回值中的 ``sync stats`` 不同

**Venachain**

```go
"startingBlock": hexutil.Uint64(progress.StartingBlock),
"currentBlock":  hexutil.Uint64(progress.CurrentBlock),
"highestBlock":  hexutil.Uint64(progress.HighestBlock),
"pulledStates":  hexutil.Uint64(progress.PulledStates),
"knownStates":   hexutil.Uint64(progress.KnownStates),
```

**Ethereum**

```go
"startingBlock":       hexutil.Uint64(progress.StartingBlock),
"currentBlock":        hexutil.Uint64(progress.CurrentBlock),
"highestBlock":        hexutil.Uint64(progress.HighestBlock),
"syncedAccounts":      hexutil.Uint64(progress.SyncedAccounts),
"syncedAccountBytes":  hexutil.Uint64(progress.SyncedAccountBytes),
"syncedBytecodes":     hexutil.Uint64(progress.SyncedBytecodes),
"syncedBytecodeBytes": hexutil.Uint64(progress.SyncedBytecodeBytes),
"syncedStorage":       hexutil.Uint64(progress.SyncedStorage),
"syncedStorageBytes":  hexutil.Uint64(progress.SyncedStorageBytes),
"healedTrienodes":     hexutil.Uint64(progress.HealedTrienodes),
"healedTrienodeBytes": hexutil.Uint64(progress.HealedTrienodeBytes),
"healedBytecodes":     hexutil.Uint64(progress.HealedBytecodes),
"healedBytecodeBytes": hexutil.Uint64(progress.HealedBytecodeBytes),
"healingTrienodes":    hexutil.Uint64(progress.HealingTrienodes),
"healingBytecode":     hexutil.Uint64(progress.HealingBytecode),
```

(rpc_diff_eth_getBalance)=
#### eth_getBalance

入参中的参数3不同

**Venachain**

```go
type BlockNumber int64
```

**Ethereum**

```go
type BlockNumberOrHash struct {
	BlockNumber      *BlockNumber `json:"blockNumber,omitempty"`
	BlockHash        *common.Hash `json:"blockHash,omitempty"`
	RequireCanonical bool         `json:"requireCanonical,omitempty"`
}
```

(rpc_diff_eth_getBlockByNumber)=
#### eth_getBlockByNumber

返回值中 ``rpcMarshalBlock`` 的 ``fields`` 结构不同

**Venachain**

```
"number":           (*hexutil.Big)(head.Number),
"hash":             b.Hash(),
"parentHash":       head.ParentHash,
"nonce":            head.Nonce,
"mixHash":          head.MixDigest,
"logsBloom":        head.Bloom,
"stateRoot":        head.Root,
"miner":            head.Coinbase,
"extraData":        hexutil.Bytes(head.Extra),
"size":             hexutil.Uint64(b.Size()),
"gasLimit":         hexutil.Uint64(head.GasLimit),
"gasUsed":          hexutil.Uint64(head.GasUsed),
"timestamp":        (*hexutil.Big)(head.Time),
"transactionsRoot": head.TxHash,
"receiptsRoot":     head.ReceiptHash,
"transactions"：    []interface{}
```

**Ethereum**

```
"number":           (*hexutil.Big)(head.Number),
"hash":             head.Hash(),
"parentHash":       head.ParentHash,
"nonce":            head.Nonce,
"mixHash":          head.MixDigest,
"sha3Uncles":       head.UncleHash,
"logsBloom":        head.Bloom,
"stateRoot":        head.Root,
"miner":            head.Coinbase,
"difficulty":       (*hexutil.Big)(head.Difficulty),
"extraData":        hexutil.Bytes(head.Extra),
"size":             hexutil.Uint64(head.Size()),
"gasLimit":         hexutil.Uint64(head.GasLimit),
"gasUsed":          hexutil.Uint64(head.GasUsed),
"timestamp":        hexutil.Uint64(head.Time),
"transactionsRoot": head.TxHash,
"receiptsRoot":     head.ReceiptHash,
"baseFeePerGas":    (*hexutil.Big)(head.BaseFee)
"transactions"：    []interface{}
"uncles":           []common.Hash
"totalDifficulty":  (*hexutil.Big)(s.b.GetTd(ctx, b.Hash()))
```

#### eth_getBlockByHash

返回值中 ``rpcMarshalBlock`` 的结构不同，见 [**eth_getBlockByNumber**](rpc_diff_eth_getBlockByNumber)

#### eth_getCode

入参中的参数3不同，见 [**eth_getBalance**](rpc_diff_eth_getBalance)

#### eth_getStorageAt

入参中的参数4不同，见 [**eth_getBalance**](rpc_diff_eth_getBalance)

#### eth_call

入参不同

**Venachain**

```go
func (s *PublicBlockChainAPI) Call(ctx context.Context, args CallArgs, blockNr rpc.BlockNumber) (hexutil.Bytes, error) {}

// 参数2
type CallArgs struct {
	From     common.Address  `json:"from"`
	To       *common.Address `json:"to"`
	Gas      hexutil.Uint64  `json:"gas"`
	GasPrice hexutil.Big     `json:"gasPrice"`
	Value    hexutil.Big     `json:"value"`
	Data     hexutil.Bytes   `json:"data"`
}

// 参数3
type BlockNumber int64
```

**Ethereum**

```go
func (s *PublicBlockChainAPI) Call(ctx context.Context, args TransactionArgs, blockNrOrHash rpc.BlockNumberOrHash, overrides *StateOverride) (hexutil.Bytes, error) {}

// 参数2
type TransactionArgs struct {
	From                 *common.Address `json:"from"`
	To                   *common.Address `json:"to"`
	Gas                  *hexutil.Uint64 `json:"gas"`
	GasPrice             *hexutil.Big    `json:"gasPrice"`
	MaxFeePerGas         *hexutil.Big    `json:"maxFeePerGas"`
	MaxPriorityFeePerGas *hexutil.Big    `json:"maxPriorityFeePerGas"`
	Value                *hexutil.Big    `json:"value"`
	Nonce                *hexutil.Uint64 `json:"nonce"`

	// We accept "data" and "input" for backwards-compatibility reasons.
	// "input" is the newer name and should be preferred by clients.
	// Issue detail: https://github.com/ethereum/go-ethereum/issues/15628
	Data  *hexutil.Bytes `json:"data"`
	Input *hexutil.Bytes `json:"input"`

	// Introduced by AccessListTxType transaction.
	AccessList *types.AccessList `json:"accessList,omitempty"`
	ChainID    *hexutil.Big      `json:"chainId,omitempty"`
}

// 参数3
type BlockNumberOrHash struct {
	BlockNumber      *BlockNumber `json:"blockNumber,omitempty"`
	BlockHash        *common.Hash `json:"blockHash,omitempty"`
	RequireCanonical bool         `json:"requireCanonical,omitempty"`
}

// 参数4
type StateOverride map[common.Address]OverrideAccount
```

#### eth_estimateGas

入参不同

**Venachain**

```go
func (s *PublicBlockChainAPI) EstimateGas(ctx context.Context, args CallArgs) (hexutil.Uint64, error) {}

// 参数2
type CallArgs struct {
	From     common.Address  `json:"from"`
	To       *common.Address `json:"to"`
	Gas      hexutil.Uint64  `json:"gas"`
	GasPrice hexutil.Big     `json:"gasPrice"`
	Value    hexutil.Big     `json:"value"`
	Data     hexutil.Bytes   `json:"data"`
}
```

**Ethereum**

```go
func (s *PublicBlockChainAPI) EstimateGas(ctx context.Context, args TransactionArgs, blockNrOrHash *rpc.BlockNumberOrHash) (hexutil.Uint64, error) {}

// 参数2
type TransactionArgs struct {
	From                 *common.Address `json:"from"`
	To                   *common.Address `json:"to"`
	Gas                  *hexutil.Uint64 `json:"gas"`
	GasPrice             *hexutil.Big    `json:"gasPrice"`
	MaxFeePerGas         *hexutil.Big    `json:"maxFeePerGas"`
	MaxPriorityFeePerGas *hexutil.Big    `json:"maxPriorityFeePerGas"`
	Value                *hexutil.Big    `json:"value"`
	Nonce                *hexutil.Uint64 `json:"nonce"`

	// We accept "data" and "input" for backwards-compatibility reasons.
	// "input" is the newer name and should be preferred by clients.
	// Issue detail: https://github.com/ethereum/go-ethereum/issues/15628
	Data  *hexutil.Bytes `json:"data"`
	Input *hexutil.Bytes `json:"input"`

	// Introduced by AccessListTxType transaction.
	AccessList *types.AccessList `json:"accessList,omitempty"`
	ChainID    *hexutil.Big      `json:"chainId,omitempty"`
}

// 参数3
type BlockNumberOrHash struct {
	BlockNumber      *BlockNumber `json:"blockNumber,omitempty"`
	BlockHash        *common.Hash `json:"blockHash,omitempty"`
	RequireCanonical bool         `json:"requireCanonical,omitempty"`
}
```

(rpc_diff_eth_getTransactionByBlockNumberAndIndex)=
#### eth_getTransactionByBlockNumberAndIndex

返回值 ``RPCTransaction`` 不同

**Venachain**

```go
type RPCTransaction struct {
	BlockHash        common.Hash     `json:"blockHash"`
	BlockNumber      *hexutil.Big    `json:"blockNumber"`
	From             common.Address  `json:"from"`
	Gas              hexutil.Uint64  `json:"gas"`
	GasPrice         *hexutil.Big    `json:"gasPrice"`
	Hash             common.Hash     `json:"hash"`
	Input            hexutil.Bytes   `json:"input"`
	Nonce            hexutil.Uint64  `json:"nonce"`
	To               *common.Address `json:"to"`
	TransactionIndex hexutil.Uint    `json:"transactionIndex"`
	Value            *hexutil.Big    `json:"value"`
	V                *hexutil.Big    `json:"v"`
	R                *hexutil.Big    `json:"r"`
	S                *hexutil.Big    `json:"s"`
}

```

**Ethereum**

```go
type RPCTransaction struct {
	BlockHash        *common.Hash      `json:"blockHash"`
	BlockNumber      *hexutil.Big      `json:"blockNumber"`
	From             common.Address    `json:"from"`
	Gas              hexutil.Uint64    `json:"gas"`
	GasPrice         *hexutil.Big      `json:"gasPrice"`
	GasFeeCap        *hexutil.Big      `json:"maxFeePerGas,omitempty"`
	GasTipCap        *hexutil.Big      `json:"maxPriorityFeePerGas,omitempty"`
	Hash             common.Hash       `json:"hash"`
	Input            hexutil.Bytes     `json:"input"`
	Nonce            hexutil.Uint64    `json:"nonce"`
	To               *common.Address   `json:"to"`
	TransactionIndex *hexutil.Uint64   `json:"transactionIndex"`
	Value            *hexutil.Big      `json:"value"`
	Type             hexutil.Uint64    `json:"type"`
	Accesses         *types.AccessList `json:"accessList,omitempty"`
	ChainID          *hexutil.Big      `json:"chainId,omitempty"`
	V                *hexutil.Big      `json:"v"`
	R                *hexutil.Big      `json:"r"`
	S                *hexutil.Big      `json:"s"`
}
```

#### eth_getTransactionByBlockHashAndIndex

返回值 ``RPCTransaction`` 不同，见 [**eth_getTransactionByBlockNumberAndIndex**](rpc_diff_eth_getTransactionByBlockNumberAndIndex)

#### eth_getTransactionCount

入参中的参数3不同，见 [**eth_getBalance**](rpc_diff_eth_getBalance)

#### eth_GetTransactionByHash

返回值 ``RPCTransaction`` 不同，见 [**eth_getTransactionByBlockNumberAndIndex**](rpc_diff_eth_getTransactionByBlockNumberAndIndex)

#### eth_GetTransactionReceipt

返回值中的fields不同

**Venachain**

```go
"blockHash":         blockHash,
"blockNumber":       hexutil.Uint64(blockNumber),
"transactionHash":   hash,
"transactionIndex":  hexutil.Uint64(index),
"from":              from,
"to":                tx.To(),
"gasUsed":           hexutil.Uint64(receipt.GasUsed),
"cumulativeGasUsed": hexutil.Uint64(receipt.CumulativeGasUsed),
"contractAddress":   nil,
"logs":              receipt.Logs,
"logsBloom":         receipt.Bloom,
"root":				 hexutil.Bytes(receipt.PostState),
"status":			 hexutil.Uint(receipt.Status),
"logs":			     [][]*types.Log{},
"contractAddress":   common.Address
```

**Ethereum**

```go
"blockHash":         blockHash,
"blockNumber":       hexutil.Uint64(blockNumber),
"transactionHash":   hash,
"transactionIndex":  hexutil.Uint64(index),
"from":              from,
"to":                tx.To(),
"gasUsed":           hexutil.Uint64(receipt.GasUsed),
"cumulativeGasUsed": hexutil.Uint64(receipt.CumulativeGasUsed),
"contractAddress":   nil,
"logs":              receipt.Logs,
"logsBloom":         receipt.Bloom,
"type":              hexutil.Uint(tx.Type()),
"effectiveGasPrice": hexutil.Uint64,
"root":				 hexutil.Bytes(receipt.PostState),
"status":			 hexutil.Uint(receipt.Status),
"logs":			     []*types.Log{},
"contractAddress":   common.Address
```

(rpc_diff_eth_sendTransaction)=
#### eth_sendTransaction

入参 ``args`` 结构不同

**Venachain**

```go
type SendTxArgs struct {
	From     common.Address  `json:"from"`
	To       *common.Address `json:"to"`
	Gas      *hexutil.Uint64 `json:"gas"`
	GasPrice *hexutil.Big    `json:"gasPrice"`
	Value    *hexutil.Big    `json:"value"`
	Nonce    *hexutil.Uint64 `json:"nonce"`
	// We accept "data" and "input" for backwards-compatibility reasons. "input" is the
	// newer name and should be preferred by clients.
	Data  *hexutil.Bytes `json:"data"`
	Input *hexutil.Bytes `json:"input"`
}
```

**Ethereum**

```go
type TransactionArgs struct {
	From                 *common.Address `json:"from"`
	To                   *common.Address `json:"to"`
	Gas                  *hexutil.Uint64 `json:"gas"`
	GasPrice             *hexutil.Big    `json:"gasPrice"`
	MaxFeePerGas         *hexutil.Big    `json:"maxFeePerGas"`
	MaxPriorityFeePerGas *hexutil.Big    `json:"maxPriorityFeePerGas"`
	Value                *hexutil.Big    `json:"value"`
	Nonce                *hexutil.Uint64 `json:"nonce"`

	// We accept "data" and "input" for backwards-compatibility reasons.
	// "input" is the newer name and should be preferred by clients.
	// Issue detail: https://github.com/ethereum/go-ethereum/issues/15628
	Data  *hexutil.Bytes `json:"data"`
	Input *hexutil.Bytes `json:"input"`

	// Introduced by AccessListTxType transaction.
	AccessList *types.AccessList `json:"accessList,omitempty"`
	ChainID    *hexutil.Big      `json:"chainId,omitempty"`
}
```

(rpc_diff_eth_signTransaction)=
#### eth_signTransaction

入参 ``args`` 结构不同，见 [**eth_sendTransaction**](rpc_diff_eth_sendTransaction)

返回值 ``SignTransactionResult`` 中的 ``Transaction`` 结构不同

**Venachain**

```go
type Transaction struct {
	data txdata
	// caches
	hash   atomic.Value
	size   atomic.Value
	from   atomic.Value
	router int32
}

type txdata struct {
	AccountNonce uint64          `json:"nonce"    gencodec:"required"`
	Price        *big.Int        `json:"gasPrice" gencodec:"required"`
	GasLimit     uint64          `json:"gas"      gencodec:"required"`
	Recipient    *common.Address `json:"to"       rlp:"nil"` // nil means contract creation
	Amount       *big.Int        `json:"value"    gencodec:"required"`
	Payload      []byte          `json:"input"    gencodec:"required"`
	//CnsData      []byte          `json:"cnsData"`

	// Signature values
	V *big.Int `json:"v" gencodec:"required"`
	R *big.Int `json:"r" gencodec:"required"`
	S *big.Int `json:"s" gencodec:"required"`

	// This is only used when marshaling to JSON.
	Hash *common.Hash `json:"hash" rlp:"-"`
}
```

**Ethereum**

```go
type Transaction struct {
	inner TxData    // Consensus contents of a transaction
	time  time.Time // Time first seen locally (spam avoidance)

	// caches
	hash atomic.Value
	size atomic.Value
	from atomic.Value
}

type TxData interface {
	txType() byte // returns the type ID
	copy() TxData // creates a deep copy and initializes all fields

	chainID() *big.Int
	accessList() AccessList
	data() []byte
	gas() uint64
	gasPrice() *big.Int
	gasTipCap() *big.Int
	gasFeeCap() *big.Int
	value() *big.Int
	nonce() uint64
	to() *common.Address

	rawSignatureValues() (v, r, s *big.Int)
	setSignatureValues(chainID, v, r, s *big.Int)
}
```

#### eth_pendingTransactions

返回值中的 ``RPCTransaction`` 不同，见 [**eth_getTransactionByBlockNumberAndIndex**](rpc_diff_eth_getTransactionByBlockNumberAndIndex)

#### eth_resend

入参 ``args`` 结构不同，见 [**eth_sendTransaction**](rpc_diff_eth_sendTransaction)

## admin

| RPC                     | Venachain | Ethereum |
| ----------------------- | --------- | -------- |
| admin_peers             | ✔         | ✔        |
| admin_nodeInfo          | ✔         | ✔        |
| admin_datadir           | ✔         | ✔        |
| admin_addPeer           |           | ✔        |
| admin_removePeer        |           | ✔        |
| admin_addTrustedPeer    |           | ✔        |
| admin_removeTrustedPeer |           | ✔        |
| admin_peerEvents        |           | ✔        |
| admin_startHTTP         |           | ✔        |
| admin_stopHTTP          |           | ✔        |
| admin_startRPC          |           | ✔        |
| admin_stopRPC           |           | ✔        |
| admin_startWS           |           | ✔        |
| admin_stopWS            |           | ✔        |
| admin_exportChain       |           | ✔        |
| admin_importChain       |           | ✔        |

具体接口文档请见：[**admin接口文档**](./methods/admin.md)

### 接口区别

#### admin_peers

返回值的 ``PeerInfo`` 结构不同

**Venachain**

```go
type PeerInfo struct {
	ID      string   `json:"id"`   // Unique node identifier (also the encryption key)
	Name    string   `json:"name"` // Name of the node, including client type, version, OS, custom data
	Caps    []string `json:"caps"` // Sum-protocols advertised by this particular peer
	Network struct {
		LocalAddress  string `json:"localAddress"`  // Local endpoint of the TCP data connection
		RemoteAddress string `json:"remoteAddress"` // Remote endpoint of the TCP data connection
		Inbound       bool   `json:"inbound"`
		Trusted       bool   `json:"trusted"`
		Static        bool   `json:"static"`
		Consensus     bool   `json:"consensus"`
	} `json:"network"`
	Protocols map[string]interface{} `json:"protocols"` // Sub-protocol specific metadata fields
}
```

**Ethereum**

```go
type PeerInfo struct {
	ENR     string   `json:"enr,omitempty"` // Ethereum Node Record
	Enode   string   `json:"enode"`         // Node URL
	ID      string   `json:"id"`            // Unique node identifier
	Name    string   `json:"name"`          // Name of the node, including client type, version, OS, custom data
	Caps    []string `json:"caps"`          // Protocols advertised by this peer
	Network struct {
		LocalAddress  string `json:"localAddress"`  // Local endpoint of the TCP data connection
		RemoteAddress string `json:"remoteAddress"` // Remote endpoint of the TCP data connection
		Inbound       bool   `json:"inbound"`
		Trusted       bool   `json:"trusted"`
		Static        bool   `json:"static"`
	} `json:"network"`
	Protocols map[string]interface{} `json:"protocols"` // Sub-protocol specific metadata fields
}
```

#### admin_nodeinfo

返回值的 ``NodeInfo`` 结构不同

**Venachain**

```go
type NodeInfo struct {
	ID    string `json:"id"`    // Unique node identifier (also the encryption key)
	Name  string `json:"name"`  // Name of the node, including client type, version, OS, custom data
	Enode string `json:"enode"` // Enode URL for adding this peer from remote peers
	IP    string `json:"ip"`    // IP address of the node
	Ports struct {
		Discovery int `json:"discovery"` // UDP listening port for discovery protocol
		Listener  int `json:"listener"`  // TCP listening port for RLPx
	} `json:"ports"`
	ListenAddr string                 `json:"listenAddr"`
	Protocols  map[string]interface{} `json:"protocols"`
}
```

**Ethereum**

```go
type NodeInfo struct {
	ID    string `json:"id"`    // Unique node identifier (also the encryption key)
	Name  string `json:"name"`  // Name of the node, including client type, version, OS, custom data
	Enode string `json:"enode"` // Enode URL for adding this peer from remote peers
	ENR   string `json:"enr"`   // Ethereum Node Record
	IP    string `json:"ip"`    // IP address of the node
	Ports struct {
		Discovery int `json:"discovery"` // UDP listening port for discovery protocol
		Listener  int `json:"listener"`  // TCP listening port for RLPx
	} `json:"ports"`
	ListenAddr string                 `json:"listenAddr"`
	Protocols  map[string]interface{} `json:"protocols"`
}
```

## personal

| RPC                             | Venachain | Ethereum |
| ------------------------------- | --------- | -------- |
| personal_listAccounts           | ✔         | ✔        |
| personal_listWallests           | ✔         | ✔        |
| personal_openWallet             | ✔         | ✔        |
| personal_driveAccount           |           | ✔        |
| personal_newAccount             | ✔         | ✔        |
| personal_importRawKey           |           | ✔        |
| personal_unLockAccount          | ✔         | ✔        |
| personal_lockAccount            | ✔         | ✔        |
| personal_sendTransaction        | ✔         | ✔        |
| personal_signTransaction        | ✔         | ✔        |
| personal_sign                   | ✔         | ✔        |
| personal_ecRecover              | ✔         | ✔        |
| personal_signAndSendTransaction | ✔         | ✔        |
| personal_initializeWallet       |           | ✔        |
| personal_unpair                 |           | ✔        |

具体接口文档请见：[**personal接口文档**](./methods/personal.md)

### 接口区别

#### personal_sendTransaction

入参 ``args`` 结构不同，见 [**eth_sendTransaction**](rpc_diff_eth_sendTransaction)

#### personal_signTransaction

入参 ``args`` 结构不同，见 [**eth_sendTransaction**](rpc_diff_eth_sendTransaction)

返回值 ``SignTransactionResult`` 中的 ``Transaction`` 结构不同，见 [**eth_signTransaction**](rpc_diff_eth_signTransaction)

#### personal_signAndSendTransaction

入参 ``args`` 结构不同，见 [**eth_sendTransaction**](rpc_diff_eth_sendTransaction)

## txpool

| RPC                | Venachain | Ethereum |
| ------------------ | --------- | -------- |
| txpool_content     | ✔         | ✔        |
| txpool_status      | ✔         | ✔        |
| txpool_inspect     | ✔         | ✔        |
| txpool_contentFrom |           | ✔        |

具体接口文档请见：[**txpool接口文档**](./methods/txpool.md)

### 接口区别

#### txpool_context

返回值中的 ``RPCTransaction`` 不同，见 [**eth_getTransactionByBlockNumberAndIndex**](rpc_diff_eth_getTransactionByBlockNumberAndIndex)

## iris

Iris是Venachain使用的共识算法。

| RPC                      | Venachain | Ethereum |
| ------------------------ | --------- | -------- |
| iris_getSnapshot         | ✔         |          |
| iris_getSnapshotAtHash   | ✔         |          |
| iris_getValidators       | ✔         |          |
| iris_getValidatorsAtHash | ✔         |          |
| iris_candidates          | ✔         |          |

具体接口文档请见：[**iris接口文档**](./methods/iris.md)

## venachain

Venachain的venachain rpc接口与其eth rpc接口，除了namespace不同，其余内容一致。

| RPC                                              | Venachain | Ethereum |
| ------------------------------------------------ | --------- | -------- |
| venachain_gasPrice                               | ✔         |          |
| venachain_protocolVersion                        | ✔         |          |
| venachain_syncing                                | ✔         |          |
| venachain_chainId                                | ✔         |          |
| venachain_blockNumber                            | ✔         |          |
| venachain_getBalance                             | ✔         |          |
| venachain_getAccountBaseInfo                     | ✔         |          |
| venachain_getBlockByNumber                       | ✔         |          |
| venachain_getBlockByHash                         | ✔         |          |
| venachain_getCode                                | ✔         |          |
| venachain_getStorageAt                           | ✔         |          |
| venachain_call                                   | ✔         |          |
| venachain_estimateGas                            | ✔         |          |
| venachain_getBlockTransactionCountByNumber       | ✔         |          |
| venachain_getBlockTransactionCountByHash         | ✔         |          |
| venachain_getTransactionByBlockNumberAndIndex    | ✔         |          |
| venachain_getTransactionByBlockHashAndIndex      | ✔         |          |
| venachain_getRawTransactionByBlockNumberAndIndex | ✔         |          |
| venachain_getRawTransactionByBlockHashAndIndex   | ✔         |          |
| venachain_getTransactionCount                    | ✔         |          |
| venachain_getTransactionByHash                   | ✔         |          |
| venachain_getRawTransactionByHash                | ✔         |          |
| venachain_getTransactionReceipt                  | ✔         |          |
| venachain_sendTransaction                        | ✔         |          |
| venachain_sendRawTransaction                     | ✔         |          |
| venachain_sign                                   | ✔         |          |
| venachain_signTransaction                        | ✔         |          |
| venachain_pendingTransactions                    | ✔         |          |
| venachain_pendingTransactionsLength              | ✔         |          |
| venachain_resend                                 | ✔         |          |
| venachain_accounts                               | ✔         |          |
| venachain_newPendingTransactionFilter            | ✔         |          |
| venachain_newBlockFilter                         | ✔         |          |
| venachain_newFilter                              | ✔         |          |
| venachain_getLogs                                | ✔         |          |
| venachain_uninstallFilter                        | ✔         |          |
| venachain_getFilterLogs                          | ✔         |          |
| venachain_getFilterChanges                       | ✔         |          |

具体接口文档请见：[**venachain接口文档**](./methods/venachain.md)
