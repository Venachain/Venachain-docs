==========
编译与部署
==========

PlatONE详细部署步骤可简要分为如下3步：

1) 源码下载与编译;

2) 初始化节点和创世区块，并启动节点;

3) 部署系统合约。

本文将详细介绍如何在单机和多机上部署PlatONE。

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

2. 单机部署
===========

2.0 生成配置文件
^^^^^^^^^^^^^^^^^^^^^

请参考：

- :ref:`配置文件格式 <remote-conf-structure>`

- :ref:`生成配置文件教程 <remote-prepare>`

生成后将配置文件放入 ``/release/linux/data/node-${node_name}/`` 下

2.1 初始化节点和创世区块
^^^^^^^^^^^^^^^^^^^^^^^^

2.1.1 创建genesis.json文件
--------------------------

当您启动区块链时，首先需要创建一个genesis.json文件，节点通过genesis.json文件来生成创世区块。

执行下面指令一键生成genesis.json:

.. code:: bash

   cd ${WORKSPACE}/scripts
   ./platonectl.sh setupgen -n 0 --ip 172.25.1.13 --p2p_port 16791 --interpreter all --auto

各个参数的意义如下所示：

.. code:: bash

   --nodeid, -n      node id (default: 0)
   --ip              node ip (default: 127.0.0.1)
   --p2p_port        node p2p_port (default: 16791)
   --interpreter, -i evm， wasm or all （default: wasm）

上面的命令，首先会在\ ``{WORKSPACE}/data/node-0``\ 目录下，生成节点的公私钥、IP端口等信息。 然后在\ ``{WORKSPACE}/conf``\ 目录下生成一个\ ``genesis.json``\ 文件。

.. code:: bash

   $ ls  ${WORKSPACE}/data/node-0
   node.address node.ip node.p2p_port node.prikey node.pubkey

.. code:: bash

   $ ls  ${WORKSPACE}/conf
   genesis.json contracts ...


2.1.2 初始化节点和创世区块
--------------------------

执行如下命令，会根据genesis.json文件，在数据目录下产生创世区块，并配置节点的RPC和websocket端口信息。

.. code:: bash

   cd ${WORKSPACE}/scripts/
   ./platonectl.sh init -n 0 --ip 172.25.1.13 --rpc_port 6791 --p2p_port 16791 --ws_port 26791 --auto

各个参数的意义如下所示：

.. code:: bash

   --nodeid, -n      node id (default: 0)
   --ip              node ip (default: 127.0.0.1)
   --p2p_port        node p2p_port (default: 16791)
   --rpc_port        node rpc_port (default: 6791)
   --ws_port         node websoket port (default: 26791)

2.1.3 启动节点
--------------

默认启动命令：

.. code:: bash

   cd ${WORKSPACE}/scripts/
   ./platonectl.sh start -n 0

节点启动后，可以通过节点运行日志跟踪节点的运行状态。

.. code:: bash

   节点数据： ${WORKSPACE}/data/node-0/
   节点运行日志：  ${WORKSPACE}/data/node-0/logs/platone_log/

在启动节点时, 可以指定日志文件夹的路径,
指定platone启动时额外的命令行参数等. (注意: 路径连接符’/’ 需要进行转义,
参数option的值, 必须加上引号)

-  **日志位置**：生产环境需要指定日志存放路径

   -  ``--logdir, -d log dir (default: ../data/node_dir/logs/)``

-  **日志等级**：通过\ ``-e``
   指定了\ **额外参数**\ ，通过\ ``-e '--verbosity 2'``\ 可以用来指定日志等级为2。
-  通过\ ``--bootnodes``\ 指定区块链入口节点，节点启动时会主动连接指定为bootnodes的节点，以接入区块链网络。

如下命令指定了log日志目录、日志级别以及启动时要连接的节点：

.. code:: bash

   ./platonectl.sh start -n x -d "\/opt\/logs"  -e "--verbosity 3 --debug --bootnodes enode://8ab91d36a58e03c7d5528ea9186474cf5bfbec46d24cd59cf5eef1b63b2f4120334ca2a6af9ae495fa1931cdfe684caa74c86ad77fcfa0f044f4da30f7a83a4e@172.25.1.13:16791"

日志文件夹中包含wasm执行的日志与platone运行的日志. 随时间推移,
日志文件会越积越多, 建议进行挂载, 或者进行定期删除等操作。

2.2 部署系统合约
----------------

【方法一】执行脚本
>>>>>>>>>>>>>>>>>>

创建管理员账号并部署系统合约

.. code:: bash

   ./platonectl.sh deploysys -n 0

本步骤会首先在节点侧创建一个账号，需要手动输入密码，该账号即为链的超级管理员。然后，使用该账号向链部署系统合约。

如果创建账号时，跳过手动输入密码的过程，可以加上\ ``--auto true``\ ，这样就可以使用默认密码\ ``0``\ 创建账号。

【方法二】执行命令行
>>>>>>>>>>>>>>>>>>>>

1) 生成ctool.json


进入\ ``PlatONE-Go/cmd/SysContracts/build/systemContract``\ 目录,
确保此时platone已启动。 使用vi创建ctool.json文件，
写下如下内容。根据此时启动的节点的情况,
替换如下模板中的NODE-IP、RPC-PORT、DEFAULT-ACCOUNT。


.. code:: bash

   vi ctool.json

.. code:: json

   {
     "url":"http://NODE-IP:RPC-PORT",
     "gas":"0x0",
     "gasPrice":"0x0",
     "from":"0xDEFAULT-ACCOUNT"
   }

-  NODE-IP: 节点启动时设置的ip选项。
-  RPC-PORT：节点启动是设置的rpc_port 端口。
-  DEFAULT-ACCOUNT：在3.1.1第2小节创建的用户账号。

2) 部署系统合约

部署系统合约前需要unlock部署合约的账户地址，首先进入到console,解锁用户账户。

.. code:: bash

   platone attach http://NODE-IP:RPC-PORT

.. code:: console

   Welcome to the PlatONE JavaScript console!

   instance: PlatONEnetwork/platone/v0.2.0-stable-56ea60ae/linux-amd64/go1.11.4
   coinbase: 0x0fbd63b374002cb15aca95202fe10b63bda3fdcb
   at block: 4012 (Tue, 27 Aug 2019 10:54:40 CST)
    datadir: /home/wxuser/wywforfun/PlatONE-Go/build/bin/data
    modules: admin:1.0 eth:1.0 net:1.0 personal:1.0 rpc:1.0 web3:1.0

   >

3) 然后解锁用户账户， 需输入账号对应的密码。

.. code:: bash

   >personal.unlockAccount("DEFAULT-ACCOUNT")
   Unlock account DEFAULT-ACCOUNT
   Passphrase:
   true

4) 然后可以退出console进行合约部署

-  NODE-IP，RPC-PORT，DEFAULT-ACCOUNT
   的值，需要和4.1章中ctool.json中设置的值一致。

进入\ ``PlatONE-Go/cmd/SysContracts/build/systemContract``\ 目录

.. code:: bash

   # 部署cnsManager系统合约
   ctool deploy --config ctool.json --code cnsManager/cnsManager.wasm --abi cnsManager/cnsManager.cpp.abi.json
   # 部署paramManager系统合约
   ctool deploy --config ctool.json --code paramManager/paramManager.wasm --abi paramManager/paramManager.cpp.abi.json
   # 部署userManager系统合约
   ctool deploy --config ctool.json --code userManager/userManager.wasm --abi userManager/userManager.cpp.abi.json
   # 部署userRegister系统合约
   ctool deploy --config ctool.json --code userRegister/userRegister.wasm --abi userRegister/userRegister.cpp.abi.json
   # 部署roleManager系统合约
   ctool deploy --config ctool.json --code roleManager/roleManager.wasm --abi roleManager/roleManager.cpp.abi.json
   # 部署roleRegister系统合约
   ctool deploy --config ctool.json --code roleRegister/roleRegister.wasm --abi roleRegister/roleRegister.cpp.abi.json
   # 部署nodeManager系统合约
   ctool deploy --config ctool.json --code nodeManager/nodeManager.wasm --abi nodeManager/nodeManager.cpp.abi.json
   # 部署nodeRegister系统合约
   ctool deploy --config ctool.json --code nodeRegister/nodeRegister.wasm --abi nodeRegister/nodeRegister.cpp.abi.json

至此，一个单节点的PlatONE联盟链搭建完毕。


3. 多机部署（适用于生产环境/多机测试环境）
==========================================

案例: A, B, C, D四台主机 (**各个主机自动时间同步**)

-  A: 172.25.1.13
-  B: 172.25.1.14
-  C: 172.25.1.15
-  D: 172.25.1.16

.. _准备工作-1:

3.1. 准备工作
^^^^^^^^^^^^^

首先在主机A上，下载源码并编译，参照第1部分。

然后将编译好的PlatONE-Go/release目录，分发到B、C、D主机。

.. code:: bash

   scp -r PlatONE-Go/release user@172.25.1.14:~/
   scp -r PlatONE-Go/release user@172.25.1.15:~/
   scp -r PlatONE-Go/release user@172.25.1.16:~/

3.2. 在A主机搭建单节点区块链
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

参照1.1~1.5小节，在节点A上搭建单节点区块链，然后将genesis.json文件广播出来给其他节点，放置于PlatONE-Go/release/linux/conf目录下。

.. code:: bash

   scp -r genesis.json user@172.25.1.14:~/PlatONE-Go/release/linux/conf
   scp -r genesis.json user@172.25.1.15:~/PlatONE-Go/release/linux/conf
   scp -r genesis.json user@172.25.1.16:~/PlatONE-Go/release/linux/conf

3.3. 在B、C、D生成创世区块及节点信息
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

3.4. A主机管理员添加B、C、D节点至系统合约
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

以添加B节点为例：

此时A主机的data/node-1目录已经有了B节点的信息（IP、p2p端口、rpc端口和公钥）

将B主机上的节点加入到当前区块链

.. code:: bash

   ./platonectl.sh addnode -n 1

本步骤会在系统合约中写入了B节点信息，B节点成为观察者节点（可以同步交易及数据，但是不参与共识出块）

3.5. B、C、D主机启动节点
^^^^^^^^^^^^^^^^^^^^^^^^

以B节点为例

.. code:: bash

   ./platonectl.sh start -n 1

B节点启动后会主动连接A节点，加入网络，成为观察者节点。

3.6. 将B、C、D升级为共识节点
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

根据业务需求，可以将观察者节点升级为共识节点。

以添加B节点为例，由A节点的管理员操作如下命令，即可将B节点升级为共识节点：

.. code:: bash

   ./platonectl.sh updatesys -n 1

4. 重新初始化platone节点
========================

确保platone进程已经被杀死，再删除data目录。

.. code:: bash

   cd build/bin
   rm -rf data/platone

然后可以再重新初始化。


5. 备份与还原
=============

该功能支持节点未启动，以及chaindb中数据损坏的场景下，通过线下传递区块数据的方式，将某节点落后的区块数据补齐

5.1. 备份
^^^^^^^^^

通过export功能，将某节点指定范围内的经过RLP编码后的区块数据导出到某个文件中

.. code:: bash

   ./platone --datadir <待导出节点的chaindata路径> export <输出文件名> <导出区块高度下界> <导出区块高度上界>

示例

.. code:: bash

   ./platone export --datadir ../data/node-0/  block-0-14.data 0 14

5.2. 还原
^^^^^^^^^

5.2.1. 清理节点
---------------

清掉四个节点的数据目录，并根据已有的genesis初始化链

.. code:: bash

   rm -rf  ../data/node-*/platone/*

.. code:: console

   ./platone init --datadir ../data/node-0 ../conf/genesis.json
   ./platone init --datadir ../data/node-1 ../conf/genesis.json
   ./platone init --datadir ../data/node-2 ../conf/genesis.json
   ./platone init --datadir ../data/node-3 ../conf/genesis.json

5.2.2. 导入区块数据
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

6. 生产日志清理策略参考
=======================

我们模拟了正常交易压力下的日志量：单节点上，24小时产出约为300M大小的日志。

-  假设在500G数据盘的规划下，按照70%的阈值保留，去除链DB数据（建议保留至少100GB），那么可以保留约27个月的数据。

-  但由于交易峰值出现的可能性，建议同时实施空间大小阈值的清理策略，即当日志总量达到500GB*70%-100GB
   =250GB 时，实施对最早的一个月数据的清理。

**总结**：时间维度和空间维度的日志清理策略同时实施。

7. 运行状态检查&错误排查
========================

- 在将链交付给业务前，我们可以从以下维度验证链的运行正确性，包括但不限于以下步骤：

**链运行状态检查**：

链运行日志，观察是否正常出块。（正常出块间隔在1～3秒之间）

**系统合约部署情况检查**：

-  系统合约的部署日志在 wasm_log文件夹中，可以监控日志中是否出现了 \ ``error``\ 关键词，排查合约是否正常部署。

-  通过\ ``./platonectl.sh get`` 命令，确认所有节点已经被记录到了节点管理合约。

**监控链运行过程**:

- 监控运行过程中是否有出现\ ``error``\ 或者\ ``warning``\ 关键词。（部分和节点瞬时联通性相关的，如节点互ping心跳包导致的报错信息可忽略。）
