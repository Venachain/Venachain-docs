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

.. code:: console

   result:
   {
         "code":0,
         "msg":"success",
         "data":[{
                  "name":"0",
                  "owner":"",
                  "desc":"",
                  "type":1,
                  "status":1,
                  "externalIP":"127.0.0.1",
                  "internalIP":"127.0.0.1",
                  "publicKey":"c3aa288636206cf9dd974913e5659c60263f57baed342437dde1a949e4c9401dfba05cfb5cc9015675e56985a9ea0694fdb97a042139af2af0f2289cb177c358",
                  "rpcPort":6791,
                  "p2pPort":16791
         }]
   }

查看节点信息
===============

以下命令可用于获取节点状态和信息:

.. code:: bash

   ./venachainctl.sh status -n 0

console:

.. code:: console

   running -> node_id:  0
            node info:
                     account: 926ee5594dadc8d65ea29f376423812becc84e4e
                     node.address: e8679931E1d48915cA551563aCeD01472d40206E
                     node.pubkey: c3aa288636206cf9dd974913e5659c60263f57baed342437dde1a949e4c9401dfba05cfb5cc9015675e56985a9ea0694fdb97a042139af2af0f2289cb177c358
                     node.ip_addr: 127.0.0.1
                     node.rpc_port: 6791
                     node.p2p_port: 16791
                     node.ws_port: 26791

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
   