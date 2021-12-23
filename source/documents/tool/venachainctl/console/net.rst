======
net
======

net.listening
=================

**描述** 

表示当前连接的节点，是否正在 ``listen`` 网络连接与否。 ``listen`` 可以理解为接收。

- 同步方式:

.. code:: bash

   web3.net.listening

- 异步方式:

.. code:: bash

   web3.net.getListener(callback(error, result){ … })

**返回值**

- ``Boolean`` - ``true`` 表示连接上的节点正在 ``listen`` 网络请求，否则返回 ``false`` 。

**操作**

.. code:: bash

   > net.listening

**输出结果**

.. code:: console

   true

net.peerCount
==================

**描述** 

返回连接节点已连上的其它以太坊节点的数量。

- 同步方式:

.. code:: bash

   web3.net.peerCount

- 异步方式:

.. code:: bash

   web3.net.getPeerCount(callback(error, result){ … })

**返回值**

- ``Number`` - 连接节点连上的其它以太坊节点的数量

**操作**

.. code:: bash

   > net.peerCount

**输出结果**

.. code:: console

   0