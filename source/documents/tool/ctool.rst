==================
链交互工具 (ctool)
==================

1. 配置文件 config.json
=======================

ctool中的部分参数会从配置文件中读取，若未指定\ ``-config``\ ，默认情况下从当前目录读取\ ``config.json``\ 文件。

config.json文件展示：

.. code:: json

   {
     "url":"http://192.168.9.73:6789",
     "gas": "0x76c0",
     "gasPrice": "0x9184e72a000",
     "from":"0xfb8c2fa47e84fbde43c97a0859557a36a5fb285b"
   }

2. 合约部署
===========

.. code:: bash

   Usage:
	./ctool deploy
	-abi        abi json file path (must)
	-code       wasm file path (must)
	-config     config path  (optional)
   
   # 示例
   ./ctool deploy -abi "D:\\resource\\temp\\contractc.cpp.abi.json" -code "D:\\resource\\temp\\contractc.wasm"

3. 合约调用
===========

3.1 普通调用
^^^^^^^^^^^^

.. code:: bash

   Usage:
	./ctool invoke
	-addr     contract address (must)
	-func     functon name and param (must)
	-abi      abi json file path (must)
	-type     transaction type ,default 2 (optional)
   
   # 示例
   ./ctool invoke -addr "0xFC43e7f481b9d3F75CcfFc8D23eAC522E96dE570" -func "transfer("a",b,c) " -abi "D:\\resource\\temp\\contractc.cpp.abi.json" -type

3.2 合约名称调用
^^^^^^^^^^^^^^^^

.. code:: bash

   Usage:
	./ctool cnsInvoke
	-cns      contract name (must)
	-func     functon name and param (must)
	-abi      abi json file path (must)
	-type     transaction type ,default 2 (optional)
	-config   config path  (optional)
   
   #示例
  ./ctool cnsInvoke --cns "test" -func "transfer("a",b,c) " -abi "D:\\resource\\temp\\contractc.cpp.abi.json"

4. 转账
=======

.. code:: bash

   Usage:
	./ctool sendTransaction
	-from       msg sender (must)
	-to         msg acceptor (must)
	-value      transfer value (must)
	-config     config path (optional)
   
   # 示例
   ./ctool transfer --from "0xb239401ecf8427f17c6de134d6a6bddd3100251f" --to "0X123" --value "10"

5. 交易回执查询
===============

.. code:: bash

   Usage:
	./ctool getTxReceipt
	-hash       txhash (must)
	-config     config path (optional)
   
   # 示例
   ./ctool getTxReceipt -hash <transaction_hash> -config "../config.json"

6. 防火墙操作
=============

.. code:: bash

   Usage:
	./ctool fwInvoke
	-addr     contract address (must)
	-func     functon name and param (must)
	-config   config path (optional)

   # 开启防火墙示例
   ./ctool fwInvoke --addr "0xacda4dfbbd6d093cf7e348abb33296d9aeb0f23c" --func '__sys_FwOpen()' --config "../config.json"
