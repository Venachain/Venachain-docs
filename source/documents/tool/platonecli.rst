============================
链交互工具 (platonecli)
============================

0. 全局选项
=============

0.1 账户地址选择
^^^^^^^^^^^^^^^^^

**描述**: 使用本地账户地址发送交易。

**参数**:

- ``--keyfile <file>``: 指定本地keystore文件的位置

.. note:: 使用命令时都需要指定 ``keyfile`` 路径，并根据指示输入对应发送账户的 ``passphrase`` 。

0.2 远程节点连接
^^^^^^^^^^^^^^^^^^^^^^

**描述**: 指定远程节点的IP进行连接

**参数**:

- ``--url <string>``: 通过–url与全局变量config.Url获取URL

      + 当未提供–url时,使用变量config.Url中的值
      + 当提供–url时，读取–url中的值并覆盖config.json中url键对应的值
      + 既未提供–url且全局变量config.Url的值为空，报错并提示错误

0.3 不等待交易回执结果 –sync\*
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**描述**: 主要用于链交互写操作，默认为同步等待交易回执查询的返回结果。

**参数**:

- ``--sync``: 不查询交易回执，直接返回结果，返回结果为Transaction Hash，在网络环境较差时建议使用该参数。若要进一步查询交易回执，在一段时间后可通过\ ``contract methods``\ 命令进行查询.

0.4 其他全局选项
^^^^^^^^^^^^^^^^^^^^^^^^^^

**描述**: 链交互操作需要进行的全局配置

**参数**:

- ``--gas <num>``: 设置代码执行的最大允许量，以避免无限循环。

- ``--gas-price <num>``: 设置执行链交互命令时单位gas所需消耗的XX数量。

.. _cli-account:

1. 用户操作 account
========================

针对账户地址的相关操作。

1.1. 用户注册 account add
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**描述**:对拥有的账户进行用户注册（User），审核通过的账户地址及其个人信息将被记录在用户平台。
**参数**:

- 必选参数:

.. code:: bash

   <account>:       用户账户地址
   <name>:          用户名

- 可选参数:

.. code:: bash

   --tel:                      用户电话信息
   --email:                    用户邮箱信息
   --organization string:      用户所属机构

**操作**:

.. code:: bash

   ./platonecli account add "0xb239401ecf8427f17c6de134d6a6bddd3100251f" "Alice" --phone "13111111111" --email "alice@wx.bc.com" --organization wxbc --keyfile ../conf/keyfile.json

**输出结果**:

.. code:: json

     {
       "status": "Operation Succeeded",
       "logs": [
           "Event addUser: 0 Success "
       ],
       "blockNumber": 227,
       "GasUsed": 113404,
       "From": "0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18",
       "To": "0x1000000000000000000000000000000000000001",
       "TxHash": ""
     }

1.2. 用户信息更新 account update
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**描述**: 更新用户的电话、邮箱等相关信息，普通用户（无角色/无权限用户无法修改用户的信息，仅管理员账户可操作。

**参数**:

- 必选参数:

.. code:: bash

   <address>:     （选择进行更新的）用户账户地址

- 可选参数:

.. code:: bash

   --phone <number>:         用户电话信息（更新）
   --email string:           用户邮箱信息（更新）
   --organization string:    用户所属机构（更新）

**操作**:

.. code:: bash

   # optional flags:
   ## 修改用户电话
   ./platonecli account update "0xb239401ecf8427f17c6de134d6a6bddd3100251f" --phone "13241231233" --keyfile ../conf/keyfile.json

   ## 修改用户邮箱
   ./platonecli account update "0xb239401ecf8427f17c6de134d6a6bddd3100251f" --email "123@qq.com" --keyfile ../conf/keyfile.json

   ## 修改用户所属机构
   ./platonecli account update "0xb239401ecf8427f17c6de134d6a6bddd3100251f" --organization "wxbc" --keyfile ../conf/keyfile.json

**输出结果**:

.. code:: json

     {
       "status": "Operation Succeeded",
       "logs": [
           "Event updateUserDescInfo: 0 Success "
       ],
       "blockNumber": 228,
       "GasUsed": 110548,
       "From": "0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18",
       "To": "0x1000000000000000000000000000000000000001",
       "TxHash": ""
    }

1.3. 用户信息查询 account query
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**描述**: 根据查询键值以及辅助选项进行信息的筛选查询，返回所有匹配成功的数据对象

**参数**:

- 可选参数: 用户信息查询，用于用户信息更新。

.. code:: bash

   --user:            查询键，通过用户账户地址或账户名称进行查询（返回结果唯一）
   --all:             查询全部用户

**操作**:

用户信息和用户角色信息分别来自不同系统合约的存储中，重构后我们把用户信息与角色信息在内部进行关联后再反馈给用户。

- 重构后:

.. code:: bash

   # 1 通过用户账户地址查询用户信息
   ./platonecli account query --user "0xb239401ecf8427f17c6de134d6a6bddd3100251f" --keyfile ../conf/keyfile.json

   # 2 通过用户账户名查询用户信息
   ./platonecli account query --user "Alice" --keyfile ../conf/keyfile.json

**输出结果**:

.. code:: json

      {
        "address":"0xb239401ecf8427f17c6de134d6a6bddd3100251f",
        "authorizer":"0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18",
        "name":"Alice"
      }

.. _cli-cns:

2. 合约命名系统 cns
=======================

针对合约命名系统服务的相关操作。

2.1. cns解析 cns resolve
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**描述**: 通过合约名称以及版本号（默认为”latest”）解析出对应的账户地址。一个合约名可以对应多个（在注册的）合约地址，通过版本号解析出对应的合约地址，但在cns平台中已注销的合约名对应的版本号无法解析出相应的账户地址。

**参数**:

- 必选参数:

.. code:: bash

   <name>:     合约在cns中注册的合约名称

- 可选参数:

.. code:: bash

   --ver string:     合约在cns中注册的版本号，默认为"latest"

**操作**:

.. code:: bash

   # 查询最新版本
   ./platonecli cns resolve "test"  --keyfile ../conf/keyfile.json

   # 查询指定版本
   ./platonecli cns resolve "test" --version "1.0.0.0"  --keyfile ../conf/keyfile.json

**输出结果**:

.. code:: console

   result: <address>

   # 对应合约名称的版本已注销
   result: <null>

2.2. cns注册 cns register
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**描述**: 将合约注册到cns平台中，注册后的合约不仅可以通过合约账户地址进行调用执行，还可以通过其对应的合约名称进行执行。

**参数**:

- 必选参数:

.. code:: bash

   <name>:          在cns中注册的合约名称
   <version>:       在cns中注册的版本号，（补充）。格式:"X.X.X.X"
   <address>:       进行注册的合约的账户地址

**操作**:

.. code:: bash

   ./platonecli cns register "test" "1.0.0.0" "0x2ee8d0545ebd20f9a992ff54cb0f21a153539206"  --keyfile ../conf/keyfile.json

**输出结果**:

.. code:: json

      {
        "status": "Operation Succeeded",
        "logs": [
            "Event [CNS] Notify: 0 [CNS] cns register succeed "
        ],
        "blockNumber": 190,
        "GasUsed": 105856,
        "From": "0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18",
        "To": "0x0000000000000000000000000000000000000011",
        "TxHash": ""
      }


2.3. cns重定向 cns redirect
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**描述**: 制定cns名称对应的合约版本。

**参数**:

- 必选参数:

.. code:: bash

   <name>:          在cns中注册的合约名称
   <version>:       在cns中注册的版本号。格式:"X.X.X.X"
   <address>:       进行注册的合约的账户地址

**操作**:

.. code:: bash

   ./platonecli cns register "test" "1.1.0.0" "0x2ee8d0545ebd20f9a992ff54cb0f21a153539206"  --keyfile ../conf/keyfile.json

**输出结果**:

.. code:: json

      {
        "status": "Operation Succeeded",
        "logs": [
            "Event [CNS] Notify: 0 [CNS] cns redirect succeed "
        ],
        "blockNumber": 191,
        "GasUsed": 102864,
        "From": "0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18",
        "To": "0x0000000000000000000000000000000000000011",
        "TxHash": ""
      }

2.4 cns信息查询 cns query
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**描述**: 根据查询键值以及辅助选项进行cns注册信息的筛选查询，返回所有匹配成功的数据对象。

**参数**:

- 可选参数:

.. code:: bash

      --contract <address>:     查询键，通过合约账户地址或者合约名称进行查询
      --user <address>:         查询键，通过用户账户地址进行查询，查询该用户注册在cns的合约
      --all:                    查询键，显示所有cns中所有注册的对象（不显示已注销的信息）
      --pageNum:                展示页面页码
      --pageSize:               展示页面大小

**操作**:

.. code:: bash

      # 1 查询已注册的合约
      ./platonecli cns query --all --keyfile ../conf/keyfile.json 
      # 2 通过合约名称进行查询 - 查询该名称注册历史
      ./platonecli cns query --contract "test" --keyfile ../conf/keyfile.json 
      # 3 通过注册者进行查询
      ./platonecli cns query "0x01a369998e4a141c5e2b40dbcbaf4a601d57cfa5" --pageNum "10" --pageSize "0" --keyfile ../conf/keyfile.json 
      # 4 通过合约地址进行查询
      ## 目前接口为通过地址查询未被注销的合约
      ./platonecli cns query --contract "0x01a369998e4a141c5e2b40dbcbaf4a601d57cfa5" --keyfile ../conf/keyfile.json 

**输出结果**:

.. code:: json

      {
        "code":0,
        "msg":"success",
        "data":[{
            "name":"eeeee",
            "version":"0.0.0.1",
            "address":"0x12a0de8326d814e1569d6a0e111be02b19741694",
            "origin":"0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18",
            "create_time":1600758772
        },
        {
            "name":"tofu",
            "version":"0.0.0.1",
            "address":"0x9185686d2a1fc1bbadaba646d7323f597fae0073",
            "origin":"0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18",
            "create_time":1600761759
        },
        {
            "name":"test",
            "version":"0.0.0.2",
            "address":"0x12a0de8326d814e1569d6a0e111be02b19741694",
            "origin":"0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18",
            "create_time":1600918255
        },
        {
            "name":"test",
            "version":"0.0.0.3",
            "address":"0xdb907806b906cfaa9049e5774e03263c6ff203e8",
            "origin":"0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18",
            "create_time":1601350402
        },
        {
            "name":"damn",
            "version":"0.0.0.1",
            "address":"0xe3471eace6b0eca6150d3a41051d8c7212c35da7",
            "origin":"0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18",
            "create_time":1601364209
        },
        {
            "name":"ljj",
            "version":"1.0.0.0",
            "address":"0x388d05bad3aab0fdd4a5256d4732c2129037cf19",
            "origin":"0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18",
            "create_time":1602234874
        }]
      }

2.5 cns状态查询 cns state
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**描述**: 通过查询键查询一个合约在cns平台中的注册状态，注册状态分为:注册中（返回true）或者已经注销（返回false）。

**参数**:

- 必选参数:

.. code:: bash

      <contract>:         查询键，根据合约账户地址或合约账户名称进行查询

**操作**:

.. code:: bash

      # 查询合约地址是否注册
      ./platonecli cns state "0x2ee8d0545ebd20f9a992ff54cb0f21a153539206" --keyfile ../conf/keyfile.json
      # 查询合约名称是否被注册
      ./platonecli cns state "test" --keyfile ../conf/keyfile.json

**输出结果**:

.. code:: console

      # 已注册
      result: the contract is registered in CNS
      # 未注册
      result: the contract is not registered in CNS

.. _cli-contract:

3. 合约操作 contract
=========================

针对合约的相关操作

.. _cli-contract-execute:

3.1 合约调用 contract execute
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**描述**: 调用并执行合约中的方法。支持wasm虚拟机合约和evm虚拟机合约方法的调用。仍然可以通过该命令实现系统合约的调用

**参数**:

- 必选参数:

.. code:: bash

      <contract>:           合约账户地址或合约cns注册名称
      <function>:           被执行合约的具体方法，
      --abi <file>:         合约abi文件路径。

- 可选参数:

.. code:: bash

      --param value:        合约方法的入参，当有多个入参时，一个--param对应一个参数。格式:--param <value1> --param <value2>
      --vm value:           选择进行执行的合约（目前支持evm合约，wasm合约，默认为wasm合约）

**操作**:

.. code:: bash

      # 通过合约地址调用合约
      ## wasm合约（默认）
      ./platonecli contract execute "0x2ee8d0545ebd20f9a992ff54cb0f21a153539206" "setName" --param wxbc  --abi "../../../cmd/platonecli/test/test_case/wasm/contracta.cpp.abi.json" --keyfile ../conf/keyfile.json
      ## evm合约
      ./platonecli contract execute ... ... --param --vm evm --keyfile ../conf/keyfile.json

      # 通过合约名称调用合约（cns服务）
      ./platonecli contract execute "test" "setName" --param wxbc --abi "../../../cmd/platonecli/test/test_case/wasm/contracta.cpp.abi.json" --keyfile ../conf/keyfile.json

**输出结果**:

.. code:: bash

      # 同步查询
      result:Operation Succeeded

3.2 合约方法查询 contract methods
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**描述**: 根据合约的cns注册名或者地址查询合约方法。

**参数**:

- 必选参数:

.. code:: bash

      --abi <path>:      合约abi文件路径

**操作**:

.. code:: bash

      ./platonecli contract methods --abi "../../../cmd/platonecli/test/test_case/wasm/contracta.cpp.abi.json"

**输出结果**:

.. code:: console

      # 查询结果
      -------------------contract methods list------------------------
      function: atransfer(from string,to string,asset int32)
      function: atransfer1(from string,to string,asset int32) int32
      function: atransfer2(from string,to string,asset int32) string
      function: adcall(from string,to string,asset int32)
      function: adcallInt64(from string,to string,asset int32) int32
      function: adcallString(from string,to string,asset int32 string

.. _cli-contract-deploy:

3.3 合约部署 contract deploy
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**描述**: 合约部署者将编写好的合约部署到链上。支持wasm虚拟机合约和evm虚拟机合约部署。

**参数**:

- 必选参数:

.. code:: bash

      <codeFile>:      合约编译后得到的二进制代码文件路径

- 可选参数:

.. code:: bash

      --abi <file>:    合约abi文件路径，部署wasm合约必须提供，部署evm合约不需要提供
      --vm value:       选择进行部署的合约（目前支持evm合约，wasm合约，默认为wasm合约）

**操作**:

.. code:: bash

      ## wasm合约
      ./platonecli contract deploy "../../../cmd/platonecli/test/test_case/wasm/contracta.wasm" --abi "../../../cmd/platonecli/test/test_case/wasm/contracta.cpp.abi.json"  --keyfile ../conf/keyfile.json
      ## evm合约
      ./platonecli contract deploy ../../../cmd/platonecli/test/test_case/sol/storage_byzantium_065.bin --abi ../../../cmd/platonecli/test/test_case/sol/storage_byzantium_065.abi --keyfile ../conf/keyfile.json -vm evm 

**输出结果**:

- 成功

.. code:: json

        {
        "status": "Operation Succeeded",
        "contractAddress": "0x388d05bad3aab0fdd4a5256d4732c2129037cf19",
        "blockNumber": 168,
        "GasUsed": 1451477,
        "From": "0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18",
        "To": "",
        "TxHash": ""
        }

- 可能失败的原因，具体错误信息请参照代码

   + rlp编码失败

   + Http发送失败

      - 无返回结果
      - 发送出错
      - 发送成功，状态码不为200

   + Rpc调用失败

      - RPC JSON消息解析失败
      - RPC调用失败:<失败信息>

   + 交易回执查询失败

      - 查询超时
      - 交易执行失败，状态码为0x0

3.4 回执查询 contract receipt
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**描述**: 根据交易的哈希值查询交易回执。

**参数**:

- 必选参数:

.. code:: bash

      <tx hash>:      交易的哈希值

**操作**:

.. code:: bash

      ./platonecli contract receipt 0x86d35fdd3bd67969ba71acba50076551ba8de31230b3bbfa8a536177c1610c23

**输出结果**:

.. code:: json

        {
        "blockHash": "0x308cd14101c4687b8966433f155e7272b8dbe6baa761c9b2d9e2aee225f39bad",
        "blockNumber": "0xa8",
        "contractAddress": "0x388d05bad3aab0fdd4a5256d4732c2129037cf19",
        "cumulativeGasUsed": "0x1625d5",
        "from": "0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18",
        "gasUsed": "0x1625d5",
        "root": "",
        "to": "",
        "transactionHash": "0x86d35fdd3bd67969ba71acba50076551ba8de31230b3bbfa8a536177c1610c23",
        "transactionIndex": "0x0",
        "logs": [],
        "status": "0x1"
        }

.. _cli-firewall:


4. 防火墙服务 fw
=====================

针对合约防火墙的相关操作

4.1. 防火墙信息查询 fw query
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**描述**: 通过查询键，查询指定合约的防火墙信息

**参数**:

- 必选参数:

.. code:: bash

      <addres>:    查询键，通过合约账户地址进行查询（返回结果唯一）

**操作**:

.. code:: bash

      ./platonecli fw query "0x37bb31bc209d1d0d049fa3de34609b4de8d8c6d0"  --keyfile ../conf/keyfile.json  

**输出结果**:

.. code:: json

        {
        "ContractAddr":"0x37bb31bc209d1d0d049fa3de34609b4de8d8c6d0",
        "Active":false,
        "AcceptedList":null,
        "RejectedList":null
        }

4.2. 防火墙开启 fw start
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**描述**: 对指定合约开启防火墙服务

**参数**:

- 必选参数:

.. code:: bash

      <addres>:    （进行防火墙设置的）合约账户地址

**操作**:

.. code:: bash

      ./platonecli fw start "0x37bb31bc209d1d0d049fa3de34609b4de8d8c6d0"  --keyfile ../conf/keyfile.json  

**输出结果**:

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

4.3. 防火墙关闭 fw stop
^^^^^^^^^^^^^^^^^^^^^^^^^

**描述**: 对指定合约关闭防火墙服务

**参数**:

- 必选参数:

.. code:: bash

      <addres>:    （进行防火墙设置的）合约账户地址

**操作**:

.. code:: bash

      ./platonecli fw stop "0x37bb31bc209d1d0d049fa3de34609b4de8d8c6d0"  --keyfile ../conf/keyfile.json  

**输出结果**:

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

4.4. 防火墙规则导出 fw export
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**描述**: 将指定合约的防火墙规则导出到指定位置的防火墙规则文件中

**参数**:

- 必选参数:

.. code:: bash

      <addres>:             合约账户地址

- 可选参数:

.. code:: bash

      --file <file>:      导出的防火墙规则文件存储的路径，默认路径为./config

**操作**:

.. code:: bash

      # 导出防火墙规则到指定路径
      ./platonecli fw export "0x37bb31bc209d1d0d049fa3de34609b4de8d8c6d0" --file <file path> --keyfile ../conf/keyfile.json

**输出结果**:

.. code:: console

      result: Operation Succeeded


4.5. 防火墙规则导入 fw import
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**描述**: 将XX格式防火墙文件中的防火墙规则导入指定合约的防火墙规则中

**参数**:

- 必选参数:

.. code:: bash

      --addr <addres>:     （进行防火墙设置的）合约账户地址

-  可选参数:

.. code:: bash

      --file <file>:      导入的防火墙规则文件的路径，默认文件为。/config/fireWall.json

**操作**:

.. code:: bash

      ./platonecli fw import "0x37bb31bc209d1d0d049fa3de34609b4de8d8c6d0" --file <file path> --keyfile ../conf/keyfile.json

**输出结果**:

.. code:: console

      result: Operation Succeeded

4.6. 防火墙规则添加 fw new
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**描述**: 新建一条或多条指定合约的防火墙规则。一条防火墙规则包含具体的防火墙操作（accept或reject操作），需要进行过滤的账户地址以及需要进行限制访问的合约接口名。

**参数**:

- 必选参数:

.. code:: bash

      <addrres>:             (进行防火墙设置的)合约账户地址
      <action>:             防火墙操作:允许accept或拒绝reject
      <account>:            指定被过滤的一个或多个用户账户地址，'*'表示防火墙规则对所有用户账户地址生效。格式["<address1>","<address2>"]，单个账户地址可省略[]。
      <api>:                指定过滤的合约接口名，'*'表示该合约的所有接口(目前无法使用*)。格式["<funcname1>","<funcname2>"]，单个接口名可省略[]。示例--api "getName"

.. note:: 目前只支持单条防火墙规则的添加，即单个账户地址+单个接口

**操作**:

.. code:: bash

      ## 新增一条防火墙规则
      ./platonecli fw new 0x37bb31bc209d1d0d049fa3de34609b4de8d8c6d0 accept 0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18 function1  --keyfile ../conf/keyfile.json

**输出结果**:

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

4.7. 防火墙规则删除 fw delete
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**描述**: 删除一条指定合约的防火墙规则。

**参数**:

- 必选参数:

.. code:: bash

      <addres>:               （进行防火墙设置的）合约账户地址
      <action>:               防火墙操作:允许approve(allow?)或拒绝reject(block?)
      <account>:              指定被过滤的一个或多个用户账户地址，'*'表示防火墙规则对所有用户账户地址生效。格式["<address1>","<address2>"]，单个账户地址可省略[]。
      <api>:                  指定过滤的合约接口名，'*'表示该合约的所有接口。格式["<funcname1>","<funcname2>"]，单个接口名可省略[]。示例--api "getName"

.. note:: 目前只支持单条防火墙规则的删除，即单个账户地址+单个接口

**操作**:

.. code:: bash

      ./platonecli fw delete 0x37bb31bc209d1d0d049fa3de34609b4de8d8c6d0 accept 0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18 function1  --keyfile ../conf/keyfile.json

**输出结果**:

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

4.8. 防火墙规则重置 fw reset
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**描述**: 将指定合约的防火墙accept操作或者reject操作对应的所有规则清空，并再写入成一条对应操作的新的规则。

**参数**:

- 必选参数:

.. code:: bash

      <addres>:               （进行防火墙设置的）合约账户地址
      <action>:               防火墙操作:允许accept(allow?)或拒绝reject(block?)
      <account>:              指定被过滤的一个或多个用户账户地址，'*'表示防火墙规则对所有用户账户地址生效。格式["<address1>","<address2>"]，单个账户地址可省略[]。
      <api>:                  指定过滤的合约接口名，'*'表示该合约的所有接口。格式["<funcname1>","<funcname2>"]，单个接口名可省略[]。示例--api "getName"

.. note:: 目前只支持单条防火墙规则的重置，即单个账户地址+单个接口

**操作**:

.. code:: bash

      ./platonecli fw reset 0x37bb31bc209d1d0d049fa3de34609b4de8d8c6d0 reject 0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18 function1  --keyfile ../conf/keyfile.json

**输出结果**:

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

**描述**: 清空指定合约的防火墙的approve操作或reject操作的全部规则

**参数**:

- 必选参数:

.. code:: bash

      <addres>:             （进行防火墙设置的）合约账户地址

- 可选参数:

.. code:: bash

      --action string       清除对应操作的防火墙规则。防火墙操作:允许approve(allow?)或拒绝reject(block?)
      --all                 清除所有操作的防火墙规则

**操作**:

.. code:: bash

      # 清除对应防火墙操作规则
      ./platonecli fw clear "0x37bb31bc209d1d0d049fa3de34609b4de8d8c6d0" --action "Reject"  --keyfile ../conf/keyfile.json
      # 清除所有防火墙规则
      ./platonecli fw clear "0x37bb31bc209d1d0d049fa3de34609b4de8d8c6d0" --all  --keyfile ../conf/keyfile.json

**输出结果**:

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

4.9. 防火墙规则清理 fw clear
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**描述**: 清空指定合约的防火墙的approve操作或reject操作的全部规则

**参数**:

- 必选参数:

.. code:: bash

      <addres>:             （进行防火墙设置的）合约账户地址

- 可选参数:

.. code:: bash

      --action string       清除对应操作的防火墙规则。防火墙操作:允许approve(allow?)或拒绝reject(block?)
      --all                 清除所有操作的防火墙规则

**操作**:

.. code:: bash

      # 清除对应防火墙操作规则
      ./platonecli fw clear "0xacda4dfbbd6d093cf7e348abb33296d9aeb0f23c" --action "Reject" --keyfile ../conf/keyfile.json
      # 清除所有防火墙规则
      ./platonecli fw clear "0xacda4dfbbd6d093cf7e348abb33296d9aeb0f23c" --all --keyfile ../conf/keyfile.json

**输出结果**:

.. code:: console

      result: Operation Succeeded

.. _cli-node:

5. 节点操作 node
=====================

针对节点的相关操作

5.1. 节点添加 node add
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**描述**: 将节点添加到PlatONE网络中。没有被管理员添加到节点列表的节点无法参与PlatONE网络中节点的区块同步，共识等等。第一次被添加的节点类型都为观察者节点。观察者节点后续可以由管理员修改成为共识节点参与共识。

.. note::

   - 如果节点列表中已有同名且状态为有效的节点，则注册失败。
   - 如果节点列表中已有同公钥的节点（无论节点状态），则注册失败。

**参数**:

- 必选参数:

.. code:: bash

      <name>:            节点名称
      <publicKey>:       节点公钥，用于节点间安全通信。节点的公私钥对可由ethkey工具产生。
      <externalIP>:      节点外网IP
      <internalIP>:      节点内网IP

- 可选参数:

.. code:: bash

      --rpcPort int<num>:      用于rpc远程调用的网络端口，默认端口6791
      --p2pPort int<num>:      用于p2p通信的网络端口，默认端口16791
      --desc string:           节点描述
      --delayNum <num>:        共识节点延迟设置的区块高度，默认实时设置

**操作**:

.. code:: bash

      ./platonecli node add "test" "feffe2938d427088f5fcce94a9245760b92c468d3ca25ab5ef2b1cdccf0ed911963b74ca2dffef20ef135966e34ebcc905d1f12c1df09f05974a617cf8afe8e8" "127.0.0.1" "127.0.0.1" --keyfile ../conf/keyfile.json

**输出结果**:

.. code:: json

      {
        "status": "Operation Succeeded",
        "logs": [
            "Event Notify: 0 add node success. node:{\"name\":\"test\",\"owner\":\"\",\"desc\":\"\",\"type\":0,\"status\":1,\"externalIP\":\"127.0.0.1\",\"internalIP\":\"127.0.0.1\",\"publicKey\":\"feffe2938d427088f5fcce94a9245760b92c468d3ca25ab5ef2b1cdccf0ed911963b74ca2dffef20ef135966e34ebcc905d1f12c1df09f05974a617cf8afe8e8\",\"rpcPort\":6791,\"p2pPort\":16791} "
        ],
        "blockNumber": 234,
        "GasUsed": 118776,
        "From": "0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18",
        "To": "0x1000000000000000000000000000000000000002",
        "TxHash": ""
      }

5.2. 节点删除 node delete
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**描述**: 将节点从节点列表中删除。在下一次peers更新后，被删除的节点会被PlatONE网络中的其他节点断开连接。

**参数**:

- 必选参数:

.. code:: bash

      <name>:        节点名称

**操作**:

.. code:: bash

      ./platonecli node delete "test" --keyfile ../conf/keyfile.json

.. note::

      - 不存在用户直接修改status的情况。确保status只能从1->2。
      - 状态修改后，节点的完整信息依旧可以通过query命令查询到

**输出结果**:

.. code:: json

      {
        "status": "Operation Succeeded",
        "logs": [
            "Event Notify: 0 update node success. info:{\"status\":2} "
        ],
        "blockNumber": 235,
        "GasUsed": 102932,
        "From": "0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18",
        "To": "0x1000000000000000000000000000000000000002",
        "TxHash": ""
      } 

5.3. 节点信息查询 node query
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**描述**: 通过查询键对节点信息进行查询，返回匹配成功的数据对象。

**参数**:

- 可选参数:

.. code:: bash

      --all                 查询键，查询所有节点(包含已被删除的节点)
      --name string:        查询键，通过节点名称进行查询（返回结果可能不唯一）
      --status string:      查询键，通过节点状态进行查询。valid(1)为有效状态，invalid(2)为无效（删除）状态
      --type string:        查询键，通过节点类型进行查询。observer(0)为观察者节点，consensus(1)为共识节点
      --publicKey string:   查询键，通过节点公钥进行查询（返回结果唯一）

**操作**:

.. code:: bash

      ## 返回网络中所有节点
      ./platonecli node query --all --keyfile ../conf/keyfile.json
      ## 根据查询键进行搜索
      ./platonecli node query --name "test" --keyfile ../conf/keyfile.json

      ./platonecli node query --status "valid" --keyfile ../conf/keyfile.json

      ./platonecli node query --type "consensus" --keyfile ../conf/keyfile.json

      ./platonecli node query -publicKey feffe2938d427088f5fcce94a9245760b92c468d3ca25ab5ef2b1cdccf0ed911963b74ca2dffef20ef135966e34ebcc905d1f12c1df09f05974a617cf8afe8e8 --keyfile ../conf/keyfile.json 
      ## 组合查询
      ./platonecli node query --status "valid" --name "root" --keyfile ../conf/keyfile.json

**输出结果**:

读操作

.. code:: console

      result: %s

示例

.. code:: js

      {
        "code":0,
        "msg":"success",
        "data":[{
          "name": ...,
          "owner": ...,
          "desc": ...,
          "type": ...,
          "publickey": ...,
          "externalIP": ...,
          "internalIP": ...,
          "rpcPort": ...,
          "p2pPort": ...,
          "status": ...,
          "delynum": `omitempty`
          }
        ]
      }

.. note:: 无"approver"字段

5.4 节点统计 node stat
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**描述**: 通过查询键对节点信息进行查询，对匹配成功的数据对象进行统计，返回统计值。

**参数**:

- 可选参数:

.. code:: bash

      --status string:    查询键，通过节点状态进行统计。"valid"为有效状态(1)，"invalid"为无效（删除）状态(2)
      --type string:      查询键，通过节点类型进行统计。"observer"为观察者节点(0)，"consensus"为共识节点(1)

**操作**:

.. code:: bash

      # 指定公钥对应的节点数目
      ./platonecli  node stat --status "valid" --keyfile ../conf/keyfile.json

**输出结果**:

.. code:: console

      # 读操作
      * result: <num>

5.5. 节点更新 node update
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**描述**:

   - 更新节点的\ ``desc``\ 、\ ``delayNum``\ 与\ ``type``\ 字段中的信息。无法更新权限同级及其以上角色节的信息。
   - 状态无效的节点依旧可以更新相应信息(bug?)

**参数**:

- 必选参数:

.. code:: bash

      <name>:            节点名称

- 可选参数:

.. code:: bash

      --desc string:     节点描述
      --type string:     节点类型，"observer"(0)为观察者节点，"consensus"(1)为共识节点。
      --delay <num>:     共识节点延迟设置的区块高度，默认实时设置

**操作**:

.. code:: bash

      # 更新节点type信息
      ./platonecli  node update "test" --type "consensus" --keyfile ../conf/keyfile.json
      # 更新节点desc信息
      ./platonecli  node update "test" --desc "this is a description" --keyfile ../conf/keyfile.json
        # 更新节点delayNum信息
      ./platonecli  node update "test" --delay 10 --keyfile ../conf/keyfile.json

**输出结果**:

.. code:: console

      # 同步查询
      result: NodeManager update key: type

6. rest 服务器 rest
========================

.. code:: bash

      ## 开启rest服务器
      ./platonecli rest

.. _cli-role:

7. 角色权限操作 role
========================

针对角色权限的相关操作

7.1. 设置超级管理员权限 role setSuperAdmin
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. warning:: 只能设置一次

**描述**: 链部署后可以调用该方法将当前账户进行超级管理员权限设置。

**操作**:

.. code:: bash

      ./platonecli role setSuperAdmin  --keyfile ../conf/keyfile.json

**输出结果**:

.. code:: json

      {
        "status": "Operation Succeeded",
        "logs": [
            "Event setSuperAdmin: Set SuperAdmin Succeed "
        ],
        "blockNumber": 2,
        "GasUsed": 102184,
        "From": "0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18",
        "To": "0x1000000000000000000000000000000000000001",
        "TxHash": ""
      }


7.2. 转移超级管理员权限 role transferSuperAdmin
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**描述**: 转移超级管理员权限（调用者需为当前的超级管理员）。

**参数**:

- 必选参数:

.. code:: bash

      <address>: 转移后的超级管理员地址

**操作**:

.. code:: bash

      ./platonecli role transferSuperAdmin "0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18"  --keyfile ../conf/keyfile.json

**输出结果**:

.. code:: json

      {
        "status": "Operation Succeeded",
        "logs": [
            "Event setSuperAdmin: Set SuperAdmin Succeed "
        ],
        "blockNumber": 2,
        "GasUsed": 102184,
        "From": "0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18",
        "To": "0x1000000000000000000000000000000000000001",
        "TxHash": ""
      }


7.3. 角色添加 role addXXX
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**描述**: 为某个账户地址添加指定角色的权限。

**参数**:

- 必选参数:

.. code:: bash

      <address>: 被赋予角色权限的账户地址

**操作**:

.. code:: bash

      #链管理员
      ./platonecli role addChainAdmin 0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18  --keyfile ../conf/keyfile.json
      #群组管理员
      ./platonecli role addGroupAdmin 0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18  --keyfile ../conf/keyfile.json
      #节点管理员
      ./platonecli role addNodeAdmin 0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18  --keyfile ../conf/keyfile.json
      #合约管理员
      ./platonecli role addContractAdmin 0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18  --keyfile ../conf/keyfile.json
      #普通合约部署者
      ./platonecli role addContractDeployer 0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18  --keyfile ../conf/keyfile.json

**输出结果**:

.. code:: json

      {
        "status": "Operation Succeeded",
        "logs": [
            "Event addGroupAdminByAddress: 0 Success "
        ],
        "blockNumber": 197,
        "GasUsed": 105788,
        "From": "0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18",
        "To": "0x1000000000000000000000000000000000000001",
        "TxHash": ""
      }

7.4. 角色删除 role delXXX
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**描述**: 为某个账户地址删除指定角色的权限。

**参数**:

- 必选参数:

.. code:: bash

      <address>: 被赋予角色权限的账户地址

**操作**:

.. code:: bash

      #链管理员
      ./platonecli role delChainAdmin 0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18  --keyfile ../conf/keyfile.json
      #群组管理员
      ./platonecli role delGroupAdmin 0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18  --keyfile ../conf/keyfile.json
      #节点管理员
      ./platonecli role delNodeAdmin 0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18  --keyfile ../conf/keyfile.json
      #合约管理员
      ./platonecli role delContractAdmin 0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18  --keyfile ../conf/keyfile.json
      #普通合约部署者
      ./platonecli role delContractDeployer 0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18  --keyfile ../conf/keyfile.json

**输出结果**:

.. code:: json

      {
        "status": "Operation Succeeded",
        "logs": [
            "Event delGroupAdminByAddress: 0 Success "
        ],
        "blockNumber": 198,
        "GasUsed": 105788,
        "From": "0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18",
        "To": "0x1000000000000000000000000000000000000001",
        "TxHash": ""
      }


7.5. 获取权限地址列表 role getAddrListOfRole
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**描述**: 获取权限地址列表。

**参数**:

- 必选参数:

.. code:: bash

      <role>: 角色可以且只能为"SUPER_ADMIN", "CHAIN_ADMIN", "GROUP_ADMIN", "NODE_ADMIN", "CONTRACT_ADMIN" ， "CONTRACT_DEPLOYER"其中之一

**操作**:

.. code:: bash

      #以SUPER_ADMIN为例
      ./platonecli role getAddrListOfRole "SUPER_ADMIN"  --keyfile ../conf/keyfile.json

**输出结果**:

.. code:: console

      # 以SUPER_ADMIN为例
      ["0x10ad2ec4831a1f89ec870a3224fead87cdb75931"]

7.6. 权限检查 role hasRole
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**描述**: 检查某账户地址是否拥有指定用户权限。

**参数**:

- 必选参数:

.. code:: bash

      <address>: 待检查账户地址
      <role>: 角色可以且只能为"SUPER_ADMIN", "CHAIN_ADMIN", "GROUP_ADMIN", "NODE_ADMIN", "CONTRACT_ADMIN" ， "CONTRACT_DEPLOYER"其中之一

**操作**:

.. code:: bash

      #以SUPER_ADMIN为例
      ./platonecli role hasRole 0x10ad2ec4831a1f89ec870a3224fead87cdb75931 SUPER_ADMIN  --keyfile ../conf/keyfile.json

**输出结果**:

.. code:: console

      # 以SUPER_ADMIN为例
      # 有权限
      result: 1
      # 无权限
      result: 0

7.7. 权限获取 role getRoles
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**描述**: 获取某账户地址用户权限。

**参数**:

- 必选参数:

.. code:: bash

      <address>: 待检查账户地址

**操作**:

.. code:: bash

      #以SUPER_ADMIN为例
      ./platonecli role getRoles 0x10ad2ec4831a1f89ec870a3224fead87cdb75931  --keyfile ../conf/keyfile.json

**输出结果**:

.. code:: console

      ["SUPER_ADMIN"]

.. _cli-sysconfig:

8. 系统参数操作 sysconfig
==================================

针对系统参数的相关操作

8.1. 系统参数设置 sysconfig set
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**描述**: 系统参数设置。

**参数**:

- 可选参数:

.. code:: bash

      --block-gaslimit : the gas limit of the block
      --tx-gaslimit : the gas limit of transactions
      --tx-use-gas : if transactions use gas, 'use-gas' for transactions use gas, 'not-use' for not
      --audit-con : approve the deployed contracts, 'audit' for allowing contracts audit, 'not-audit' for not
      --check-perm : check the sender permission when deploying contracts, 'with-perm' for checking permission, 'without-perm' for not
      --empty-block : consensus produces empty block, 'allow-empty' for allowing to produce empty block, 'notallow-empty' for not
      --gas-contract : register the gas contract by contract name
      --vrf-params : modify vrf parameters

**操作**:

.. code:: bash

      #注意一次只能操作一个系统参数
      ./platonecli sysconfig set  --audit-con audit  --keyfile ../conf/keyfile.json

**输出结果**:

以audit-con为例

.. code:: json

      {
        "status": "Operation Succeeded",
        "logs": [
            "Event IsApproveDeployedContract: 0 param set successful. "
        ],
        "blockNumber": 200,
        "GasUsed": 103352,
        "From": "0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18",
        "To": "0x1000000000000000000000000000000000000004",
        "TxHash": ""
      }

**操作**

.. code:: bash

      ./platonecli sysconfig set --vrf-params '{"electionEpoch":7,"nextElectionBlock":0,"validatorCount":3}' --keyfile ../conf/keyfile.json

8.2. 系统参数获取 sysconfig get
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**描述**: 系统参数获取。

**参数**:

- 可选参数:

.. code:: bash

      --block-gaslimit : the gas limit of the block
      --tx-gaslimit : the gas limit of transactions
      --tx-use-gas : if transactions use gas, 'use-gas' for transactions use gas, 'not-use' for not
      --audit-con : approve the deployed contracts, 'audit' for allowing contracts audit, 'not-audit' for not
      --check-perm : check the sender permission when deploying contracts, 'with-perm' for checking permission, 'without-perm' for not
      --empty-block : consensus produces empty block, 'allow-empty' for allowing to produce empty block, 'notallow-empty' for not
      --gas-contract : register the gas contract by contract name
      --vrf-param : get vrf parameters

**操作**:

.. code:: bash

      #注意一次只能获取一个系统参数
      ./platonecli sysconfig get  --audit-con  --keyfile ../conf/keyfile.json

**输出结果**:

.. code:: console

      # 以audit-con为例
      result:IsApproveDeployedContract: audit

**操作**

.. code:: bash

      ./platonecli sysconfig get --vrf-params  --keyfile ../conf/keyfile.json

.. _cli-ca:

9. CA操作 CA
=================

针对CA相关操作

9.1. 生成密钥对 CA generateKey
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**描述**: 生成密钥对并保存至本地。

**参数**:

- 可选参数:

.. code:: bash

      --private : while target is public, this flag is needed

- 必选参数:

.. code:: bash

      --file: file path and name
      --curve: SM2 or secp256k1
      --target : public, private or pair
      --format : PEM, HEX or TXT 

**操作**:

.. code:: bash

      #1. 根据私钥生成公钥（使用SM2，生成PEM格式）
      ./platonecli ca generateKey --curve SM2 --target public --format PEM --private key.PEM --file public.PEM

      #2. 根据给定的算法生成hex格式私钥
      ./platonecli ca generateKey --curve secp256k1 --target private --format HEX --file private.PEM

      #3. 生成公私钥对
      ./platonecli ca generateKey -curve SM2 --target pair --format PEM --file key.PEM

9.2. 生成证书签名请求文件 CA generateCSR
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**描述**: 生成证书签名请求文件并保存至本地。

**参数**:

- 必选参数:

.. code:: bash

      -- organization:                organization name
      -- commonName:                  user name
      -- dgst:                        Signature algorithm（sm3 or SHA256）
      -- file:                        file path and name（PEM）
      -- private:                     private key

**操作**:

.. code:: bash

      ./platonecli ca generateCSR --organization wxbc --commonName test --dgst sm3 --private key.PEM --file testCSR.PEM

9.3. 生成自签名证书 CA genSelfSignCert
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**描述**: 生成自签名证书文件并保存至本地。

**参数**:

- 必选参数:

.. code:: bash

      -- organization:                organization name
      -- commonName:                  user name
      -- dgst:                        Signature algorithm（sm3 or SHA256）
      -- serial:                      uint32
      -- file:                        target file path and name（PEM）
      -- private:                     private key

**操作**:

.. code:: bash

      ./platonecli ca genSelfSignCert --organization wxbc --commonName test --dgst sm3 --serial 1 --file testcert.PEM --private key.PEM

9.4. 根据CSR生成证书 CA generateCA
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**描述**: 生成证书文件并保存至本地。

**参数**:

- 必选参数:

.. code:: bash

      -- csr:                CSRfile path
      -- dgst:               Signature algorithm（sm3 or SHA256）
      -- serial:             uint32
      -- file:               target file path and name
      -- private:            keyfile
      -- ca:                 ca cert path # 发证方证书

**操作**:

.. code:: bash

      ./platonecli ca create --dgst sm3 --serial 2 --ca cacert.PEM --csr testCSR.PEM --file test1cert.PEM --private key.PEM

9.5. 验证证书 CA verifyCA
^^^^^^^^^^^^^^^^^^^^^^^^^^^

**描述**: 验证证书。

**参数**:

- 必选参数:

.. code:: bash

      -- ca: organization certificate
      -- cert: certificatefile path(cert)

**操作**:

.. code:: bash

      ./platonecli ca verify --ca cacert.PEM --cert test1cert.PEM 
