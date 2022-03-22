.. _develop-rpc-methods-web3:

======
web3
======

web3_clientVersion
=====================

返回当前的客户端版本。

参数
^^^^^^

无

返回值
^^^^^^^^^

- ``string`` : 当前客户端版本

示例代码
^^^^^^^^^

- 请求：

.. code:: sh

    curl -H "Content-Type: application/json" -X POST -d '{"jsonrpc":"2.0","method":"web3_clientVersion","params":[],"id":1}' "http://127.0.0.1:6791"

- 响应：

.. code:: json

    {
        "jsonrpc":"2.0",
        "id":1,
        "result":"Venachain/venachain/v1.0.1-stable-74f96652/linux-amd64/go1.16.7"
    }

web3_sha3
==============

返回指定数据的 ``Keccak-256`` （不同于标准的 ``SHA3-256`` 算法）哈希值。

参数
^^^^^^^

- ``string`` : 要计算SHA3哈希的数据：

.. code:: js

    params: [
        "0x68656c6c6f20776f726c64"
    ]

返回值
^^^^^^^^

- ``data`` : 指定字符串的SHA3结果。

示例代码
^^^^^^^^^

- 请求：

.. code:: sh

    curl -H "Content-Type: application/json" -X POST -d '{"jsonrpc":"2.0","method":"web3_sha3","params":["0x68656c6c6f20776f726c64"],"id":1}' "http://127.0.0.1:6791"

- 响应：

.. code:: json

    {
        "jsonrpc":"2.0",
        "id":1,
        "result":"0x47173285a8d7341e5e972fc677286384f802f8ef42a5ec5f03bbfa254cb01fad"
    }