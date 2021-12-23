.. _quick-deploy:

========
快速部署
========
本节将介绍如何在同一台机器中快速启动一个节点或启动四个节点的区块链，用于体验测试Venachain联盟链。

.. warning:: 在部署开始前请检查环境依赖是否已满足，可阅读 :ref:`环境依赖 <deploy-env>` 。 

.. note:: 本文方案适用于简单测试环境。开发环境部署参见 :ref:`正常部署 <normal-deploy>` 。


开启单节点
=============

快速启动单节点区块链:

.. code:: bash

   cd ${WORKSPACE}/scripts/
   ./venachainctl.sh one

通过以上步骤，可以在本机快速搭建一条单节点的区块链，节点数据目录及端口等信息使用如下默认配置：

.. code:: bash

   节点数据： ${WORKSPACE}/data/node-0/
   节点运行日志：  ${WORKSPACE}/data/node-0/logs/venachain_log/
   节点公钥： ${WORKSPACE}/data/node-0/node.pubkey

   IP： ${WORKSPACE}/data/node-0/node.ip
   RPC端口： ${WORKSPACE}/data/node-0/node.rpc_port (默认6791)
   p2p端口： ${WORKSPACE}/data/node-0/node.p2p_port （默认16791）

开启四节点
=============

在一台机器上快速启动四节点区块链:

.. code:: bash

   cd ${WORKSPACE}/scripts/
   ./venachainctl.sh four

通过以上步骤，可以在本机快速搭建一条包含4节点的区块链，节点数据目录及端口等信息使用如下默认配置：

.. code:: bash

   节点数据： ${WORKSPACE}/data/node-i (i为0~3的整数，分别表示四个节点)
   节点运行日志：  ${WORKSPACE}/data/node-i/logs/venachain_log/
   节点公钥： ${WORKSPACE}/data/node-i/node.pubkey

   RPC端口： 6791, 6792, 6793, 6794
   p2p端口： 16791, 16792, 16793, 16794
