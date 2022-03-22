.. _develop-rpc-methods-net:

======
net
======

net_listening
===============

返回节点是否正在侦听网络连接的标识，该调用总是返回true。

参数
^^^^^^^

无

返回值
^^^^^^^^^

- ``bool`` : 该调用总是返回true

示例代码
^^^^^^^^^^^

- 请求：

.. code:: sh

    curl -H "Content-Type: application/json" -X POST -d '{"jsonrpc":"2.0","method":"net_listening","params":[],"id":1}' "http://127.0.0.1:6791"

- 响应：

.. code:: json

    {
        "jsonrpc":"2.0",
        "id":1,
        "result":true
    }

net_peerCount
================

返回当前客户端所连接的对端节点数量。

参数
^^^^^^

无

返回值
^^^^^^^

- ``quantity`` : 所连接对端节点旳数量。

示例代码
^^^^^^^^^

- 请求：

.. code:: sh

    curl -H "Content-Type: application/json" -X POST -d '{"jsonrpc":"2.0","method":"net_peerCount","params":[],"id":1}' "http://127.0.0.1:6791"

- 响应：

.. code:: json

    {
        "jsonrpc":"2.0",
        "id":1,
        "result":"0x3"
    }

net_version
==============

返回当前 Venachain 底层 P2P 网络协议的版本号

参数
^^^^^^^

无

返回值
^^^^^^^^^^

- ``string`` : Venahcain 底层 P2P 网络协议版本号

示例代码
^^^^^^^^^^

-  请求：

.. code:: sh

    curl -H "Content-Type: application/json" -X POST -d '{"jsonrpc":"2.0","method":"net_version","params":[],"id":1}' "http://127.0.0.1:6791"

-  响应：

.. code:: json

    {
        "jsonrpc":"2.0",
        "id":1,
        "result":"1"
    }