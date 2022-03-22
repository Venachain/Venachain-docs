.. _develop-rpc-methods-txpool:

========
txpool
========

txpool_content
==================

返回交易池中所有挂起的交易，按 account 分组并按 nonce 排序。

参数
^^^^^

无

返回值
^^^^^^^^

``map[string]map[string]map[string]Object``

- ``map[string]object`` : 返回的数据分组
    + key ``string`` : "pending"
    + value ``map[string]object`` : 处于挂起状态的交易信息，key:account，value:可序列化为 RPC交易的交易对象
        - key ``data`` : account hex
        - value ``map[string]object``
            + blockHash ``data`` : 区块哈希
            + blockNumber ``quantity`` : 区块高度
            + from ``data`` : 交易发起人地址
            + to ``data`` : 交易目标地址
            + gas ``quantity`` : gas限制
            + gasPrice ``quantity`` : gas价格
            + hash ``data`` : 交易哈希
            + input ``data`` : 交易数据
            + nonce ``quantity`` : 随机数=
            + transactionIndex ``quantity`` : 交易在区块中的下标=
            + value ``quantity`` : 交易金额
            + v、r、s ``quantity`` : 签名值
    + key ``string`` : "queued"
    + value ``map[string]object`` : 处于排队状态的交易信息，key:account hex，value:可序列化为 RPC 交易的交易对象

示例代码
^^^^^^^^^

- 请求：

.. code:: sh

    curl -H "Content-Type: application/json" -X POST -d '{"jsonrpc":"2.0","method":"txpool_content","params":[],"id":1}' "http://127.0.0.1:6791"

- 响应：

.. code:: json

    {
        "jsonrpc":"2.0",
        "id":1,
        "result":{
            "pending":{
                "0xeDfff6Ca4E89D4D301Fe95c2f385ed636CbEEFC7":{
                    "166832055586603":{
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
                    },
                    "413528732940450":{
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
                    }
                }
            },
            "queued":{

            }
        }
    }

txpool_status
==================

返回交易池中处于挂起状态的交易数量。

参数
^^^^^^

无

返回值
^^^^^^^^^

- ``map[string]int`` : 返回交易池中挂起和排队的交易数
    + key ``string`` : "pending"
    + value ``quantity`` : 处于 pending 状态的交易数量
    + key ``string`` : "queued"
    + value ``quantity`` : 处于排队中的交易数量

示例代码
^^^^^^^^^^^

- 请求：

.. code:: sh

    curl -H "Content-Type: application/json" -X POST -d '{"jsonrpc":"2.0","method":"txpool_status","params":[],"id":1}' "http://127.0.0.1:6791"

- 响应：

.. code:: json

    {
        "jsonrpc":"2.0",
        "id":1,
        "result":{
            "pending":"0x2",
            "queued":"0x0"
        }
    }

txpool_inspect
==================

按账户分组，返回交易池中所有处于挂起状态的交易的去向及其费用消耗信息。

参数
^^^^^^^

无

返回值
^^^^^^^^^

``map[string]map[string]map[string]string``

- ``map[string]object`` 交易的去向及其费用消耗信息
    + key ``string`` : "pending"
    + value ``map[string]object`` : 处于挂起状态的交易的去向及其费用消耗信息
        - key ``data`` : 账户
        - value ``map[string]string`` 按 nonce排序的交易的去向及其费用消耗信息
            + key ``string`` : nonce 随机数
            + value ``string`` : 该随机数对应的交易的去向及其费用消耗信息
    + key ``string`` : "queued"
    + value ``map[string]object`` : 处于排队状态的交易的去向及其费用消耗信息

示例代码
^^^^^^^^^^

- 请求：

.. code:: sh

    curl -H "Content-Type: application/json" -X POST -d '{"jsonrpc":"2.0","method":"txpool_inspect","params":[],"id":1}' "http://127.0.0.1:6791"

- 响应：

.. code:: json

    {
        "jsonrpc":"2.0",
        "id":1,
        "result":{
            "pending":{
                "0xeDfff6Ca4E89D4D301Fe95c2f385ed636CbEEFC7":{
                    "166832055586603":"0x1000000000000000000000000000000000000002: 0 wei + 0 gas × 0 wei",
                    "413528732940450":"0x1000000000000000000000000000000000000002: 0 wei + 0 gas × 0 wei"
                }
            },
            "queued":{

            }
        }
    }