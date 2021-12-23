.. _deploy-nodes:

==========================================
多机部署
==========================================

多机部署适用于生产环境/多机测试环境

案例: A, B, C, D四台主机 (**各个主机自动时间同步**)

-  A: 172.25.1.13
-  B: 172.25.1.14
-  C: 172.25.1.15
-  D: 172.25.1.16

1. 准备工作
===============

首先在主机A上，下载源码并编译，参照第1部分。

然后将编译好的Venachain/release目录，分发到B、C、D主机。

.. code:: bash

   scp -r Venachain/release user@172.25.1.14:~/
   scp -r Venachain/release user@172.25.1.15:~/
   scp -r Venachain/release user@172.25.1.16:~/

2. 在A主机搭建单节点区块链
=================================

参照 :ref:`单机部署 <deploy-node>` ，在节点A上搭建单节点区块链，然后将genesis.json文件广播出来给其他节点，放置于Venachain/release/linux/conf目录下。

.. code:: bash

   scp -r genesis.json user@172.25.1.14:~/Venachain/release/linux/conf
   scp -r genesis.json user@172.25.1.15:~/Venachain/release/linux/conf
   scp -r genesis.json user@172.25.1.16:~/Venachain/release/linux/conf

3. 在B、C、D生成创世区块及节点信息
====================================

以B为例：

.. code:: bash

   cd  ~/Venachain/release/linux/scripts
   ./venachainctl.sh init -n 1 --ip 172.25.1.14 --rpc_port 6791 --p2p_port 16791 --ws_port 26791 --auto true

此步骤会根据 ``genesis.json`` 文件生成创世区块，以及节点的连接信息（IP端口、节点密钥）

将节点信息发送至Ａ节点管理员，以便于管理员将新节点加入区块链网络。

节点信息包括节点IP、节点p2p端口、RPC端口和节点公钥，需要将如下四个文件发送至A主机的相应目录。(若A主机不存在data/node-1目录，则创建该目录，以存放节点信息)

.. code:: bash

   # node.ip, node.p2p_port, node.rpc_port, node.pubkey
   # --> user@172.25.1.14:~/Venachain/release/linux/data/node-1
   scp node.ip user@172.25.1.14:~/Venachain/release/linux/data/node-1
   scp node.p2p_port user@172.25.1.14:~/Venachain/release/linux/data/node-1
   scp node.rpc_port user@172.25.1.14:~/Venachain/release/linux/data/node-1
   scp node.pubkey user@172.25.1.14:~/Venachain/release/linux/data/node-1

4. B、C、D主机启动节点
==============================

以B节点为例

.. code:: bash

   ./venachainctl.sh start -n 1

B节点启动后会主动连接A节点，加入网络，成为观察者节点。

5. A主机管理员添加B、C、D节点至系统合约
============================================

以添加B节点为例：

此时A主机的data/node-1目录已经有了B节点的信息（IP、p2p端口、rpc端口和公钥）

将B主机上的节点加入到当前区块链

.. code:: bash

   ./venachainctl.sh addnode -n 1

本步骤会在系统合约中写入了B节点信息，B节点成为观察者节点（可以同步交易及数据，但是不参与共识出块）

6. 将B、C、D升级为共识节点
================================

根据业务需求，可以将观察者节点升级为共识节点。

以添加B节点为例，由A节点的管理员操作如下命令，即可将B节点升级为共识节点：

.. code:: bash

   ./venachainctl.sh updatesys -n 1