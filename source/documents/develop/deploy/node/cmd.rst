============================
命令行分布部署
============================

1. 初始化节点和创世区块
===========================

1.1. 创建genesis.json文件
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1) 生成 **节点** 密钥对，需要进入目录PlatONE-Go/build/bin

.. code:: bash

      ./ethkey genkeypair

.. code:: console

      Address   :  0xC71433b47f1b0053f935AEf64758153B24cE7445
      PrivateKey:  b428720a89d003a1b393c642e6e32713dd6a6f82fe4098b9e3a90eb38e23b6bb
      PublicKey :  68bb049008c7226de3188b6376127354507e1b1e553a2a8b988bb99b33c4d995e426596fc70ce12f7744100bc69c5f0bce748bc298bf8f0d0de1f5929850b5f4

输出说明：

-  ``Address`` : 节点地址。
-  ``PrivateKey`` : 节点私钥。
-  ``PublicKey`` : 节点公钥。

2) 将节点私钥存储在
   ./data/platone/nodekey中，私钥是上一步生成的PrivateKey。

.. code:: bash

      mkdir -p ./data/platone
      echo "b428720a89d003a1b393c642e6e32713dd6a6f82fe4098b9e3a90eb38e23b6bb" > ./data/platone/nodekey
      cat ./data/platone/nodekey
      sudo updatedb
      locate nodekey

.. code:: console

      /home/wxuser/work/golang/src/github.com/PlatONEnetwork/PlatONE-Go/build/bin/data/platone/nodekey

3) 配置初始化文件

在 /PlatONE_Network/PlatONE-Go/release/linux/conf/ 目录下存在genesis.json的模版 ``genesis.json.istanbul.template`` . 通过修改其中的参数生成 ``genesis.json`` 文件。

.. code:: bash

   cp ./PlatONE-Go/release/linux/conf/genesis.json.istanbul.template ./PlatONE-Go/build/bin

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

在 ``PlatONE-Go/build/bin`` 目录下执行下面指令初始化创世区块：

.. code:: console

   $ platone --datadir ./data init genesis.json

结果如下：

.. code:: console

   INFO [01-09|17:31:58.832] Maximum peer count                       ETH=50 LES=0 total=50
   INFO [01-09|17:31:58.833] Allocated cache and file handles         database=/home/wxuser/manual-Platon/build/bin/data/platon/chaindata cache=16 handles=16
   INFO [01-09|17:31:58.839] Writing custom genesis block
   INFO [01-09|17:31:58.840] Persisted trie from memory database      nodes=1 size=150.00B time=34.546µs gcnodes=0 gcsize=0.00B gctime=0s livenodes=1 livesize=0.00B
   INFO [01-09|17:31:58.840] Successfully wrote genesis state         database=chaindata                                                  hash=4fe06b…382a26
   INFO [01-09|17:31:58.840] Allocated cache and file handles         database=/home/wxuser/manual-Platon/build/bin/data/platon/lightchaindata cache=16 handles=16
   INFO [01-09|17:31:58.848] Writing custom genesis block
   INFO [01-09|17:31:58.848] Persisted trie from memory database      nodes=1 size=150.00B time=238.177µs gcnodes=0 gcsize=0.00B gctime=0s livenodes=1 livesize=0.00B
   INFO [01-09|17:31:58.848] Successfully wrote genesis state         database=lightchaindata                                                  hash=4fe06b…382a26

查看目录：

.. code:: bash

   ll -R data/

结果如下：

.. code:: console

   data/:
   total 0
   drwx------ 2 wxuser wxuser 91 Jan  9 17:25 keystore
   drwxr-xr-x 4 wxuser wxuser 45 Jan  9 17:31 platon

   data/keystore:
   total 4
   -rw------- 1 wxuser wxuser 491 Jan  9 17:25 UTC--2019-01-09T09-25-28.487164507Z--60208c048e7eb8e38b0fac40406b819ce95aa7af

   data/platon:
   total 0
   drwxr-xr-x 2 wxuser wxuser 85 Jan  9 17:31 chaindata
   drwxr-xr-x 2 wxuser wxuser 85 Jan  9 17:31 lightchaindata

   data/platon/chaindata:
   total 16
   -rw-r--r-- 1 wxuser wxuser 1802 Jan  9 17:31 000001.log
   -rw-r--r-- 1 wxuser wxuser   16 Jan  9 17:31 CURRENT
   -rw-r--r-- 1 wxuser wxuser    0 Jan  9 17:31 LOCK
   -rw-r--r-- 1 wxuser wxuser  358 Jan  9 17:31 LOG
   -rw-r--r-- 1 wxuser wxuser   54 Jan  9 17:31 MANIFEST-000000

   data/platon/lightchaindata:
   total 16
   -rw-r--r-- 1 wxuser wxuser 1802 Jan  9 17:31 000001.log
   -rw-r--r-- 1 wxuser wxuser   16 Jan  9 17:31 CURRENT
   -rw-r--r-- 1 wxuser wxuser    0 Jan  9 17:31 LOCK
   -rw-r--r-- 1 wxuser wxuser  358 Jan  9 17:31 LOG
   -rw-r--r-- 1 wxuser wxuser   54 Jan  9 17:31 MANIFEST-000000

2. 启动节点
===============

1) 在 ``PlatONE-Go/build/bin`` 目录下执行下面指令：

.. code:: bash

   ./platone --identity "platon" --datadir ./data --port 16789 --rpcaddr 0.0.0.0 --rpcport 6789 --rpcapi "db,eth,net,web3,admin,personal" --rpc --nodiscover --nodekey "./data/platone/nodekey" --verbosity 4 --wasmlog ./wasm.log --bootnodes "enode://68bb049008c7226de3188b6376127354507e1b1e553a2a8b988bb99b33c4d995e426596fc70ce12f7744100bc69c5f0bce748bc298bf8f0d0de1f5929850b5f4@127.0.0.1:16789"

.. note:: ``--verbosity 4`` 会将wasm log打出来， ``--wasmlog`` 指定将log输出到哪个文件, ``--bootnodes`` 需要指定genesis.json中observeNodes字段中的一个或者多个enode节点

.. code:: console

   INFO [01-09|17:42:01.165] Maximum peer count                       ETH=50 LES=0 total=50
   INFO [01-09|17:42:01.166] Starting peer-to-peer node               instance=Geth/node1/v1.8.16-stable-7ee6fe39/linux-amd64/go1.11.4
   INFO [01-09|17:42:01.166] Allocated cache and file handles         database=/home/wxuser/manual-Platon/build/bin/data/platon/chaindata cache=768 handles=512
   INFO [01-09|17:42:01.183] Initialised chain configuration          config="{ChainID: 300 Homestead: 1 DAO: <nil> DAOSupport: false EIP150: 2 EIP155: 3 EIP158: 3 Byzantium: 4 Constantinople: <nil> Engine: &{0 0 0 0 0 [{127.0.0.1 16789 16789 68bb049008c7226de3188b6376127354507e1b1e553a2a8b988bb99b33c4d995e426596fc70ce12f7744100bc69c5f0bce748bc298bf8f0d0de1f5929850b5f4 [149 178 250 27 246 47 49 86 100 108 50 3 199 20 51 180 127 27 0 83 249 53 174 246 71 88 21 59 36 206 116 69] {0 0 <nil>}}] 00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000 <nil>}}"
   INFO [01-09|17:42:01.183] Initialising Ethereum protocol           versions="[63 62]" network=300
   INFO [01-09|17:42:01.184] Loaded most recent local header          number=0 hash=4fe06b…382a26 age=1mo5d6h
   INFO [01-09|17:42:01.184] Loaded most recent local full block      number=0 hash=4fe06b…382a26 age=1mo5d6h
   INFO [01-09|17:42:01.184] Loaded most recent local fast block      number=0 hash=4fe06b…382a26 age=1mo5d6h
   INFO [01-09|17:42:01.184] Read the StateDB instance from the cache map sealHash=bbbae7…30dbfb
   INFO [01-09|17:42:01.184] Loaded local transaction journal         transactions=0 dropped=0
   INFO [01-09|17:42:01.185] Regenerated local transaction journal    transactions=0 accounts=0
   INFO [01-09|17:42:01.185] Loaded local mpc transaction journal     mpc transactions=0 dropped=0
   INFO [01-09|17:42:01.185] Init mpc processor success               osType=linux icepath= httpEndpoint=http://127.0.0.1:6789
   INFO [01-09|17:42:01.185] commitDuration                           commitDuration=950.000
   INFO [01-09|17:42:01.185] Set the block time at the end of the last round of consensus startTimeOfEpoch=1543979656
   INFO [01-09|17:42:01.185] Starting P2P networking
   INFO [01-09|17:42:03.298] UDP listener up                          self=enode://aa18a88c1463c1f1026c6cb0b781027d898d19ed9c11b10ad7a3a9ee2d0c09ab607d9b24bc4580bd816c0194215461cd88bf65955e0d87cf69e0157d464c582b@[::]:16789
   INFO [01-09|17:42:03.299] Transaction pool price threshold updated price=1000000000
   INFO [01-09|17:42:03.300] IPC endpoint opened                      url=/home/wxuser/manual-Platon/build/bin/data/platon.ipc
   INFO [01-09|17:42:03.300] RLPx listener up                         self=enode://aa18a88c1463c1f1026c6cb0b781027d898d19ed9c11b10ad7a3a9ee2d0c09ab607d9b24bc4580bd816c0194215461cd88bf65955e0d87cf69e0157d464c582b@[::]:16789
   INFO [01-09|17:42:03.300] HTTP endpoint opened                     url=http://0.0.0.0:6789                                  cors= vhosts=localhost
   INFO [01-09|17:42:03.300] Transaction pool price threshold updated price=1000000000

2) platone 与log相关的启动参数

启动platone时, 指定 ``--moduleLogParams`` 参数可以把platone的log分块写入文件。

.. code:: bash

   --moduleLogParams '{"platone_log": ["/"], "__dir__": ["../../logs"], "__size__": ["67108864"]}'

参数说明:

-  ``platone_log``: 指定输出platone中哪个模块的日志。 如:

   + ``"platone_log": ["/consensus", "/p2p"]``，则只输出consensus模块和p2p模块中打印的日志。

   +  ``"platone_log": ["/"]`` 则表示输出所有模块的日志。

-  ``__dir__``: 指定的log输出的目录位置。

-  ``__size__``: 指定log写入文件的分块大小。

随时间推移, 日志文件会越积越多, 建议进行挂载, 或者进行定期删除等操作。

更多的platone启动参数, 可以执行以下命令, 进行查看。

.. code:: bash

   platone -h


3. 节点加入区块链
=====================

3.1. 生成账户
^^^^^^^^^^^^^^^^^^^ 

.. code:: bash

   curl --silent --write-out --output /dev/null -H "Content-Type: application/json" --data "{\"jsonrpc\":\"2.0\",\"method\":\"personal_newAccount\",\"params\":[\"${phrase}\"],\"id\":${node_id}}"  http://${IP}:${RPC_PORT}

- ``IP`` 为当前部署节点的ip地址

- ``RPC_PORT`` 是当前部署节点的rpc接口

- ``phrase`` 是要设置的密码

- ``node_id`` 为节点名

会在 ``${workspace}/data/node-${node_id}/keystore`` 下生成 ``UTC*`` 文件，在 ``${workspace}/conf`` 下生成 ``keyfile.json``

3.2. 解锁账户
^^^^^^^^^^^^^^^^^^

.. code:: bash

   curl -H "Content-Type: application/json" --data "{\"jsonrpc\":\"2.0\",\"method\":\"personal_unlockAccount\",\"params\":[\"${ACCOUNT}\",\"${phrase}\",0],\"id\":${node_id}}"  http://${IP}:${RPC_PORT}

- ``IP`` 为当前部署节点的ip地址

- ``RPC_PORT`` 是当前部署节点的rpc接口

- ``ACCOUNT`` 是前一步生成的 ``0x`` 开头的账户地址

- ``phrase`` 是要设置的密码

- ``node_id`` 为节点名

3.3. 升级账户权限
^^^^^^^^^^^^^^^^^^^^^^^^^^

- 升级账户为系统管理员

.. code:: bash

   ./platonecli role setSuperAdmin  --keyfile ../conf/keyfile.json --url ${IP}:${RPC_PORT}

- ``IP`` 为当前部署节点的ip地址

- ``RPC_PORT`` 是当前部署节点的rpc接口

- 升级账户为链管理员

.. code:: bash

   ./platonecli role addChainAdmin ${ACCOUNT}  --keyfile ../conf/keyfile.json --url ${IP}:${RPC_PORT}

- ``ACCOUNT`` 是 ``3.1.`` 中生成的 ``0x`` 开头的账户地址

- ``IP`` 为当前部署节点的ip地址

- ``RPC_PORT`` 是当前部署节点的rpc接口

.. note::

   关于角色权限操作可参考 :ref:`角色权限操作 <cli-role>`

3.4. 将节点添加至区块链
^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: bash

   ./platonecli node add "${node_id}" "${pubkey}" "${external_ip}" "${internal_ip}" --keyfile ../conf/keyfile.json --url ${IP}:${RPC_PORT}

- ``node_id`` 是节点名

- ``pubkey`` 是节点公钥

- ``extenal_ip`` 外网地址

- ``internal_ip`` 内网地址

- ``IP`` 为当前部署节点的ip地址

- ``RPC_PORT`` 是当前部署节点的rpc接口

3.5. 将节点更新为共识节点
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: bash

   ./platonecli  node update "${node_id}" --type "consensus" --keyfile ../conf/keyfile.json

- ``node_name`` 是节点名

.. note::

   关于节点操作可参考 :ref:`节点操作 <cli-node>`



