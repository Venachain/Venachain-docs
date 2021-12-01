.. _cli-firewall:

=====================
防火墙服务 fw
=====================

针对合约防火墙的相关操作

防火墙信息查询 fw query
==========================

**描述**

通过查询键，查询指定合约的防火墙信息

**参数**

- 必选参数:

.. code:: bash

      <addres>:    查询键，通过合约账户地址进行查询（返回结果唯一）

**操作**

.. code:: bash

      ./platonecli fw query "0x37bb31bc209d1d0d049fa3de34609b4de8d8c6d0"  --keyfile ../conf/keyfile.json  

**输出结果**

.. code:: json

        {
        "ContractAddr":"0x37bb31bc209d1d0d049fa3de34609b4de8d8c6d0",
        "Active":false,
        "AcceptedList":null,
        "RejectedList":null
        }

防火墙开启 fw start
=========================

**描述**

对指定合约开启防火墙服务

**参数**

- 必选参数:

.. code:: bash

      <addres>:    （进行防火墙设置的）合约账户地址

**操作**

.. code:: bash

      ./platonecli fw start "0x37bb31bc209d1d0d049fa3de34609b4de8d8c6d0"  --keyfile ../conf/keyfile.json  

**输出结果**

.. code:: bash

        {
        "status": "Operation Succeeded",
        "logs": [
            "Event Notify: 0 fw start success "
        ],
        "blockNumber": 175,
        "GasUsed": 35108,
        "From": "0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18",
        "To": "0x1000000000000000000000000000000000000005",
        "TxHash": ""
        }

防火墙关闭 fw stop
========================

**描述**

对指定合约关闭防火墙服务

**参数**

- 必选参数:

.. code:: bash

      <addres>:    （进行防火墙设置的）合约账户地址

**操作**

.. code:: bash

      ./platonecli fw stop "0x37bb31bc209d1d0d049fa3de34609b4de8d8c6d0"  --keyfile ../conf/keyfile.json  

**输出结果**

.. code:: bash

        {
        "status": "Operation Succeeded",
        "logs": [
            "Event Notify: 0 fw close success "
        ],
        "blockNumber": 177,
        "GasUsed": 35176,
        "From": "0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18",
        "To": "0x1000000000000000000000000000000000000005",
        "TxHash": ""
        }

防火墙规则导出 fw export
=============================

**描述**

将指定合约的防火墙规则导出到指定位置的防火墙规则文件中

**参数**

- 必选参数:

.. code:: bash

      <addres>:             合约账户地址

- 可选参数:

.. code:: bash

      --file <file>:      导出的防火墙规则文件存储的路径，默认路径为./config

**操作**

.. code:: bash

      # 导出防火墙规则到指定路径
      ./platonecli fw export "0x37bb31bc209d1d0d049fa3de34609b4de8d8c6d0" --file <file path> --keyfile ../conf/keyfile.json

**输出结果**

.. code:: console

      result: Operation Succeeded


防火墙规则导入 fw import
===========================

**描述**

将XX格式防火墙文件中的防火墙规则导入指定合约的防火墙规则中

**参数**

- 必选参数:

.. code:: bash

      --addr <addres>:     （进行防火墙设置的）合约账户地址

-  可选参数:

.. code:: bash

      --file <file>:      导入的防火墙规则文件的路径，默认文件为。/config/fireWall.json

**操作**

.. code:: bash

      ./platonecli fw import "0x37bb31bc209d1d0d049fa3de34609b4de8d8c6d0" --file <file path> --keyfile ../conf/keyfile.json

**输出结果**

.. code:: console

      result: Operation Succeeded

防火墙规则添加 fw new
==========================

**描述**

新建一条或多条指定合约的防火墙规则。一条防火墙规则包含具体的防火墙操作（accept或reject操作），需要进行过滤的账户地址以及需要进行限制访问的合约接口名。

**参数**

- 必选参数:

.. code:: bash

      <addrres>:             (进行防火墙设置的)合约账户地址
      <action>:             防火墙操作:允许accept或拒绝reject
      <account>:            指定被过滤的一个或多个用户账户地址，'*'表示防火墙规则对所有用户账户地址生效。格式["<address1>","<address2>"]，单个账户地址可省略[]。
      <api>:                指定过滤的合约接口名，'*'表示该合约的所有接口(目前无法使用*)。格式["<funcname1>","<funcname2>"]，单个接口名可省略[]。示例--api "getName"

.. note:: 目前只支持单条防火墙规则的添加，即单个账户地址+单个接口

**操作**

.. code:: bash

      ## 新增一条防火墙规则
      ./platonecli fw new 0x37bb31bc209d1d0d049fa3de34609b4de8d8c6d0 accept 0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18 function1  --keyfile ../conf/keyfile.json

**输出结果**

.. code:: json

        {
        "status": "Operation Succeeded",
        "logs": [
            "Event Notify: 0 fw add success "
        ],
        "blockNumber": 179,
        "GasUsed": 39120,
        "From": "0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18",
        "To": "0x1000000000000000000000000000000000000005",
        "TxHash": ""
        }

防火墙规则删除 fw delete
===============================

**描述**

删除一条指定合约的防火墙规则。

**参数**

- 必选参数:

.. code:: bash

      <addres>:               （进行防火墙设置的）合约账户地址
      <action>:               防火墙操作:允许approve(allow?)或拒绝reject(block?)
      <account>:              指定被过滤的一个或多个用户账户地址，'*'表示防火墙规则对所有用户账户地址生效。格式["<address1>","<address2>"]，单个账户地址可省略[]。
      <api>:                  指定过滤的合约接口名，'*'表示该合约的所有接口。格式["<funcname1>","<funcname2>"]，单个接口名可省略[]。示例--api "getName"

.. note:: 目前只支持单条防火墙规则的删除，即单个账户地址+单个接口

**操作**

.. code:: bash

      ./platonecli fw delete 0x37bb31bc209d1d0d049fa3de34609b4de8d8c6d0 accept 0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18 function1  --keyfile ../conf/keyfile.json

**输出结果**

.. code:: json

        {
        "status": "Operation Succeeded",
        "logs": [
            "Event Notify: 0 fw delete success "
        ],
        "blockNumber": 181,
        "GasUsed": 39120,
        "From": "0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18",
        "To": "0x1000000000000000000000000000000000000005",
        "TxHash": ""
        }

防火墙规则重置 fw reset
=============================

**描述**

将指定合约的防火墙accept操作或者reject操作对应的所有规则清空，并再写入成一条对应操作的新的规则。

**参数**

- 必选参数:

.. code:: bash

      <addres>:               （进行防火墙设置的）合约账户地址
      <action>:               防火墙操作:允许accept(allow?)或拒绝reject(block?)
      <account>:              指定被过滤的一个或多个用户账户地址，'*'表示防火墙规则对所有用户账户地址生效。格式["<address1>","<address2>"]，单个账户地址可省略[]。
      <api>:                  指定过滤的合约接口名，'*'表示该合约的所有接口。格式["<funcname1>","<funcname2>"]，单个接口名可省略[]。示例--api "getName"

.. note:: 目前只支持单条防火墙规则的重置，即单个账户地址+单个接口

**操作**

.. code:: bash

      ./platonecli fw reset 0x37bb31bc209d1d0d049fa3de34609b4de8d8c6d0 reject 0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18 function1  --keyfile ../conf/keyfile.json

**输出结果**

.. code:: json

      {
        "status": "Operation Succeeded",
        "logs": [
            "Event Notify: 0 fw reset success "
        ],
        "blockNumber": 182,
        "GasUsed": 36332,
        "From": "0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18",
        "To": "0x1000000000000000000000000000000000000005",
        "TxHash": ""
       }

**描述**

清空指定合约的防火墙的approve操作或reject操作的全部规则

**参数**

- 必选参数:

.. code:: bash

      <addres>:             （进行防火墙设置的）合约账户地址

- 可选参数:

.. code:: bash

      --action string       清除对应操作的防火墙规则。防火墙操作:允许approve(allow?)或拒绝reject(block?)
      --all                 清除所有操作的防火墙规则

**操作**

.. code:: bash

      # 清除对应防火墙操作规则
      ./platonecli fw clear "0x37bb31bc209d1d0d049fa3de34609b4de8d8c6d0" --action "Reject"  --keyfile ../conf/keyfile.json
      # 清除所有防火墙规则
      ./platonecli fw clear "0x37bb31bc209d1d0d049fa3de34609b4de8d8c6d0" --all  --keyfile ../conf/keyfile.json

**输出结果**

.. code:: json

        {
        "status": "Operation Succeeded",
        "logs": [
            "Event Notify: 0 fw clear success "
        ],
        "blockNumber": 183,
        "GasUsed": 35652,
        "From": "0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18",
        "To": "0x1000000000000000000000000000000000000005",
        "TxHash": ""
        }

防火墙规则清理 fw clear
============================

**描述**

清空指定合约的防火墙的approve操作或reject操作的全部规则

**参数**

- 必选参数:

.. code:: bash

      <addres>:             （进行防火墙设置的）合约账户地址

- 可选参数:

.. code:: bash

      --action string       清除对应操作的防火墙规则。防火墙操作:允许approve(allow?)或拒绝reject(block?)
      --all                 清除所有操作的防火墙规则

**操作**

.. code:: bash

      # 清除对应防火墙操作规则
      ./platonecli fw clear "0xacda4dfbbd6d093cf7e348abb33296d9aeb0f23c" --action "Reject" --keyfile ../conf/keyfile.json
      # 清除所有防火墙规则
      ./platonecli fw clear "0xacda4dfbbd6d093cf7e348abb33296d9aeb0f23c" --all --keyfile ../conf/keyfile.json

**输出结果**

.. code:: console

      result: Operation Succeeded