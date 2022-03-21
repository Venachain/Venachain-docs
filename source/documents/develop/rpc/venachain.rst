===========
venachain
===========

venachain_gasPrice
======================

返回当前的gas价格，单位：wei。

参数
^^^^^

无

返回值
^^^^^^^^^^

- ``quantity`` : 以wei为单位的当前gas价格

示例代码
^^^^^^^^^

- 请求：

.. code:: sh

    curl -H "Content-Type: application/json" -X POST -d '{"jsonrpc":"2.0","method":"venachain_gasPrice","params":[],"id":1}' "http://127.0.0.1:6791"

- 响应：

.. code:: json

    {
        "jsonrpc":"2.0",
        "id":1,
        "result":"0x0"
    }

venachain_protocolVersion
===============================

返回当前 Venachain 协议的版本号。

参数
^^^^^^

无

返回值
^^^^^^^^^^^

- ``quantity`` : 当前 Venachain 协议的版本号

示例代码
^^^^^^^^^^^

- 请求：

.. code:: sh

    curl -H "Content-Type: application/json" -X POST -d '{"jsonrpc":"2.0","method":"venachain_protocolVersion","params":[],"id":1}' "http://127.0.0.1:6791"

- 响应：

.. code:: json

    {
        "jsonrpc":"2.0",
        "id":1,
        "result":"0x1"
    }

venachain_syncing
====================

这个属性是只读的。对于已经同步的客户端，返回false。对于未同步客户端，返回一个描述同步状态的信息对象。

参数
^^^^^^

无

返回值
^^^^^^^^^^

- ``object | bool`` : 同步状态对象或false。同步对象的结构如下：
    + startingBlock ``quantity`` : 起始块的块高
    + currentBlock ``quantity`` : 节点当前正在同步的区块的块高，同 :ref:`venachain_blockNumber <rpc-venachain-blockNumber>`
    + highestBlock ``quantity`` : 预估要同步到的最高块的块高
    + pulledStates ``quantity`` : 到目前为止已处理的状态条目数
    + knownStates ``quantity`` : 仍需要处理的已知状态条目数

示例代码
^^^^^^^^^^^^

- 请求：

.. code:: sh

    curl -H "Content-Type: application/json" -X POST -d '{"jsonrpc":"2.0","method":"venachain_syncing","params":[],"id":1}' "http://127.0.0.1:6791"

- 响应：

.. code:: json

    {
        "jsonrpc":"2.0",
        "id":1,
        "result":false
    }

.. _rpc-venachain-blockNumber:

venachain_blockNumber
=======================

返回最新区块的高度（区块号）。

参数
^^^^^^

无

返回值
^^^^^^^^

- ``quantity`` : 返回最新区块的块高

示例代码
^^^^^^^^^^^^

- 请求：

.. code:: sh

    curl -H "Content-Type: application/json" -X POST -d '{"jsonrpc":"2.0","method":"venachain_blockNumber","params":[],"id":1}' "http://127.0.0.1:6791"

- 响应：

.. code:: json

    {
        "jsonrpc":"2.0",
        "id":1,
        "result":"0x8"
    }

venachain_getBalance
=========================

返回指定地址账户的余额。

参数
^^^^^^

- ``data`` : 20字节，要检查余额的地址
- ``quantity | TAG`` : 区块的块高，或者字符串"latest", "earliest" 或 "pending"

.. code:: js

    params: [
        '0x407d73d8a49eeb85d32cf465507dd71d507100c1',
        'latest'
    ]

返回值
^^^^^^^

- ``quantity`` : 当前余额，单位：wei

示例代码
^^^^^^^^^^

- 请求：

.. code:: sh

    curl -H "Content-Type: application/json" -X POST -d '{"jsonrpc":"2.0","method":"venachain_getBalance","params":["0x8fa786734bfcb8351be33a4168dd5530ddd7578f", "0x8"],"id":1}' "http://127.0.0.1:6791"

- 响应：

.. code:: json

    {
        "jsonrpc":"2.0",
        "id":1,
        "result":"0x0"
    }

venachain_getAccountBaseInfo
================================

返回给定块高的区块中给定地址的帐户信息。

参数
^^^^^^

- ``data`` : 20字节，账户地址
- ``quantity | TAG`` : 区块的块高，或者字符串"latest", "earliest" 或 "pending"

返回值
^^^^^^^^

- ``object`` : 账户信息
    + Address ``data`` : 账户地址
    + Creator ``data`` : 创建该账户的账户地址
    + IsContract ``bool`` : 是否是合约账户
    + Nonce ``int`` : 随机数
    + Balance ``int`` : 账户余额

请求示例
^^^^^^^^^

- 请求：

.. code:: sh

    curl -H "Content-Type: application/json" -X POST -d '{"jsonrpc":"2.0","method":"venachain_getAccountBaseInfo","params":["0x8fa786734bfcb8351be33a4168dd5530ddd7578f", "0x8"],"id":1}' "http://127.0.0.1:6791"

- 响应：

.. code:: json

    {
        "jsonrpc":"2.0",
        "id":1,
        "result":{
            "Address":"0x8fa786734bfcb8351be33a4168dd5530ddd7578f",
            "Creator":"0x0000000000000000000000000000000000000000",
            "IsContract":false,
            "Nonce":0,
            "Balance":0
        }
    }

venachain_getBlockByNumber
==============================

返回指定块高的区块。

参数
^^^^^^^

- ``quantity | TAG`` : 区块的块高，或字符串"earliest"、"latest" 或"pending"
- ``bool`` : 为true时返回完整的交易对象，否则仅返回交易哈希

.. code:: js

    params: [
        '0x3', // 3
        true
    ]

返回值
^^^^^^^^^

请参考 :ref:`venachain_getBlockByHash <rpc-venachain-getBlockByHash>` 的返回值。

示例代码
^^^^^^^^^^

- 请求：

.. code:: sh

    curl -H "Content-Type: application/json" -X POST -d '{"jsonrpc":"2.0","method":"venachain_getBlockByNumber","params":["0x8", true],"id":1}' "http://127.0.0.1:6791"

- 响应：

.. code:: json

    {
        "jsonrpc":"2.0",
        "id":1,
        "result":{
            "extraData":"0xdd830100018976656e61636861696e88676f312e31362e37856c696e75780000f8f6ea94401a9d9c1ae6ea3cc732b5883b3baaf409eb7347947f8ddf8e88eb3b71b27112d29770a230c0ab29cab841bc9eef32e21929a2bbbfbf14315b150c48e359e94fc8ae4a072cd89528b8dfec7aee21d07f09c441dd576f17c9eb7ee5c501f069d3abecbc0db510642b02015c01f886b8411c8d87bdb08019d0b4785cab9f651d64992f40d57a0809203d87fb3bb94c13b737bb7089929a8ef2a61a7333a233b5e2ebfdfff0333c69b8df9500b8dd0b3a0800b84148cf2a36b636ee5cd70ee79937d81bd92f9932d166bbc4433dfa005c2e4993380c8bed3d2e37c1914d4549bec09303bd432d9cc2247487f3120830d49739442701",
            "gasLimit":"0x2540be400",
            "gasUsed":"0x320f8",
            "hash":"0x9bc44f307452f977546449b83d9f42dbf12ebff8145b55e603764757bb89fbe8",
            "logsBloom":"0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000001080000000000000000000000000000000000000000000000000000000040000000000000000000000000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
            "miner":"0x401a9d9c1ae6ea3cc732b5883b3baaf409eb7347",
            "mixHash":"0x63746963616c2062797a616e74696e65206661756c7420746f6c6572616e6365",
            "nonce":"0x02120b4d7acd3f42df3431ed8c986e3c8f66f5790cd09b38df72055a4880df148ceba8e5a7c11a1685dbe2f38a668b1af2a0bb77402f71b20eda19764329189ebf4f6bd85ea47195667498a9c2adb51a32",
            "number":"0x8",
            "parentHash":"0x652e307060534f9fb7a6cc45aea16db615bc70dd9e31384ede62dd2d62279a0c",
            "receiptsRoot":"0xed91ca502ca2c1d55c2eff56b886d8cbbc7c697df6191fc1a23983d6038f13d4",
            "size":"0x44f",
            "stateRoot":"0xf31e4ef4065e6eb90a566fc56ff8b6faccbc72e5d5168c10865e59990440bc09",
            "timestamp":"0x17f8782d4fd",
            "transactions":[
                {
                    "blockHash":"0x9bc44f307452f977546449b83d9f42dbf12ebff8145b55e603764757bb89fbe8",
                    "blockNumber":"0x8",
                    "from":"0x663d20db8dc9ed89e077b6dcbf10994d22aa3985",
                    "gas":"0x0",
                    "gasPrice":"0x0",
                    "hash":"0xa4e3fda24d0dee3bad752a684558f8119ba95253469e24819c8aee064be8f4fd",
                    "input":"0xdc88000000000000000286757064617465328a7b2274797065223a317d",
                    "nonce":"0x168ba35e21a9f",
                    "to":"0x1000000000000000000000000000000000000002",
                    "transactionIndex":"0x0",
                    "value":"0x0",
                    "v":"0x27c",
                    "r":"0x860c8238b1736e0cab0119dfe034738689a7d1723933a60a8524f6bba8879883",
                    "s":"0x57aed64e7111ad0b342f057fbac911794078e1a6e745fbd3e307599d4570ae1b"
                },
                {
                    "blockHash":"0x9bc44f307452f977546449b83d9f42dbf12ebff8145b55e603764757bb89fbe8",
                    "blockNumber":"0x8",
                    "from":"0x663d20db8dc9ed89e077b6dcbf10994d22aa3985",
                    "gas":"0x0",
                    "gasPrice":"0x0",
                    "hash":"0xb6ddddf35ec148ff680e4db36ee3938e3e5e43969ed966b93f60938feb287f1b",
                    "input":"0xdc88000000000000000286757064617465338a7b2274797065223a317d",
                    "nonce":"0x347404f2c4e5",
                    "to":"0x1000000000000000000000000000000000000002",
                    "transactionIndex":"0x1",
                    "value":"0x0",
                    "v":"0x27b",
                    "r":"0x32a4abddd01acfea641f1ea2626dd84138afd38be960f29dda383de79e2d5ff3",
                    "s":"0x713bca045d5c029038584d652047b5c6df0d633a6f183cbe4fe511c45c0e0cbd"
                }
            ],
            "transactionsRoot":"0x4e802017a8dc41bd0d3b23c81207708c9a06eaf26bcb4975bada08f50db09087"
        }
    }

.. _rpc-venachain-getBlockByHash:

venachain_getBlockByHash
=========================

返回具有指定哈希的块。

参数
^^^^^^^

- ``data`` : 32字节，区块哈希
- ``bool`` : 为true时返回完整的交易对象，否则仅返回交易哈希

.. code:: js

    params: [
        '0x9bc44f307452f977546449b83d9f42dbf12ebff8145b55e603764757bb89fbe8',
        true
    ]

返回值
^^^^^^^^^^

- ``object`` : 匹配的块对象，如果未找到块则返回null，结构如下：
    + extraData ``data`` : 区块额外数据
    + gasLimit ``quantity`` : 本区块允许的最大gas用量
    + gasUsed ``quantity`` : 本区块中所有交易使用的总gas用量
    + hash ``data`` : 区块哈希，挂起块为null
    + logsBloom ``data`` : 区块日志的bloom过滤器哈希，挂起块为null
    + miner ``data`` : 挖矿奖励的接收账户
    + mixHash ``data`` : 混合哈希，与nonce一起用于工作量证明，挂起块为null
    + nonce ``quantity`` : 随机数，与mixHash一起用于工作量证明，挂起块为null
    + number ``quantity`` : 区块的块高，挂起块为null
    + parentHash ``data`` : 父区块的哈希
    + receiptsRoot ``data`` : 区块中交易收据树的根节点哈希
    + size ``quantity`` : 本区块字节数
    + stateRoot ``data`` : 区块中状态树的根节点哈希
    + timestamp ``quantity`` : 区块时间戳
    + transactions ``object array`` : 交易对象数组，或32字节长的交易哈希数组
    + transactionsRoot ``data`` : 区块中的交易树根节点哈希

示例代码
^^^^^^^^^^

- 请求：

.. code:: sh

    curl -H "Content-Type: application/json" -X POST -d '{"jsonrpc":"2.0","method":"venachain_getBlockByHash","params":["0x9bc44f307452f977546449b83d9f42dbf12ebff8145b55e603764757bb89fbe8", true],"id":1}' "http://127.0.0.1:6791"

- 响应：

.. code:: json

    {
        "jsonrpc":"2.0",
        "id":1,
        "result":{
            "extraData":"0xdd830100018976656e61636861696e88676f312e31362e37856c696e75780000f8f6ea94401a9d9c1ae6ea3cc732b5883b3baaf409eb7347947f8ddf8e88eb3b71b27112d29770a230c0ab29cab841bc9eef32e21929a2bbbfbf14315b150c48e359e94fc8ae4a072cd89528b8dfec7aee21d07f09c441dd576f17c9eb7ee5c501f069d3abecbc0db510642b02015c01f886b8411c8d87bdb08019d0b4785cab9f651d64992f40d57a0809203d87fb3bb94c13b737bb7089929a8ef2a61a7333a233b5e2ebfdfff0333c69b8df9500b8dd0b3a0800b84148cf2a36b636ee5cd70ee79937d81bd92f9932d166bbc4433dfa005c2e4993380c8bed3d2e37c1914d4549bec09303bd432d9cc2247487f3120830d49739442701",
            "gasLimit":"0x2540be400",
            "gasUsed":"0x320f8",
            "hash":"0x9bc44f307452f977546449b83d9f42dbf12ebff8145b55e603764757bb89fbe8",
            "logsBloom":"0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000001080000000000000000000000000000000000000000000000000000000040000000000000000000000000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
            "miner":"0x401a9d9c1ae6ea3cc732b5883b3baaf409eb7347",
            "mixHash":"0x63746963616c2062797a616e74696e65206661756c7420746f6c6572616e6365",
            "nonce":"0x02120b4d7acd3f42df3431ed8c986e3c8f66f5790cd09b38df72055a4880df148ceba8e5a7c11a1685dbe2f38a668b1af2a0bb77402f71b20eda19764329189ebf4f6bd85ea47195667498a9c2adb51a32",
            "number":"0x8",
            "parentHash":"0x652e307060534f9fb7a6cc45aea16db615bc70dd9e31384ede62dd2d62279a0c",
            "receiptsRoot":"0xed91ca502ca2c1d55c2eff56b886d8cbbc7c697df6191fc1a23983d6038f13d4",
            "size":"0x44f",
            "stateRoot":"0xf31e4ef4065e6eb90a566fc56ff8b6faccbc72e5d5168c10865e59990440bc09",
            "timestamp":"0x17f8782d4fd",
            "transactions":[
                {
                    "blockHash":"0x9bc44f307452f977546449b83d9f42dbf12ebff8145b55e603764757bb89fbe8",
                    "blockNumber":"0x8",
                    "from":"0x663d20db8dc9ed89e077b6dcbf10994d22aa3985",
                    "gas":"0x0",
                    "gasPrice":"0x0",
                    "hash":"0xa4e3fda24d0dee3bad752a684558f8119ba95253469e24819c8aee064be8f4fd",
                    "input":"0xdc88000000000000000286757064617465328a7b2274797065223a317d",
                    "nonce":"0x168ba35e21a9f",
                    "to":"0x1000000000000000000000000000000000000002",
                    "transactionIndex":"0x0",
                    "value":"0x0",
                    "v":"0x27c",
                    "r":"0x860c8238b1736e0cab0119dfe034738689a7d1723933a60a8524f6bba8879883",
                    "s":"0x57aed64e7111ad0b342f057fbac911794078e1a6e745fbd3e307599d4570ae1b"
                },
                {
                    "blockHash":"0x9bc44f307452f977546449b83d9f42dbf12ebff8145b55e603764757bb89fbe8",
                    "blockNumber":"0x8",
                    "from":"0x663d20db8dc9ed89e077b6dcbf10994d22aa3985",
                    "gas":"0x0",
                    "gasPrice":"0x0",
                    "hash":"0xb6ddddf35ec148ff680e4db36ee3938e3e5e43969ed966b93f60938feb287f1b",
                    "input":"0xdc88000000000000000286757064617465338a7b2274797065223a317d",
                    "nonce":"0x347404f2c4e5",
                    "to":"0x1000000000000000000000000000000000000002",
                    "transactionIndex":"0x1",
                    "value":"0x0",
                    "v":"0x27b",
                    "r":"0x32a4abddd01acfea641f1ea2626dd84138afd38be960f29dda383de79e2d5ff3",
                    "s":"0x713bca045d5c029038584d652047b5c6df0d633a6f183cbe4fe511c45c0e0cbd"
                }
            ],
            "transactionsRoot":"0x4e802017a8dc41bd0d3b23c81207708c9a06eaf26bcb4975bada08f50db09087"
        }
    }

venachain_getCode
==================

返回给定块高的区块中存储在给定地址的合约代码。

参数
^^^^^^^

- ``data`` : 20字节，合约地址
- ``quantity | TAG`` : 区块的块高，或字符串"latest"、"earliest" 或"pending"

.. code:: js

    params: [
        '0xa94f5374fce5edbc8e2a8697c15331677e6ebf0b',
        'latest'
    ]

返回值
^^^^^^^^^

- ``data`` : 指定地址处的合约代码

示例代码
^^^^^^^^^

- 请求：

.. code:: sh

    curl -H "Content-Type: application/json" -X POST -d '{"jsonrpc":"2.0","method":"venachain_getCode","params":["0xa94f5374fce5edbc8e2a8697c15331677e6ebf0b","0xb"],"id":1}' "http://127.0.0.1:6791"

- 响应：

.. code:: json

    {
        "jsonrpc":"2.0",
        "id":1,
        "result":"0x开头的合约代码数据"
    }

venachain_getStorageAt
==========================

返回指定块高的区块中指定地址存储的key对应的value。

参数
^^^^^^

- ``data`` : 20字节，存储地址
- ``string`` : key
- ``quantity | TAG`` : 区块的块高，或字符串"latest"、"earliest" 或"pending"

返回值
^^^^^^^^^

- ``data`` : 指定区块中指定地址所存储的指定key对应的值

示例代码
^^^^^^^^^

根据要提取的存储计算正确的位置。考虑下面的合约，由 ``0x391694e7e0b0cce554cb130d723a9d27458f9298`` 部署在地址 ``0x295a70b2de5e3953354a6a8344e616ed314d7251`` ：

.. code:: go

    contract Storage {
        uint pos0;
        mapping(address => uint) pos1;

        function Storage() {
            pos0 = 1234;
            pos1[msg.sender] = 5678;
        }
    }

提取pos0的值很直接：

.. code:: sh

    curl -H "Content-Type: application/json" -X POST -d '{"jsonrpc":"2.0","method":"venachain_getStorageAt","params":["0x295a70b2de5e3953354a6a8344e616ed314d7251","0x0", "latest"],"id":1}' "http://127.0.0.1:6791"

响应结果：

.. code:: json

    {
        "jsonrpc":"2.0",
        "id":1,
        "result":"0x00000000000000000000000000000000000000000000000000000000000004d2"
    }

要提取映射表中的成员就难一些了。映射表中成员位置的计算如下：

.. code:: sh

    keccack(LeftPad32(key, 0), LeftPad32(map position, 0))

这意味着为了提取 ``pos1["0x391694e7e0b0cce554cb130d723a9d27458f9298"]`` 的值，我们需要如下计算：

.. code:: sh

    keccak(decodeHex("000000000000000000000000391694e7e0b0cce554cb130d723a9d27458f9298" + "0000000000000000000000000000000000000000000000000000000000000001"))

venachain console控制台自带的web3库可以用来进行这个计算：

.. code:: sh

    > var key = "000000000000000000000000391694e7e0b0cce554cb130d723a9d27458f9298" + "0000000000000000000000000000000000000000000000000000000000000001"
    undefined
    > web3.sha3(key, {"encoding": "hex"})
    "0x6661e9d6d8b923d5bbaab1b96e1dd51ff6ea2a93520fdc9eb75d059238b8c5e9"

现在可以提取指定位置的值了：

.. code:: sh

    curl -H "Content-Type: application/json" -X POST -d '{"jsonrpc":"2.0","method":"venachain_getStorageAt","params":["0x295a70b2de5e3953354a6a8344e616ed314d7251","0x6661e9d6d8b923d5bbaab1b96e1dd51ff6ea2a93520fdc9eb75d059238b8c5e9", "latest"],"id":1}' "http://127.0.0.1:6791"

相应结果如下：

.. code:: json

    {
        "jsonrpc":"2.0",
        "id":1,
        "result":"0x000000000000000000000000000000000000000000000000000000000000162e"
    }

.. _rpc-venachain-call:

venachain_call
=================

立刻执行一个新的消息调用，无需在区块链上创建交易。

参数
^^^^^^^

- ``object`` : 交易调用对象
    + from ``data`` : 20 Bytes，发送交易的原地址，可选
    + to ``data`` : 20 Bytes，交易目标地址
    + gas ``quantity`` : 交易可用gas量，可选。venachain_call不消耗gas，但是某些执行环节需要这个参数
    + gasPrice ``quantity`` : gas价格，可选
    + value ``quantity`` : 交易发送的以太数量，可选
    + data ``data`` : 方法签名和编码参数的哈希，可选
- ``quantity`` : 区块的块高，或字符串"latest"、"earliest"或"pending"

返回值
^^^^^^^^^

- ``data`` : 所执行合约的返回值

示例代码
^^^^^^^^^

- 请求：

.. code:: sh

    curl -H "Content-Type: application/json" -X POST -d '{"jsonrpc":"2.0","method":"venachain_call","params":[{"from":"0x054c138b7ed3083ee9a36e1b09542fccdc155d75","to":"0x0000000000000000000000000000000000000011","gas":"0x106000","gasPrice":"0x0","value":"0x1","data":""},"0x8"],"id":1}' "http://127.0.0.1:6791"

- 响应：

.. code:: json

    {
        "jsonrpc":"2.0",
        "id":1,
        "result":"0x00000003"
    }

venachain_estimateGas
======================

执行并估算一个交易需要的gas用量。该次交易不会写入区块链。

.. note:: 由于多种原因，例如EVM的机制及节点的性能，估算的数值可能比实际用量大的多。

参数
^^^^^

- ``object`` : 交易调用对象
    + from ``data`` : 20 Bytes，发送交易的原地址，可选
    + to ``data`` : 20 Bytes，交易目标地址，可选
    + gas ``quantity`` : 交易可用gas量，可选。venachain_call不消耗gas，但是某些执行环节需要这个参数
    + gasPrice ``quantity`` : gas价格，可选
    + value ``quantity`` : 交易发送的以太数量，可选
    + data ``data`` : 方法签名和编码参数的哈希，可选

所有的属性都是可选的。如果没有指定gas用量上限，venachain将使用挂起块的gas上限。
在这种情况下，返回的gas估算量可能不足以执行实际的交易。

返回值
^^^^^^^

- ``quantity`` : gas用量估算值

示例代码
^^^^^^^^^^

- 请求：

.. code:: sh

    curl -H "Content-Type: application/json" -X POST -d '{"jsonrpc":"2.0","method":"venachain_estimateGas","params":[{"gas":"0x106000","gasPrice":"0x0","value":"0x1","data":""}],"id":1}' "http://127.0.0.1:6791"

- 响应：

.. code:: js

    {
      "id":1,
      "jsonrpc": "2.0",
      "result": "0x5208" // 21000
    }

venachain_getBlockTransactionCountByNumber
==================================================

返回指定块高的区块内的交易数量。

参数
^^^^^^

- ``quantity | TAG`` : 区块的块高，或字符串"earliest"、"latest"或"pending"

.. code:: js

    params: [
        '0x1', // 1
    ]

返回值
^^^^^^^^

- ``quantity`` : 指定块内的交易数量

示例代码
^^^^^^^^^^^

- 请求：

.. code:: sh

    curl -H "Content-Type: application/json" -X POST -d '{"jsonrpc":"2.0","method":"venachain_getBlockTransactionCountByNumber","params":["0x1"],"id":1}' "http://127.0.0.1:6791"

- 响应：

.. code:: json

    {
        "jsonrpc":"2.0",
        "id":1,
        "result":"0x3"
    }

venachain_getBlockTransactionCountByHash
===============================================

返回指定块内的交易数量，使用哈希来指定块。

参数
^^^^^^

- ``data`` : 32字节，区块哈希

.. code:: js

    params: [
        '0x8188ed673fec3050184d2afe7b2c562f920ac97145ebc566ade868e6a450ae91'
    ]

返回值
^^^^^^^

- ``quantity`` : 指定块内的交易数量

示例代码
^^^^^^^^^^^

- 请求：

.. code:: sh

    curl -H "Content-Type: application/json" -X POST -d '{"jsonrpc":"2.0","method":"venachain_getBlockTransactionCountByHash","params":["0x8188ed673fec3050184d2afe7b2c562f920ac97145ebc566ade868e6a450ae91"],"id":1}' "http://127.0.0.1:6791"

- 响应：

.. code:: json

    {
        "jsonrpc":"2.0",
        "id":1,
        "result":"0x3"
    }

venachain_getTransactionByBlockNumberAndIndex
====================================================

返回指定块高的区块内具有指定索引序号的交易。

参数
^^^^^^

- ``quantity | TAG`` : 区块的块高，或字符串"earliest"、"latest"或"pending"
- ``quantity`` : 交易索引序号

.. code:: js

    params: [
        '0x1',  // 1
        '0x2'   // 2
    ]

返回值
^^^^^^^^

请参考 :ref:`venachain_getTransactionByHash <rpc-venachain-getTransactionByHash>` 的返回值。

示例代码
^^^^^^^^^^^

-  请求：

.. code:: sh

    curl -H "Content-Type: application/json" -X POST -d '{"jsonrpc":"2.0","method":"venachain_getTransactionByBlockNumberAndIndex","params":["0x1", "0x2"],"id":1}' "http://127.0.0.1:6791"

-  响应

.. code:: json

    {
        "jsonrpc":"2.0",
        "id":1,
        "result":{
            "blockHash":"0x8188ed673fec3050184d2afe7b2c562f920ac97145ebc566ade868e6a450ae91",
            "blockNumber":"0x1",
            "from":"0x663d20db8dc9ed89e077b6dcbf10994d22aa3985",
            "gas":"0x0",
            "gasPrice":"0x0",
            "hash":"0xff59c370ffbd0cdccaa22969f8df57995f69e671208cc3c7e42e678d827d0072",
            "input":"0xf9014f88000000000000000283616464b9013f7b226e616d65223a2230222c2274797065223a322c227075626c69634b6579223a223530316236333639313633373765393335363966326637643736626462653166373463643938373663356465633631643565313538336265666464613236396162366264613638313634303234343432393533346437653939373136383333383830653235613133356135346131343531643435653565653361383664346535222c2264657363223a22222c2265787465726e616c4950223a223132372e302e302e31222c22696e7465726e616c4950223a223132372e302e302e31222c22727063506f7274223a363739312c22703270506f7274223a31363739312c226f776e6572223a22307836363364323064623864633965643839653037376236646362663130393934643232616133393835222c22737461747573223a317d",
            "nonce":"0x52e32b81ec21",
            "to":"0x1000000000000000000000000000000000000002",
            "transactionIndex":"0x2",
            "value":"0x0",
            "v":"0x27c",
            "r":"0xf4f0b8bdd3321e27dd11e2ad3046252f78941a38c34825f70d20e845e36bf4c8",
            "s":"0x3f4d5a4d0a8ef9946165106b4bb7f17c73d00d04917cfea9e82ba225860b15a8"
        }
    }

venachain_getTransactionByBlockHashAndIndex
==================================================

返回指定块内具有指定索引序号的交易。

参数
^^^^^

- ``data`` : 32字节，块哈希
- ``quantity`` : 交易在块内的索引序号

.. code:: js

    params: [
        '0x8188ed673fec3050184d2afe7b2c562f920ac97145ebc566ade868e6a450ae91',
        '0x2' // 0
    ]

返回值
^^^^^^^

查阅 :ref:`venachain_getTransactionByHash <rpc-venachain-getTransactionByHash>` 的返回值

示例代码
^^^^^^^^^^^^

- 请求：

.. code:: sh

    curl -H "Content-Type: application/json" -X POST -d '{"jsonrpc":"2.0","method":"venachain_getTransactionByBlockHashAndIndex","params":["0x8188ed673fec3050184d2afe7b2c562f920ac97145ebc566ade868e6a450ae91", "0x2"],"id":1}' "http://127.0.0.1:6791"

- 响应

.. code:: json

    {
        "jsonrpc":"2.0",
        "id":1,
        "result":{
            "blockHash":"0x8188ed673fec3050184d2afe7b2c562f920ac97145ebc566ade868e6a450ae91",
            "blockNumber":"0x1",
            "from":"0x663d20db8dc9ed89e077b6dcbf10994d22aa3985",
            "gas":"0x0",
            "gasPrice":"0x0",
            "hash":"0xff59c370ffbd0cdccaa22969f8df57995f69e671208cc3c7e42e678d827d0072",
            "input":"0xf9014f88000000000000000283616464b9013f7b226e616d65223a2230222c2274797065223a322c227075626c69634b6579223a223530316236333639313633373765393335363966326637643736626462653166373463643938373663356465633631643565313538336265666464613236396162366264613638313634303234343432393533346437653939373136383333383830653235613133356135346131343531643435653565653361383664346535222c2264657363223a22222c2265787465726e616c4950223a223132372e302e302e31222c22696e7465726e616c4950223a223132372e302e302e31222c22727063506f7274223a363739312c22703270506f7274223a31363739312c226f776e6572223a22307836363364323064623864633965643839653037376236646362663130393934643232616133393835222c22737461747573223a317d",
            "nonce":"0x52e32b81ec21",
            "to":"0x1000000000000000000000000000000000000002",
            "transactionIndex":"0x2",
            "value":"0x0",
            "v":"0x27c",
            "r":"0xf4f0b8bdd3321e27dd11e2ad3046252f78941a38c34825f70d20e845e36bf4c8",
            "s":"0x3f4d5a4d0a8ef9946165106b4bb7f17c73d00d04917cfea9e82ba225860b15a8"
        }
    }

venachain_getRawTransactionByBlockNumberAndIndex
======================================================

返回指块高的区块内具有指定索引序号的的交易。

参数
^^^^^^^^

- ``quantity | TAG`` : 区块的块高，或字符串"earliest"、"latest"或"pending"
- ``quantity`` : 交易索引序号

.. code:: js

    params: [
        '0x1',  // 1
        '0x2'   // 2
    ]

返回值
^^^^^^^

- ``data`` : 交易数据

示例代码
^^^^^^^^^^

- 请求：

.. code:: sh

    curl -H "Content-Type: application/json" -X POST -d '{"jsonrpc":"2.0","method":"venachain_getRawTransactionByBlockNumberAndIndex","params":["0x1", "0x2"],"id":1}' "http://127.0.0.1:6791"

- 响应：

.. code:: json

    {
        "jsonrpc":"2.0",
        "id":1,
        "result":"0xf901b98652e32b81ec21808094100000000000000000000000000000000000000280b90152f9014f88000000000000000283616464b9013f7b226e616d65223a2230222c2274797065223a322c227075626c69634b6579223a223530316236333639313633373765393335363966326637643736626462653166373463643938373663356465633631643565313538336265666464613236396162366264613638313634303234343432393533346437653939373136383333383830653235613133356135346131343531643435653565653361383664346535222c2264657363223a22222c2265787465726e616c4950223a223132372e302e302e31222c22696e7465726e616c4950223a223132372e302e302e31222c22727063506f7274223a363739312c22703270506f7274223a31363739312c226f776e6572223a22307836363364323064623864633965643839653037376236646362663130393934643232616133393835222c22737461747573223a317d82027ca0f4f0b8bdd3321e27dd11e2ad3046252f78941a38c34825f70d20e845e36bf4c8a03f4d5a4d0a8ef9946165106b4bb7f17c73d00d04917cfea9e82ba225860b15a8"
    }

venachain_getRawTransactionByBlockHashAndIndex
=======================================================

返回指定块内具有指定索引序号的交易。

参数
^^^^^^^

- ``data`` : 32字节，区块哈希
- ``quantity`` : 交易在块内的索引序号

.. code:: js

    params: [
        '0x8188ed673fec3050184d2afe7b2c562f920ac97145ebc566ade868e6a450ae91',
        '0x2' // 0
    ]

返回值
^^^^^^^^^

- ``data`` : 交易数据

示例代码
^^^^^^^^^^

- 请求：

.. code:: sh

    curl -H "Content-Type: application/json" -X POST -d '{"jsonrpc":"2.0","method":"venachain_getRawTransactionByBlockHashAndIndex","params":["0x8188ed673fec3050184d2afe7b2c562f920ac97145ebc566ade868e6a450ae91", "0x2"],"id":1}' "http://127.0.0.1:6791"

- 响应：

.. code:: json

    {
        "jsonrpc":"2.0",
        "id":1,
        "result":"0xf901b98652e32b81ec21808094100000000000000000000000000000000000000280b90152f9014f88000000000000000283616464b9013f7b226e616d65223a2230222c2274797065223a322c227075626c69634b6579223a223530316236333639313633373765393335363966326637643736626462653166373463643938373663356465633631643565313538336265666464613236396162366264613638313634303234343432393533346437653939373136383333383830653235613133356135346131343531643435653565653361383664346535222c2264657363223a22222c2265787465726e616c4950223a223132372e302e302e31222c22696e7465726e616c4950223a223132372e302e302e31222c22727063506f7274223a363739312c22703270506f7274223a31363739312c226f776e6572223a22307836363364323064623864633965643839653037376236646362663130393934643232616133393835222c22737461747573223a317d82027ca0f4f0b8bdd3321e27dd11e2ad3046252f78941a38c34825f70d20e845e36bf4c8a03f4d5a4d0a8ef9946165106b4bb7f17c73d00d04917cfea9e82ba225860b15a8"
    }

venachain_getTransactionCount
================================

返回指定地址发生的交易数量。

参数
^^^^^^

- ``data`` : 20字节，账户地址
- ``quantity | TAG`` : 区块的块高，或字符串"earliest"、"latest"或"pending"

.. code:: js

    params: [
        '8fa786734bfcb8351be33a4168dd5530ddd7578f',
        '0x2' // 2
    ]

返回值
^^^^^^^^

- ``quantity`` : 从指定地址发出的交易数量

示例代码
^^^^^^^^^^

- 请求：

.. code:: sh

    curl -H "Content-Type: application/json" -X POST -d '{"jsonrpc":"2.0","method":"venachain_getTransactionCount","params":["0xf92ce14220d5c938342651a43c14a311ae37f43b", "0x3"],"id":1}' "http://127.0.0.1:6791"

- 响应：

.. code:: json

    {
        "jsonrpc":"2.0",
        "id":1,
        "result":"0x4"
    }

.. _rpc-venachain-getTransactionByHash:

venachain_getTransactionByHash
==================================

返回指定哈希对应的交易。

参数
^^^^^^^

- ``data`` : 32 字节，交易哈希

.. code:: js

    params: [
        "0x2955c837a4aafe976f9fe37904c62b23fa8eff08d592bae598a1fd930036ec92"
    ]

返回值
^^^^^^^

- ``object`` : 交易对象，如果没有找到匹配的交易则返回null。结构如下：
    + blockHash ``data`` : 32字节，交易所在块的哈希，对于挂起块，该值为null
    + blockNumber ``quantity`` : 交易所在块的编号，对于挂起块，该值为null
    + from ``data`` : 20字节，交易发送方地址
    + gas ``quantity`` : 发送方提供的gas可用量
    + gasPrice ``quantity`` : 发送方提供的gas价格，单位:wei
    + hash ``data`` : 32字节，交易哈希
    + input ``data`` : 交易数据
    + nonce ``quantity`` : 本次交易之前发送方已经生成的交易数量
    + to ``data`` : 20字节，交易接收方地址，对于合约创建交易，该值为null
    + transactionIndex ``quantity`` : 交易在块中的索引位置，挂起块该值为null
    + value ``quantity`` : 发送的以太数量，单位:wei
    + v、r、s ``quantity`` : 签名值

示例代码
^^^^^^^^

- 请求：

.. code:: sh

    curl -H "Content-Type: application/json" -X POST -d '{"jsonrpc":"2.0","method":"venachain_getTransactionByHash","params":["0x2955c837a4aafe976f9fe37904c62b23fa8eff08d592bae598a1fd930036ec92"],"id":1}' "http://127.0.0.1:6791"

- 响应：

.. code:: json

    {
        "jsonrpc":"2.0",
        "id":1,
        "result":{
            "blockHash":"0x413e3658ce2b0daf6df7ccc0da7587f470d0a47f89ca2e6eaadc5400833897c5",
            "blockNumber":"0x5",
            "from":"0xf92ce14220d5c938342651a43c14a311ae37f43b",
            "gas":"0x0",
            "gasPrice":"0x0",
            "hash":"0x2955c837a4aafe976f9fe37904c62b23fa8eff08d592bae598a1fd930036ec92",
            "input":"0xf9012788000000000000000283616464b901177b226e616d65223a2232222c2274797065223a322c227075626c69634b6579223a226634326339386464376233623365633133646237613234623438386333616662656262303762633361396136316530666337333736643233616634613231313963316561323037363635656164353537353662323132656634333866623764333039356431636261303963313133616564383666343461373533373665323132222c2264657363223a22222c2265787465726e616c4950223a223132372e302e302e31222c22696e7465726e616c4950223a223132372e302e302e31222c22727063506f7274223a363739332c22703270506f7274223a31363739332c226f776e6572223a223078222c22737461747573223a317d",
            "nonce":"0xd44aaf185f05",
            "to":"0x1000000000000000000000000000000000000002",
            "transactionIndex":"0x0",
            "value":"0x0",
            "v":"0x27c",
            "r":"0x59bb7e913f7d6e8518acb57da3d794c6caf7feb722336281b655c9da4eecd165",
            "s":"0x2cd654b08d84cd0da9e53e50e2cd06f5716b9f9a23e7b2c2e9e15c6274b7def1"
        }
    }

venachain_getRawTransactionByHash
====================================

返回给定哈希对应的交易。

参数
^^^^^^

- ``data`` : 32 字节，交易哈希

返回值
^^^^^^^^

- ``data`` : 交易信息

示例代码
^^^^^^^^

- 请求：

.. code:: sh

    curl -H "Content-Type: application/json" -X POST -d '{"jsonrpc":"2.0","method":"venachain_getRawTransactionByHash","params":["0x2955c837a4aafe976f9fe37904c62b23fa8eff08d592bae598a1fd930036ec92"],"id":1}' "http://127.0.0.1:6791"

- 响应：

.. code:: json

    {
        "jsonrpc":"2.0",
        "id":1,
        "result":"0xf9019186d44aaf185f05808094100000000000000000000000000000000000000280b9012af9012788000000000000000283616464b901177b226e616d65223a2232222c2274797065223a322c227075626c69634b6579223a226634326339386464376233623365633133646237613234623438386333616662656262303762633361396136316530666337333736643233616634613231313963316561323037363635656164353537353662323132656634333866623764333039356431636261303963313133616564383666343461373533373665323132222c2264657363223a22222c2265787465726e616c4950223a223132372e302e302e31222c22696e7465726e616c4950223a223132372e302e302e31222c22727063506f7274223a363739332c22703270506f7274223a31363739332c226f776e6572223a223078222c22737461747573223a317d82027ca059bb7e913f7d6e8518acb57da3d794c6caf7feb722336281b655c9da4eecd165a02cd654b08d84cd0da9e53e50e2cd06f5716b9f9a23e7b2c2e9e15c6274b7def1"
    }

.. _rpc-venachain-getTransactionReceipt:

venachain_getTransactionReceipt
===================================

返回指定交易的收据，使用哈希指定交易。需要指出的是，挂起的交易其收据无效。

参数
^^^^^

- ``data`` : 32字节，交易哈希

.. code:: js

    params: [
        '0x2955c837a4aafe976f9fe37904c62b23fa8eff08d592bae598a1fd930036ec92'
    ]

返回值
^^^^^^^^^

``map[string]object``

- ``object`` : 交易收据对象，如果收据不存在则为null。交易对象的结构如下：
    + blockHash ``data`` : 32字节，交易所在块的哈希
    + blockNumber ``quantity`` : 交易所在块的编号
    + contractAddress ``data`` : 20字节，对于合约创建交易，该值为新创建的合约地址，否则为null
    + cumulativeGasUsed ``quantity`` : 交易所在块消耗的gas总量
    + from ``data`` : 20字节，交易发送方地址
    + gasUsed ``quantity`` : 该次交易消耗的gas用量
    + logs ``object array`` : 本次交易生成的日志对象数组
    + logsBloom ``data`` : 256字节，bloom过滤器，轻客户端用来快速提取相关日志
    + to ``data`` : 20字节，交易接收方地址，对于合约创建交易该值为null
    + transactionHash ``data`` : 32字节，交易哈希
    + transactionIndex ``quantity`` : 交易在块内的索引序号

返回的结果对象中还包括下面二者之一 :

- root ``data`` : 32字节，后交易状态根(pre Byzantium)
- status ``quantity`` : 0x1 (成功) 或 0x0 (失败)

示例代码
^^^^^^^^^^

请求：

.. code:: sh

    curl -H "Content-Type: application/json" -X POST -d '{"jsonrpc":"2.0","method":"venachain_getTransactionReceipt","params":["0x2955c837a4aafe976f9fe37904c62b23fa8eff08d592bae598a1fd930036ec92"],"id":1}' "http://127.0.0.1:6791"

响应：

.. code:: json

    {
        "jsonrpc":"2.0",
        "id":1,
        "result":{
            "blockHash":"0x413e3658ce2b0daf6df7ccc0da7587f470d0a47f89ca2e6eaadc5400833897c5",
            "blockNumber":"0x5",
            "contractAddress":null,
            "cumulativeGasUsed":"0x1d7f0",
            "from":"0xf92ce14220d5c938342651a43c14a311ae37f43b",
            "gasUsed":"0x1d7f0",
            "logs":[
                {
                    "address":"0x1000000000000000000000000000000000000002",
                    "topics":[
                        "0x8cd284134f0437457b5542cb3a7da283d0c38208c497c5b4b005df47719f98a1"
                    ],
                    "data":"0xf9013280b9012e616464206e6f646520737563636573732e206e6f64653a7b226e616d65223a2232222c226f776e6572223a223078222c2264657363223a22222c2274797065223a322c22737461747573223a312c2265787465726e616c4950223a223132372e302e302e31222c22696e7465726e616c4950223a223132372e302e302e31222c227075626c69634b6579223a226634326339386464376233623365633133646237613234623438386333616662656262303762633361396136316530666337333736643233616634613231313963316561323037363635656164353537353662323132656634333866623764333039356431636261303963313133616564383666343461373533373665323132222c22727063506f7274223a363739332c22703270506f7274223a31363739337d",
                    "blockNumber":"0x5",
                    "transactionHash":"0x2955c837a4aafe976f9fe37904c62b23fa8eff08d592bae598a1fd930036ec92",
                    "transactionIndex":"0x0",
                    "blockHash":"0x413e3658ce2b0daf6df7ccc0da7587f470d0a47f89ca2e6eaadc5400833897c5",
                    "logIndex":"0x0",
                    "removed":false
                }
            ],
            "logsBloom":"0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000001080000000000000000000000000000000000000000000000000000000040000000000000000000000000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
            "status":"0x1",
            "to":"0x1000000000000000000000000000000000000002",
            "transactionHash":"0x2955c837a4aafe976f9fe37904c62b23fa8eff08d592bae598a1fd930036ec92",
            "transactionIndex":"0x0"
        }
    }

.. _rpc-venachain-sendTransaction:

venachain_sendTransaction
==============================

创建一个新的消息调用交易，如果数据字段中包含代码，则创建一个合约。

参数
^^^^^

- ``object`` : 交易对象，结果如下：
    + from ``data`` : 20字节，发送交易的源地址
    + to ``data`` : 20字节，交易的目标地址，当创建新合约时可选
    + gas ``quantity`` : 交易执行可用gas量，可选，默认值1500000000，未用gas将返还。
    + gasPrice ``quantity`` : gas价格，可选
    + value ``quantity`` : 交易发送的金额，可选
    + data ``data`` : 交易数据
    + nonce ``quantity`` : 随机数，可选，可以使用同一个nonce来实现挂起的交易的重写

.. code:: js

    params: [{
        "from": "0xf92ce14220d5c938342651a43c14a311ae37f43b",
        "to": "0xf92ce14220d5c938342651a43c14a311ae37f43b",
        "gas": "0x76c0", // 30400
        "gasPrice": "0x9184e72a000", // 10000000000000
        "value": "0x0", // 0
        "data": ""
    }]

返回值
^^^^^^^^^^

- ``data`` : 32字节，交易哈希，如果交易还未生效则返回0值哈希。

当创建合约时，在交易生效后，请使用 :ref:`venachain_getTransactionReceipt <rpc-venachain-getTransactionReceipt>` 调用获取合约地址。

示例代码
^^^^^^^^^^

- 请求：

.. code:: sh

    curl -H "Content-Type: application/json" -X POST -d '{"jsonrpc":"2.0","method":"venachain_sendTransaction","params": [{"from":"0xf92ce14220d5c938342651a43c14a311ae37f43b","to":"0xf92ce14220d5c938342651a43c14a311ae37f43b","gas":"0x76c0","gasPrice":"0x9184e72a000","value":"0x0","data":""}],"id":1}' "http://127.0.0.1:6791"

- 响应：

.. code:: json

    {
        "jsonrpc":"2.0",
        "id":1,
        "result":"0x71d530db63180f1013a2b04f32a40b41c591267837045b1ddf0a4716cc01f456"
    }
    
venachain_sendRawTransaction
=================================

为签名交易创建一个新的消息调用交易或合约。

参数
^^^^^^

- ``data`` : 签名的交易数据

.. code:: js

    params: ["0xf86c8516306402898609184e72a0008276c094f92ce14220d5c938342651a43c14a311ae37f43b808082027ba0a95b1e4570cfcba6d53789fa3db85e7ad2a746aa43cd29a62b7c0355f6886d29a061b0f7b5ab98ab078b86cd3e1a1f3ccc56f829983ae06bce27f1d38d9dabc5e4"]

返回值
^^^^^^^^^

- ``data`` : 32字节，交易哈希，如果交易未生效则返回全0哈希。

当创建合约时，在交易生效后，请使用 :ref:`venachain_getTransactionReceipt <rpc-venachain-getTransactionReceipt>` 获取合约地址。

示例代码
^^^^^^^^^

- 请求：

.. code:: sh

    curl -H "Content-Type: application/json" -X POST -d  '{"jsonrpc":"2.0","method":"venachain_sendRawTransaction","params":["0xf86c8516306402898609184e72a0008276c094f92ce14220d5c938342651a43c14a311ae37f43b808082027ba0a95b1e4570cfcba6d53789fa3db85e7ad2a746aa43cd29a62b7c0355f6886d29a061b0f7b5ab98ab078b86cd3e1a1f3ccc56f829983ae06bce27f1d38d9dabc5e4"],"id":1}' "http://127.0.0.1:6791"

- 响应：

.. code:: json

    {
        "jsonrpc":"2.0",
        "id":1,
        "result":"0x05fcf6108303f47a02a50fe0c170c670b25815a8d535feb7bb94590f18e4f6df"
    }

.. _rpc-venachain-sign:

venachain_sign
===================

使用如下公式计算 Venachain 签名 ``sign(keccak256("\x19Venachain Signed Message:\n" + len(message) + message)))`` 。

通过给消息添加一个前缀，可以让结果签名被识别为 Venachain 签名。这可以组织恶意DApp签名任意数据（例如交易）并使用签名冒充受害者。

.. note:: 进行签名的地址必须是解锁的。

参数
^^^^^^^

账户、消息

- ``data`` : 20字节，进行签名的地址
- ``data`` : 要签名的消息

返回值
^^^^^^^^^

- ``data`` : 签名后的数据

示例代码
^^^^^^^^^

- 请求：

.. code:: sh

    curl -H "Content-Type: application/json" -X POST -d '{"jsonrpc":"2.0","method":"venachain_sign","params":["0xf92ce14220d5c938342651a43c14a311ae37f43b", "0xdeadbeaf"],"id":1}' "http://127.0.0.1:6791"

- 响应：

.. code:: json

    {
        "jsonrpc":"2.0",
        "id":1,
        "result":"0xd27f10403d5fb20701208f98fe12d2959062b42e96734834a04379a5985f6967123d7f21067fe17b3b56f29f0a838c28b686afc8a1f5f299ebe7d5125fd819281c"
    }

venachain_signTransaction
================================

使用交易发起人的帐户对给定的交易进行签名。节点需要具有与给定交易发起人地址对应的帐户的私钥，并且需要对其进行解锁。

参数
^^^^^^^^

- ``object`` : 交易对象，详情请查看 :ref:`venachain_sendTransaction <rpc-venachain-sendTransaction>`

.. code:: js

    params: [{
        "from": "0xf92ce14220d5c938342651a43c14a311ae37f43b",
        "to": "0xf92ce14220d5c938342651a43c14a311ae37f43b",
        "gas": "0x76c0", // 30400
        "gasPrice": "0x9184e72a000", // 10000000000000
        "nonce":"0x1630640289",
        "value": "0x0", // 0
        "data": ""
    }]

返回值
^^^^^^^^

- ``object`` : 一个RLP编码的交易签名结果对象
    + raw ``string``
    + tx ``object`` : 交易信息对象
        - nonce ``quantity`` : 账户随机数
        - gasPrice ``quantity`` : gas价格
        - gas ``quantity`` : gas限制
        - to ``data`` : 交易目标地址
        - value ``quantity`` : 交易金额
        - input ``data`` : 交易信息
        - hash ``data`` : 交易哈希
        - v、r、s ``quantity`` : 签名值

示例代码
^^^^^^^^^^

- 请求：

.. code:: sh

    curl -H "Content-Type: application/json" -X POST -d '{"jsonrpc":"2.0","method":"venachain_signTransaction","params":[{"from":"0xf92ce14220d5c938342651a43c14a311ae37f43b", "to":"0xf92ce14220d5c938342651a43c14a311ae37f43b", "gas":"0x76c0", "nonce":"0x1630640289", "gasPrice":"0x9184e72a000", "value":"0x0", "data":""}],"id":1}' "http://127.0.0.1:6791"

- 响应：

.. code:: json

    {
        "jsonrpc":"2.0",
        "id":1,
        "result":{
            "raw":"0xf86c8516306402898609184e72a0008276c094f92ce14220d5c938342651a43c14a311ae37f43b808082027ba0a95b1e4570cfcba6d53789fa3db85e7ad2a746aa43cd29a62b7c0355f6886d29a061b0f7b5ab98ab078b86cd3e1a1f3ccc56f829983ae06bce27f1d38d9dabc5e4",
            "tx":{
                "nonce":"0x1630640289",
                "gasPrice":"0x9184e72a000",
                "gas":"0x76c0",
                "to":"0xf92ce14220d5c938342651a43c14a311ae37f43b",
                "value":"0x0",
                "input":"0x",
                "v":"0x27b",
                "r":"0xa95b1e4570cfcba6d53789fa3db85e7ad2a746aa43cd29a62b7c0355f6886d29",
                "s":"0x61b0f7b5ab98ab078b86cd3e1a1f3ccc56f829983ae06bce27f1d38d9dabc5e4",
                "hash":"0x05fcf6108303f47a02a50fe0c170c670b25815a8d535feb7bb94590f18e4f6df"
            }
        }
    }

venachain_pendingTransactions
====================================

返回交易池中处于 pending 状态的交易，其交易发起人地址是此节点管理的帐户之一。

参数
^^^^^^^

无

返回值
^^^^^^^^

- ``object array`` : 可序列化为 RPC 交易的交易对象
    + blockHash ``data`` : 区块哈希
    + blockNumber ``quantity`` : 区块高度
    + from ``data`` : 交易发起人地址
    + gas ``quantity`` : gas限制
    + gasPrice ``quantity`` : gas价格
    + hash ``data`` : 交易哈希
    + input ``data`` : 交易数据
    + nonce ``quantity`` : 随机数
    + to ``dara`` : 交易目标地址
    + transactionIndex ``quantity`` : 交易在区块中的下标
    + value ``quantity`` : 交易金额
    + v、r、s ``quantity`` : 签名值


示例代码
^^^^^^^^^^

- 请求：

.. code:: sh

    curl -H "Content-Type: application/json" -X POST -d '{"jsonrpc":"2.0","method":"venachain_pendingTransactions","params":[],"id":1}' "http://127.0.0.1:6791"

- 响应：

.. code:: json

    {
        "jsonrpc":"2.0",
        "id":1,
        "result":[
            {
                "blockHash":"0x0000000000000000000000000000000000000000000000000000000000000000",
                "blockNumber":null,
                "from":"0xedfff6ca4e89d4d301fe95c2f385ed636cbeefc7",
                "gas":"0x0",
                "gasPrice":"0x0",
                "hash":"0x9724bf059ccf19bb4572ba8837465fd945a388151e6f7f9f342b87de64e442d7",
                "input":"0xd78800000000000000028d736574537570657241646d696e",
                "nonce":"0x1469fd401068a",
                "to":"0x1000000000000000000000000000000000000001",
                "transactionIndex":"0x0",
                "value":"0x0",
                "v":"0x27c",
                "r":"0x2cdfd7f95d151c51e8646cc6dc5a10b8df3a643e8ea146fa0a321f16a5f1663",
                "s":"0xef964620fa06b738d3c3a4738825df8e89aebe3ea04838395c816cf09b0639c"
            },
            {
                "blockHash":"0x0000000000000000000000000000000000000000000000000000000000000000",
                "blockNumber":null,
                "from":"0xedfff6ca4e89d4d301fe95c2f385ed636cbeefc7",
                "gas":"0x0",
                "gasPrice":"0x0",
                "hash":"0x2ed650c6f585d1e78ef96e952bb498958032e1ead1e9c2b1e299a7ade56c5c47",
                "input":"0xf84b88000000000000000296616464436861696e41646d696e427941646472657373aa307865646666663663613465383964346433303166653935633266333835656436333663626565666337",
                "nonce":"0x2ee7e4047daae",
                "to":"0x1000000000000000000000000000000000000001",
                "transactionIndex":"0x0",
                "value":"0x0",
                "v":"0x27c",
                "r":"0x9eb046ff9e54ecf06d8c89c2ab93f2de5b8c2cf42f5a9e73d8b35151cf72db8a",
                "s":"0x7adbbbb40f361ab8fe8a31573a99c42a2adfbad75c2363ad7b5f4d8469e79e3f"
            },
            {
                "blockHash":"0x0000000000000000000000000000000000000000000000000000000000000000",
                "blockNumber":null,
                "from":"0xedfff6ca4e89d4d301fe95c2f385ed636cbeefc7",
                "gas":"0x0",
                "gasPrice":"0x0",
                "hash":"0x35f4f3dae8d6215de0bf18c7084028ae125dd0edc49f1242f380e3c51b52b11b",
                "input":"0xf9014f88000000000000000283616464b9013f7b226e616d65223a2230222c2274797065223a302c227075626c69634b6579223a223765306435656631393561636239613832653139663738346631646231346664336534313430336537356436353764383836373539326230313234623738353266386531643231346134393433646430356461323031356533396336306164643332323362653830653138393231363335306565373835613239373336353939222c2264657363223a22222c2265787465726e616c4950223a223132372e302e302e31222c22696e7465726e616c4950223a223132372e302e302e31222c22727063506f7274223a363739312c22703270506f7274223a31363739312c226f776e6572223a22307839323762613730313163353564383239323935633363623730613161346430346539346133333136222c22737461747573223a317d",
                "nonce":"0x1781a293b38a2",
                "to":"0x1000000000000000000000000000000000000002",
                "transactionIndex":"0x0",
                "value":"0x0",
                "v":"0x27b",
                "r":"0x1997eddb0d5634d1dca09338d5142d0d7780f1f4615fad94b8f4952455b2c91f",
                "s":"0x33c68867ff0cf003e5e914d08138970f5edf638c3f31ae418943e6d26c0b93eb"
            },
            {
                "blockHash":"0x0000000000000000000000000000000000000000000000000000000000000000",
                "blockNumber":null,
                "from":"0xedfff6ca4e89d4d301fe95c2f385ed636cbeefc7",
                "gas":"0x0",
                "gasPrice":"0x0",
                "hash":"0xce292bb52aa5db9ff6634d16b2c369fdb5574458be1720f9237ef2737672e2d4",
                "input":"0xdc88000000000000000286757064617465308a7b2274797065223a317d",
                "nonce":"0x97bb9d690f2b",
                "to":"0x1000000000000000000000000000000000000002",
                "transactionIndex":"0x0",
                "value":"0x0",
                "v":"0x27b",
                "r":"0xc2cfdfeee1a476c6bdf7dbba76faceaf3550bfd495f758b45b00411e52b033e6",
                "s":"0x5ccf1f1c9d04d32367c70cf4460575af58d66116ba763de3741111257b662040"
            }
        ]
    }

venachain_pendingTransactionsLength
=========================================

返回交易池中处于 pending 中的交易数据量。

参数
^^^^^

无。

返回值
^^^^^^^

- ``int`` : 处于 pending 中的交易数据量

示例代码
^^^^^^^^^^

- 请求：

.. code:: sh

    curl -H "Content-Type: application/json" -X POST -d '{"jsonrpc":"2.0","method":"venachain_pendingTransactionsLength","params":[],"id":1}' "http://127.0.0.1:6791"

- 响应：

.. code:: json

    {
        "jsonrpc":"2.0",
        "id":1,
        "result":4
    }

venachain_resend
==================

用一个新的交易和新的GasPrice与GasLimit，去替换掉交易池中的指定交易，被替换的交易将被从交易池中删除。

参数
^^^^^^

- ``object`` : 交易池的交易对象
    -  from ``data`` : 交易发起人地址
    -  to ``data`` : 交易目标地址
    -  gas ``quantity`` : gas限制
    -  gasPrice ``quantity`` : gas价格
    -  value ``quantity`` : 交易金额
    -  nonce ``quantity`` : 随机数
    -  data ``data`` : 交易数据
    -  input ``data`` : 交易数据
- ``quantity`` : gasPrice，gas价格
- ``quantity`` : gasLimit，gas限制

.. note:: 出于向后兼容的原因，我们接受“data”和“input”两个参数，“input”是较新的名称，客户应首选该名称。

返回值
^^^^^^

- ``data`` : 32字节，交易哈希

示例代码
^^^^^^^^^^

- 请求：

.. code:: sh

    curl -H "Content-Type: application/json" -X POST -d '{"jsonrpc":"2.0","method":"venachain_resend","params":[{"from":"0xf92ce14220d5c938342651a43c14a311ae37f43b", "to":"0xd46e8dd67c5d32be8058bb8eb970870f07244567", "gas":"0x76c0", "nonce":"0x3", "gasPrice":"0x9184e72a000", "value":"0x9184e72a", "data":"0xd46e8dd67c5d32be8d46e8dd67c5d32be8058bb8eb970870f072445675058bb8eb970870f072445675"},"0x9184e72a005", "0x9184e72a0a0"],"id":1}' "http://127.0.0.1:6791"

- 响应：

.. code:: json

    {
        "id":1,
        "jsonrpc":"2.0",
        "result":"0xe670ec64341771606e55d6b4ca35a1a6b75ee3d5145a99d05921026d1527331"
    }

venachain_accounts
========================

返回此节点管理的帐户集合

参数
^^^^^^

无

返回值
^^^^^^^^^^^

- ``data array`` : 一个账户地址的数组

示例代码
^^^^^^^^^^^^^

- 请求：

.. code:: sh

    curl -H "Content-Type: application/json" -X POST -d '{"jsonrpc":"2.0","method":"venachain_accounts","params":[],"id":1}' "http://127.0.0.1:6791"

- 响应：

.. code:: json

    {
        "jsonrpc":"2.0",
        "id":1,
        "result":[
            "0xf92ce14220d5c938342651a43c14a311ae37f43b",
            "0xedfff6ca4e89d4d301fe95c2f385ed636cbeefc7"
        ]
    }

.. _rpc-venachain-newPendingTransactionFilter:

venachain_newPendingTransactionFilter [websocket]
===============================================================

在节点中创建一个监听 **pending交易** 产生的过滤器，当产生新的 pending 交易时进行通知。
要检查状态是否发生变化，请调用 :ref:`venachain_getFilterChanges <rpc-venachain-getFilterChanges>`

.. note:: 此接口所描述的通知是通过 websocket 来通知的，即需要使用 websocket 连接先发送请求对指定事件进行订阅，在事件触发后才能收到该事件的通知。

参数
^^^^^^

无

返回值
^^^^^^^^^^

- ``data`` : 过滤器哈希

示例代码
^^^^^^^^^^^

- 请求：

.. code:: sh

    curl -H "Content-Type: application/json" -X POST -d '{"jsonrpc":"2.0","method":"venachain_newPendingTransactionFilter","params":[],"id":1}' "http://127.0.0.1:6791"

- 响应：

.. code:: json

    {
        "jsonrpc":"2.0",
        "id":1,
        "result":"0x629e6882c8995ab2e46fa99bcf719eba"
    }

.. _rpc-venachain-newBlockFilter:

venachain_newBlockFilter [websocket]
=============================================

在节点中创建一个监听 **新区块** 产生的过滤器，当新区块生成时进行通知。要检查状态是否变化，请调用 :ref:`venachain_getFilterChanges <rpc-venachain-getFilterChanges>`


.. note:: 此接口所描述的通知是通过 websocket 来通知的，即需要使用 websocket 连接先发送请求对指定事件进行订阅，在事件触发后才能收到该事件的通知。

参数
^^^^^^^

无

返回值
^^^^^^^^

- ``data`` : 过滤器哈希

示例代码
^^^^^^^^^

- 请求：

.. code:: sh

    curl -H "Content-Type: application/json" -X POST -d '{"jsonrpc":"2.0","method":"venachain_newBlockFilter","params":[],"id":1}' "http://127.0.0.1:6791"

- 响应：

.. code:: json

    {
        "jsonrpc":"2.0",
        "id":1,
        "result":"0x49f265419399903a285d325aa8955866"
    }

.. _rpc-venachain-newFilter:

venachain_newFilter [websocket]
===================================

基于给定的选项创建一个过滤器对象，当状态发生变化时进行通知。要检查状态是否变化，请调用 :ref:`venachain_getFilterChanges <rpc-venachain-getFilterChanges>`
关于特定主题【topic】过滤器的说明:主题【topic】是顺序相关的。如果一个交易的日志有主题 ``[A, B]`` ，那么将被以下的主题过滤器匹配：

- [] : 任何主题
- [A] : 先匹配A主题
- [null, B] : 先匹配其他主题，再匹配B主题
- [A, B] : 先匹配A主题，再匹配B主题，最后匹配其他主题
- [[A, B], [A, B]] : 先匹配A主题或B主题，再匹配A主题或B主题，最后匹配其他主题

.. note:: 此接口所描述的通知是通过 websocket 来通知的，即需要使用 websocket 连接先发送请求对指定事件进行订阅，在事件触发后才能收到该事件的通知。

参数
^^^^^^

- ``object`` : 过滤器选项对象
    + blockhash ``data`` : 可选，由 :ref:`venachain_getLogs <rpc-venachain-getLogs>` 使用，仅从具有此哈希的块返回日志。
    + fromblock ``quantity | TAG`` : 可选，默认值:"latest"。区块的块高，或字符串"latesr"表示最后挖出的块，"pending"或"earliest"用于未挖出的交易。
    + toblock ``quantity | TAG`` : 可选，默认值:"latest"。区块的块高，或字符串"latesr"表示最后挖出的块，"pending"或"earliest"用于未挖出的交易。
    + address ``data array`` : 可选，合约地址或生成日志的一组地址。
    + topic ``data array array`` : 主题

返回值
^^^^^^^^

- ``data`` : 过滤器哈希

示例代码
^^^^^^^^^

- 请求：

.. code:: sh

    curl -H "Content-Type: application/json" -X POST -d '{"jsonrpc":"2.0","method":"venachain_newFilter","params":[{"topics":["0x000000000000000000000000a94f5374fce5edbc8e2a8697c15331677e6ebf0b"]}],"id":1}' "http://127.0.0.1:6791"

- 响应：

.. code:: json

    {
        "jsonrpc":"2.0",
        "id":1,
        "result":"0x1d72025d3166c71be2cd4dbb413ede5b"
    }

.. _rpc-venachain-getLogs:

venachain_getLogs
===================

返回指定过滤器中的所有日志。

参数
^^^^^

- ``object`` : 过滤器对象，请参考 :ref:`venachain_newFilter <rpc-venachain-newFilter>` 调用的参数

.. code:: js

    params: [{
      "topics": ["0x000000000000000000000000a94f5374fce5edbc8e2a8697c15331677e6ebf0b"]
    }]

返回值
^^^^^^^^^^

请参考 :ref:`venachain_getFilterChanges <rpc-venachain-getFilterChanges>`

示例代码
^^^^^^^^^^

- 请求：

.. code:: sh

    curl -H "Content-Type: application/json" -X POST --data '{"jsonrpc":"2.0","method":"venachain_getLogs","params":[{"topics":["0x000000000000000000000000a94f5374fce5edbc8e2a8697c15331677e6ebf0b"]}],"id":1}' "http://127.0.0.1:6791"

- 响应

请参考 :ref:`venachain_getFilterChanges <rpc-venachain-getFilterChanges>`

venachain_uninstallFilter
===============================

卸载指定哈希的过滤器。当不再需要监听时，总是需要执行该调用。另外，过滤器如果在一定时间内未接收到 :ref:`venachain_getFilterChanges <rpc-venachain-getFilterChanges>` 调用会自动超时。

参数
^^^^^^^

- ``data`` : 过滤器哈希

.. code:: js

    params: [
        "0x49f265419399903a285d325aa8955866"
    ]

返回值
^^^^^^^^^^^

- ``bool`` : 如果成功卸载则返回true，否则返回false

示例代码
^^^^^^^^^^^

- 请求：

.. code:: sh

    curl -H "Content-Type: application/json" -X POST -d '{"jsonrpc":"2.0","method":"venachain_uninstallFilter","params":["0x49f265419399903a285d325aa8955866"],"id":1}' "http://127.0.0.1:6791"

- 响应：

.. code:: json

    {
        "jsonrpc":"2.0",
        "id":1,
        "result":true
    }

venachain_getFilterLogs
========================

返回指定哈希的过滤器中的全部日志。

参数
^^^^^^^

- ``data`` : 过滤器哈希

.. code:: js

    params: [
      "0x1d72025d3166c71be2cd4dbb413ede5b"
    ]

返回值
^^^^^^^^

请参考 :ref:`venachain_getFilterChanges <rpc-venachain-getFilterChanges>`

示例代码
^^^^^^^^^^^

- 请求：

.. code:: sh

    curl -H "Content-Type: application/json" -X POST -d '{"jsonrpc":"2.0","method":"venachain_getFilterLogs","params":["0x1d72025d3166c71be2cd4dbb413ede5b"],"id":1}' "http://127.0.0.1:6791"

- 响应

请参考 :ref:`venachain_getFilterChanges <rpc-venachain-getFilterChanges>`

.. _rpc-venachain-getFilterChanges:

venachain_getFilterChanges
====================================

轮询指定的过滤器，并返回自上次轮询之后新生成的日志数组。

参数
^^^^^^

- ``data`` : 过滤器哈希

.. code:: js

    params: [
      "0x1d72025d3166c71be2cd4dbb413ede5b"
    ]

返回值
^^^^^^^^^

``array`` : 日志对象数组，如果没有新生成的日志，则返回空数组。

1) 使用 :ref:`venachain_newBlockFilter <rpc-venachain-newBlockFilter>` 创建的过滤器将返回块哈希（32字节），例如 ``["0x3454645634534..."]`` 。
2) 使用 :ref:`venachain_newPendingTransactionFilter <rpc-venachain-newPendingTransactionFilter>` 创建的过滤器将返回交易哈希(32字节)，例如 ``["0x6345343454645..."]`` 。
3) 使用 :ref:`venachain_newFilter <rpc-venachain-newFilter>` 创建的过滤器，日志对象具有如下参数：
    - removed ``bool`` : 如果日志已被删除则返回true，如果是有效日志则返回false
    - logIndex ``quantity`` : 日志在块内的索引序号。对于挂起日志，该值为null
    - blockNumber ``quantity`` : 该日志所在区块的块高。对于挂起日志，该值为null
    - blockHash ``data`` : 该日志所在块的哈希。对于挂起日志，该值为null
    - transactionHash ``data`` : 创建该日志的交易的哈希。对于挂起日志，该值为null
    - transactionIndex ``quantity`` : 创建日志的交易索引序号，对于挂起日志，该值为null
    - address ``data`` : 该日志的源地址
    - data ``data``- 包含该日志的一个或多个32字节无索引参数
    - topics ``data array`` : 0~4个32字节索引日志参数的数据。在solidity中，第一个主题是事件签名，例如 ``Deposit(address,bytes32,uint256)`` ，除非你声明的是匿名事件

示例代码
^^^^^^^^^^

- 请求：

.. code:: sh

    curl -H "Content-Type: application/json" -X POST -d '{"jsonrpc":"2.0","method":"venachain_getFilterChanges","params":["0x1d72025d3166c71be2cd4dbb413ede5b"],"id":1}' "http://127.0.0.1:6791"

- 响应：

.. code:: js

    {
        "id":1,
        "jsonrpc":"2.0",
        "result":[
            {
                "logIndex":"0x1",   // 1
                "blockNumber":"0x1b4",  // 436
                "blockHash":"0x8216c5785ac562ff41e2dcfdf5785ac562ff41e2dcfdf829c5a142f1fccd7d",
                "transactionHash":"0xdf829c5a142f1fccd7d8216c5785ac562ff41e2dcfdf5785ac562ff41e2dcf",
                "transactionIndex":"0x0",   // 0
                "address":"0x16c5785ac562ff41e2dcfdf829c5a142f1fccd7d",
                "data":"0x0000000000000000000000000000000000000000000000000000000000000000",
                "topics":[
                    "0x59ebeb90bc63057b6515673c3ecf9438e5058bca0f92585014eced636878c9a5"
                ]
            },
            {
                ...
            }
        ]
    }