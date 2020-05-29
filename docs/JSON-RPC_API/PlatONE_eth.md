# eth Namespace

## web3_clientVersion

Returns the current client version.
返回当前

### Parameters

none
无

### Returns

`String` - The current client version.
`String` - 当前......

### Example

```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"web3_clientVersion","params":[],"id":67}'

// Result
{
  "id":67,
  "jsonrpc":"2.0",
  "result": "PlatONEnetwork/platone/v0.9.9-stable/linux-amd64/go1.11.4"
}
```

***

## web3_sha3

Returns Keccak-256 (*not* the standardized SHA3-256) of the given data.
返回Keccak-256算法哈希后的数据。
注：该接口并未使用sha-3标准

### Parameters

`DATA` - the data to convert into a SHA3 hash.

### Example Parameters

```js
params: [
  "0x74657374"  // the hex string of text message "test"
]
```

### Returns

`DATA` - The SHA3 result of the given string.

### Example

```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"web3_sha3","params":["0x74657374"],"id":64}'

// Result
{
  "id":64,
  "jsonrpc": "2.0",
  "result": "0x9c22ff5f21f0b81b113e63f7db6da94fedef11b2119b4088b89664fb9a3cb658"
}
```

***

## ~~net_version~~

Returns the current network id.

### Parameters
none

### Returns

`String` - The current network id.
- `"1"`: Ethereum Mainnet
- `"2"`: Morden Testnet  (deprecated)
- `"3"`: Ropsten Testnet
- `"4"`: Rinkeby Testnet
- `"5"`: Goerli Testnet
- `"42"`: Kovan Testnet

### Example

```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"net_version","params":[],"id":67}'

// Result
{
  "id":67,
  "jsonrpc": "2.0",
  "result": "3"
}
```

***

## net_listening

Returns `true` if client is actively listening for network connections.

### Parameters
none

### Returns

`Boolean` - `true` when listening, otherwise `false`.

### Example

```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"net_listening","params":[],"id":67}'

// Result
{
  "id":67,
  "jsonrpc":"2.0",
  "result":true
}
```

***

## net_peerCount

Returns number of peers currently connected to the client.

### Parameters

none

### Returns

`QUANTITY` - integer of the number of connected peers.

### Example

`./platonecil.sh four`

```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"net_peerCount","params":[],"id":74}'

// Result
{
  "id":74,
  "jsonrpc": "2.0",
  "result": "0x3"   // 3个节点
}
```

***

## eth_protocolVersion???

Returns the current ethereum protocol version.

### Parameters
none

### Returns

`String` - The current ethereum protocol version.

### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_protocolVersion","params":[],"id":67}'

// Result
{
  "id":67,
  "jsonrpc": "2.0",
  "result": "0x3f"
}
```

***

## eth_newHeads*

***

## eth_monitor*

***

## eth_syncing???

Returns an object with data about the sync status or `false`.

### Parameters
none

### Returns

`Object|Boolean`, An object with sync status data or `FALSE`, when not syncing:
  - `startingBlock`: `QUANTITY` - The block at which the import started (will only be reset, after the sync reached his head)
  - `currentBlock`: `QUANTITY` - The current block, same as eth_blockNumber
  - `highestBlock`: `QUANTITY` - The estimated highest block

### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_syncing","params":[],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": {
    startingBlock: '0x384',
    currentBlock: '0x386',
    highestBlock: '0x454'
  }
}
// Or when not syncing
{
  "id":1,
  "jsonrpc": "2.0",
  "result": false
}
```

***

## eth_coinbase

Returns the client coinbase address.

### Parameters

none

### Returns

`DATA`, 20 bytes - the current coinbase address.

### Example

```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_coinbase","params":[],"id":64}'

// Result
{
  "id":64,
  "jsonrpc": "2.0",
  "result": "0x0a86ced495e8d452a52f24e4ff7dd59bb532bd94"
}
```

***

## eth_etherbase*???

Returns the client etherbase address.

### Parameters

none

### Returns

`DATA`, 20 bytes - the current etherbase address.

### Example

```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_etherbase","params":[],"id":64}'

// Result
{
  "id":64,
  "jsonrpc": "2.0",
  "result": "0x0a86ced495e8d452a52f24e4ff7dd59bb532bd94"
}
```

***

## ~~eth_mining~~

Returns `true` if client is actively mining new blocks.

### Parameters
none

### Returns

`Boolean` - returns `true` of the client is mining, otherwise `false`.

### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_mining","params":[],"id":71}'

// Result
{
  "id":71,
  "jsonrpc": "2.0",
  "result": true
}

```

***

## eth_gasPrice???

Returns the current price per gas in (**TODO**).

### Parameters
none

### Returns

`QUANTITY` - integer of the current gas price in wei.

### Example

no gas Price in PlatONE?

```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_gasPrice","params":[],"id":73}'

// Result
{
  "id":73,
  "jsonrpc": "2.0",
  "result": "0x0"
}
```

***

## eth_accounts

Returns a list of addresses owned by client.


### Parameters
none

### Returns

`Array of DATA`, 20 Bytes - addresses owned by the client.

### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_accounts","params":[],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": ["0x909daf40f4df3cb7e8f7b88570f3f15f2fa3a121", "0x60102f6ad17a35c456086e1e669cf5fb0d7438d2"]
}
```

***

## eth_blockNumber

Returns the number of most recent block.

### Parameters
none

### Returns

`QUANTITY` - integer of the current block number the client is on.

### Example

```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_blockNumber","params":[],"id":1}'

// Result
{
  "id":83,
  "jsonrpc": "2.0",
  "result": "0x15" // The height of the block is 21
}
```

***

## eth_getBalance
i
Returns the balance of the account of given address.

### Parameters

1. `DATA`, 20 Bytes - address to check for balance.
2. `QUANTITY|TAG` - integer block number, or the string `"latest"`, `"earliest"` or `"pending"`, see the [default block parameter](#the-default-block-parameter)

### Example Parameters

```js
params: [
   '0x5181aa915f4fc653d7a924b764931e273ec71798',
   'latest'
]
```

### Returns

`QUANTITY` - integer of the current balance in wei.


### Example

```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getBalance","params":["0x5181aa915f4fc653d7a924b764931e273ec71798", "latest"],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0x0" // The balance is 0
}
```

Notation: currently there is no balance in PlatONE

***

## eth_getAccountBaseInfo*

Returns the balance of the account of given address.

### Parameters

1. `DATA`, 20 Bytes - address to check for balance.
2. `QUANTITY|TAG` - integer block number, or the string `"latest"`, `"earliest"` or `"pending"`, see the [default block parameter](#the-default-block-parameter)

### Example Parameters

```js
params: [
   '0x5181aa915f4fc653d7a924b764931e273ec71798',
   'latest'
]
```

### Returns

`QUANTITY` - integer of the current balance in wei.


### Example

```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getBalance","params":["0x5181aa915f4fc653d7a924b764931e273ec71798", "latest"],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": {
      "IsContract":false,
      "Nonce":1,
      "Balance":0,
      "Address":0x5181aa915f4fc653d7a924b764931e273ec71798,
      "Creator":0x0000000000000000000000000000000000000000}
}
```

## eth_getStorageAt (**TODO**)

Returns the value from a storage position at a given address. 

### Parameters

1. `DATA`, 20 Bytes - address of the storage.
2. `QUANTITY` - integer of the position in the storage.
3. `QUANTITY|TAG` - integer block number, or the string `"latest"`, `"earliest"` or `"pending"`, see the [default block parameter](#the-default-block-parameter)

### Returns

`DATA` - the value at this storage position.

### Example
Calculating the correct position depends on the storage to retrieve. Consider the following contract deployed at `0x030695ef05b886b13862698885903d7d5e5a2805` by address `0xb8364878c7dd8ba362dfab428d45723c29257b5b`.

```
contract Storage {
    uint pos0;
    mapping(address => uint) pos1;
    
    function Storage() {
        pos0 = 1234;
        pos1[msg.sender] = 5678;
    }
}
```

Retrieving the value of pos0 is straight forward:

```js
curl -X POST --data '{"jsonrpc":"2.0", "method": "eth_getStorageAt", "params": ["0x030695ef05b886b13862698885903d7d5e5a2805", "0x0", "latest"], "id": 1}' localhost:8545

{"jsonrpc":"2.0","id":1,"result":"0x0000000000000000000000000000000000000000000000000000000000000000"}
```

Retrieving an element of the map is harder. The position of an element in the map is calculated with:
```js
keccack(LeftPad32(key, 0), LeftPad32(map position, 0))
```

This means to retrieve the storage on pos1["0x391694e7e0b0cce554cb130d723a9d27458f9298"] we need to calculate the position with:
```js
keccak(decodeHex("000000000000000000000000391694e7e0b0cce554cb130d723a9d27458f9298" + "0000000000000000000000000000000000000000000000000000000000000001"))
```
The platone console which comes with the web3 library can be used to make the calculation:
```js
> var key = "000000000000000000000000391694e7e0b0cce554cb130d723a9d27458f9298" + "0000000000000000000000000000000000000000000000000000000000000001"
undefined
> web3.sha3(key, {"encoding": "hex"})
"0x6661e9d6d8b923d5bbaab1b96e1dd51ff6ea2a93520fdc9eb75d059238b8c5e9"
```
Now to fetch the storage:
```js
curl -X POST --data '{"jsonrpc":"2.0", "method": "eth_getStorageAt", "params": ["0x295a70b2de5e3953354a6a8344e616ed314d7251", "0x6661e9d6d8b923d5bbaab1b96e1dd51ff6ea2a93520fdc9eb75d059238b8c5e9", "latest"], "id": 1}' localhost:8545

{"jsonrpc":"2.0","id":1,"result":"0x000000000000000000000000000000000000000000000000000000000000162e"}

```

***

## eth_getTransactionCount

Returns the number of transactions *sent* from an address.

### Parameters

1. `DATA`, 20 Bytes - address.
2. `QUANTITY|TAG` - integer block number, or the string `"latest"`, `"earliest"` or `"pending"`, see the [default block parameter](#the-default-block-parameter)

### Example Parameters

```js
params: [
   '0xf0c11b00f37cff38b4eb29be2ad6d541b841e466',
   'latest' // state at the latest block
]
```

### Returns

`QUANTITY` - integer of the number of transactions send from this address.

### Example

```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getTransactionCount","params":["0xf0c11b00f37cff38b4eb29be2ad6d541b841e466","latest"],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0x1" // 1
}
```

***

## eth_getBlockTransactionCountByHash

Returns the number of transactions in a block from a block matching the given block hash.


### Parameters

1. `DATA`, 32 Bytes - hash of a block.

### Example Parameters

```js
params: [
   '0xcd021448dcc4f9a6a7fd553b541199a2ebaab1de8e739bc6680eb0e62a870e11'
]
```

### Returns

`QUANTITY` - integer of the number of transactions in this block.

### Example

```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getBlockTransactionCountByHash","params":["0xcd021448dcc4f9a6a7fd553b541199a2ebaab1de8e739bc6680eb0e62a870e11"],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0x1" // 1
}
```

***

## eth_getBlockTransactionCountByNumber

Returns the number of transactions in a block matching the given block number.

### Parameters

1. `QUANTITY|TAG` - integer of a block number, or the string `"earliest"`, `"latest"` or `"pending"`, as in the [default block parameter](#the-default-block-parameter).

### Example Parameters

```js
params: [
   'latest',
]
```

### Returns

`QUANTITY` - integer of the number of transactions in this block.

### Example

```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getBlockTransactionCountByNumber","params":["latest"],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0x1" // There is 1 transaction in the current block
}
```

***

## eth_getCode

Returns code at a given address.

### Parameters

1. `DATA`, 20 Bytes - address.
2. `QUANTITY|TAG` - integer block number, or the string `"latest"`, `"earliest"` or `"pending"`, see the [default block parameter](#the-default-block-parameter).

### Example Parameters
```js
params: [
   '0x030695ef05b886b13862698885903d7d5e5a2805',
   'latest'
]
```

### Returns

`DATA` - the code from the given address.


### Example

Deploy a solidity contract

```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getCode","params":["0x030695ef05b886b13862698885903d7d5e5a2805", "latest"],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0x608060405234801561001057600080fd5b506004361061005e576000357c0100000000000000000000000000000000000000000000000000000000900480630c6af81614610063578063262a9dff1461009457806367e0badb146100b8575b600080fd5b6100926004803603602081101561007957600080fd5b81019080803560030b90602001909291905050506100dc565b005b61009c610102565b604051808260030b60030b815260200191505060405180910390f35b6100c0610114565b604051808260030b60030b815260200191505060405180910390f35b806000806101000a81548163ffffffff021916908360030b63ffffffff16021790555050565b6000809054906101000a900460030b81565b60008060009054906101000a900460030b90509056fea265627a7a723158202f431f1087cf66d88ca0871e94603a8d6b3fe9c05c6c334762b801417af7959864736f6c63430005110032"
}
```

***

## eth_sign

The sign method calculates an Ethereum specific signature with: `sign(keccak256("\x19Ethereum Signed Message:\n" + len(message) + message)))`.

By adding a prefix to the message makes the calculated signature recognisable as an Ethereum specific signature. This prevents misuse where a malicious DApp can sign arbitrary data (e.g. transaction) and use the signature to impersonate the victim.

**Note** the address to sign with must be unlocked. 

### Parameters

account, message

1. `DATA`, 20 Bytes - address.
2. `DATA`, N Bytes - message to sign.

### Returns

`DATA`: Signature

### Example

```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_sign","params":["0xf0c11b00f37cff38b4eb29be2ad6d541b841e466", "0x74657374"],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0xf7b7afeeb701fa7a3af0a7ef6fbb79a8f8cdc8dff6d2a9ee4d9a945a9cf381cc2f7b9e2e8c995617973fbc02749f8d69d7724798e0f091f3bc3986db0693b8831c"
}
```

An example how to use solidity ecrecover to verify the signature calculated with `eth_sign` can be found [here](https://gist.github.com/bas-vk/d46d83da2b2b4721efb0907aecdb7ebd). The contract is deployed on the testnet Ropsten and Rinkeby.

***

## eth_signTransaction*(**TODO**)

The sign method calculates an Ethereum specific signature with: `sign(keccak256("\x19Ethereum Signed Message:\n" + len(message) + message)))`.

By adding a prefix to the message makes the calculated signature recognisable as an Ethereum specific signature. This prevents misuse where a malicious DApp can sign arbitrary data (e.g. transaction) and use the signature to impersonate the victim.

**Note** the address to sign with must be unlocked. 

### Parameters

account, message

1. `DATA`, 20 Bytes - address.
2. `DATA`, N Bytes - message to sign.

### Returns

`DATA`: Signature

### Example

```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_signTransaction","params":[<tx>],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": {
    raw:0xf86001808094f0c11b00f37cff38b4eb29be2ad6d541b841e46680808082027ca02caa8afb77e3bdb970824307025fbc7a923e6889ff6118e5da030c9a126d8c10a068137260fedd5706634cd4c2e6e0619d45af2dc815f256fbbbf58268050732af
    tx:{
      gas:0x0 to:0xf0c11b00f37cff38b4eb29be2ad6d541b841e466
      value:0x0
      txType:0x0
      r:0x2caa8afb77e3bdb970824307025fbc7a923e6889ff6118e5da030c9a126d8c10
      hash:0xff41a07732e8dd27c78e2ccb12a0f5ba0d4434cc1c5b0da6d3821888797b4c72
      nonce:0x1
      gasPrice:0x0
      input:0x
      v:0x27c
      s:0x68137260fedd5706634cd4c2e6e0619d45af2dc815f256fbbbf58268050732af
    }
  }
}
```

***

## eth_sendTransaction (TODO)

Creates new message call transaction or a contract creation, if the data field contains code.

### Parameters

1. `Object` - The transaction object
  - `from`: `DATA`, 20 Bytes - The address the transaction is send from.
  - `to`: `DATA`, 20 Bytes - (optional when creating new contract) The address the transaction is directed to.
  - `gas`: `QUANTITY`  - (optional, default: 90000) Integer of the gas provided for the transaction execution. It will return unused gas.
  - `gasPrice`: `QUANTITY`  - (optional, default: To-Be-Determined) Integer of the gasPrice used for each paid gas
  - `value`: `QUANTITY`  - (optional) Integer of the value sent with this transaction
  - `data`: `DATA`  - The compiled code of a contract OR the hash of the invoked method signature and encoded parameters. For details see [Ethereum Contract ABI](https://github.com/ethereum/wiki/wiki/Ethereum-Contract-ABI)
  - `nonce`: `QUANTITY`  - (optional) Integer of a nonce. This allows to overwrite your own pending transactions that use the same nonce.

### Example Parameters
```js
params: [{
  "from": "0xb60e8dd61c5d32be8058bb8eb970870f07233155",
  "to": "0xd46e8dd67c5d32be8058bb8eb970870f07244567",
  "gas": "0x76c0", // 30400
  "gasPrice": "0x9184e72a000", // 10000000000000
  "value": "0x9184e72a", // 2441406250
  "data": "0xd46e8dd67c5d32be8d46e8dd67c5d32be8058bb8eb970870f072445675058bb8eb970870f072445675"
}]
```

### Returns

`DATA`, 32 Bytes - the transaction hash, or the zero hash if the transaction is not yet available.

Use [eth_getTransactionReceipt](#eth_gettransactionreceipt) to get the contract address, after the transaction was mined, when you created a contract.

### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_sendTransaction","params":[{see above}],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0xe670ec64341771606e55d6b4ca35a1a6b75ee3d5145a99d05921026d1527331"
}
```

***

## eth_sendRawTransaction

Creates new message call transaction or a contract creation for signed transactions.

### Parameters

1. `DATA`, The signed transaction data.

### Example Parameters
```js
params: ["0xd46e8dd67c5d32be8d46e8dd67c5d32be8058bb8eb970870f072445675058bb8eb970870f072445675"]
```

### Returns

`DATA`, 32 Bytes - the transaction hash, or the zero hash if the transaction is not yet available.

Use [eth_getTransactionReceipt](#eth_gettransactionreceipt) to get the contract address, after the transaction was mined, when you created a contract.

### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_sendRawTransaction","params":[{see above}],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0xe670ec64341771606e55d6b4ca35a1a6b75ee3d5145a99d05921026d1527331"
}
```

***

## eth_resend*(**TODO**)

***

## eth_call (TODO)

Executes a new message call immediately without creating a transaction on the block chain.


### Parameters

1. `Object` - The transaction call object
  - `from`: `DATA`, 20 Bytes - (optional) The address the transaction is sent from.
  - `to`: `DATA`, 20 Bytes  - The address the transaction is directed to.
  - `gas`: `QUANTITY`  - (optional) Integer of the gas provided for the transaction execution. eth_call consumes zero gas, but this parameter may be needed by some executions.
  - `gasPrice`: `QUANTITY`  - (optional) Integer of the gasPrice used for each paid gas
  - `value`: `QUANTITY`  - (optional) Integer of the value sent with this transaction
  - `data`: `DATA`  - (optional) Hash of the method signature and encoded parameters. For details see [Ethereum Contract ABI in the Solidity documentation](https://solidity.readthedocs.io/en/latest/abi-spec.html)
2. `QUANTITY|TAG` - integer block number, or the string `"latest"`, `"earliest"` or `"pending"`, see the [default block parameter](#the-default-block-parameter)

### Returns

`DATA` - the return value of executed contract.

### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_call","params":[{see above}],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0x"
}
```

***

## eth_estimateGas

Generates and returns an estimate of how much gas is necessary to allow the transaction to complete. The transaction will not be added to the blockchain. Note that the estimate may be significantly more than the amount of gas actually used by the transaction, for a variety of reasons including EVM mechanics and node performance.

### Parameters

See [eth_call](#eth_call) parameters, expect that all properties are optional. If no gas limit is specified platone uses the block gas limit from the pending block as an upper bound. As a result the returned estimate might not be enough to executed the call/transaction when the amount of gas is higher than the pending block gas limit.

### Returns

`QUANTITY` - the amount of gas used.

### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_estimateGas","params":[{see above}],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0x5208" // 21000
}
```

***

## eth_getBlockByHash

Returns information about a block by hash.

### Parameters

1. `DATA`, 32 Bytes - Hash of a block.
2. `Boolean` - If `true` it returns the full transaction objects, if `false` only the hashes of the transactions.

### Example Parameters

```js
params: [
    '0xac7f7120b8f512641b101f06657f94667e2db15277047f3067855b9ed6d15910',
    true
]
```

### Returns

`Object` - A block object, or `null` when no block was found:

  - `number`: `QUANTITY` - the block number. `null` when its pending block.
  - `hash`: `DATA`, 32 Bytes - hash of the block. `null` when its pending block.
  - `parentHash`: `DATA`, 32 Bytes - hash of the parent block.
  - `nonce`: `DATA`, 8 Bytes - hash of the generated proof-of-work. `null` when its pending block.
  - `sha3Uncles`: `DATA`, 32 Bytes - SHA3 of the uncles data in the block.
  - `logsBloom`: `DATA`, 256 Bytes - the bloom filter for the logs of the block. `null` when its pending block.
  - `transactionsRoot`: `DATA`, 32 Bytes - the root of the transaction trie of the block.
  - `stateRoot`: `DATA`, 32 Bytes - the root of the final state trie of the block.
  - `receiptsRoot`: `DATA`, 32 Bytes - the root of the receipts trie of the block.
  - `miner`: `DATA`, 20 Bytes - the address of the beneficiary to whom the mining rewards were given.
  - `difficulty`: `QUANTITY` - integer of the difficulty for this block.
  - `totalDifficulty`: `QUANTITY` - integer of the total difficulty of the chain until this block.
  - `extraData`: `DATA` - the "extra data" field of this block.
  - `size`: `QUANTITY` - integer the size of this block in bytes.
  - `gasLimit`: `QUANTITY` - the maximum gas allowed in this block.
  - `gasUsed`: `QUANTITY` - the total used gas by all transactions in this block.
  - `timestamp`: `QUANTITY` - the unix timestamp for when the block was collated.
  - `transactions`: `Array` - Array of transaction objects, or 32 Bytes transaction hashes depending on the last given parameter.
  - `uncles`: `Array` - Array of uncle hashes.

### Example

```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getBlockByHash","params":["0xac7f7120b8f512641b101f06657f94667e2db15277047f3067855b9ed6d15910", true],"id":1}'

// Result
{
"id":1,
"jsonrpc":"2.0",
"result": {
    extraData:0xda82090987706c61746f6e6588676f312e31312e34856c696e75780000000000f90164f854940a86ced495e8d452a52f24e4ff7dd59bb532bd94943bcea1c5eda42c85e8d5fa8e116416e86cec98d094a5e197d546ede9fb87291ff63a52ec9d3d70925794d4e43fa0703c28132933e428b661d2b92ca7974bb841d9dad8beec17806e461bfe7fee9229b17f0728c03c5e794e00ed92a238b61faa603ac0f82b6ecc3c4ece84cea696b4f9f5bb67273b9c419fd6399ab345f06ad000f8c9b841622c589cba3237d13f84632dd2327ba7a90d7505d77b70f71ee679e2e5f1662a72acb4e5e343ee9408028e3fb4b7bf0007b6593d3377428742044bf857e7127400b841ee5459983f1632868b4da84382dd662bc015af22c61a8eeba3d7813137f06e5a14634cd61f1f79c483b76c5de42d3c0661477da9313f4629aa9f500720a810e801b84149980adb4c8b0ba46ee23a529011e4a16c3566f15bd3389b3d8f207f0108928118eff1a89235cfb77b7c3e788942fdcfe0ae00d5b49fa7e3fc4331c3e81b5c8f01
    logsBloom:0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
    nonce:0x0000000000000000
    stateRoot:0xc40e61a103616a5181604cb7942bbd75214058fe20f9b5453bb9a5cf61a56f20
    receiptsRoot:0xa8f05235b16a403a6193e9eca1c4f92c41f71a418892c6a29c49a4dac1d1802b
    transactionsRoot:0x4b60eed3e4337b99b13ae9537280c6263847dd3b8f686a8f3a39352f8efa88b2
    gasLimit:0x2540be400
    gasUsed:0x15fd0
    hash:0xac7f7120b8f512641b101f06657f94667e2db15277047f3067855b9ed6d15910
    mixHash:0x63746963616c2062797a616e74696e65206661756c7420746f6c6572616e6365
    parentHash:0x5f043f03b91dfec45adb5f60ec8a57ff41c5d07323f8d6a3d07111faa5619c44
    size:0x565
    transactions:{
        from:0x5181aa915f4fc653d7a924b764931e273ec71798
        value:0x0 s:0x6169b6a59c7c122e083d8d52272dc3028709e1a401ae7c9098f17c02f3250c65
        blockNumber:0x15
        hash:0x6767e3c43aa59f33a4606db9c91cc9511f2ea623c3dd8a7fa67fd90a423e0762
        r:0xfff2a5e6b1cc7c521843763254c03b8c9456444a9c65c02d8cd948f90228d042
        nonce:0x4dba7b0f9da1d7eb
        transactionIndex:0x0
        txType:0x1
        blockHash:0xac7f7120b8f512641b101f06657f94667e2db15277047f3067855b9ed6d15910
        gas:0x0
        gasPrice:0x0
        input:0x608060405260008060006101000a81548163ffffffff021916908360030b63ffffffff16021790555034801561003457600080fd5b5061015f806100446000396000f3fe608060405234801561001057600080fd5b506004361061005e576000357c0100000000000000000000000000000000000000000000000000000000900480630c6af81614610063578063262a9dff1461009457806367e0badb146100b8575b600080fd5b6100926004803603602081101561007957600080fd5b81019080803560030b90602001909291905050506100dc565b005b61009c610102565b604051808260030b60030b815260200191505060405180910390f35b6100c0610114565b604051808260030b60030b815260200191505060405180910390f35b806000806101000a81548163ffffffff021916908360030b63ffffffff16021790555050565b6000809054906101000a900460030b81565b60008060009054906101000a900460030b90509056fea265627a7a723158202f431f1087cf66d88ca0871e94603a8d6b3fe9c05c6c334762b801417af7959864736f6c63430005110032
        to:<nil>
        v:0x27c}
    miner:0x0a86ced495e8d452a52f24e4ff7dd59bb532bd94
    number:0x15
    timestamp:0x5ed06c50
  }
}
```

***

## eth_getBlockByNumber

Returns information about a block by block number.

### Parameters

1. `QUANTITY|TAG` - integer of a block number, or the string `"earliest"`, `"latest"` or `"pending"`, as in the [default block parameter](#the-default-block-parameter).
2. `Boolean` - If `true` it returns the full transaction objects, if `false` only the hashes of the transactions.

### Example Parameter

```js
params: [
   'latest',
   true
]
```

### Returns

See [eth_getBlockByHash](#eth_getblockbyhash)

### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getBlockByNumber","params":["latest", true],"id":1}'
```

Result see [eth_getBlockByHash](#eth_getblockbyhash)

***

## eth_getTransactionByHash

Returns the information about a transaction requested by transaction hash.

### Parameters

1. `DATA`, 32 Bytes - hash of a transaction

### Example Parameters

```js
params: [
   "0x7aac91a30b45d03f43d7949e673b1243c93d0adc7190f184681e83eac705a19f"
]
```

### Returns

`Object` - A transaction object, or `null` when no transaction was found:

  - `blockHash`: `DATA`, 32 Bytes - hash of the block where this transaction was in. `null` when its pending.
  - `blockNumber`: `QUANTITY` - block number where this transaction was in. `null` when its pending.
  - `from`: `DATA`, 20 Bytes - address of the sender.
  - `gas`: `QUANTITY` - gas provided by the sender.
  - `gasPrice`: `QUANTITY` - gas price provided by the sender in Wei.
  - `hash`: `DATA`, 32 Bytes - hash of the transaction.
  - `input`: `DATA` - the data send along with the transaction.
  - `nonce`: `QUANTITY` - the number of transactions made by the sender prior to this one.
  - `to`: `DATA`, 20 Bytes - address of the receiver. `null` when its a contract creation transaction.
  - `transactionIndex`: `QUANTITY` - integer of the transaction's index position in the block. `null` when its pending.
  - `value`: `QUANTITY` - value transferred in Wei.
  - `v`: `QUANTITY` - ECDSA recovery id
  - `r`: `QUANTITY` - ECDSA signature r
  - `s`: `QUANTITY` - ECDSA signature s

### Example

```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getTransactionByHash","params":["0x7aac91a30b45d03f43d7949e673b1243c93d0adc7190f184681e83eac705a19f"],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":{
    "blockHash":"0x1d59ff54b1eb26b013ce3cb5fc9dab3705b415a67127a003c3e61eb445bb8df2",
    "blockNumber":"0x5daf3b", // 6139707
    "from":"0xa7d9ddbe1f17865597fbd27ec712455208b6b76d",
    "gas":"0xc350", // 50000
    "gasPrice":"0x4a817c800", // 20000000000
    "hash":"0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b",
    "input":"0x68656c6c6f21",
    "nonce":"0x15", // 21
    "to":"0xf02c1c8e6114b1dbe8937a39260b5b0a374432bb",
    "transactionIndex":"0x41", // 65
    "value":"0xf3dbb76162000", // 4290000000000000
    "v":"0x25", // 37
    "r":"0x1b5e176d927f8e9ab405058b2d2457392da3e20f328b16ddabcebc33eaac5fea",
    "s":"0x4ba69724e8f69de52f0125ad8b3c5c2cef33019bac3249e2c0a2192766d1721c"
  }
}
```

```
nonce:0x8a2b894cf840ec4b
hash:0x7aac91a30b45d03f43d7949e673b1243c93d0adc7190f184681e83eac705a19f
to:<nil>
blockHash:0x2b44af6bf92f16a53e2f75bb7ea09543217f9bc9b6743f796badaaf2a4ab5e15
from:0xf0c11b00f37cff38b4eb29be2ad6d541b841e466
value:0x0
r:0x4e4fed2429b3816a4f6ba6a973a7e882b69c8e7e00f9bc1aac4ef16debc10b1
blockNumber:0x17
gas:0x0
gasPrice:0x0
input:0x608060405260008060006101000a81548163ffffffff021916908360030b63ffffffff16021790555034801561003457600080fd5b5061015f806100446000396000f3fe608060405234801561001057600080fd5b506004361061005e576000357c0100000000000000000000000000000000000000000000000000000000900480630c6af81614610063578063262a9dff1461009457806367e0badb146100b8575b600080fd5b6100926004803603602081101561007957600080fd5b81019080803560030b90602001909291905050506100dc565b005b61009c610102565b604051808260030b60030b815260200191505060405180910390f35b6100c0610114565b604051808260030b60030b815260200191505060405180910390f35b806000806101000a81548163ffffffff021916908360030b63ffffffff16021790555050565b6000809054906101000a900460030b81565b60008060009054906101000a900460030b90509056fea265627a7a723158202f431f1087cf66d88ca0871e94603a8d6b3fe9c05c6c334762b801417af7959864736f6c63430005110032
transactionIndex:0x0
v:0x27c
s:0x7eae6bad6a318409a467d187819511f7bf47a5a0970955a1bc34285e6b3429ee
txType:0x1
```

***

## eth_getTransactionByBlockHashAndIndex

Returns information about a transaction by block hash and transaction index position.

### Parameters

1. `DATA`, 32 Bytes - hash of a block.
2. `QUANTITY` - integer of the transaction index position.

### Example Parameters

```js
params: [
   '0xcd021448dcc4f9a6a7fd553b541199a2ebaab1de8e739bc6680eb0e62a870e11',
   '0x0' // 0
]
```

### Returns

See [eth_getTransactionByHash](#eth_gettransactionbyhash)

### Example

```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getTransactionByBlockHashAndIndex","params":["0xcd021448dcc4f9a6a7fd553b541199a2ebaab1de8e739bc6680eb0e62a870e11", "0x0"],"id":1}'
```

Result see [eth_getTransactionByHash](#eth_gettransactionbyhash)

***

## eth_getTransactionByBlockNumberAndIndex

Returns information about a transaction by block number and transaction index position.

### Parameters

1. `QUANTITY|TAG` - a block number, or the string `"earliest"`, `"latest"` or `"pending"`, as in the [default block parameter](#the-default-block-parameter).
2. `QUANTITY` - the transaction index position.

### Example Parameters

```js
params: [
   'latest', 
   '0x0' // 0
]
```

### Returns

See [eth_getTransactionByHash](#eth_gettransactionbyhash)

### Example

```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getTransactionByBlockNumberAndIndex","params":["latest", "0x0"],"id":1}'
```

Result see [eth_getTransactionByHash](#eth_gettransactionbyhash)

***

## eth_getRawTransactionByHash*

Returns the information about a raw transaction requested by transaction hash.

### Parameters

1. `DATA`, 32 Bytes - hash of a transaction

### Example Parameters

```js
params: [
   "0x7aac91a30b45d03f43d7949e673b1243c93d0adc7190f184681e83eac705a19f"
]
```

### Returns

`DATA`

### Example

```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getTransactionByHash","params":["0x7aac91a30b45d03f43d7949e673b1243c93d0adc7190f184681e83eac705a19f"],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":{
    0xf901f9888a2b894cf840ec4b80808080b901a3608060405260008060006101000a81548163ffffffff021916908360030b63ffffffff16021790555034801561003457600080fd5b5061015f806100446000396000f3fe608060405234801561001057600080fd5b506004361061005e576000357c0100000000000000000000000000000000000000000000000000000000900480630c6af81614610063578063262a9dff1461009457806367e0badb146100b8575b600080fd5b6100926004803603602081101561007957600080fd5b81019080803560030b90602001909291905050506100dc565b005b61009c610102565b604051808260030b60030b815260200191505060405180910390f35b6100c0610114565b604051808260030b60030b815260200191505060405180910390f35b806000806101000a81548163ffffffff021916908360030b63ffffffff16021790555050565b6000809054906101000a900460030b81565b60008060009054906101000a900460030b90509056fea265627a7a723158202f431f1087cf66d88ca0871e94603a8d6b3fe9c05c6c334762b801417af7959864736f6c634300051100320182027ca004e4fed2429b3816a4f6ba6a973a7e882b69c8e7e00f9bc1aac4ef16debc10b1a07eae6bad6a318409a467d187819511f7bf47a5a0970955a1bc34285e6b3429ee
  }
}
```

***

## eth_getRawTransactionByBlockHashAndIndex*

Returns information about a raw transaction by block hash and transaction index position.

### Parameters

1. `DATA`, 32 Bytes - hash of a block.
2. `QUANTITY` - integer of the transaction index position.

### Example Parameters

```js
params: [
   '0xcd021448dcc4f9a6a7fd553b541199a2ebaab1de8e739bc6680eb0e62a870e11',
   '0x0' // 0
]
```

### Returns

See [eth_getRawTransactionByHash](#eth_getrawtransactionbyhash)

### Example

```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getTransactionByBlockHashAndIndex","params":["0xcd021448dcc4f9a6a7fd553b541199a2ebaab1de8e739bc6680eb0e62a870e11", "0x0"],"id":1}'
```

Result see [eth_getRawTransactionByHash](#eth_getrawtransactionbyhash)

***

## eth_getRawTransactionByBlockNumberAndIndex*

Returns information about a raw transaction by block number and transaction index position.

### Parameters

1. `QUANTITY|TAG` - a block number, or the string `"earliest"`, `"latest"` or `"pending"`, as in the [default block parameter](#the-default-block-parameter).
2. `QUANTITY` - the transaction index position.

### Example Parameters

```js
params: [
   'latest', 
   '0x0' // 0
]
```

### Returns

See [eth_getRawTransactionByHash](#eth_getrawtransactionbyhash)

### Example

```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getTransactionByBlockNumberAndIndex","params":["latest", "0x0"],"id":1}'
```

Result see [eth_getRawTransactionByHash](#eth_getrawtransactionbyhash)

***

## eth_getTransactionReceipt (TODO)

Returns the receipt of a transaction by transaction hash.

**Note** That the receipt is not available for pending transactions.

### Parameters

1. `DATA`, 32 Bytes - hash of a transaction

### Example Parameters
```js
params: [
   '0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238'
]
```

### Returns

`Object` - A transaction receipt object, or `null` when no receipt was found:

  - `transactionHash `: `DATA`, 32 Bytes - hash of the transaction.
  - `transactionIndex`: `QUANTITY` - integer of the transaction's index position in the block.
  - `blockHash`: `DATA`, 32 Bytes - hash of the block where this transaction was in.
  - `blockNumber`: `QUANTITY` - block number where this transaction was in.
  - `from`: `DATA`, 20 Bytes - address of the sender.
  - `to`: `DATA`, 20 Bytes - address of the receiver. null when it's a contract creation transaction.
  - `cumulativeGasUsed `: `QUANTITY ` - The total amount of gas used when this transaction was executed in the block.
  - `gasUsed `: `QUANTITY ` - The amount of gas used by this specific transaction alone.
  - `contractAddress `: `DATA`, 20 Bytes - The contract address created, if the transaction was a contract creation, otherwise `null`.
  - `logs`: `Array` - Array of log objects, which this transaction generated.
  - `logsBloom`: `DATA`, 256 Bytes - Bloom filter for light clients to quickly retrieve related logs.
  
It also returns _either_ :

  - `root` : `DATA` 32 bytes of post-transaction stateroot (pre Byzantium)
  - `status`: `QUANTITY` either `1` (success) or `0` (failure) 

### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getTransactionReceipt","params":["0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238"],"id":1}'

// Result
{
"id":1,
"jsonrpc":"2.0",
"result": {
     transactionHash: '0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238',
     transactionIndex:  '0x1', // 1
     blockNumber: '0xb', // 11
     blockHash: '0xc6ef2fc5426d6ad6fd9e2a26abeab0aa2411b7ab17f30a99d3cb96aed1d1055b',
     cumulativeGasUsed: '0x33bc', // 13244
     gasUsed: '0x4dc', // 1244
     contractAddress: '0xb60e8dd61c5d32be8058bb8eb970870f07233155', // or null, if none was created
     logs: [{
         // logs as returned by getFilterLogs, etc.
     }, ...],
     logsBloom: "0x00...0", // 256 byte bloom filter
     status: '0x1'
  }
}
```

***

## eth_pendingTransactions*(**TODO**)

Returns the pending transactions list.

### Parameters

none

### Returns

`Array` - A list of pending transactions.

### Example

```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_pendingTransactions","params":[],"id":1}'

// Result
{
"id":1,
"jsonrpc":"2.0",
"result": [{ see_
   }]
}
```

***

## eth_pendingTransactionsLength*

Returns the length of the pending transactions list.

### Parameters

none

### Returns

`Array` - A list of pending transactions.

### Example

```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_pendingTransactionsLength","params":[],"id":1}'

// Result
{
"id":1,
"jsonrpc":"2.0",
"result": 0
}
```

***

## eth_newFilter (TODO)

Creates a filter object, based on filter options, to notify when the state changes (logs).
To check if the state has changed, call [eth_getFilterChanges](#eth_getfilterchanges).

### A note on specifying topic filters:
Topics are order-dependent. A transaction with a log with topics [A, B] will be matched by the following topic filters:
* `[]` "anything"
* `[A]` "A in first position (and anything after)"
* `[null, B]` "anything in first position AND B in second position (and anything after)"
* `[A, B]` "A in first position AND B in second position (and anything after)"
* `[[A, B], [A, B]]` "(A OR B) in first position AND (A OR B) in second position (and anything after)"

### Parameters

1. `Object` - The filter options:
  - `fromBlock`: `QUANTITY|TAG` - (optional, default: `"latest"`) Integer block number, or `"latest"` for the last mined block or `"pending"`, `"earliest"` for not yet mined transactions.
  - `toBlock`: `QUANTITY|TAG` - (optional, default: `"latest"`) Integer block number, or `"latest"` for the last mined block or `"pending"`, `"earliest"` for not yet mined transactions.
  - `address`: `DATA|Array`, 20 Bytes - (optional) Contract address or a list of addresses from which logs should originate.
  - `topics`: `Array of DATA`,  - (optional) Array of 32 Bytes `DATA` topics. Topics are order-dependent. Each topic can also be an array of DATA with "or" options.

### Example Parameters
```js
params: [{
  "fromBlock": "0x1",
  "toBlock": "0x2",
  "address": "0x8888f1f195afa192cfee860698584c030f4c9db1",
  "topics": ["0x000000000000000000000000a94f5374fce5edbc8e2a8697c15331677e6ebf0b", null, ["0x000000000000000000000000a94f5374fce5edbc8e2a8697c15331677e6ebf0b", "0x0000000000000000000000000aff3454fce5edbc8cca8697c15331677e6ebccc"]]
}]
```

### Returns

`QUANTITY` - A filter id.

### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_newFilter","params":[{"topics":["0x0000000000000000000000000000000000000000000000000000000012341234"]}],"id":73}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0x1" // 1
}
```

***

## eth_newBlockFilter (TODO)

Creates a filter in the node, to notify when a new block arrives.
To check if the state has changed, call [eth_getFilterChanges](#eth_getfilterchanges).

### Parameters
None

### Returns

`QUANTITY` - A filter id.

### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_newBlockFilter","params":[],"id":73}'

// Result
{
  "id":1,
  "jsonrpc":  "2.0",
  "result": "0x1" // 1
}
```

***

## eth_newPendingTransactions*

***

## eth_newPendingTransactionFilter (TODO)

Creates a filter in the node, to notify when new pending transactions arrive.
To check if the state has changed, call [eth_getFilterChanges](#eth_getfilterchanges).

### Parameters
None

### Returns

`QUANTITY` - A filter id.

### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_newPendingTransactionFilter","params":[],"id":73}'

// Result
{
  "id":1,
  "jsonrpc":  "2.0",
  "result": "0x1" // 1
}
```

***

## eth_uninstallFilter (TODO)

Uninstalls a filter with given id. Should always be called when watch is no longer needed.
Additonally Filters timeout when they aren't requested with [eth_getFilterChanges](#eth_getfilterchanges) for a period of time.

### Parameters

1. `QUANTITY` - The filter id.

### Example Parameters
```js
params: [
  "0xb" // 11
]
```

### Returns

`Boolean` - `true` if the filter was successfully uninstalled, otherwise `false`.

### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_uninstallFilter","params":["0xb"],"id":73}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": true
}
```

***

## eth_getFilterChanges (TODO)

Polling method for a filter, which returns an array of logs which occurred since last poll.

### Parameters

1. `QUANTITY` - the filter id.

### Example Parameters
```js
params: [
  "0x16" // 22
]
```

### Returns

`Array` - Array of log objects, or an empty array if nothing has changed since last poll.

- For filters created with `eth_newBlockFilter` the return are block hashes (`DATA`, 32 Bytes), e.g. `["0x3454645634534..."]`.
- For filters created with `eth_newPendingTransactionFilter ` the return are transaction hashes (`DATA`, 32 Bytes), e.g. `["0x6345343454645..."]`.
- For filters created with `eth_newFilter` logs are objects with following params:

  - `removed`: `TAG` - `true` when the log was removed, due to a chain reorganization. `false` if its a valid log.
  - `logIndex`: `QUANTITY` - integer of the log index position in the block. `null` when its pending log.
  - `transactionIndex`: `QUANTITY` - integer of the transactions index position log was created from. `null` when its pending log.
  - `transactionHash`: `DATA`, 32 Bytes - hash of the transactions this log was created from. `null` when its pending log.
  - `blockHash`: `DATA`, 32 Bytes - hash of the block where this log was in. `null` when its pending. `null` when its pending log.
  - `blockNumber`: `QUANTITY` - the block number where this log was in. `null` when its pending. `null` when its pending log.
  - `address`: `DATA`, 20 Bytes - address from which this log originated.
  - `data`: `DATA` - contains the non-indexed arguments of the log.
  - `topics`: `Array of DATA` - Array of 0 to 4 32 Bytes `DATA` of indexed log arguments. (In *solidity*: The first topic is the *hash* of the signature of the event (e.g. `Deposit(address,bytes32,uint256)`), except you declared the event with the `anonymous` specifier.)

### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getFilterChanges","params":["0x16"],"id":73}'

// Result
{
  "id":1,
  "jsonrpc":"2.0",
  "result": [{
    "logIndex": "0x1", // 1
    "blockNumber":"0x1b4", // 436
    "blockHash": "0x8216c5785ac562ff41e2dcfdf5785ac562ff41e2dcfdf829c5a142f1fccd7d",
    "transactionHash":  "0xdf829c5a142f1fccd7d8216c5785ac562ff41e2dcfdf5785ac562ff41e2dcf",
    "transactionIndex": "0x0", // 0
    "address": "0x16c5785ac562ff41e2dcfdf829c5a142f1fccd7d",
    "data":"0x0000000000000000000000000000000000000000000000000000000000000000",
    "topics": ["0x59ebeb90bc63057b6515673c3ecf9438e5058bca0f92585014eced636878c9a5"]
    },{
      ...
    }]
}
```

***

## eth_getFilterLogs (TODO)

Returns an array of all logs matching filter with given id.

### Parameters

1. `QUANTITY` - The filter id.

### Example Parameters
```js
params: [
  "0x16" // 22
]
```

### Returns

See [eth_getFilterChanges](#eth_getfilterchanges)

### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getFilterLogs","params":["0x16"],"id":74}'
```

Result see [eth_getFilterChanges](#eth_getfilterchanges)

***

## eth_getLogs (TODO)

Returns an array of all logs matching a given filter object.

### Parameters

1. `Object` - The filter options:
  - `fromBlock`: `QUANTITY|TAG` - (optional, default: `"latest"`) Integer block number, or `"latest"` for the last mined block or `"pending"`, `"earliest"` for not yet mined transactions.
  - `toBlock`: `QUANTITY|TAG` - (optional, default: `"latest"`) Integer block number, or `"latest"` for the last mined block or `"pending"`, `"earliest"` for not yet mined transactions.
  - `address`: `DATA|Array`, 20 Bytes - (optional) Contract address or a list of addresses from which logs should originate.
  - `topics`: `Array of DATA`,  - (optional) Array of 32 Bytes `DATA` topics. Topics are order-dependent. Each topic can also be an array of DATA with "or" options.
  - `blockhash`:  `DATA`, 32 Bytes - (optional) With the addition of EIP-234 (platone >= v1.8.13 or Parity >= v2.1.0), `blockHash` is a new filter option which restricts the logs returned to the single block with the 32-byte hash `blockHash`.  Using `blockHash` is equivalent to `fromBlock` = `toBlock` = the block number with hash `blockHash`.  If `blockHash` is present in the filter criteria, then neither `fromBlock` nor `toBlock` are allowed.

### Example Parameters
```js
params: [{
  "topics": ["0x000000000000000000000000a94f5374fce5edbc8e2a8697c15331677e6ebf0b"]
}]
```

### Returns

See [eth_getFilterChanges](#eth_getfilterchanges)

### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getLogs","params":[{"topics":["0x000000000000000000000000a94f5374fce5edbc8e2a8697c15331677e6ebf0b"]}],"id":74}'
```

Result see [eth_getFilterChanges](#eth_getfilterchanges)

***

## eth_logs*

***
