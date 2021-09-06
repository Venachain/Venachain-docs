==========
编译与部署
==========

PlatONE详细部署步骤可简要分为如下3步：

1) 源码下载与编译;

2) 初始化节点和创世区块，并启动节点;

3) 部署系统合约。

本文将详细介绍如何在单机和多机上部署PlatONE。

.. note:: 下文中有些步骤提供了两种执行方法，第一种是使用脚本一键完成，第二种则需要手动创建文件或使用命令行来完成。虽然第二种方法操作更为复杂，但也更为灵活，便于开发者按需构建理想的联盟链网络。

1. 准备工作
===========

1.1. gcc版本
^^^^^^^^^^^^

切换gcc版本到 7.3.1，建议直接设置在环境变量中。

.. code:: bash

   vi ~/.bashrc
   source scl_source enable devtoolset-7

使用vi打开文件，并在文件最后一行加上source scl_source enable
devtoolset-7，保存并退出即可。

1.2. git相关
^^^^^^^^^^^^

1) 命令行输入以下命令, 或者设置为环境变量。

   .. code:: bash

      REPO_ADDR_HTTP="https://git-c.i.wxblockchain.com/PlatONE/src/node/PlatONE-Go.git"
      REPO_ADDR_GIT="git@git-c.i.wxblockchain.com:PlatONE/src/node/PlatONE-Go.git"


2) 关闭SSL检查：

   -  git clone 时提示Peer’s certificate issuer has been marked as not trusted by the user;

   - 解决方案：在/etc/profile文件的最后一行加入\ ``export GIT_SSL_NO_VERIFY=1``\ ，再\ ``source``\ 一下即可

   .. code:: bash

      sudo vi /etc/profile
      …
      export GIT_SSL_NO_VERIFY=1
      source /etc/profile

1.3. 源码下载与编译
^^^^^^^^^^^^^^^^^^^


**下载源码**

.. code:: bash

	git clone --recursive https://github.com/PlatONEnterprise/PlatONE-Go.git
	export WORKSPACE=${PWD}/PlatONE-Go/release/linux

**编译**：

.. code:: bash

   cd PlatONE-Go
   make all

1.4 清理环境
^^^^^^^^^^^^

.. code:: bash

   cd ${WORKSPACE}/scripts/
   ./platonectl.sh clear -a

2. 单机部署(脚本方式)
============================

2.1. 初始化节点和创世区块
^^^^^^^^^^^^^^^^^^^^^^^^^^

2.1.1. 创建genesis.json文件
----------------------------

当您启动区块链时，首先需要创建一个genesis.json文件，节点通过genesis.json文件来生成创世区块。

执行下面指令一键生成genesis.json:

.. code:: bash

   cd ${WORKSPACE}/scripts
   ./platonectl.sh setupgen -n ${node_id} --ip ${ip} --p2p_port ${p2p_port} --interpreter ${interpreter} --auto true

各个参数的意义如下所示：

.. code:: bash

   --nodeid, -n      node id (default: 0)
   --ip              node ip (default: 127.0.0.1)
   --p2p_port        node p2p_port (default: 16791)
   --interpreter, -i evm， wasm or all （default: wasm）

上面的命令，首先会在\ ``{WORKSPACE}/data/node-${node_id}``\ 目录下，生成节点的公私钥、IP端口等信息。 然后在\ ``{WORKSPACE}/conf``\ 目录下生成一个\ ``genesis.json``\ 文件。

.. code:: bash

   $ ls  ${WORKSPACE}/data/node-${node_id}
   node.address node.ip node.p2p_port node.prikey node.pubkey

.. code:: bash

   $ ls  ${WORKSPACE}/conf
   genesis.json contracts ...

2.1.2. 初始化节点和创世区块
-----------------------------

执行如下命令，会根据genesis.json文件，在数据目录下产生创世区块，并配置节点的RPC和websocket端口信息。

.. code:: bash

   cd ${WORKSPACE}/scripts/
   ./platonectl.sh init -n ${node_id} --ip ${ip} --rpc_port ${rpc_port} --p2p_port ${p2p_port} --ws_port ${ws_port} --auto "true"

各个参数的意义如下所示：

.. code:: bash

   --nodeid, -n      node id (default: 0)
   --ip              node ip (default: 127.0.0.1)
   --p2p_port        node p2p_port (default: 16791)
   --rpc_port        node rpc_port (default: 6791)
   --ws_port         node websoket port (default: 26791)

2.1.3. 启动节点
-----------------

默认启动命令：

.. code:: bash

   cd ${WORKSPACE}/scripts/
   ./platonectl.sh start -n ${node_id}

节点启动后，可以通过节点运行日志跟踪节点的运行状态。

.. code:: bash

   节点数据： ${WORKSPACE}/data/node-${node_id}/
   节点运行日志：  ${WORKSPACE}/data/node-${node_id}/logs/platone_log/

在启动节点时, 可以指定日志文件夹的路径,
指定platone启动时额外的命令行参数等. (注意: 路径连接符’/’ 需要进行转义，参数option的值, 必须加上引号)

-  **日志位置**：生产环境需要指定日志存放路径

   -  ``--logdir, -d log dir (default: ../data/node_${node_id}/logs/)``

-  **日志等级**：通过\ ``-e`` 指定了\ **额外参数**\ ，通过\ ``-e '--verbosity 2'``\ 可以用来指定日志等级为2。

-  通过\ ``--bootnodes``\ 指定区块链入口节点，节点启动时会主动连接指定为bootnodes的节点，以接入区块链网络。

如下命令指定了log日志目录、日志级别以及启动时要连接的节点：

.. code:: bash

   ./platonectl.sh start -n x -d "\/opt\/logs"  -e "--verbosity 2 --debug --bootnodes enode://${pubkey}@${ip}:${p2p_port}"

日志文件夹中包含wasm执行的日志与platone运行的日志. 随时间推移,
日志文件会越积越多, 建议进行挂载, 或者进行定期删除等操作。

2.2. 节点加入区块链
^^^^^^^^^^^^^^^^^^^^^^^^

2.2.1. 创建管理员账号并部署系统合约
--------------------------------------

.. code:: bash

   ./platonectl.sh deploysys -n ${node_id}

本步骤会首先在节点侧创建一个账号，需要手动输入密码，该账号即为链的超级管理员。然后，使用该账号向链部署系统合约。

如果创建账号时，跳过手动输入密码的过程，可以加上\ ``--auto true``\ ，这样就可以使用默认密码\ ``0``\ 创建账号。

2.2.2. 添加节点至区块链
---------------------------

.. code:: bash

   ./platonectl.sh addnode -n ${node_id}

2.2.3. 更新节点为共识节点
-----------------------------

.. code:: bash

   ./platonectl.sh updatesys -n ${node_id}

至此，通过脚本方式实现单机部署完成

3. 单机部署(命令行方式)
==============================

3.1. 初始化节点和创世区块
^^^^^^^^^^^^^^^^^^^^^^^^^^

3.1.1. 创建genesis.json文件
-----------------------------

1) 配置环境变量, 进入PlatONE-Go/build/bin

.. code:: bash

      export PATH=${PATH}:${PWD}

2) 生成新的\ **用户账户**\ ，需要用户设置密码用于解锁用户账户，在示例中密码设为“0”。

.. code:: bash

      ./platone --datadir ./data account new

.. code:: console

      INFO [01-09|17:25:14.269] Maximum peer count                       ETH=50 LES=0 total=50
      Your new account is locked with a password. Please give a password. Do not forget this password.
      Passphrase:
      Repeat passphrase:
      Address: {60208c048e7eb8e38b0fac40406b819ce95aa7af}

3) 查看账户

.. code:: bash

      ll data/keystore/

.. code:: console

      -rw------- 1 wxuser wxuser 491 Jan  9 17:25 UTC--2019-01-09T09-25-28.487164507Z--60208c048e7eb8e38b0fac40406b819ce95aa7af

4) 生成\ **节点**\ 密钥对，需要进入目录PlatONE-Go/build/bin

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

5) 将节点私钥存储在
   ./data/platone/nodekey中，私钥是上一步生成的PrivateKey。

.. code:: bash

      mkdir -p ./data/platone
      echo "b428720a89d003a1b393c642e6e32713dd6a6f82fe4098b9e3a90eb38e23b6bb" > ./data/platone/nodekey
      cat ./data/platone/nodekey
      sudo updatedb
      locate nodekey

.. code:: console

      /home/wxuser/work/golang/src/github.com/PlatONEnetwork/PlatONE-Go/build/bin/data/platone/nodekey

6) 配置初始化文件

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
- ``VALIDATOR``  替换为"enode://pubkey@ip:port"。pubkey是4.1.1生成的节点的公钥，ip和p2p_port可以根据情况自定义。
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


3.1.2. 初始化节点和创世区块
----------------------------

在\ ``PlatONE-Go/build/bin``\ 目录下执行下面指令初始化创世区块：

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

3.1.3. 启动节点
------------------

1) 在\ ``PlatONE-Go/build/bin``\ 目录下执行下面指令：

.. code:: bash

   ./platone --identity "platon" --datadir ./data --port 16789 --rpcaddr 0.0.0.0 --rpcport 6789 --rpcapi "db,eth,net,web3,admin,personal" --rpc --nodiscover --nodekey "./data/platone/nodekey" --verbosity 4 --wasmlog ./wasm.log --bootnodes "enode://68bb049008c7226de3188b6376127354507e1b1e553a2a8b988bb99b33c4d995e426596fc70ce12f7744100bc69c5f0bce748bc298bf8f0d0de1f5929850b5f4@127.0.0.1:16789"

.. note:: ``--verbosity`` 4 会将wasm log打出来， ``--wasmlog`` 指定将log输出到哪个文件, ``--bootnodes`` 需要指定genesis.json中observeNodes字段中的一个或者多个enode节点

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

启动platone时, 指定\ ``--moduleLogParams``\ 参数可以把platone的log分块写入文件。

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


3.2. 节点加入区块链
^^^^^^^^^^^^^^^^^^^^^^

3.2.1. 生成账户
---------------------

.. code:: bash

   curl --silent --write-out --output /dev/null -H "Content-Type: application/json" --data "{\"jsonrpc\":\"2.0\",\"method\":\"personal_newAccount\",\"params\":[\"${phrase}\"],\"id\":${node_id}}"  http://${IP}:${RPC_PORT}

- ``IP`` 为当前部署节点的ip地址

- ``RPC_PORT`` 是当前部署节点的rpc接口

- ``phrase`` 是要设置的密码

- ``node_id`` 为节点名

会在 ``${workspace}/data/node-${node_id}/keystore`` 下生成 ``UTC*`` 文件，在 ``${workspace}/conf`` 下生成 ``keyfile.json``

3.2.2. 解锁账户
----------------------

.. code:: bash

   curl -H "Content-Type: application/json" --data "{\"jsonrpc\":\"2.0\",\"method\":\"personal_unlockAccount\",\"params\":[\"${ACCOUNT}\",\"${phrase}\",0],\"id\":${node_id}}"  http://${IP}:${RPC_PORT}

- ``IP`` 为当前部署节点的ip地址

- ``RPC_PORT`` 是当前部署节点的rpc接口

- ``ACCOUNT`` 是前一步生成的 ``0x`` 开头的账户地址

- ``phrase`` 是要设置的密码

- ``node_id`` 为节点名

3.2.3. 升级账户权限
-------------------------

- 升级账户为系统管理员

.. code:: bash

   ./platonecli role setSuperAdmin  --keyfile ../conf/keyfile.json --url ${IP}:${RPC_PORT}

- ``IP`` 为当前部署节点的ip地址

- ``RPC_PORT`` 是当前部署节点的rpc接口

- 升级账户为链管理员

.. code:: bash

   ./platonecli role addChainAdmin ${ACCOUNT}  --keyfile ../conf/keyfile.json --url ${IP}:${RPC_PORT}

- ``ACCOUNT`` 是 ``1)`` 中生成的 ``0x`` 开头的账户地址

- ``IP`` 为当前部署节点的ip地址

- ``RPC_PORT`` 是当前部署节点的rpc接口

.. note::

   关于角色权限操作可参考 :ref:`角色权限操作 <cli-role>`

3.2.4. 将节点添加至区块链
---------------------------------

.. code:: bash

   ./platonecli node add "${node_id}" "${pubkey}" "${external_ip}" "${internal_ip}" --keyfile ../conf/keyfile.json --url ${IP}:${RPC_PORT}

- ``node_id`` 是节点名

- ``pubkey`` 是节点公钥

- ``extenal_ip`` 外网地址

- ``internal_ip`` 内网地址

- ``IP`` 为当前部署节点的ip地址

- ``RPC_PORT`` 是当前部署节点的rpc接口

3.2.5. 将节点更新为共识节点
------------------------------------

.. code:: bash

   ./platonecli  node update "${node_id}" --type "consensus" --keyfile ../conf/keyfile.json

- ``node_name`` 是节点名

.. note::

   关于节点操作可参考 :ref:`节点操作 <cli-node>`

至此，命令行方式实现单节点部署完成


4. 多机部署（适用于生产环境/多机测试环境）
==========================================

案例: A, B, C, D四台主机 (**各个主机自动时间同步**)

-  A: 172.25.1.13
-  B: 172.25.1.14
-  C: 172.25.1.15
-  D: 172.25.1.16

4.1. 准备工作
^^^^^^^^^^^^^

首先在主机A上，下载源码并编译，参照第1部分。

然后将编译好的PlatONE-Go/release目录，分发到B、C、D主机。

.. code:: bash

   scp -r PlatONE-Go/release user@172.25.1.14:~/
   scp -r PlatONE-Go/release user@172.25.1.15:~/
   scp -r PlatONE-Go/release user@172.25.1.16:~/

4.2. 在A主机搭建单节点区块链
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

参照 第 ``2.`` 节 或第 ``3.`` 节，在节点A上搭建单节点区块链，然后将genesis.json文件广播出来给其他节点，放置于PlatONE-Go/release/linux/conf目录下。

.. code:: bash

   scp -r genesis.json user@172.25.1.14:~/PlatONE-Go/release/linux/conf
   scp -r genesis.json user@172.25.1.15:~/PlatONE-Go/release/linux/conf
   scp -r genesis.json user@172.25.1.16:~/PlatONE-Go/release/linux/conf

4.3. 在B、C、D生成创世区块及节点信息
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

以B为例：

.. code:: bash

   cd  ~/PlatONE-Go/release/linux/scripts
   ./platonectl.sh init -n 1 --ip 172.25.1.14 --rpc_port 6791 --p2p_port 16791 --ws_port 26791 --auto true

此步骤会根据genesis.json文件生成创世区块，以及节点的连接信息（IP端口、节点密钥）

将节点信息发送至Ａ节点管理员，以便于管理员将新节点加入区块链网络。

节点信息包括节点IP、节点p2p端口、RPC端口和节点公钥，需要将如下四个文件发送至A主机的相应目录。(若A主机不存在data/node-1目录，则创建该目录，以存放节点信息)

.. code:: bash

   # node.ip, node.p2p_port, node.rpc_port, node.pubkey
   # --> user@172.25.1.14:~/PlatONE-Go/release/linux/data/node-1
   scp node.ip user@172.25.1.14:~/PlatONE-Go/release/linux/data/node-1
   scp node.p2p_port user@172.25.1.14:~/PlatONE-Go/release/linux/data/node-1
   scp node.rpc_port user@172.25.1.14:~/PlatONE-Go/release/linux/data/node-1
   scp node.pubkey user@172.25.1.14:~/PlatONE-Go/release/linux/data/node-1

4.4. B、C、D主机启动节点
^^^^^^^^^^^^^^^^^^^^^^^^

以B节点为例

.. code:: bash

   ./platonectl.sh start -n 1

B节点启动后会主动连接A节点，加入网络，成为观察者节点。

4.5. A主机管理员添加B、C、D节点至系统合约
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

以添加B节点为例：

此时A主机的data/node-1目录已经有了B节点的信息（IP、p2p端口、rpc端口和公钥）

将B主机上的节点加入到当前区块链

.. code:: bash

   ./platonectl.sh addnode -n 1

本步骤会在系统合约中写入了B节点信息，B节点成为观察者节点（可以同步交易及数据，但是不参与共识出块）

4.6. 将B、C、D升级为共识节点
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

根据业务需求，可以将观察者节点升级为共识节点。

以添加B节点为例，由A节点的管理员操作如下命令，即可将B节点升级为共识节点：

.. code:: bash

   ./platonectl.sh updatesys -n 1

5. 重新初始化platone节点
========================

确保platone进程已经被杀死，再删除data目录。

.. code:: bash

   cd build/bin
   rm -rf data/platone

然后可以再重新初始化。


6. 备份与还原
=============

该功能支持节点未启动，以及chaindb中数据损坏的场景下，通过线下传递区块数据的方式，将某节点落后的区块数据补齐

6.1. 备份
^^^^^^^^^

通过export功能，将某节点指定范围内的经过RLP编码后的区块数据导出到某个文件中

.. code:: bash

   ./platone --datadir <待导出节点的chaindata路径> export <输出文件名> <导出区块高度下界> <导出区块高度上界>

示例

.. code:: bash

   ./platone export --datadir ../data/node-0/  block-0-14.data 0 14

6.2. 还原
^^^^^^^^^

6.2.1. 清理节点
---------------

清掉四个节点的数据目录，并根据已有的genesis初始化链

.. code:: bash

   rm -rf  ../data/node-*/platone/*

.. code:: console

   ./platone init --datadir ../data/node-0 ../conf/genesis.json
   ./platone init --datadir ../data/node-1 ../conf/genesis.json
   ./platone init --datadir ../data/node-2 ../conf/genesis.json
   ./platone init --datadir ../data/node-3 ../conf/genesis.json

6.2.2. 导入区块数据
-------------------

通过import功能，将导出的区块数据导入指定节点

.. code:: bash

   ./platone --datadir <待导入节点的chaindata路径> import <区块文件名>

示例

.. code:: bash

   # 给节点0导入数据
   ./platone import --datadir ../data/node-0 block-0-14.data
   # 然后启动节点0
   cd ../scripts
   ./platonectl.sh start -n 0
   # 此时观察log会发现节点0的区块高度已经成为14了，其他节点可以启动，然后跟节点0连接，同步其数据，最终整个区块链高度都是14了

7. 生产日志清理策略参考
=======================

我们模拟了正常交易压力下的日志量：单节点上，24小时产出约为300M大小的日志。

-  假设在500G数据盘的规划下，按照70%的阈值保留，去除链DB数据（建议保留至少100GB），那么可以保留约27个月的数据。

-  但由于交易峰值出现的可能性，建议同时实施空间大小阈值的清理策略，即当日志总量达到500GB*70%-100GB
   =250GB 时，实施对最早的一个月数据的清理。

**总结**：时间维度和空间维度的日志清理策略同时实施。

8. 运行状态检查&错误排查
========================

- 在将链交付给业务前，我们可以从以下维度验证链的运行正确性，包括但不限于以下步骤：

**链运行状态检查**：

链运行日志，观察是否正常出块。（正常出块间隔在1～3秒之间）

**系统合约部署情况检查**：

-  系统合约的部署日志在 wasm_log文件夹中，可以监控日志中是否出现了 \ ``error``\ 关键词，排查合约是否正常部署。

-  通过\ ``./platonectl.sh get`` 命令，确认所有节点已经被记录到了节点管理合约。

**监控链运行过程**:

- 监控运行过程中是否有出现\ ``error``\ 或者\ ``warning``\ 关键词。（部分和节点瞬时联通性相关的，如节点互ping心跳包导致的报错信息可忽略。）
