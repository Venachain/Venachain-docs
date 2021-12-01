=============
personal
=============

personal.newAccount
=========================

**描述** 

创建一个新账户。

.. code:: bash

   web3.eth.personal.newAccount(password, [callback])

**参数**

- ``password``: ``String`` - 用来加密账户的密码

**返回值**

一个Promise对象，其解析值为新创建的账户。

**操作**

.. code:: bash

   > personal.newAccount('password')

**输出结果**

.. code:: console

   "0x21baa00b024172344b029feb72839607875b990d"

personal.unlockAccount
=========================

**描述** 

解锁账户

.. code:: bash

   web3.eth.personal.unlockAccount(address, password, unlockDuraction [, callback])

**参数**

- ``address``: ``String`` - 要解锁的账户地址。
- ``password``: ``String`` - 账户密码。
- ``unlockDuration``: ``Number`` - 将帐户保持在解锁状态的持续时间。

**操作**

.. code:: bash

   > personal.unlockAccount(eth.accounts[1])

**输出结果**

.. code:: console

   true

personal.lockAccount
=======================

**描述** 

锁定给定帐户。

.. code:: bash

   web3.eth.personal.lockAccount(address [, callback])

**参数**

- ``address``: ``String`` - 要锁定的账户地址。
- ``Function`` - (可选)回调函数，其第一个参数为错误对象，第二个参数为签名结果。

**返回值**

- Promise<boolean>

**操作**

.. code:: bash

   > personal.lockAccount(eth.accounts[3])

**输出结果**

.. code:: console

   true

personal.listAccounts
===========================

**描述** 

显示当前的所有账户。

**操作**

.. code:: bash

   > personal.listAccounts

**输出结果**

.. code:: console

   ["0x4bf2e487a1f57d26d57ad9b9821a827e45b9e8e6", "0x7dc33e8ea643ae40459eec74a3879a7018328160", "0x6d5c88df6960fb62a24f816774745ff66c75fee3", "0x21baa00b024172344b029feb72839607875b990d"]

personal.listWallets
========================

**描述** 

返回当前的钱包状态

**操作**

.. code:: bash

   > personal.listWallets

**输出结果**

.. code:: js

   [{
       accounts: [{
           address: "0x4bf2e487a1f57d26d57ad9b9821a827e45b9e8e6",
           url: "keystore:///home/wxuser/go/src/PlatONE_Network/PlatONE-Go/release/linux/data/node-0/keystore/UTC--2021-03-09T06-38-24.634693807Z--4bf2e487a1f57d26d57ad9b9821a827e45b9e8e6"
       }],
       status: "Unlocked",
       url: "keystore:///home/wxuser/go/src/PlatONE_Network/PlatONE-Go/release/linux/data/node-0/keystore/UTC--2021-03-09T06-38-24.634693807Z--4bf2e487a1f57d26d57ad9b9821a827e45b9e8e6"
   }, ],
       accounts: [{
           address: "0x21baa00b024172344b029feb72839607875b990d",
           url: "keystore:///home/wxuser/go/src/PlatONE_Network/PlatONE-Go/release/linux/data/node-0/keystore/UTC--2021-03-09T08-40-58.891340059Z--21baa00b024172344b029feb72839607875b990d"
       }],
       status: "Locked",
       url: "keystore:///home/wxuser/go/src/PlatONE_Network/PlatONE-Go/release/linux/data/node-0/keystore/UTC--2021-03-09T08-40-58.891340059Z--21baa00b024172344b029feb72839607875b990d"
   }]

personal.sign
===============

**描述** 

使用账户给某个消息签名

.. code:: bash

   web3.eth.personal.sign(dataToSign, address, password [, callback])

**参数**

- ``String`` - 要签名的数据。 如果是字符串会被转换为 16 进制。
- ``String`` - 用来签名的账户地址。
- ``String`` - 用来签名的账户密码。
- ``Function`` - (可选)回调函数，其第一个参数为错误对象，第二个参数为签名结果。

**返回值**

- ``Promise<string>`` - 签名字符串。

操作:

.. code:: bash

   > personal.sign("0x",eth.accounts[0],"0")

**输出结果**

.. code:: console

   "0x2946ee06feb7a9913af2bc95ba9d6882e25bff547111bd201463752ce20eceda22cfe72f8e4b69030f2470c8ab27dd4eed34a20d941309d751019c4099f70ec51b"

personal.ecRecover
======================

**描述** 

恢复数据签名帐户。

.. code:: bash

   eth.personal.ecRecover(dataThatWasSigned, signature [, callback])

**参数**

- ``String`` - 被签名的数据。 如果是字符串会被转换为 16 进制。
- ``String`` - 签名。
- ``Function`` - (可选)回调函数，其第一个参数为错误对象，第二个参数为签名结果。

**返回值**

- ``Promise<string>`` - 签名账户。

**操作**

.. code:: bash

   > personal.ecRecover("0x","0xb8ba89dfe28d49ca8448120e4f779fbd1b9bfbf0b8bebdfd58a3712346291aab550f8eeafb3c906e046ac0fc72c569f2f18ba2a2868ed56e92feb 47d602fa5261b")

**输出结果**

.. code:: console

   "0x4bf2e487a1f57d26d57ad9b9821a827e45b9e8e6"

personal.signTransaction
============================

**描述** 

对交易进行签名，账户必须先解锁。

.. code:: bash

    web3.eth.personal.signTransaction(transaction, password [, callback])

**参数**

- ``Object`` - 要签名的交易数据。
- ``String`` - 用来签名交易的 ``from`` 账户密码。
- ``Function`` - (可选)回调函数，其第一个参数为错误对象，第二个参数为签名结果。

**返回值**

- ``Promise<Object>`` - RLP 编码的交易对象，其 ``raw`` 属性可以用来发送交易。

**操作**

.. code:: bash

   > personal.signTransaction({from:eth.accounts[0],gasPrice:'0',to:eth.accounts[1],gas:'2100',value:0,nonce:'0'},"0")

**输出结果**

.. code:: js

   {
     raw: "0xf8628080820834947dc33e8ea643ae40459eec74a3879a701832816080808082027ba058516e83733e92cdfb8dba6c9353ce43e8df43170728e50d099993f56afe29e5a0151a494a4807f972c28de1bfd6b0578fde01615a86899d75d2fa2017508f8e1c",
     tx: {
       gas: "0x834",
       gasPrice: "0x0",
       hash: "0x27c49a8146b4a652fd90e0d441554799931e4d7c1278dfd2187f90ebe8f9da6c",
       input: "0x",
       nonce: "0x0",
       r: "0x58516e83733e92cdfb8dba6c9353ce43e8df43170728e50d099993f56afe29e5",
       s: "0x151a494a4807f972c28de1bfd6b0578fde01615a86899d75d2fa2017508f8e1c",
       to: "0x7dc33e8ea643ae40459eec74a3879a7018328160",
       txType: "0x0",
       v: "0x27b",
       value: "0x0"
     }
   }

personal.sendTransaction
=============================

**描述** 

通过账户管理 API 来发送交易。

.. code:: bash

   web3.eth.personal.sendTransaction(transactionOptions, password [, callback])

**参数**

- ``Object`` - 交易对象属性。
- ``String`` - 当前帐户的密码。
- ``Function`` - (可选)回调函数，其第一个参数为错误对象，第二个参数为签名结果。

**返回值**

- ``Promise<string>`` - 交易哈希。

**操作**

.. code:: bash

   > personal.sendTransaction({from:eth.accounts[0],gasPrice:'0',to:eth.accounts[1],gas:'2100',value:0,nonce:'0'},"0")

输出结果:

.. code:: console

   "0x27c49a8146b4a652fd90e0d441554799931e4d7c1278dfd2187f90ebe8f9da6c"