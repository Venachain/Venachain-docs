==========================
脚本分步部署
==========================

1. 初始化节点和创世区块
=======================

1.1. 创建密钥与创世区块(仅firstnode执行)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- 会在 ``{WORKSPACE}/data/node-${node_id}`` 目录下，生成节点的公私钥、 IP 端口等信息。 
- 会在 ``{WORKSPACE}/conf`` 目录下生成一个 ``genesis.json`` 文件。

**命令**

.. code:: bash

   cd ${WORKSPACE}/scripts
   ./venachainctl.sh setupgen -n ${node_id} --ip ${ip} --p2p_port ${p2p_port} --interpreter ${interpreter} --auto true

   ## 示例
   ./venachainctl.sh setupgen --auto true

**参数**

.. code:: bash

   --nodeid, -n         node id (default: 0)
   --ip                 node ip (default: 127.0.0.1)
   --p2p_port           node p2p_port (default: 16791)
   --interpreter, -i    evm， wasm or all （default: wasm）
   --auto               generate node key automatically

**说明**

- auto参数
   + 如果为true
      - 如果原本已在 ``{WORKSPACE}/data/node-${node_id}`` 下存在公钥、私钥和地址，那么会被覆盖
      - 如果原本已在 ``{WORKSPACE}/conf`` 下存在 ``genesis.json`` ，那么原文件会被覆盖
   + 如果不为true
      1) 询问是否要生成密钥，如果已有密钥，可以选择 ``n``
      2) 如果要生成新密钥且原本已在 ``{WORKSPACE}/data/node-${node_id}`` 下存在公钥、私钥和地址，会询问是否要覆盖
- 文件备份 
   + 被覆盖的密钥会被备份至 ``{WORKSPACE}/data/node-${node_id}/bak``
   + 被覆盖的 ``genesis.json`` 会被备份至 ``{WORKSPACE}/conf/bak``


1.2. 初始化
^^^^^^^^^^^^

.. note:: 部署完 firstnode 后，部署其他节点直接从本步骤开始执行

- 根据 ``genesis.json`` 文件生成创世区块，并配置节点的端口信息

**命令**

.. code:: bash

   cd ${WORKSPACE}/scripts/
   ./venachainctl.sh init -n ${node_id} --ip ${ip} --rpc_port ${rpc_port} --p2p_port ${p2p_port} --ws_port ${ws_port} --auto true

   # 示例
   ./venachainctl.sh init --auto true
   ./venachainctl.sh init -n 3 --ip 127.0.0.1 --rpc_port 6794 --p2p_port 16794 --ws_port 26794 --auto true

**参数**

.. code:: bash

   --nodeid, -n      node id (default: 0)
   --ip              node ip (default: 127.0.0.1)
   --p2p_port        node p2p_port (default: 16791)
   --rpc_port        node rpc_port (default: 6791)
   --ws_port         node websoket port (default: 26791)
   --auto            generate key automatically

**说明**

- auto参数
   + 如果为true
      - 如果原本已在 ``{WORKSPACE}/data/node-${node_id}`` 下存在公钥、私钥和地址，那么会直接读取
   + 如果不为true
      1) 询问是否要生成密钥，如果已有密钥，可以选择 ``n``
      2) 如果要生成新密钥且原本已在 ``{WORKSPACE}/data/node-${node_id}`` 下存在公钥、私钥和地址，会询问是否要覆盖
- 文件备份 
   + 被覆盖的密钥会被备份至 ``{WORKSPACE}/data/node-${node_id}/bak``

2. 启动节点
==============

**命令**

.. code:: bash

   ./venachainctl.sh -n ${node_id} --bootnodes ${bootnodes} --logsize ${logsize} --logdir ${logdir} --extraoptions ${extraoptions} --txcount ${txcount} 

   ## 示例
   ./venachainctl.sh start -n 0
   ./venachainctl.sh start -n 3 --bootnodes "enode://7a7ab8ab54810b84907cc8e445229db1da2080bad0d2f2360f0faa085d6e5fce16fe1fa13955de00503da31b701865275dff22c1ad21824cf33e7e54a4968997@127.0.0.1:16791" --logsize 66666666 --logdir "/opt/logs" --extraoptions "--verbosity 2" --txcount 2000

**参数**

.. code:: bash

   --nodeid, -n            node id , must specified
   --bootnodes, -b         bootnodes (default: read genesis.json)
   --logsize, -s           log size (default: 67108864)
   --logdir, -d            log dir path (default: {WORKSPACE}/data/node-${node_id}/logs)
   --extraoptions, -e      extra options (default: --debug)
   --txcount, -c           max tx count in a block (default: 1000)
   --all, -a               start all nodes

**说明**

-  节点启动后，可以通过节点运行日志跟踪节点的运行状态。

   + 节点数据： ``${WORKSPACE}/data/node-${node_id}/``

   + 节点运行日志： ``${logdir}/venachain_log/``

   + 日志文件夹中包含 wasm 执行的日志与 venachain 运行的日志. 随时间推移， 日志文件会越积越多, 建议进行挂载, 或者进行定期删除等操作。

-  logdir：参数加引号，用绝对路径

   .. code:: bash

      -- logdir "/opt/logs"

-  extraoptions：参数要加引号

   +  verbosity：指定日志等级


3. 节点加入区块链
====================

firstnode加入区块链
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- 本步骤会首先在节点侧创建一个账号，需要手动输入密码，该账号即为链的超级管理员。然后，使用该账号向链部署系统合约。最后，将节点加入区块链并更新为共识节点

**命令**

.. code:: bash

   ./venachainctl.sh deploysys -n ${node_id} --auto true


**参数**

.. code:: bash

   --nodeid, -n      node id , must specified
   --auto            create account automatically with phrase 0

**说明**

- 在 ``{WORKSPACE}/data/node-${node_id}/keystore`` 生成 ``UTC*`` 文件
- 在 ``{WORKSPACE}/conf`` 下会生成 ``keyfile.json``
- auto参数
   + 如果为 true
      会用默认密码 0 创建账号
   + 如果不为 true
      手动输入设置密码

非firstnode加入区块链
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. note:: firstnode 必须已加入区块链

- 将节点加入区块链并更新为共识节点

**命令**

.. code:: bash

   ## 节点加入区块链

   ./venachainctl.sh addnode -n ${node_id}

.. code:: bash

   ## 节点更新为共识节点

   ./venachainctl.sh updatesys -n ${node_id}