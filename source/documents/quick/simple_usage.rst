========
使用方法
========

.. note:: 详细使用方法请阅读 :ref:`节点部署工具venachainctl <tool-venachainctl>`

连接到console
================

您可以打开一个交互式JavaScript环境，与链交互 :

.. code-block:: bash

   ./venachainctl.sh console -n 0

.. note:: 详细使用方法请阅读 :ref:`venachainctl console的使用方法 <venachainctl-console>`

获取所有节点
===============

您可以从系统合约中获取所有节点信息

.. code:: bash

   ./venachainctl.sh get

查看节点信息
===============

以下命令可用于获取节点状态和信息:

.. code:: bash

   ./venachainctl.sh status -n 0

console:

.. code:: console

   disable -> node_id:  0
             node info:
                     node.address: f92f1c1469d1a8c38964c63d62d3167842ce70cd
                     node.ip: 127.0.0.1
                     node.rpc_port: 6791
                     node.p2p_port: 16791
                     node.ws_port: 26791
                     node.pubkey: 9afcb45d2725059a5a6a7f379d6404b6c914b02dd3e7a33d29c87b3f28f8f63b78ffe2736a3cb52ae45bbf57471d438eac71ddcc0bdfbaa56d65e59d457159b2

停止和清除
=============

您可以停止节点并清除节点数据：

.. code:: bash

   # 停止节点
   ./venachainctl.sh stop -n 0
   # 停止运行节点，并清除数据
   ./venachainctl.sh clear -n 0

执行如下命令可以停止所有节点，并清理链数据。

.. code:: bash

   cd ${WORKSPACE}/scripts/
   ./venachainctl.sh clear -a
   