=============================
节点部署工具 (platonectl.sh)
=============================

1. 命令详情
===========

本节详细介绍了命令。

.. code:: console

       描述
           PlatONE脚本部署
       用法:
           platonectl.sh <command> [command options] [arguments...]
       命令:
           init                             初始化节点。 请先设置genesis
           one                              完全启动一个节点
                                            默认帐户密码:0
           four                             完全启动四个节点
                                            默认帐户密码:0
           start                            尝试启动指定的节点
           stop                             尝试停止指定的节点
           restart                          尝试重新启动指定的节点
           console                          启动交互式JavaScript环境
           deploysys                        部署系统合约
           updatesys                        正常节点更新到共识节点
           addnode                          将正常节点添加到系统合同中
           clear                            清除所有节点数据
           unlock                           解锁节点帐户
           get                              显示系统合同中的所有节点
           setupgen                         创建genesis.json并编译系统契约
           status                           显示节点状态
           createacc                        创建帐户
           remote                           远程部署

1.1. init
^^^^^^^^^

例子:

.. code:: bash

   ./platonectl.sh init -h

.. code:: console

   init OPTIONS
       --nodeid, -n                 set node id (default=0)
       --ip                         set node ip (default=127.0.0.1)
       --rpc_port                   set node rpc port (default=6791)
       --p2p_port                   set node p2p port (default=16791)
       --ws_port                    set node ws port (default=26791)
       --auto                       auto=true: will no prompt to create
                                    the node key and init (default: false)
       --help, -h                   show help

例子: 
   
.. code:: bash
   
   platonectl.sh init -n 1 \
                 --ip 127.0.0.1 \
                 --rpc_port 6790 \
                 --p2p_port 16790 \
                 --ws_port 26790 \
                 --auto "true" 
   
   ## or
   platonectl.sh init

1.2. one
^^^^^^^^

例子:

.. code:: bash

   ./platonectl.sh one

这种情况下:

-  创建genesis.json
-  初始化第一个节点
-  启动第一个节点
-  部署系统合约

   -  创建用户
   -  创建ctool json
   -  部署所有系统合约
   -  将第一个节点添加到NodeManager System Contract

1.3. four
^^^^^^^^^

例子:

.. code:: bash

   ./platonectl.sh four

这种情况下:

-  创建 genesis.json
-  初始化第一个节点和其他三个
-  开启第一个节点
-  部署系统合约

   -  创建用户
   -  创建ctool json
   -  部署所有的系统合约
   -  将第一个节点添加到 NodeManager System Contract

-  添加其他三个节点到NodeManager System Contract
-  启动其他三个节点
-  更新其他三个节点类型为共识节点

1.4. start
^^^^^^^^^^

.. code:: console

   描述
       PlatONE脚本部署
   用法:
       platonectl.sh <command> [command options] [arguments...]
   命令
       start OPTIONS
           --nodeid, -n                 启动指定的节点
           --bootnodes, -b              连接到指定的bootnodes节点
                                        默认值是observeNodes中的第一个enode在genesis.json
           --logsize, -s                日志块大小（默认值:67108864）
           --logdir, -d                 log dir (默认值位置:../data/node_dir/logs/)
                                        设置时路径连接符'/'需要进行转义: 如 ".\/logs"
           --extraoptions, -e           platone命令启动时, 额外需要设置的命令行参数.
                                        (默认值: --debug)
           --all, -a                    启动所有节点
           --help, -h                   显示帮助

1.5. stop
^^^^^^^^^

.. code:: console

   描述
       PlatONE脚本部署
   用法:
       platonectl.sh <command> [command options] [arguments...]
   命令
       stop OPTIONS
           --nodeid, -n                 停止指定的节点
           --all, -a                    停止所有节点
           --help, -h                   显示帮助

1.6. restart
^^^^^^^^^^^^

.. code:: console

   描述
       PlatONE脚本部署
   用法:
       platonectl.sh <command> [command options] [arguments...]
   命令
       restart OPTIONS
           --nodeid, -n                 重新启动指定的节点
           --all, -a                    重启所有节点
           --help, -h                   显示帮助

1.7. console
^^^^^^^^^^^^

.. code:: console

   描述
       PlatONE脚本部署
   用法:
       platonectl.sh <command> [command options] [arguments...]
   命令
       ./platonectl console -n 0

详情请见 :ref:`platone console 使用介绍 <platone-console>`

1.8. deploysys
^^^^^^^^^^^^^^

.. code:: console

   描述
       PlatONE脚本部署
   用法:
       platonectl.sh <command> [command options] [arguments...]
   命令
       deploysys OPTIONS
           --nodeid, -n                 指定的节点标识（默认值:0）
           --auto                       auto=true: 将使用默认节点密码:0
                                        创建帐户，并解锁帐户（默认值:false）
           --help, -h                   显示帮助

1.9. updatesys
^^^^^^^^^^^^^^

.. code:: console

   描述
       PlatONE脚本部署
   用法:
       platonectl.sh <command> [command options] [arguments...]
   命令
       updatesys OPTIONS
           --nodeid, -n                 指定的节点ID
           --content, -c                更新内容 (默认值:'{“type”:1}'）
                                        注意参数格式
           --help, -h                   显示帮助

1.10. addnode
^^^^^^^^^^^^^

.. code:: console

   描述
       PlatONE脚本部署
   用法:
       platonectl.sh <command> [command options] [arguments...]
   命令
       addnode OPTIONS
           --nodeid, -n                 指定的节点ID。必须指定
           --desc                       指定的节点desc
           --p2p_port                   指定的节点p2p_port
                                        如果nodeid指定的节点是本地的，
                                        那么你不需要指定这个选项。
           --rpc_port                   指定的节点rpc_port
                                        如果nodeid指定的节点是本地的，
                                        那么你不需要指定这个选项。
           --ip                         指定的节点ip
                                        如果nodeid指定的节点是本地的，
                                        那么你不需要指定这个选项。
           --pubkey                     指定的节点pubkey
                                        如果nodeid指定的节点是本地的，
                                        那么你不需要指定这个选项。
           --account                    指定的节点帐户
                                        如果nodeid指定的节点是本地的，
                                        那么你不需要指定这个选项。
           --help, -h                   显示帮助

1.11. clear
^^^^^^^^^^^

.. code:: console

   描述
       PlatONE脚本部署
   用法:
       platonectl.sh <command> [command options] [arguments...]
   命令
       clear OPTIONS
           --nodeid, -n                 清除指定的节点数据
           --all, -a                    清除所有节点数据
           --help, -h                   显示帮助

1.12. unlock
^^^^^^^^^^^^

.. code:: console

   描述
       PlatONE脚本部署
   用法:
       platonectl.sh <command> [command options] [arguments...]
   命令
       unlock OPTIONS
           --nodeid, -n                 解锁指定的节点帐户
           --help, -h                   显示帮助

1.13. get
^^^^^^^^^

从NodeManager系统合同中获取所有节点

例子:

.. code:: bash

   ./platonectl.sh get

1.14. setupgen
^^^^^^^^^^^^^^

.. code:: console

   描述
       PlatONE脚本部署
   用法:
       platonectl.sh <command> [command options] [arguments...]
   命令
       setupgen OPTIONS
           --nodeid, -n                 第一个节点id（默认值:0）
           --ip                         第一个节点ip（默认值:127.0.0.1）
           --p2p_port                   第一个节点p2p_port（默认值:16791）
           --auto                       auto=true: 将自动创建新的节点密钥并将自动创建
                                        不再编译系统合约（默认= false）
           --observeNodes, -o           设置genesis observeNodes
                                       （默认值是第一个节点的enode代码）
           --validatorNodes, -v         设置genesis validatorNodes
                                       （默认值是第一个节点的enode代码）
           --interpreter, -i            选择虚拟机解释器:wasm, evm, all (default: wasm)
           --help, -h                   显示帮助

1.15. status
^^^^^^^^^^^^

.. code:: console

   描述
       PlatONE脚本部署
   用法:
       platonectl.sh <command> [command options] [arguments...]
   命令
       status OPTIONS                   显示所有节点状态
           --nodeid, -n                 显示指定的节点状态信息
           --all, -a                    显示所有节点状态信息
           --help, -h                   显示帮助

1.16. createacc
^^^^^^^^^^^^^^^

.. code:: console

   描述
       PlatONE脚本部署
   用法:
       platonectl.sh <command> [command options] [arguments...]
   命令
       createacc OPTIONS
           --nodeid, -n                 为指定节点创建帐户
           --help, -h                   显示帮助

1.17. remote
^^^^^^^^^^^^^

.. _platone-console:

2. platonectl console 使用介绍
=================================

2.1. eth
^^^^^^^^^^

2.1.1. eth.defaultAccount
-----------------------------------

**描述**: 设置默认账户

默认的地址在使用下述方法时使用，你也可以选择通过指定\ ``from``\ 属性，来覆盖这个默认设置。

.. code:: bash

   eth.sendTransaction()
   eth.call()

**参数**:

- ``String`` : 20字节的当前设置的默认地址。

**操作**

.. code:: bash

   > eth.defaultAccount = '0x8888f1f195afa192cfee860698584c030f4c9db1';

**输出结果**

.. code:: console

    "0x8888f1f195afa192cfee860698584c030f4c9db1"

2.1.2. eth.defaultBlock
------------------------------

**描述**:

使用下述方法时，会使用默认块设置，你也可以通过传入\ ``defaultBlock``\ 来覆盖默认配置。

.. code:: bash

   eth.getBalance()
   eth.getCode()
   eth.getTransactionCount()
   eth.getStorageAt()
   eth.call()
   contract.myMethod.call()
   contract.myMethod.estimateGas()

**参数**:

可选的块参数，可能下述值中的一个:

- ``Number`` - 区块号
- ``earliest``: ``String`` - 创世块
- ``latest``: ``String`` - 最近刚出的最新块，当前的区块头
- ``pending``: ``String`` - 当前正在\ ``mine``\ 的区块，包含正在打包的交易

.. note:: 默认值是\ ``latest``

**返回值**:

- ``Number|String`` - 默认要查状态的区块号。

**操作**

.. code:: bash

   > eth.defaultBlock = 18

**输出结果**

.. code:: console

   18

2.1.3. eth.syncing
-------------------------------

**描述** 这个属性是只读的。如果正在同步，返回同步对象。否则返回\ ``false``\ 。

- 同步方式:

.. code:: bash

   eth.syncing

- 异步方式:

.. code:: bash

   eth.getSyncing(callback(error, result){ … })

**返回值**

``Object|Boolean`` -
如果正在同步，返回含下面属性的同步对象。否则返回\ ``false``\ 。

- ``startingBlock``: ``Number`` - 同步开始区块号
- ``currentBlock``: ``Number`` - 节点当前正在同步的区块号
- ``highestBlock``: ``Number`` - 预估要同步到的区块

**操作**

.. code:: bash

   > eth.syncing

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

2.1.4. eth.gasPrice
-----------------------------

**描述** 属性是只读的，返回当前的gas价格。这个值由最近几个块的gas价格的中值\ `6 <https://web3.tryblockchain.org/Web3.js-api-refrence.html#fn6>`__\ 决定。

- 同步方式:

.. code:: bash

   eth.gasPrice

- 异步方式:

.. code:: bash

   eth.getGasPrice(callback(error, result){ … })

**返回值**

- ``BigNumber`` - 当前的gas价格的\ ``BigNumber``\ 实例，以\ ``wei``\ 为单位。

**操作**

.. code:: bash

   > eth.gasPrice.toString()

**输出结果**

.. code:: console

   "0"

2.1.5. eth.accounts
----------------------------

**描述** 只读属性，返回当前节点持有的帐户列表。

- 同步方式:

.. code:: bash

   eth.accounts

- 异步方式:

.. code:: bash

   th.getAccounts(callback(error, result){ … })

**返回值**

- ``Array`` - 节点持有的帐户列表。

**操作**

.. code:: bash

   > eth.accounts

**输出结果**

.. code:: console

   ["0x4bf2e487a1f57d26d57ad9b9821a827e45b9e8e6", "0x7dc33e8ea643ae40459eec74a3879a7018328160"]

2.1.6. eth.blockNumber
-------------------------------

**描述** 返回当前区块号。如果之前设置过默认区块，这里返回的就是默认区块的区块号。如果没有设置默认区块，则返回最新的区块latest。

- 同步方式:

.. code:: bash

   eth.blockNumber

- 异步方式:

.. code:: bash

   eth.getBlockNumber(callback(error, result){ … })

**返回值**

一个Promise对象，其解析值为最近一个块的编号，Number类型。

**操作**

.. code:: bash

   > eth.blockNumber

**输出结果**

.. code:: console

   20

2.1.7. eth.getBalance
------------------------------

**描述** 获得在指定区块时给定地址的余额。

.. code:: bash

   eth.getBalance(addressHexString [, defaultBlock] [, callback])

**参数**

-  ``String`` - 要查询余额的地址。
-  ``Number|String`` -（可选）如果不设置此值使用\ ``eth.defaultBlock``\ 设定的块，否则使用指定的块。
-  ``Funciton`` - （可选）回调函数，用于支持异步的方式执行[async]。

**返回值**

- ``String`` - 一个包含给定地址的当前余额的\ ``BigNumber``\ 实例，单位为\ ``wei``\ 。

**操作**

.. code:: bash

   > eth.getBalance(eth.accounts[0]);

**输出结果**

.. code:: console

   0

2.1.8. eth.getBlock
-----------------------------

**描述** 返回块号或区块哈希值所对应的区块

.. code:: bash

   eth.getBlock(blockHashOrBlockNumber [, returnTransactionObjects] [, callback])

**参数**

-  ``Number|String`` -（可选）如果未传递参数，默认使用\ ``eth.defaultBlock``\ 定义的块，否则使用指定区块。
-  ``Boolean`` -（可选）默认值为\ ``false``\ 。\ ``true``\ 会将区块包含的所有交易作为对象返回。否则只返回交易的哈希。
-  ``Function`` - 回调函数，用于支持异步的方式执行[async]。

**返回值**: - 区块对象

- ``Number`` - 区块号。当这个区块处于\ ``pending``\ 将会返回\ ``null``\ 。
- ``hash`` - 字符串，区块的哈希串。当这个区块处于\ ``pending``\ 将会返回\ ``null``\ 。
- ``parentHash`` - 字符串，32字节的父区块的哈希值。
- ``nonce`` - 字符串，8字节。POW生成的哈希。当这个区块处于\ ``pending``\ 将会返回\ ``null``\ 。
- ``sha3Uncles`` - 字符串，32字节。叔区块的哈希值。
- ``logsBloom`` - 字符串，区块日志的布隆过滤器\ `9 <https://web3.tryblockchain.org/Web3.js-api-refrence.html#fn9>`__\ 。当这个区块处于\ ``pending``\ 将会返回\ ``null``\ 。
- ``transactionsRoot`` - 字符串，32字节，区块的交易前缀树的根。
- ``stateRoot`` - 字符串，32字节。区块的最终状态前缀树的根。
- ``miner`` - 字符串，20字节。这个区块获得奖励的矿工。
- ``difficulty``: ``BigNumber`` - 当前块的难度，整数。
- ``totalDifficulty``: ``BigNumber`` - 区块链到当前块的总难度，整数。
- ``extraData`` - 字符串。当前块的\ ``extra data``\ 字段。
- ``size`` - ``Number``\ 。当前这个块的字节大小。
- ``gasLimit``: ``Number`` - 当前区块允许使用的最大\ ``gas``\ 。
- ``gasUsed`` - 当前区块累计使用的总的\ ``gas``\ 。
- ``timestamp``: `Number`` - 区块打包时的\ ``unix``\ 时间戳。
- ``transactions``: ``数组`` - 交易对象。或者是32字节的交易哈希。
- ``uncles``: ``数组`` - 叔哈希的数组。

**操作**

.. code:: bash

   > eth.getBlock(17)

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

2.1.9. eth.getBlockTransactionCount
----------------------------------------------

**描述** 返回指定区块的交易数量

.. code:: bash

   eth.getBlockTransactionCount(hashStringOrBlockNumber [, callback])

**参数**

-  ``Number|String`` -（可选）如果未传递参数，默认使用\ ``eth.defaultBlock``\ 定义的块，否则使用指定区块。
-  ``Function`` - 回调函数，用于支持异步的方式执行[async]。

**返回值**

- ``Nubmer`` - 给定区块的交易数量。

**操作**

.. code:: bash

   > eth.getBlockTransactionCount(17);

**输出结果**

.. code:: console

   1

2.1.10. eth.getTransaction
-------------------------------------

**描述** 返回匹配指定交易哈希值的交易。

.. code:: bash

   eth.getTransaction(transactionHash [, callback])

**参数**

-  ``String`` - 交易的哈希值。
-  ``Function`` - 回调函数，用于支持异步的方式执行[async]。

**返回值**

``Object`` - 一个交易对象

-  ``hash``: ``String`` - 32字节，交易的哈希值。
-  ``nonce``: ``Number`` - 交易的发起者在之前进行过的交易数量。
-  ``blockHash``: ``String`` - 32字节。交易所在区块的哈希值。当这个区块处于\ ``pending``\ 将会返回\ ``null``\ 。
-  ``blockNumber``: ``Number`` - 交易所在区块的块号。当这个区块处于\ ``pending``\ 将会返回\ ``null``\ 。
-  ``transactionIndex``: ``Number`` - 整数。交易在区块中的序号。当这个区块处于\ ``pending``\ 将会返回\ ``null``\ 。
-  ``from``: ``String`` - 20字节，交易发起者的地址。
-  ``to``: ``String`` - 20字节，交易接收者的地址。当这个区块处于\ ``pending``\ 将会返回\ ``null``\ 。
-  ``value``: ``BigNumber`` - 交易附带的货币量，单位为\ ``Wei``\ 。
-  ``gasPrice``: ``BigNumber`` - 交易发起者配置的\ ``gas``\ 价格，单位是\ ``wei``\ 。
-  ``gas``: ``Number`` - 交易发起者提供的\ ``gas``\ 。
-  ``input``: ``String`` - 交易附带的数据。

**操作**

.. code:: bash

   >  eth.getTransaction("0xf3e8bc7f626017bcd49e9f9e9a136de0625cb10aeeeaf4831b40ec1430e1ac5d"

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

2.1.11. eth.getTransactionFromBlock
---------------------------------------------

**描述** 返回指定区块的指定序号的交易。

.. code:: bash

   getTransactionFromBlock(hashStringOrNumber, indexNumber [, callback])

**参数**

-  ``String`` - 区块号或哈希。或者是\ ``earliest``\ ，\ ``latest``\ 或\ ``pending``\ 。查看\ ``eth.defaultBlock``\ 了解可选值。
-  ``Number`` - 交易的序号。
-  ``Function`` - 回调函数，用于支持异步的方式执行[async]。

**返回值**

- ``Object`` - 交易对象，详见 ``eth.getTransaction``

操作:

.. code:: bash

   > eth.getTransactionFromBlock(18,0)

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

2.1.12. eth.getTransactionReceipt
--------------------------------------------------

**描述** 通过一个交易哈希，返回一个交易的收据。

.. note:: 处于\ ``pending``\ 状态的交易，收据是不可用的。

.. code:: bash

   eth.getTransactionReceipt(hashString [, callback])

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
-  ``to``: ``String`` - 20字节，交易接收者的地址。如果是一个合约创建的交易，返回\ ``null``\ 。
-  ``cumulativeGasUsed``: ``Number`` - 当前交易执行后累计花费的\ ``gas``\ 总值\ `10 <https://web3.tryblockchain.org/Web3.js-api-refrence.html#fn10>`__\ 。
-  ``gasUsed``: ``Number`` - 执行当前这个交易单独花费的\ ``gas``\ 。
-  ``contractAddress``: ``String`` - 20字节，创建的合约地址。如果是一个合约创建交易，返回合约地址，其它情况返回\ ``null``\ 。
-  ``logs``: ``Array`` - 这个交易产生的日志对象数组。

**操作**

.. code:: bash

   > eth.getTransactionReceipt("0xf3e8bc7f626017bcd49e9f9e9a136de0625cb10aeeeaf4831b40ec1430e1ac5d")

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

2.1.13. eth.getTransactionCount
-------------------------------------------

**描述** 返回指定地址发起的交易数。

.. code:: bash

   eth.getTransactionCount(addressHexString [, defaultBlock] [, callback])

**参数**

-  ``String`` - 要获得交易数的地址。
-  ``Number|String`` -（可选）如果未传递参数，默认使用\ ``eth.defaultBlock``\ 定义的块，否则使用指定区块。
-  ``Function`` - 回调函数，用于支持异步的方式执行[async]。

**返回值**

- ``Number`` - 指定地址发送的交易数量。

**操作**

.. code:: bash

   > eth.getTransactionCount(eth.accounts[0])

**输出结果**

.. code:: console

   19

2.1.14. eth.sendTransaction
--------------------------------------

**描述** 发送一个交易到网络。

.. code:: bash

   eth.sendTransaction(transactionObject [, callback])

**参数**


``Object`` - 要发送的交易对象。

- ``from``: ``String`` - 指定的发送者的地址。如果不指定，使用\ ``eth.defaultAccount``\ 。
- ``to``: ``String`` - （可选）交易消息的目标地址，如果是合约创建，则不填.
-  ``value``: ``Number|String|BigNumber`` -（可选）交易携带的货币量，以\ ``wei``\ 为单位。如果合约创建交易，则为初始的基金。
-  ``gas``: ``Number|String|BigNumber`` - （可选）默认是自动，交易可使用的\ ``gas``\ ，未使用的\ ``gas``\ 会退回。
-  ``gasPrice``: ``Number|String|BigNumber`` -（可选）默认是自动确定，交易的\ ``gas``\ 价格，默认是网络\ ``gas``\ 价格的平均值。
-  ``data``: ``String`` - （可选）或者包含相关数据的字节字符串，如果是合约创建，则是初始化要用到的代码。
-  ``nonce``: ``Number`` - （可选）整数，使用此值，可以允许你覆盖你自己的相同\ ``nonce``\ 的，正在\ ``pending``\ 中的交易\ `11 <https://web3.tryblockchain.org/Web3.js-api-refrence.html#fn11>`__\ 。
-  ``Function`` - 回调函数，用于支持异步的方式执行[async]。

**返回值**

- ``String`` - 32字节的交易哈希串。用16进制表示。

.. warning:: 如果交易是一个合约创建，请使用\ ``eth.getTransactionReceipt()``\ 在交易完成后获取合约的地址。

**操作**

.. code:: bash

   >  eth.sendTransaction({from:eth.accounts[0],to:eth.accounts[1],value:0})

**输出结果**

.. code:: console

   "0xa7c2dfa781a6bab4016bbdc8e59b916c49c0699545a590a08efd8fc15c099664"

2.1.15. eth.estimateGas
------------------------------

**描述** 在节点的VM节点中执行一个消息调用，或交易。但是不会合入区块链中。返回使用的\ ``gas``\ 量。

.. code:: bash

   eth.estimateGas(callObject [, callback])

**参数**

同\ ``eth.sendTransaction``\ ，所有的属性都是可选的。

返回值:

- ``Number`` - 模拟的\ ``call/transcation``\ 花费的\ ``gas``\ 。

**操作**

.. code:: bash

   >  eth.estimateGas({
       to: "0xc4abd0339eb8d57087278718986382264244252f",
       data: "0xc6888fa10000000000000000000000000000000000000000000000000000000000000003"
   });

**输出结果**

.. code:: console

   21000

2.2. personal
^^^^^^^^^^^^^^^^^^^

2.2.1. personal.newAccount
------------------------------------

**描述** 创建一个新账户。

.. code:: bash

   web3.eth.personal.newAccount(password, [callback])

**参数**

- ``password``: ``String`` - 用来加密账户的密码

**返回值**

一个Promise对象，其解析值为新创建的账户。

**操作**

.. code:: bash

   >  personal.newAccount('password')

**输出结果**

.. code:: console

   "0x21baa00b024172344b029feb72839607875b990d"

2.2.2. unlockAccount
--------------------------

**描述** 解锁账户

.. code:: bash

   web3.eth.personal.unlockAccount(address, password, unlockDuraction [, callback])

**参数**

- ``address``: ``String`` - 要解锁的账户地址。
- ``password``: ``String`` - 账户密码。
- ``unlockDuration``: ``Number`` - 将帐户保持在解锁状态的持续时间。

**操作**

.. code:: bash

   > personal.unlockAccount(eth.accounts[1])

**输出结果**

.. code:: console

   true

2.2.3. lockAccount
------------------------

**描述** 锁定给定帐户。

.. code:: bash

   web3.eth.personal.lockAccount(address [, callback])

**参数**

- ``address``: ``String`` - 要锁定的账户地址。
- ``Function`` - (可选)回调函数，其第一个参数为错误对象，第二个参数为签名结果。

**返回值**

- Promise<boolean>

**操作**

.. code:: bash

   >  personal.lockAccount(eth.accounts[3])

**输出结果**

.. code:: console

   true

2.2.4. listAccounts
---------------------------

**描述** 显示当前的所有账户。

**操作**

.. code:: bash

   > personal.listAccounts

**输出结果**

.. code:: console

   ["0x4bf2e487a1f57d26d57ad9b9821a827e45b9e8e6", "0x7dc33e8ea643ae40459eec74a3879a7018328160", "0x6d5c88df6960fb62a24f816774745ff66c75fee3", "0x21baa00b024172344b029feb72839607875b990d"]

2.2.5. listWallets
--------------------------

**描述** 返回当前的钱包状态

**操作**

.. code:: bash

   > personal.listWallets

**输出结果**

.. code:: js

   [{
       accounts: [{
           address: "0x4bf2e487a1f57d26d57ad9b9821a827e45b9e8e6",
           url: "keystore:///home/wxuser/go/src/PlatONE_Network/PlatONE-Go/release/linux/data/node-0/keystore/UTC--2021-03-09T06-38-24.634693807Z--4bf2e487a1f57d26d57ad9b9821a827e45b9e8e6"
       }],
       status: "Unlocked",
       url: "keystore:///home/wxuser/go/src/PlatONE_Network/PlatONE-Go/release/linux/data/node-0/keystore/UTC--2021-03-09T06-38-24.634693807Z--4bf2e487a1f57d26d57ad9b9821a827e45b9e8e6"
   }, ],
       accounts: [{
           address: "0x21baa00b024172344b029feb72839607875b990d",
           url: "keystore:///home/wxuser/go/src/PlatONE_Network/PlatONE-Go/release/linux/data/node-0/keystore/UTC--2021-03-09T08-40-58.891340059Z--21baa00b024172344b029feb72839607875b990d"
       }],
       status: "Locked",
       url: "keystore:///home/wxuser/go/src/PlatONE_Network/PlatONE-Go/release/linux/data/node-0/keystore/UTC--2021-03-09T08-40-58.891340059Z--21baa00b024172344b029feb72839607875b990d"
   }]

2.2.6. sign
-------------------

**描述** 使用账户给某个消息签名

.. code:: bash

   web3.eth.personal.sign(dataToSign, address, password [, callback])

**参数**

- ``String`` - 要签名的数据。 如果是字符串会使用 `web3.utils.utf8ToHex <https://learnblockchain.cn/docs/web3.js/web3-utils.html#utils-utf8tohex>`__ 将其转换为 16 进制。
- ``String`` - 用来签名的账户地址。
- ``String`` - 用来签名的账户密码。
- ``Function`` - (可选)回调函数，其第一个参数为错误对象，第二个参数为签名结果。

**返回值**

- ``Promise<string>`` - 签名字符串。

操作:

.. code:: bash

   > personal.sign("0x",eth.accounts[0],"0")

**输出结果**

.. code:: console

   "0x2946ee06feb7a9913af2bc95ba9d6882e25bff547111bd201463752ce20eceda22cfe72f8e4b69030f2470c8ab27dd4eed34a20d941309d751019c4099f70ec51b"

2.2.7. ecRecover
--------------------------

**描述** 恢复数据签名帐户。

.. code:: bash

   eth.personal.ecRecover(dataThatWasSigned, signature [, callback])

**参数**

- ``String`` - 被签名的数据。 如果是字符串会使用 `web3.utils.utf8ToHex <https://learnblockchain.cn/docs/web3.js/web3-utils.html#utils-utf8tohex>`__ 将其转换为 16 进制。
- ``String`` - 签名。
- ``Function`` - (可选)回调函数，其第一个参数为错误对象，第二个参数为签名结果。

**返回值**

- ``Promise<string>`` - 签名账户。

**操作**

.. code:: bash

   > personal.ecRecover("0x","0xb8ba89dfe28d49ca8448120e4f779fbd1b9bfbf0b8bebdfd58a3712346291aab550f8eeafb3c906e046ac0fc72c569f2f18ba2a2868ed56e92feb 47d602fa5261b")

**输出结果**

.. code:: console

   "0x4bf2e487a1f57d26d57ad9b9821a827e45b9e8e6"

2.2.8. signTransaction
-----------------------------

**描述** 对交易进行签名，账户必须先解锁。

.. code:: bash

    web3.eth.personal.signTransaction(transaction, password [, callback])

**参数**

- ``Object`` - 要签名的交易数据，更多详情请看 `web3.eth.sendTransaction() <https://learnblockchain.cn/docs/web3.js/web3-eth.html#eth-sendtransaction>`__ 。
- ``String`` - 用来签名交易的 ``from`` 账户密码。
- ``Function`` - (可选)回调函数，其第一个参数为错误对象，第二个参数为签名结果。

**返回值**

- ``Promise<Object>`` - RLP 编码的交易对象，其 ``raw`` 属性可以用来通过 `web3.eth.sendSignedTransaction <https://learnblockchain.cn/docs/web3.js/web3-eth.html#eth-sendsignedtransaction>`__ 来发送交易。

**操作**

.. code:: bash

   > personal.signTransaction({from:eth.accounts[0],gasPrice:'0',to:eth.accounts[1],gas:'2100',value:0,nonce:'0'},"0")

**输出结果**

.. code:: js

   {
     raw: "0xf8628080820834947dc33e8ea643ae40459eec74a3879a701832816080808082027ba058516e83733e92cdfb8dba6c9353ce43e8df43170728e50d099993f56afe29e5a0151a494a4807f972c28de1bfd6b0578fde01615a86899d75d2fa2017508f8e1c",
     tx: {
       gas: "0x834",
       gasPrice: "0x0",
       hash: "0x27c49a8146b4a652fd90e0d441554799931e4d7c1278dfd2187f90ebe8f9da6c",
       input: "0x",
       nonce: "0x0",
       r: "0x58516e83733e92cdfb8dba6c9353ce43e8df43170728e50d099993f56afe29e5",
       s: "0x151a494a4807f972c28de1bfd6b0578fde01615a86899d75d2fa2017508f8e1c",
       to: "0x7dc33e8ea643ae40459eec74a3879a7018328160",
       txType: "0x0",
       v: "0x27b",
       value: "0x0"
     }
   }

2.2.9. sendTransaction
---------------------------

**描述** 通过账户管理 API 来发送交易。

.. code:: bash

   web3.eth.personal.sendTransaction(transactionOptions, password [, callback])

**参数**

- ``Object`` - 交易对象属性。
- ``String`` - 当前帐户的密码。
- ``Function`` - (可选)回调函数，其第一个参数为错误对象，第二个参数为签名结果。

**返回值**

- ``Promise<string>`` - 交易哈希。

**操作**

.. code:: bash

   > personal.sendTransaction({from:eth.accounts[0],gasPrice:'0',to:eth.accounts[1],gas:'2100',value:0,nonce:'0'},"0")

输出结果:

.. code:: console

   "0x27c49a8146b4a652fd90e0d441554799931e4d7c1278dfd2187f90ebe8f9da6c"

2.3. txpool
^^^^^^^^^^^^^^^^^^^

2.3.1. txpool.inspect
---------------------------

**描述** 返回当前待处理以包含在下一个块中的所有交易的文本摘要，以及计划在将来执行的那些交易的文本摘要。

.. code:: bash

   eth.personal.sendTransaction(transactionOptions, password [, callback])

**操作**

.. code:: bash

   txpool.inspect

**输出结果**

.. code:: js

   {
       'pending': {
           '0x26588a9301b0428d95e6fc3a5024fce8bec12d51': {
             31813: ["0x3375ee30428b2a71c428afa5e89e427905f95f7e: 0 wei + 500000 × 20000000000 gas"]
           },
           '0x2a65aca4d5fc5b5c859090a6c34d164135398226': {
             563662: ["0x958c1fa64b34db746925c6f8a3dd81128e40355e: 1051546810000000000 wei + 90000 × 20000000000 gas"],
             563663: ["0x77517b1491a0299a44d668473411676f94e97e34: 1051190740000000000 wei + 90000 × 20000000000 gas"],
             563664: ["0x3e2a7fe169c8f8eee251bb00d9fb6d304ce07d3a: 1050828950000000000 wei + 90000 × 20000000000 gas"],
             563665: ["0xaf6c4695da477f8c663ea2d8b768ad82cb6a8522: 1050544770000000000 wei + 90000 × 20000000000 gas"],
             563666: ["0x139b148094c50f4d20b01caf21b85edb711574db: 1048598530000000000 wei + 90000 × 20000000000 gas"],
             563667: ["0x48b3bd66770b0d1eecefce090dafee36257538ae: 1048367260000000000 wei + 90000 × 20000000000 gas"],
             563668: ["0x468569500925d53e06dd0993014ad166fd7dd381: 1048126690000000000 wei + 90000 × 20000000000 gas"],
             563669: ["0x3dcb4c90477a4b8ff7190b79b524773cbe3be661: 1047965690000000000 wei + 90000 × 20000000000 gas"],
             563670: ["0x6dfef5bc94b031407ffe71ae8076ca0fbf190963: 1047859050000000000 wei + 90000 × 20000000000 gas"]
           },
           '0x9174e688d7de157c5c0583df424eaab2676ac162': {
             3: ["0xbb9bc244d798123fde783fcc1c72d3bb8c189413: 30000000000000000000 wei + 85000 × 21000000000 gas"]
           },
           '0xb18f9d01323e150096650ab989cfecd39d757aec': {
             777: ["0xcd79c72690750f079ae6ab6ccd7e7aedc03c7720: 0 wei + 1000000 × 20000000000 gas"]
           },
           '0xb2916c870cf66967b6510b76c07e9d13a5d23514': {
             2: ["0x576f25199d60982a8f31a8dff4da8acb982e6aba: 26000000000000000000 wei + 90000 × 20000000000 gas"]
           },
           '0xbc0ca4f217e052753614d6b019948824d0d8688b': {
             0: ["0x2910543af39aba0cd09dbb2d50200b3e800a63d2: 1000000000000000000 wei + 50000 × 1171602790622 gas"]
           },
           '0xea674fdde714fd979de3edf0f56aa9716b898ec8': {
             70148: ["0xe39c55ead9f997f7fa20ebe40fb4649943d7db66: 1000767667434026200 wei + 90000 × 20000000000 gas"]
           }
         },
         'queued': {
           '0x0f6000de1578619320aba5e392706b131fb1de6f': {
             6: ["0x8383534d0bcd0186d326c993031311c0ac0d9b2d: 9000000000000000000 wei + 21000 × 20000000000 gas"]
           },
           '0x5b30608c678e1ac464a8994c3b33e5cdf3497112': {
             6: ["0x9773547e27f8303c87089dc42d9288aa2b9d8f06: 50000000000000000000 wei + 90000 × 50000000000 gas"]
           },
           '0x976a3fc5d6f7d259ebfb4cc2ae75115475e9867c': {
             3: ["0x346fb27de7e7370008f5da379f74dd49f5f2f80f: 140000000000000000 wei + 90000 × 20000000000 gas"]
           },
           '0x9b11bf0459b0c4b2f87f8cebca4cfc26f294b63a': {
             2: ["0x24a461f25ee6a318bdef7f33de634a67bb67ac9d: 17000000000000000000 wei + 90000 × 50000000000 gas"],
             6: ["0x6368f3f8c2b42435d6c136757382e4a59436a681: 17990000000000000000 wei + 90000 × 20000000000 gas", "0x8db7b4e0ecb095fbd01dffa62010801296a9ac78: 16998950000000000000 wei + 90000 × 20000000000 gas"],
             7: ["0x6368f3f8c2b42435d6c136757382e4a59436a681: 17900000000000000000 wei + 90000 × 20000000000 gas"]
           }
         }
   }

2.3.2. status
----------------------

**描述** 返回当前待处理以包含在下一个块中的所有交易的文本摘要，以及计划在将来执行的那些交易的文本摘要。

**操作**

.. code:: bash

   > txpool.status

**输出结果**

.. code:: js

   {
       pending: 10,
       queued: 7,
   }

2.3.3. content
------------------------

**描述** 返回待处理或排队的所有交易的确切详细信息。

**操作**

.. code:: bash

   > txpool.content

**输出结果**

.. code:: js

   {
     'pending': {
       '0x0216d5032f356960cd3749c31ab34eeff21b3395': {
         806: [{
           'blockHash': "0x0000000000000000000000000000000000000000000000000000000000000000",
           'blockNumber': None,
           'from': "0x0216d5032f356960cd3749c31ab34eeff21b3395",
           'gas': "0x5208",
           'gasPrice': "0xba43b7400",
           'hash': "0xaf953a2d01f55cfe080c0c94150a60105e8ac3d51153058a1f03dd239dd08586",
           'input': "0x",
           'nonce': "0x326",
           'to': "0x7f69a91a3cf4be60020fb58b893b7cbb65376db8",
           'transactionIndex': None,
           'value': "0x19a99f0cf456000"
         }]
       },
       '0x24d407e5a0b506e1cb2fae163100b5de01f5193c': {
         34: [{
           'blockHash': "0x0000000000000000000000000000000000000000000000000000000000000000",
           'blockNumber': None,
           'from': "0x24d407e5a0b506e1cb2fae163100b5de01f5193c",
           'gas': "0x44c72",
           'gasPrice': "0x4a817c800",
           'hash': "0xb5b8b853af32226755a65ba0602f7ed0e8be2211516153b75e9ed640a7d359fe",
           'input': "0xb61d27f600000000000000000000000024d407e5a0b506e1cb2fae163100b5de01f5193c00000000000000000000000000000000000000000000000053444835ec580000000000000000000000000000000000000000000000000000000000000000006000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
           'nonce': "0x22",
           'to': "0x7320785200f74861b69c49e4ab32399a71b34f1a",
           'transactionIndex': None,
           'value': "0x0"
         }]
       }
     },
     'queued': {
       '0x976a3fc5d6f7d259ebfb4cc2ae75115475e9867c': {
         3: [{
           'blockHash': "0x0000000000000000000000000000000000000000000000000000000000000000",
           'blockNumber': None,
           'from': "0x976a3fc5d6f7d259ebfb4cc2ae75115475e9867c",
           'gas': "0x15f90",
           'gasPrice': "0x4a817c800",
           'hash': "0x57b30c59fc39a50e1cba90e3099286dfa5aaf60294a629240b5bbec6e2e66576",
           'input': "0x",
           'nonce': "0x3",
           'to': "0x346fb27de7e7370008f5da379f74dd49f5f2f80f",
           'transactionIndex': None,
           'value': "0x1f161421c8e0000"
         }]
       },
         6: [{
           'blockHash': "0x0000000000000000000000000000000000000000000000000000000000000000",
           'blockNumber': None,
           'from': "0x9b11bf0459b0c4b2f87f8cebca4cfc26f294b63a",
           'gas': "0x15f90",
           'gasPrice': "0x4a817c800",
           'hash': "0xbbcd1e45eae3b859203a04be7d6e1d7b03b222ec1d66dfcc8011dd39794b147e",
           'input': "0x",
           'nonce': "0x6",
           'to': "0x6368f3f8c2b42435d6c136757382e4a59436a681",
           'transactionIndex': None,
           'value': "0xf9a951af55470000"
         }, {
           'blockHash': "0x0000000000000000000000000000000000000000000000000000000000000000",
           'blockNumber': None,
           'from': "0x9b11bf0459b0c4b2f87f8cebca4cfc26f294b63a",
           'gas': "0x15f90",
           'gasPrice': "0x4a817c800",
           'hash': "0x60803251d43f072904dc3a2d6a084701cd35b4985790baaf8a8f76696041b272",
           'input': "0x",
           'nonce': "0x6",
           'to': "0x8db7b4e0ecb095fbd01dffa62010801296a9ac78",
           'transactionIndex': None,
           'value': "0xebe866f5f0a06000"
         }],
       }
     }
   }

2.4. net
^^^^^^^^^^^^^^^^

2.4.1. listening
------------------------

**描述** 表示当前连接的节点，是否正在\ ``listen``\ 网络连接与否。\ ``listen``\ 可以理解为接收。

- 同步方式:

.. code:: bash

   web3.net.listening

- 异步方式:

.. code:: bash

   web3.net.getListener(callback(error, result){ … })

**返回值**

- ``Boolean`` - ``true``\ 表示连接上的节点正在\ ``listen``\ 网络请求，否则返回\ ``false``\ 。

**操作**

.. code:: bash

   > net.listening

**输出结果**

.. code:: console

   true

2.4.2. peerCount
>>>>>>>>>>>>>>>>>>>>>

**描述** 返回连接节点已连上的其它以太坊节点的数量。

- 同步方式:

.. code:: bash

   web3.net.peerCount

- 异步方式:

.. code:: bash

   web3.net.getPeerCount(callback(error, result){ … })

**返回值**

- ``Number`` - 连接节点连上的其它以太坊节点的数量

**操作**

.. code:: bash

   > net.peerCount

**输出结果**

.. code:: console

   0

2.5. istanbul
^^^^^^^^^^^^^^^^^^^

2.5.1. getValidators
----------------------------

**描述** 返回validator的列表

**操作**

.. code:: bash

   > istanbul.getValidators()

**输出结果**

.. code:: console

   ["0x18445ae70a14eb50d169475d3a31fa5d6b7b99f6", "0x37b9e0b482085ac9488117841218b23275a99636", "0x6e3bbe292e924971c7d938c5867728b326031b88", "0xa202006365c27422e665796577472a22f323ef8d"]

2.5.2. getCandidates
----------------------------

**描述** 返回Candidates的列表

**操作**

.. code:: bash

   > istanbul.getCandidates()

**输出结果**

.. code:: console

   ["0x18445ae70a14eb50d169475d3a31fa5d6b7b99f6", "0x37b9e0b482085ac9488117841218b23275a99636", "0x6e3bbe292e924971c7d938c5867728b326031b88", "0xa202006365c27422e665796577472a22f323ef8d"]




