==========
venachain
==========

venachain.defaultAccount
==============================

**描述**: 

设置默认账户。默认的地址在使用下述方法时使用，你也可以选择通过指定 ``from`` 属性，来覆盖这个默认设置。

.. code:: bash

   venachain.sendTransaction()
   venachain.call()

**参数**:

- ``String`` : 20字节的当前设置的默认地址。

**操作**

.. code:: bash

   > venachain.defaultAccount = '0x8888f1f195afa192cfee860698584c030f4c9db1';

**输出结果**

.. code:: console

    "0x8888f1f195afa192cfee860698584c030f4c9db1"

venachain.defaultBlock
==========================

**描述**:

使用下述方法时，会使用默认块设置，你也可以通过传入 ``defaultBlock`` 来覆盖默认配置。

.. code:: bash

   venachain.getBalance()
   venachain.getCode()
   venachain.getTransactionCount()
   venachain.getStorageAt()
   venachain.call()
   contract.myMethod.call()
   contract.myMethod.estimateGas()

**参数**:

可选的块参数，可能下述值中的一个:

- ``Number`` - 区块号
- ``earliest``: ``String`` - 创世块
- ``latest``: ``String`` - 最近刚出的最新块，当前的区块头
- ``pending``: ``String`` - 当前正在 ``mine`` 的区块，包含正在打包的交易

.. note:: 默认值是 ``latest``

**返回值**:

- ``Number|String`` - 默认要查状态的区块号。

**操作**

.. code:: bash

   > venachain.defaultBlock = 18

**输出结果**

.. code:: console

   18

venachain.syncing
====================

**描述** 

这个属性是只读的。如果正在同步，返回同步对象。否则返回 ``false`` 。

- 同步方式:

.. code:: bash

   venachain.syncing

- 异步方式:

.. code:: bash

   venachain.getSyncing(callback(error, result){ … })

**返回值**

``Object|Boolean`` -
如果正在同步，返回含下面属性的同步对象。否则返回 ``false`` 。

- ``startingBlock``: ``Number`` - 同步开始区块号
- ``currentBlock``: ``Number`` - 节点当前正在同步的区块号
- ``highestBlock``: ``Number`` - 预估要同步到的区块

**操作**

.. code:: bash

   > venachain.syncing

**输出结果**

- 不同步时输出:

.. code:: console

   false

- 同步时输出:

.. code:: js

   {
       startingBlock: 100,
       currentBlock: 312,
       highestBlock: 512,
       knownStates: 234566,
       pulledStates: 123455
   }

venachain.gasPrice
=====================

**描述** 

属性是只读的，返回当前的gas价格。这个值由最近几个块的gas价格的中值决定。

- 同步方式:

.. code:: bash

   venachain.gasPrice

- 异步方式:

.. code:: bash

   venachain.getGasPrice(callback(error, result){ … })

**返回值**

- ``BigNumber`` - 当前的gas价格的 ``BigNumber`` 实例，以 ``wei`` 为单位。

**操作**

.. code:: bash

   > venachain.gasPrice.toString()

**输出结果**

.. code:: console

   "0"

venachain.accounts
=====================

**描述** 

只读属性，返回当前节点持有的帐户列表。

- 同步方式:

.. code:: bash

   venachain.accounts

- 异步方式:

.. code:: bash

   venachain.getAccounts(callback(error, result){ … })

**返回值**

- ``Array`` - 节点持有的帐户列表。

**操作**

.. code:: bash

   > venachain.accounts

**输出结果**

.. code:: console

   ["0x4bf2e487a1f57d26d57ad9b9821a827e45b9e8e6", "0x7dc33e8ea643ae40459eec74a3879a7018328160"]

venachain.blockNumber
========================

**描述** 

返回当前区块号。如果之前设置过默认区块，这里返回的就是默认区块的区块号。如果没有设置默认区块，则返回最新的区块latest。

- 同步方式:

.. code:: bash

   venachain.blockNumber

- 异步方式:

.. code:: bash

   venachain.getBlockNumber(callback(error, result){ … })

**返回值**

一个Promise对象，其解析值为最近一个块的编号，Number类型。

**操作**

.. code:: bash

   > venachain.blockNumber

**输出结果**

.. code:: console

   20

venachain.getBalance
======================

**描述** 

获得在指定区块时给定地址的余额。

.. code:: bash

   venachain.getBalance(addressHexString [, defaultBlock] [, callback])

**参数**

-  ``String`` - 要查询余额的地址。
-  ``Number|String`` -（可选）如果不设置此值使用 ``venachain.defaultBlock`` 设定的块，否则使用指定的块。
-  ``Funciton`` - （可选）回调函数，用于支持异步的方式执行[async]。

**返回值**

- ``String`` - 一个包含给定地址的当前余额的 ``BigNumber`` 实例，单位为 ``wei`` 。

**操作**

.. code:: bash

   > venachain.getBalance(venachain.accounts[0]);

**输出结果**

.. code:: console

   0

venachain.getBlock
====================

**描述** 

返回块号或区块哈希值所对应的区块

.. code:: bash

   venachain.getBlock(blockHashOrBlockNumber [, returnTransactionObjects] [, callback])

**参数**

-  ``Number|String`` -（可选）如果未传递参数，默认使用 ``venachain.defaultBlock`` 定义的块，否则使用指定区块。
-  ``Boolean`` -（可选）默认值为 ``false`` 。 ``true`` 会将区块包含的所有交易作为对象返回。否则只返回交易的哈希。
-  ``Function`` - 回调函数，用于支持异步的方式执行[async]。

**返回值**: - 区块对象

- ``Number`` - 区块号。当这个区块处于 ``pending`` 将会返回 ``null`` 。
- ``hash`` - 字符串，区块的哈希串。当这个区块处于 ``pending`` 将会返回 ``null`` 。
- ``parentHash`` - 字符串，32字节的父区块的哈希值。
- ``nonce`` - 字符串，8字节。POW生成的哈希。当这个区块处于 ``pending`` 将会返回 ``null`` 。
- ``sha3Uncles`` - 字符串，32字节。叔区块的哈希值。
- ``logsBloom`` - 字符串，区块日志的布隆过滤器。当这个区块处于 ``pending`` 将会返回 ``null`` 。
- ``transactionsRoot`` - 字符串，32字节，区块的交易前缀树的根。
- ``stateRoot`` - 字符串，32字节。区块的最终状态前缀树的根。
- ``miner`` - 字符串，20字节。这个区块获得奖励的矿工。
- ``difficulty``: ``BigNumber`` - 当前块的难度，整数。
- ``totalDifficulty``: ``BigNumber`` - 区块链到当前块的总难度，整数。
- ``extraData`` - 字符串。当前块的 ``extra data`` 字段。
- ``size`` - ``Number`` 。当前这个块的字节大小。
- ``gasLimit``: ``Number`` - 当前区块允许使用的最大 ``gas`` 。
- ``gasUsed`` - 当前区块累计使用的总的 ``gas`` 。
- ``timestamp``: `Number`` - 区块打包时的 ``unix`` 时间戳。
- ``transactions``: ``数组`` - 交易对象。或者是32字节的交易哈希。
- ``uncles``: ``数组`` - 叔哈希的数组。

**操作**

.. code:: bash

   > venachain.getBlock(17)

**输出结果**

.. code:: js

   {
     difficulty: 0,
     extraData: "0xdb8301000087706c61746f6e6588676f312e31352e37856c696e757800000000f90164f854942d9bc006e0166d5bedd7a9e47aaa615fcecf66ff94a40d5f218c67ade9f273819cd8a52b211e4f31f494bc34e1b6dc784c7e2b21ee7c8bf8ef4317f046da94d003a6f06cfdaf9c25b708f31f135997f3a2785cb841d15f3b6c8b7688674166599374bf19d201a661c4739148da71bbdde96c07dd5f040c3734edc11144c372febecd1c98a491ed0abf0687824c0a0937198590551f00f8c9b841bc75612b71b9c05e2cf231fd4acb4704fab10374e9d43ebcdf0818489b62f2b5488053fe96c7535bf580bcf9e38e3a585e0223e58b6e624f07c5e6845c4bf0c300b841f13f66c421a9d6a7e96bee1c505cec5a95906d7fb5e695a4660f81bcceb87ff76f0eef61d7e6776546f9821d8c4624cfc88500147b4a366e12d426b55cf80a3f00b8413a6d1c1d4f211dc3d103ec1b895579e13a26713490d0430269489e7e9e629f683bbfef7479838bc41486a760db3a67f4f24c9b64b5d286bcc05ec454c870ff3901",
     gasLimit: 10000000000,
     gasUsed: 21000,
     hash: "0x747665da68cfa82c7f03cf790de4352f5f6649d79c1a4179116e1bb88efd0e2d",
     logsBloom: "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
     miner: "0xa40d5f218c67ade9f273819cd8a52b211e4f31f4",
     mixHash: "0x63746963616c2062797a616e74696e65206661756c7420746f6c6572616e6365",
     nonce: "0x02dc7ac944c9d18f3ea6283d7c88e3cbecad5278c73c3968f284aebd2969af7a670d4f47fa66d4d6e1f6dfa1470c9232b914086f34a349ccd4c13484a2b1b5038817eb03aa949e455649fd53ac5931f6f2",
     number: 17,
     parentHash: "0x6ca2094ed6ca9c785346acfef8b2644aff0ab0d0c5e82010e1377773a717a5c1",
     receiptsRoot: "0x056b23fbba480696b65fe5a59b8f2148a1299103c4f57df839233af2cf4ca2d2",
     size: 1054,
     stateRoot: "0xf77348976c49ccd704776c83efea36943ca9f61fea8863d3bd43df67378955de",
     timestamp: 1615275332922,
     totalDifficulty: 0,
     transactions: ["0x4440d637d55ccba7c87559b55b27fa10f1c7965725c76fda007ee27c38a2c74b"],
     transactionsRoot: "0x965e73160f963001a399c77c4118ab826b2994100feb3bd1fa0ab10369410028"
   }

venachain.getBlockTransactionCount
=====================================

**描述** 

返回指定区块的交易数量

.. code:: bash

   venachain.getBlockTransactionCount(hashStringOrBlockNumber [, callback])

**参数**

-  ``Number|String`` -（可选）如果未传递参数，默认使用 ``venachain.defaultBlock`` 定义的块，否则使用指定区块。
-  ``Function`` - 回调函数，用于支持异步的方式执行[async]。

**返回值**

- ``Nubmer`` - 给定区块的交易数量。

**操作**

.. code:: bash

   > venachain.getBlockTransactionCount(17);

**输出结果**

.. code:: console

   1

venachain.getTransaction
============================

**描述** 

返回匹配指定交易哈希值的交易。

.. code:: bash

   venachain.getTransaction(transactionHash [, callback])

**参数**

-  ``String`` - 交易的哈希值。
-  ``Function`` - 回调函数，用于支持异步的方式执行[async]。

**返回值**

``Object`` - 一个交易对象

-  ``hash``: ``String`` - 32字节，交易的哈希值。
-  ``nonce``: ``Number`` - 交易的发起者在之前进行过的交易数量。
-  ``blockHash``: ``String`` - 32字节。交易所在区块的哈希值。当这个区块处于 ``pending`` 将会返回 ``null`` 。
-  ``blockNumber``: ``Number`` - 交易所在区块的块号。当这个区块处于 ``pending`` 将会返回 ``null`` 。
-  ``transactionIndex``: ``Number`` - 整数。交易在区块中的序号。当这个区块处于 ``pending`` 将会返回 ``null`` 。
-  ``from``: ``String`` - 20字节，交易发起者的地址。
-  ``to``: ``String`` - 20字节，交易接收者的地址。当这个区块处于 ``pending`` 将会返回 ``null`` 。
-  ``value``: ``BigNumber`` - 交易附带的货币量，单位为 ``Wei`` 。
-  ``gasPrice``: ``BigNumber`` - 交易发起者配置的 ``gas`` 价格，单位是 ``wei`` 。
-  ``gas``: ``Number`` - 交易发起者提供的 ``gas`` 。
-  ``input``: ``String`` - 交易附带的数据。

**操作**

.. code:: bash

   >  venachain.getTransaction("0xf3e8bc7f626017bcd49e9f9e9a136de0625cb10aeeeaf4831b40ec1430e1ac5d"

**输出结果**

.. code:: js

   {
     blockHash: "0xe72c148a94ac10682617de4ab88b95f678dbf1312ab8b756f04a98ef93f3fa0e",
     blockNumber: 22,
     from: "0x4bf2e487a1f57d26d57ad9b9821a827e45b9e8e6",
     gas: 90000,
     gasPrice: 2100000,
     hash: "0xf3e8bc7f626017bcd49e9f9e9a136de0625cb10aeeeaf4831b40ec1430e1ac5d",
     input: "0x",
     nonce: 435967907061903,
     r: "0xd1e1d0799dbcad2eb0eab9fdaf8aa08766d0039e471ef6c896900d0fca93fda8",
     s: "0x3799d3087d2fd6984ac2291021fbbd2ddc469a6839eb1e1d2cc1b4d85f9a685f",
     to: "0x7dc33e8ea643ae40459eec74a3879a7018328160",
     transactionIndex: 0,
     txType: "0x0",
     v: "0x27b",
     value: 0
   }

venachain.getTransactionFromBlock
====================================

**描述** 

返回指定区块的指定序号的交易。

.. code:: bash

   getTransactionFromBlock(hashStringOrNumber, indexNumber [, callback])

**参数**

-  ``String`` - 区块号或哈希。或者是 ``earliest`` ， ``latest`` 或 ``pending`` 。查看 ``venachain.defaultBlock`` 了解可选值。
-  ``Number`` - 交易的序号。
-  ``Function`` - 回调函数，用于支持异步的方式执行[async]。

**返回值**

- ``Object`` - 交易对象，详见 ``venachain.getTransaction``

操作:

.. code:: bash

   > venachain.getTransactionFromBlock(18,0)

**输出结果**

.. code::json

   {
     blockHash: "0x9ed12a11ace0e2d00f102e9bc4095e0df8687af9aaf1046c01f7b806b717cdeb",
     blockNumber: 18,
     from: "0x4bf2e487a1f57d26d57ad9b9821a827e45b9e8e6",
     gas: 90000,
     gasPrice: 0,
     hash: "0x7f0056f08d15a9145850db725ce14922e70cc27f8e49dcf04536781adf117cf8",
     input: "0x",
     nonce: 792086861801981,
     r: "0x4cf27c9333e0dfd824f8c24b396c505b59688618ba9f6bc26e90f476165a3bf5",
     s: "0x5171da8895f1b2d9d138beb1f99154c2006d66bd6cef3eddb8f87de92acc8129",
     to: "0x7dc33e8ea643ae40459eec74a3879a7018328160",
     transactionIndex: 0,
     txType: "0x0",
     v: "0x27b",
     value: 0
   }

venachain.getTransactionReceipt
===================================

**描述** 

通过一个交易哈希，返回一个交易的收据。

.. note:: 处于 ``pending`` 状态的交易，收据是不可用的。

.. code:: bash

   venachain.getTransactionReceipt(hashString [, callback])

**参数**

-  ``String`` - 交易的哈希
-  ``Function`` - 回调函数，用于支持异步的方式执行[async]。

**返回值**

``Object`` - 交易的收据对象，如果找不到返回`null

-  ``blockHash``: ``String`` - 32字节，这个交易所在区块的哈希。
-  ``blockNumber``: ``Number`` - 交易所在区块的块号。
-  ``transactionHash``: ``String`` - 32字节，交易的哈希值。
-  ``transactionIndex``: ``Number`` - 交易在区块里面的序号，整数。
-  ``from``: ``String`` - 20字节，交易发送者的地址。
-  ``to``: ``String`` - 20字节，交易接收者的地址。如果是一个合约创建的交易，返回 ``null`` 。
-  ``cumulativeGasUsed``: ``Number`` - 当前交易执行后累计花费的 ``gas`` 总值。
-  ``gasUsed``: ``Number`` - 执行当前这个交易单独花费的 ``gas`` 。
-  ``contractAddress``: ``String`` - 20字节，创建的合约地址。如果是一个合约创建交易，返回合约地址，其它情况返回 ``null`` 。
-  ``logs``: ``Array`` - 这个交易产生的日志对象数组。

**操作**

.. code:: bash

   > venachain.getTransactionReceipt("0xf3e8bc7f626017bcd49e9f9e9a136de0625cb10aeeeaf4831b40ec1430e1ac5d")

输出结果:

.. code:: js

   {
     blockHash: "0xe72c148a94ac10682617de4ab88b95f678dbf1312ab8b756f04a98ef93f3fa0e",
     blockNumber: 22,
     contractAddress: null,
     cumulativeGasUsed: 42000,
     from: "0x4bf2e487a1f57d26d57ad9b9821a827e45b9e8e6",
     gasUsed: 21000,
     logs: [],
     logsBloom: "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
     status: "0x1",
     to: "0x7dc33e8ea643ae40459eec74a3879a7018328160",
     transactionHash: "0xf3e8bc7f626017bcd49e9f9e9a136de0625cb10aeeeaf4831b40ec1430e1ac5d",
     transactionIndex: 0
   }

venachain.getTransactionCount
===============================

**描述** 

返回指定地址发起的交易数。

.. code:: bash

   venachain.getTransactionCount(addressHexString [, defaultBlock] [, callback])

**参数**

-  ``String`` - 要获得交易数的地址。
-  ``Number|String`` -（可选）如果未传递参数，默认使用 ``venachain.defaultBlock`` 定义的块，否则使用指定区块。
-  ``Function`` - 回调函数，用于支持异步的方式执行[async]。

**返回值**

- ``Number`` - 指定地址发送的交易数量。

**操作**

.. code:: bash

   > venachain.getTransactionCount(venachain.accounts[0])

**输出结果**

.. code:: console

   19

venachain.sendTransaction
============================

**描述** 

发送一个交易到网络。

.. code:: bash

   venachain.sendTransaction(transactionObject [, callback])

**参数**


``Object`` - 要发送的交易对象。

- ``from``: ``String`` - 指定的发送者的地址。如果不指定，使用 ``venachain.defaultAccount`` 。
- ``to``: ``String`` - （可选）交易消息的目标地址，如果是合约创建，则不填.
-  ``value``: ``Number|String|BigNumber`` -（可选）交易携带的货币量，以 ``wei`` 为单位。如果合约创建交易，则为初始的基金。
-  ``gas``: ``Number|String|BigNumber`` - （可选）默认是自动，交易可使用的 ``gas`` ，未使用的 ``gas`` 会退回。
-  ``gasPrice``: ``Number|String|BigNumber`` -（可选）默认是自动确定，交易的 ``gas`` 价格，默认是网络 ``gas`` 价格的平均值。
-  ``data``: ``String`` - （可选）或者包含相关数据的字节字符串，如果是合约创建，则是初始化要用到的代码。
-  ``nonce``: ``Number`` - （可选）整数，使用此值，可以允许你覆盖你自己的相同 ``nonce`` 的，正在 ``pending`` 中的交易。
-  ``Function`` - 回调函数，用于支持异步的方式执行[async]。

**返回值**

- ``String`` - 32字节的交易哈希串。用16进制表示。

.. warning:: 如果交易是一个合约创建，请使用 ``venachain.getTransactionReceipt()`` 在交易完成后获取合约的地址。

**操作**

.. code:: bash

   >  venachain.sendTransaction({from:venachain.accounts[0],to:venachain.accounts[1],value:0})

**输出结果**

.. code:: console

   "0xa7c2dfa781a6bab4016bbdc8e59b916c49c0699545a590a08efd8fc15c099664"

venachain.estimateGas
=======================

**描述** 

在节点的VM节点中执行一个消息调用，或交易。但是不会合入区块链中。返回使用的 ``gas`` 量。

.. code:: bash

   venachain.estimateGas(callObject [, callback])

**参数**

同 ``venachain.sendTransaction`` ，所有的属性都是可选的。

返回值:

- ``Number`` - 模拟的 ``call/transcation`` 花费的 ``gas`` 。

**操作**

.. code:: bash

   >  venachain.estimateGas({
       to: "0xc4abd0339eb8d57087278718986382264244252f",
       data: "0xc6888fa10000000000000000000000000000000000000000000000000000000000000003"
   });

**输出结果**

.. code:: console

   21000