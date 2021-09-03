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



2. 部署（适用于单机/生产环境/多机测试环境）
====================================================

请参考：

- :ref:`部署 <remote-deploy>`

- :ref:`清理 <remote-clear>`

- :ref:`生成配置文件 <remote-prepare>`

- :ref:`传输 <remote-transfer>`

- :ref:`初始化 <remote-init>`

- :ref:`启动 <remote-start>`
   
3. 备份与还原
=============

该功能支持节点未启动，以及chaindb中数据损坏的场景下，通过线下传递区块数据的方式，将某节点落后的区块数据补齐

3.1. 备份
^^^^^^^^^

通过export功能，将某节点指定范围内的经过RLP编码后的区块数据导出到某个文件中

.. code:: bash

   ./platone --datadir <待导出节点的chaindata路径> export <输出文件名> <导出区块高度下界> <导出区块高度上界>

示例

.. code:: bash

   ./platone export --datadir ../data/node-0/  block-0-14.data 0 14

3.2. 还原
^^^^^^^^^

3.2.1. 清理节点
---------------

清掉四个节点的数据目录，并根据已有的genesis初始化链

.. code:: bash

   rm -rf  ../data/node-*/platone/*

.. code:: console

   ./platone init --datadir ../data/node-0 ../conf/genesis.json
   ./platone init --datadir ../data/node-1 ../conf/genesis.json
   ./platone init --datadir ../data/node-2 ../conf/genesis.json
   ./platone init --datadir ../data/node-3 ../conf/genesis.json

3.2.2. 导入区块数据
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

4. 生产日志清理策略参考
=======================

我们模拟了正常交易压力下的日志量：单节点上，24小时产出约为300M大小的日志。

-  假设在500G数据盘的规划下，按照70%的阈值保留，去除链DB数据（建议保留至少100GB），那么可以保留约27个月的数据。

-  但由于交易峰值出现的可能性，建议同时实施空间大小阈值的清理策略，即当日志总量达到500GB*70%-100GB
   =250GB 时，实施对最早的一个月数据的清理。

**总结**：时间维度和空间维度的日志清理策略同时实施。

5. 运行状态检查&错误排查
========================

- 在将链交付给业务前，我们可以从以下维度验证链的运行正确性，包括但不限于以下步骤：

**链运行状态检查**：

链运行日志，观察是否正常出块。（正常出块间隔在1～3秒之间）

**系统合约部署情况检查**：

-  系统合约的部署日志在 wasm_log文件夹中，可以监控日志中是否出现了 \ ``error``\ 关键词，排查合约是否正常部署。

-  通过\ ``./platonectl.sh get`` 命令，确认所有节点已经被记录到了节点管理合约。

**监控链运行过程**:

- 监控运行过程中是否有出现\ ``error``\ 或者\ ``warning``\ 关键词。（部分和节点瞬时联通性相关的，如节点互ping心跳包导致的报错信息可忽略。）
