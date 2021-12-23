=============
备份与还原
=============

该功能支持节点未启动，以及chaindb中数据损坏的场景下，通过线下传递区块数据的方式，将某节点落后的区块数据补齐

备份
=======

通过export功能，将某节点指定范围内的经过RLP编码后的区块数据导出到某个文件中

.. code:: bash

   ./venachain --datadir <待导出节点的chaindata路径> export <输出文件名> <导出区块高度下界> <导出区块高度上界>

示例

.. code:: bash

   ./venachain export --datadir ../data/node-0/  block-0-14.data 0 14

还原
==========

1. 清理节点
^^^^^^^^^^^^^

清掉四个节点的数据目录，并根据已有的genesis初始化链

.. code:: bash

   rm -rf  ../data/node-*/venachain/*

.. code:: console

   ./venachain init --datadir ../data/node-0 ../conf/genesis.json
   ./venachain init --datadir ../data/node-1 ../conf/genesis.json
   ./venachain init --datadir ../data/node-2 ../conf/genesis.json
   ./venachain init --datadir ../data/node-3 ../conf/genesis.json

2. 导入区块数据
^^^^^^^^^^^^^^^^^

通过import功能，将导出的区块数据导入指定节点

.. code:: bash

   ./venachain --datadir <待导入节点的chaindata路径> import <区块文件名>

示例

.. code:: bash

   # 给节点0导入数据
   ./venachain import --datadir ../data/node-0 block-0-14.data
   # 然后启动节点0
   cd ../scripts
   ./venachainctl.sh start -n 0
   # 此时观察log会发现节点0的区块高度已经成为14了，其他节点可以启动，然后跟节点0连接，同步其数据，最终整个区块链高度都是14了