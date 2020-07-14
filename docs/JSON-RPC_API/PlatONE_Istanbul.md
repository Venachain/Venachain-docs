# Istanbul Namespace

The `Istanbul` API provides access to the state of the Istanbul consensus engine. You can use this API to ...

## istanbul_getSnapshot

Retrieves a snapshot of all Isranbul state at a given block.

### Parameters

`QUANTITY|TAG` - integer block number, or the string `"latest"`, `"earliest"` or `"pending"`

### Returns

`Object` - A (**TODO**) object, or `null` when ...:

```go
// Snapshot is the state of the authorization voting at a given point in time.
type Snapshot struct {
    Epoch uint64 // The number of blocks after which to checkpoint and reset the pending votes

    Number uint64                   // Block number where the snapshot was created
    Hash   common.Hash              // Block hash where the snapshot was created
    Votes  []*Vote                  // List of votes cast in chronological order
    Tally  map[common.Address]Tally // Current vote tally to avoid recalculating
    ValSet istanbul.ValidatorSet    // Set of authorized validators at this moment
}
```

### Example

```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"istanbul_getSnapshot","params":["latest"],"id":67}'

// Result
{
  "id":67,
  "jsonrpc": "2.0",
  "result": {
      "hash":"0x6302b1ccaa6944c1b4f150a47fc7fb258fd7488b555d2fcd0378d7d3be5a8bc8",
      "votes":[],
      "tally":{},
      "validators":["0x0a86ced495e8d452a52f24e4ff7dd59bb532bd94"],
      policy:0,
      epoch:1000,
      number:7
  }
}
```

***

## istanbul_getSnapshotAtHash

Retrieves the state snapshot at a given block by block hash.

### Parameters

`DATA`, 32 Bytes - hash of a block.

### Returns

See [istanbul_getSnapshot](#istanbul_getSnapshot)

### Example

```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"istanbul_getSnapshotAtHash","params":["0xa844c6edeb614fca5bb9ca9ef05d978a8732858aabede9ceed32bbf5d3cf3a53"],"id":67}'
```

Result see [istanbul_getSnapshot](#istanbul_getSnapshot)

***

## istanbul_getValidators

Retrieves the list of validators at the specified block.

### Parameters

`QUANTITY|TAG` - integer block number, or the string `"latest"`, `"earliest"` or `"pending"`

### Returns

`Array` - the validator set.

### Example

```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"istanbul_getValidators","params":["latest"],"id":67}'

// Result
{
  "id":67,
  "jsonrpc": "2.0",
  "result": ["0x0a86ced495e8d452a52f24e4ff7dd59bb532bd94", "0x3bcea1c5eda42c85e8d5fa8e116416e86cec98d0", "0xa5e197d546ede9fb87291ff63a52ec9d3d709257", "0xd4e43fa0703c28132933e428b661d2b92ca7974b"]
}
```

***

## istanbul_getValidatorsAtHash

Retrieves the list of validators at the specified block by block hash

### Parameters

`DATA`, 32 Bytes - hash of a block.

### Returns

See [istanbul_getValidators](#istanbul_getValidators)

### Example

```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"istanbul_getValidatorsAtHash","params":["0xa844c6edeb614fca5bb9ca9ef05d978a8732858aabede9ceed32bbf5d3cf3a53"],"id":67}'
```

Result see [istanbul_getValidators](#istanbul_getValidators)

***

## istanbul_candidates

Candidates returns the current candidates the node tries to uphold and vote on.

### Parameters

none

### Returns

`Map` - KEY: the address of candidates, VALUE: `BOOLEAN`, true for voting on, otherwise false.

### Example

```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"istanbul_candidates","params":[],"id":67}'

// Result
{
  "id":67,
  "jsonrpc": "2.0",
  "result": {}
}
```

***

## istanbul_propose

Propose injects a new authorization candidate that the validator will attempt to push through.
### Parameters

none

### Returns

none

### Example

```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"istanbul_propose","params":[],"id":67}'
```

***

## istanbul_discard

Discard drops a currently running candidate, stopping the validator from casting further votes (either for or against).

### Parameters

none

### Returns

none

### Example

```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"istanbul_discard","params":[],"id":67}'
```

***
