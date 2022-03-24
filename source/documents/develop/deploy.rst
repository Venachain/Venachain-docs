.. _normal-deploy:

==========
编译与部署
==========

Venachain详细部署步骤可简要分为如下3步：

1) 源码下载与编译
2) 初始化节点和创世区块，并启动节点
3) 部署系统合约

本文将详细介绍如何在单机和多机上部署Venachain。

.. note:: 下文中有些步骤提供了两种执行方法，第一种是使用脚本完成，第二种则需要手动创建文件配合使用命令行来完成。虽然第二种方法操作更为复杂，但也更为灵活，便于开发者按需构建理想的联盟链网络。

.. note:: 如果是在Venachain项目目录下编译后进行操作的，那么 ``${WORKSPACE}`` 为 ``Venachain/release/linux`` 。如果是下载release包进行部署的，那么 ``${WORKSPACE}`` 为 ``linux`` 。

.. toctree::
   :hidden:
   
   deploy/prepare
   deploy/node
   deploy/nodes
   deploy/clear
   deploy/backup
   deploy/assistant
   