============================
命令行分布部署
============================

1. 初始化节点和创世区块
===========================

1.1. 创建genesis.json文件
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1) 生成 **节点** 密钥对，需要进入目录~/release/linux/bin

.. code:: bash

   ./ethkey genkeypair

.. code:: console

   Address   :  0xD55c2d0dd0e02F71eC06c6FFE8A9b6F11924af93
   PrivateKey:  bc8cf5265898eaf57719de05a1a1e9a02195f08144813f64613065f62535f8d6
   PublicKey :  44e28023831d1abf070f0ade4ed3d840b6e6e1102876038d72bad3434d1c1ebcbee194947a53485be68f81a92945c47e3a534c5f2f598b2f91fb90c508384558

输出说明：

-  ``Address`` : 节点地址。
-  ``PrivateKey`` : 节点私钥。
-  ``PublicKey`` : 节点公钥。

2) 将节点密钥对存储在../data/node-0/中

.. code:: bash

      mkdir -p ../data/node-0/
      echo "D55c2d0dd0e02F71eC06c6FFE8A9b6F11924af93" > ../data/node-0/node.address
      echo "bc8cf5265898eaf57719de05a1a1e9a02195f08144813f64613065f62535f8d6" > ../data/node-0/node.prikey
      echo "44e28023831d1abf070f0ade4ed3d840b6e6e1102876038d72bad3434d1c1ebcbee194947a53485be68f81a92945c47e3a534c5f2f598b2f91fb90c508384558" > ../data/node-0/node.pubkey

3) 配置初始化文件

在 ~/release/linux/conf/ 目录下存在genesis.json的模版 ``genesis.json.istanbul.template`` . 通过修改其中的参数生成 ``genesis.json`` 文件。

.. code:: bash

   cp ../conf/genesis.json.istanbul.template ../conf/genesis.json

模板文件：

.. code:: js

   {
       "config": {
       "chainId": 300,
       "interpreter": "__INTERPRETER__",
       "istanbul": {
          "timeout": 10000,
          "period": 1,
          "policy": 0,
           "firstValidatorNode": __VALIDATOR__
       }
     },
     "timestamp": "TIMESTAMP",
     "extraData": "0x00",
     "alloc": {
       "0xDEFAULT-ACCOUNT": {
         "balance": "100000000000000000000"
       }
     }
   }

其中:

- ``INTERPRETER`` = "all"。
- ``VALIDATOR``  替换为"enode://pubkey@ip:port"。pubkey是 ``1.1.`` 的 ``4）`` 中生成的节点的公钥，ip和p2p_port可以根据情况自定义。
- ``TIMESTAMP`` 替换为当前的时间戳，可用 `echo 'date +%s'` 查看。
- ``0xDEFAULT-ACCOUNT`` 替换为./ethkey genkeypair节点的地址。

替换后生成的文件：

.. code:: json

   {
       "config": {
       "chainId": 300,
       "interpreter": "all",
       "istanbul": {
          "timeout": 10000,
          "period": 1,
          "policy": 0,
          "firstValidatorNode": "enode://292333f7cf4810ccc09886c417425e29e0a3ede16bc0991715439df99f72ea5d503cbdacef77fad8bc35378cee247c0100920ac96f53889e90ece4775b775534@127.0.0.1:16791"
       }
  },
     "timestamp": "1624867380",
     "extraData": "0x00",
     "alloc": {
       "0xB025640054F21C6fb42F45fde3d90Eb7403bA8Eb": {
         "balance": "100000000000000000000"
       }
     }
   }


1.2. 初始化节点和创世区块
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

在 ``~/release/linux/bin`` 目录下执行下面指令初始化创世区块：

.. code:: console

   ./venachain --datadir ../data/node-0 init ../conf/genesis.json

结果如下：

.. code:: console

   INFO [12-23|16:36:22.113] Maximum peer count                       ETH=50 LES=0 total=50 RoutineID=1
   INFO [12-23|16:36:22.114] Allocated cache and file handles         database=/home/wujingwen/workspace/go/src/Venachain/release/linux/data/node-0/venachain/chaindata cache=16 handles=16 RoutineID=1
   INFO [12-23|16:36:22.381] Persisted trie from memory database      nodes=12 size=2.27kB time=158.5µs gcnodes=0 gcsize=0.00B gctime=0s livenodes=1 livesize=0.00B RoutineID=1
   INFO [12-23|16:36:22.382] Successfully wrote genesis state         database=chaindata                                                                                hash=f0d242…d4183b RoutineID=1
   INFO [12-23|16:36:22.382] Allocated cache and file handles         database=/home/wujingwen/workspace/go/src/Venachain/release/linux/data/node-0/venachain/lightchaindata cache=16 handles=16 RoutineID=1
   INFO [12-23|16:36:22.573] Persisted trie from memory database      nodes=12 size=2.27kB time=219.3µs gcnodes=0 gcsize=0.00B gctime=0s livenodes=1 livesize=0.00B RoutineID=1
   INFO [12-23|16:36:22.574] Successfully wrote genesis state         database=lightchaindata                                                                                hash=f0d242…d4183b RoutineID=1

查看目录：

.. code:: bash

   ll -R ../data/node-0

结果如下：

.. code:: console

   ../data/node-0:
   total 20
   drwx------ 2 root root 4096 Dec 23 16:36 keystore
   -rw-r--r-- 1 root root   41 Dec 23 16:29 node.address
   -rw-r--r-- 1 root root   65 Dec 23 16:29 node.prikey
   -rw-r--r-- 1 root root  129 Dec 23 16:29 node.pubkey
   drwxr-xr-x 4 root root 4096 Dec 23 16:36 venachain

   ../data/node-0/keystore:
   total 0

   ../data/node-0/venachain:
   total 8
   drwxr-xr-x 2 root root 4096 Dec 23 16:36 chaindata
   drwxr-xr-x 2 root root 4096 Dec 23 16:36 lightchaindata

   ../data/node-0/venachain/chaindata:
   total 20
   -rw-r--r-- 1 root root 4239 Dec 23 16:36 000001.log
   -rw-r--r-- 1 root root   16 Dec 23 16:36 CURRENT
   -rw-r--r-- 1 root root    0 Dec 23 16:36 LOCK
   -rw-r--r-- 1 root root  358 Dec 23 16:36 LOG
   -rw-r--r-- 1 root root   54 Dec 23 16:36 MANIFEST-000000

   ../data/node-0/venachain/lightchaindata:
   total 20
   -rw-r--r-- 1 root root 4239 Dec 23 16:36 000001.log
   -rw-r--r-- 1 root root   16 Dec 23 16:36 CURRENT
   -rw-r--r-- 1 root root    0 Dec 23 16:36 LOCK
   -rw-r--r-- 1 root root  358 Dec 23 16:36 LOG
   -rw-r--r-- 1 root root   54 Dec 23 16:36 MANIFEST-000000

2. 启动节点
===============

1) 在 ``~/release/linux/bin`` 目录下执行下面指令：

.. code:: bash

   ./venachain --identity "venachain" --datadir ../data/node-0 --port 16791 --rpcaddr 0.0.0.0 --rpcport 6791 --rpcapi "rpcapi db,eth,net,web3,admin,personal,txpool,istanbul" --rpc --nodiscover --nodekey "../data/node-0/node.prikey" --verbosity 4 --wasmlog ../data/node-0/logs/wasm.log --bootnodes "enode://292333f7cf4810ccc09886c417425e29e0a3ede16bc0991715439df99f72ea5d503cbdacef77fad8bc35378cee247c0100920ac96f53889e90ece4775b775534@127.0.0.1:16791"

.. note:: ``--verbosity 4`` 会将wasm log打出来， ``--wasmlog`` 指定将log输出到哪个文件, ``--bootnodes`` 需要指定genesis.json中firstValidatorNode字段中的一个或者多个enode节点

.. code:: console

   DEBUG[12-23|16:44:02.498] Sanitizing Go's GC trigger               percent=100 RoutineID=1
   INFO [12-23|16:44:02.499] Maximum peer count                       ETH=50 LES=0 total=50 RoutineID=1
   DEBUG[12-23|16:44:02.499] FS scan times                            list=14.3µs set=1.6µs diff=1.1µs RoutineID=1
   INFO [12-23|16:44:02.499] Starting peer-to-peer node               instance=VenaChain/venachain/v1.0.1-stable-98441c98/linux-amd64/go1.16.7 RoutineID=1
   INFO [12-23|16:44:02.499] Allocated cache and file handles         database=/home/wujingwen/workspace/go/src/Venachain/release/linux/data/node-0/venachain/extdb cache=768 handles=1024 RoutineID=1
   INFO [12-23|16:44:02.759] Allocated cache and file handles         database=/home/wujingwen/workspace/go/src/Venachain/release/linux/data/node-0/venachain/chaindata cache=768 handles=1024 RoutineID=1
   INFO [12-23|16:44:02.991] Chain already have been initialized.     RoutineID=1
   INFO [12-23|16:44:02.991] Initialised chain configuration          config="{ChainID: 300 Engine: istanbul}" RoutineID=1
   INFO [12-23|16:44:02.992] Initialising Ethereum protocol           versions=[1] network=1 RoutineID=1
   INFO [12-23|16:44:02.994] Loaded most recent local header          number=0 hash=f0d242…d4183b age=52y8mo5d RoutineID=1
   INFO [12-23|16:44:02.994] Loaded most recent local full block      number=0 hash=f0d242…d4183b age=52y8mo5d RoutineID=1
   INFO [12-23|16:44:02.995] Loaded most recent local fast block      number=0 hash=f0d242…d4183b age=52y8mo5d RoutineID=1
   DEBUG[12-23|16:44:02.997] get CurrentBlock() in chain              RoutineID=1
   DEBUG[12-23|16:44:02.997] reset txpool                             RoutineID=1 oldHash=000000…000000 oldNumber=0 newHash=f0d242…d4183b newNumber=0 RoutineID=1
   INFO [12-23|16:44:02.997] Regenerated local transaction journal    transactions=0 accounts=0 RoutineID=1
   DEBUG[12-23|16:44:02.998] Transaction pool info                    pool="&{config:{Locals:[] NoLocals:false Journal:/home/wujingwen/workspace/go/src/Venachain/release/linux/data/node-0/venachain/transactions.rlp Rejournal:3600000000000 PriceLimit:1 PriceBump:10 AccountSlots:16 GlobalSlots:40960 AccountQueue:64 GlobalQueue:1024 GlobalTxCount:10000 Lifetime:10800000000000} chainconfig:0xc003bac0c0 extDb:0xc0000d06e0 chain:0xc000640460 gasPrice:0xc003badf00 txFeed:{once:{done:0 m:{state:0 sema:0}} sendLock:<nil> removeSub:<nil> sendCases:[] mu:{state:0 sema:0} inbox:[] etype:<nil> closed:false} scope:{mu:{state:0 sema:0} subs:map[] closed:false} chainHeadCh:0xc00010ed80 chainHeadEventCh:0xc00010ed20 chainHeadSub:0xc00069ab40 exitCh:0xc00018efc0 signer:{chainId:0xc003bac1c0 chainIdMul:0xc003badec0} mu:{w:{state:0 sema:0} writerSem:0 readerSem:0 readerCount:0 readerWait:0} currentState:0xc0001fddc0 pendingState:0xc0027272f0 db:0xc0000d0790 currentMaxGas:4712388 locals:0xc00013b300 journal:0xc00007e640 pending:map[] all:0xc003badee0 wg:{noCopy:{} state1:[0 1 0]} txExtBuffer:0xc00010ede0 resetHead:<nil> txch:0xc00018f020 completeCnt:0 pk:0xc0002d18f0}" RoutineID=1
   DEBUG[12-23|16:44:02.998] get CurrentBlock() in chain              RoutineID=92
   INFO [12-23|16:44:02.998] commitDuration in Millisecond            commitDuration=2850 RoutineID=1
   INFO [12-23|16:44:02.999] Starting P2P networking                  RoutineID=1
   DEBUG[12-23|16:44:02.999] Begin consensus for new block            number=1 gasLimit=10000000000 parentHash=f0d242…d4183b parentNumber=0 parentStateRoot=e07971…1cdac1 timestamp=1640249042999 RoutineID=93
   INFO [12-23|16:44:02.999] RLPx listener up                         self="enode://44e28023831d1abf070f0ade4ed3d840b6e6e1102876038d72bad3434d1c1ebcbee194947a53485be68f81a92945c47e3a534c5f2f598b2f91fb90c508384558@[::]:16791?discport=0" RoutineID=100
   INFO [12-23|16:44:02.999] ********** current peers length ********** len=0 RoutineID=102
   INFO [12-23|16:44:03.000] ********** current peers length ********** len=0 RoutineID=102
   INFO [12-23|16:44:03.000] the miner was not running, initialize it RoutineID=1
   INFO [12-23|16:44:03.000] Transaction pool price threshold updated price=1000000000 RoutineID=1

2) venachain 与log相关的启动参数

启动venachain时, 指定 ``--moduleLogParams`` 参数可以把venachain的log分块写入文件。

.. code:: bash

   --moduleLogParams '{"venachain_log": ["/"], "__dir__": ["'${LOG_DIR}'"], "__size__": ["'${LOG_SIZE}'"]}'

参数说明:

-  ``venachain_log``: 指定输出venachain中哪个模块的日志。 如:

   + ``"venachain_log": ["/consensus", "/p2p"]``，则只输出consensus模块和p2p模块中打印的日志。

   +  ``"venachain_log": ["/"]`` 则表示输出所有模块的日志。

-  ``__dir__``: 指定的log输出的目录位置。

-  ``__size__``: 指定log写入文件的分块大小。

随时间推移, 日志文件会越积越多, 建议进行挂载, 或者进行定期删除等操作。

更多的venachain启动参数, 可以执行以下命令, 进行查看。

.. code:: bash

   ./venachain -h


3. 节点加入区块链
=====================

3.1. 生成账户
^^^^^^^^^^^^^^^^^^^ 

.. code:: bash

   curl --silent --write-out --output /dev/null -H "Content-Type: application/json" --data "{\"jsonrpc\":\"2.0\",\"method\":\"personal_newAccount\",\"params\":[\"${phrase}\"],\"id\":1}"  http://${IP}:${RPC_PORT}

   ## 例
   curl --silent --write-out --output /dev/null -H "Content-Type: application/json" --data "{\"jsonrpc\":\"2.0\",\"method\":\"personal_newAccount\",\"params\":[\"0\"],\"id\":1}"  http://127.0.0.1:6791

- ``IP`` 为当前部署节点的ip地址

- ``RPC_PORT`` 是当前部署节点的rpc接口

- ``phrase`` 是要设置的密码

会在 ``~/release/linux/data/node-0/keystore`` 下生成 ``UTC*`` 文件

3.2. 解锁账户

.. code:: bash

   cp ../data/node-0/keystore/UTC* ../conf/keyfile.json

3.3. 解锁账户
^^^^^^^^^^^^^^^^^^

.. code:: bash

   curl -H "Content-Type: application/json" --data "{\"jsonrpc\":\"2.0\",\"method\":\"personal_unlockAccount\",\"params\":[\"${ACCOUNT}\",\"${phrase}\",0],\"id\":1}"  http://${IP}:${RPC_PORT}

   ## 例
   curl -H "Content-Type: application/json" --data "{\"jsonrpc\":\"2.0\",\"method\":\"personal_unlockAccount\",\"params\":[\"0x2b63c4404f74ff8af325afe494c4f0a9b3a2c821\",\"0\",0],\"id\":1}"  http://127.0.0.1:6791

- ``IP`` 为当前部署节点的ip地址

- ``RPC_PORT`` 是当前部署节点的rpc接口

- ``ACCOUNT`` 是 ``UTC*`` 文件中 ``address`` 的value值，加上 ``0x`` 前缀

- ``phrase`` 是要设置的密码

- ``node_id`` 为节点名

3.4. 升级账户权限
^^^^^^^^^^^^^^^^^^^^^^^^^^

- 升级账户为系统管理员

.. code:: bash

   ./venachaincli role setSuperAdmin  --keyfile ../conf/keyfile.json --url {IP}:${RPC_PORT}

   ## 例
   ./venachaincli role setSuperAdmin  --keyfile ../conf/keyfile.json --url 127.0.0.1:6791

- ``IP`` 为firstnode的ip地址

- ``RPC_PORT`` 是firstnode的rpc接口

- 升级账户为链管理员

.. code:: bash

   ./venachaincli role addChainAdmin ${ACCOUNT}  --keyfile ../conf/keyfile.json --url {IP}:${RPC_PORT}

   ## 例
   ./venachaincli role addChainAdmin 0x2b63c4404f74ff8af325afe494c4f0a9b3a2c821  --keyfile ../conf/keyfile.json --url 127.0.0.1:6791

- ``ACCOUNT`` 是 ``3.3.`` 中的相同

- ``IP`` 为firstnode的ip地址

- ``RPC_PORT`` 是firstnode的rpc接口

.. note::

   关于角色权限操作可参考 :ref:`角色权限操作 <cli-role>`

3.4. 将节点添加至区块链
^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: bash

   ./venachaincli node add "${node_id}" "${pubkey}" "${external_ip}" "${internal_ip}" --keyfile ../conf/keyfile.json --url ${IP}:${RPC_PORT}

   ## 例
   ./venachaincli node add "0" "292333f7cf4810ccc09886c417425e29e0a3ede16bc0991715439df99f72ea5d503cbdacef77fad8bc35378cee247c0100920ac96f53889e90ece4775b775534" "127.0.0.1" "127.0.0.1" --keyfile ../conf/keyfile.json --url 127.0.0.1:6791

- ``node_id`` 是节点名

- ``pubkey`` 是节点公钥

- ``extenal_ip`` 外网地址

- ``internal_ip`` 内网地址

- ``IP`` 为当前部署节点的ip地址

- ``RPC_PORT`` 是当前部署节点的rpc接口

3.5. 将节点更新为共识节点
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: bash

   ./venachaincli  node update "${node_id}" --type "consensus" --keyfile ../conf/keyfile.json

   ## 例
   ./venachaincli  node update "0" --type "consensus" --keyfile ../conf/keyfile.json


- ``node_id`` 是节点名

.. note::

   关于节点操作可参考 :ref:`节点操作 <cli-node>`



