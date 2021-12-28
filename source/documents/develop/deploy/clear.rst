==========
停止与清除
==========

停止节点
=========

**命令**

.. code:: bash

    ./venachainctl.sh stop -n ${node_id}

**参数**

.. code:: bash

    --nodeid, -n      		node id , must specified
    --all, -a         		stop all nodes

清除节点
=========

**命令**

.. code:: bash

    ./venachainctl.sh clear -n ${node_id}

**参数**

.. code:: bash

    --nodeid, -n      		node id , must specified
    --all, -a         		clear all nodes

**说明**

- 执行命令后，会先停止节点，再清除 ``${WORKSPACE}/data/node-${node_id}/``

- all参数：

  + 如果选择all，那么 ``genesis.json``  会被清除并备份至 ``{WORKSPACE}/conf/bak``, ``keyfile.json`` 仍被保留在 ``{WORKSPACE}/conf``

  + 如果不选择all，``genesis.json`` 仍会被保留在 ``{WORKSPACE}/conf`` 下

