================
合约部署与调用
================

1. 解锁账户
============

首次创建账户或者是长时间没用使用账户发送过交易，账户会被锁定，不能发送交易。需要使用如下命令，重新为账户解锁。

.. code:: bash

       curl -H "Content-Type: application/json" --data "{\"jsonrpc\":\"2.0\",\"method\":\"personal_unlockAccount\",\"params\":[\"0x2b63c4404f74ff8af325afe494c4f0a9b3a2c821\",\"0\",0],\"id\":1}"  http://127.0.0.1:6791

2. 部署合约
============

Venachain提供了 ``vcl`` 工具，可以进行合约的部署。具体请参考 :ref:`合约部署 <cli-contract-deploy>`

3. 调用合约
=============

Venachain提供了 ``vcl`` 工具，可以进行合约的调用。具体请参考 :ref:`合约调用 <cli-contract-execute>`


