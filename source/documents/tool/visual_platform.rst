============
可视化平台
============

1. 部署指南
=============

1.1. 前言
^^^^^^^^^^^^^

PlatONE可视化运维管理平台是集区块浏览器、系统配置管理、节点部署运维等功能于一体的管理平台。系统部署涉及多个组件的协调配合，此文档详细说明部署流程。

-  `系统架构设计 <https://git-c.i.wxblockchain.com/PlatONE/src/node/PlatONE-Go/blob/feature/precompiled-system-contract/cmd/data-manager/doc/PlatONE%E8%BF%90%E7%BB%B4%E7%AE%A1%E7%90%86%E5%B9%B3%E5%8F%B0%E6%9E%B6%E6%9E%84%E8%AE%BE%E8%AE%A1%E6%96%87%E6%A1%A3.md>`__

1.2. 节点部署
^^^^^^^^^^^^^^

在部署可视化平台前，必须有至少一个节点正在运行，由于可视化平台要支持节点部署与启动、多链群组管理等功能，因此需用最新的搭链脚本\ ``console.py``\ 进行节点部署。

注意：可视化平台只支持PlatONE 1.0及以后的版本。

-  `PlatONE新版群组console说明文档 <../命令行cmd/console操作文档.md>`__

1.2.1. PlatONE REST Server部署
----------------------------------

``PlatONE REST Server``\ 是提供与区块链交互服务的统一接口层，并提供账户密钥管理功能。

文件结构：

.. code:: console

   .
   ├── config.json
   ├── keystore
   │   ├── UTC--2020-09-11--0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18
   │   ├── UTC--2020-09-15--0x33091DfEE788E631e45157Bd9461306De5980032
   │   ├── UTC--2020-09-15--0x3f145FF5b017952Db91cD4C656B932B80f75681c
   │   └── UTC--2020-09-24--0xfFEA148706f854d3049fd016a93e4b8aF4de8B8c
   └── platonecli

启动方式：

.. code:: bash

   ./platonecli rest

服务启动后，默认监听端口7000，会读取\ ``./config.json``\ 获取默认配置（如果有,可以没有），内容如下:

.. code:: json

   {
     "url":"节点IP:节点RPC端口",
     "keystore":"DEFAULT_KEYSTORE_FILE",
     "account":"0xDEFAULT_ACCOUNT"
   }

其中，\ ``keystore``\ 和\ ``account``\ 是区块链管理员账户的\ ``密钥文件路径``\ 和\ ``账户地址``\ ，可以在\ ``节点data目录/keystore``\ 下找到，
文件名形如\ ``UTC--2020-09-11T11-50-43.133274056Z--8d4d2ed9ca6c6279bab46be1624cf7adbab89e18``

在\ ``PlatONE REST Server``\ 启动目录下新建\ ``keystore``\ 文件夹，将管理员密钥文件拷贝过去，现在管理员密钥就在本服务的控制下啦，请接着将相关信息写进\ ``config.json``\ 并启动服务。

-  `相关文档 <https://git-c.i.wxblockchain.com/PlatONE/doc/Dev/tree/develop/system-design/platonecli/system-design>`__

1.2.2. PlatONE Data Manager部署
------------------------------------

``PlatONE Data Manager``\ 负责持续从链上获取信息，并格式化为我们想要的数据格式，然后将其存储到DB中（目前仅支持mongodb），并为前端提供数据接口。

部署需要先安装\ ``mongodb``\ ，在当前目录配置文件中写入必要信息，然后启动。

文件结构：

.. code:: console

   .
   ├── config.toml
   └── datamanager

启动方式：

.. code:: bash

   ./datamanager

服务启动后默认监听端口8000

-  `PlatONE Data
   Manager部署文档 <https://git-c.i.wxblockchain.com/PlatONE/src/node/PlatONE-Go/blob/feature/precompiled-system-contract/cmd/data-manager/doc/%E9%83%A8%E7%BD%B2%E6%96%87%E6%A1%A3.md>`__
-  `PlatONE Data
   Manager设计文档合集 <https://git-c.i.wxblockchain.com/PlatONE/src/node/PlatONE-Go/tree/feature/precompiled-system-contract/cmd/data-manager/doc>`__

1.2.3. PlatONE API Server部署
---------------------------------

``PlatONE API Server``\ 负责为前端可视化页面提供后端接口服务。

文件结构：

.. code:: console

   .
   ├── config.toml
   ├── apiserver
   └── keys
       └── ca.cert

其中\ ``config.toml``\ 是配置文件，\ ``ca.cert``\ 是访问\ ``platone-monitor(下述)``\ 的证书文件。

启动方式：

.. code:: bash

   ## 在当前目录配置文件中填入必要信息，然后
   ./apiserver

-  `PlatONE API
   Server部署文档 <https://git-c.i.wxblockchain.com/PlatONE/src/node/platone-manager/platone-api-server/blob/master/README.md>`__
-  `PlatONE API
   Server接口文档 <https://git-c.i.wxblockchain.com/PlatONE/src/node/platone-manager/platone-api-server/blob/master/doc/api-server.md>`__

服务启动后，默认监听端口9999；初次启动时，需要初始化一个系统管理员账号，用GET方式访问初始化端口:

.. code:: bash

   curl http://localhost:9999/init?Name=admin&Password=admin&Address=0x8d4d2Ed9cA6c6279BaB46Be1624cF7ADbAB89E18&Passphrase=0

其中\ ``Name``\ 为账号，\ ``Password``\ 为登录密码，\ ``Address``\ 为管理员账户地址（PlatONE
REST
Server部署时配置过），\ ``Passphrase``\ 为解锁私钥文件的密码（默认是0）

1.2.4. 系统前端部署
--------------------

系统前端用vue框架开发，可以build出来用nginx等web服务器部署，或者直接用vue的serve服务启动部署，本文描述后者的部署方式。

1. 安装node和npm
2. 获取源代码

.. code:: bash

   git clone https://git-c.i.wxblockchain.com/PlatONE/src/node/platone-manager/platone-frontend.git

3. 编辑\ ``src/config.js``\ ，将\ ``dataUrl``\ 设置为\ ``PlatONE Data Manager``\ 的接口地址、将\ ``apiServerUrl``\ 设置为\ ``PlatONE API Server``\ 的接口地址。
4. 启动前端服务

.. code:: bash

   npm install
   npm run prod

前端默认端口为8080

-  `演示入口 <http://10.250.122.10:8080/>`__

1.2.5. PlatONE Monitor部署
-----------------------------

``PlatONE Monitor``\ 与节点部署在同一台服务器，对外提供本地节点部署、启动、停止等服务，即在每一个节点服务器上，都需要部署一个\ ``PlatONE Monitor``\ 服务。

文件结构：

.. code:: console

   .
   ├── config
   │   └── config.toml
   ├── keys
   │   ├── service.key
   │   └── service.pem
   └── monitor

其中\ ``config.toml``\ 是服务配置文件、\ ``keys``\ 下面的文件是TLS加密所需的证书，由前述\ ``PlatONE API Server``\ 中的\ ``ca.cert``\ 签发。

启动方式：

.. code:: bash

   ## 在当前目录配置文件中填入必要信息，然后
   ./monitor

-  `接口文档 <https://git-c.i.wxblockchain.com/PlatONE/src/node/platone-manager/platone-monitor/blob/master/server/proto/monitor.proto>`__

2. 使用说明
==============

2.1. 用户登陆
^^^^^^^^^^^^^^^

2.1.1. 用户登录
------------------

登陆网址http://10.250.122.10:8080/login 进入用户登陆界面

.. figure:: ../../images/tool/vp_login.png

系统部署初需要制定超级管理员用户名密码以及关联到链上链创建者的账户，这里用户名和密码均为admin

2.1.2 主页
---------------

主页展示信息如下： |main|

2.2. 用户管理
^^^^^^^^^^^^^^^^^^^

2.2.1. 添加用户
-------------------

1）点击左侧导航栏，选择“系统管理”，进入系统管理页面： |systemmanager|

2）在系统管理页面右上部分为用户管理模块，该模块如下： |usermanager|

3）点击右上角添加按钮，跳出添加用户所需填写的相关信息： |adduserform|

4）点击确认提交信息，提交后用户被正确添加： |adduserresult|

2.2.2. 更新用户
-----------------

1）在用户管理页面点击想要更改信息的用户名，进入用户信息更改页面：
|updateuserform|

2）可以修改用户密码及权限，如图所示，更改后点击确认，并得到结果：
|updateuserresult|

2.3. PlatONE系统参数设置
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1）点击左侧导航栏，选择“系统管理”，进入系统管理页面： |systemmanager|

2）在系统管理页面左上部分为PlatONE系统参数设置模块，该模块如下：
|systemconfig|

3）可在该模块修改系统参数设置并点击确认进行提交

.. tip:: ``TxGasLimit`` 需小于 ``BlockGasLimit``

2.4. 节点管理
^^^^^^^^^^^^^^^^^^^

2.4.1. 部署节点
---------------------

1）点击左侧导航栏，选择“系统管理”，进入系统管理页面： |systemmanager|

2）在系统管理页面左下部分为节点管理模块，该模块如下： |nodemanager|

3）点击右上角部署新节点，弹出节点部署窗口，可输入如下节点各类信息：
|addnode|

2.4.2. 添加节点准入
------------------------

1）点击右上角添加节点准入，弹出节点准入信息窗口 |nodepermission|

2）输入节点信息后点击确认

2.4.3. 节点管理（开启/关闭/重启）
-----------------------------------

1）点击某个节点，可针对该节点进行启动、停止和重启操作 |nodepop|

2.5. CNS管理
^^^^^^^^^^^^^^^^^^^

2.5.1. CNS列表
--------------------

1）点击左侧导航栏，选择“系统管理”，进入系统管理页面： |systemmanager|

2）在系统管理页面右下部分为CNS管理模块，下图为CNS列表页面： |cnslist|

2.5.2. CNS注册
---------------------

1）点击页面右上角添加新的CNS注册信息，弹出CNS注册窗口： |cnsregister|

2）注册成功后页面显示新的CNS信息： |cnsresult|

2.6. 区块浏览
^^^^^^^^^^^^^^^^^^^

2.6.1. 区块浏览
-------------------

1）点击左侧导航栏，选择“区块浏览”，进入区块浏览页面： |systemmanager|

2）区块浏览页面包含区块高度、出块节点、gas消耗等多维度的区块信息，页面如下：
|block|

3）点击某条区块信息可进入该区块交易的详情页面，在页面上侧还提供区块查询功能

2.7. 交易浏览
^^^^^^^^^^^^^^

2.7.1. 交易浏览
-------------------

1）点击左侧导航栏，选择“交易浏览”，进入交易浏览页面： |systemmanager|

2）交易浏览页面包含交易哈希、交易双方地址等多维度的区块信息，页面如下：
|tx|

3）点击某条交易可进入该交易的详情页面： |txdetail|

2.8. 合约浏览
^^^^^^^^^^^^^

2.8.1. 合约浏览
-------------------

1）点击左侧导航栏，选择“合约浏览”，进入合约浏览页面： |systemmanager|

2）合约浏览页面包含等多维度的区块信息，页面如下： |contract|

3）点击某条交易可进入该交易的详情页面： |contractdetail|

2.8.2. 合约部署
--------------------

1）点击左上合约部署按钮，弹出合约部署窗口： |deploy|

2）上传wasm和abi文件，点击确认部署合约

.. |main| image:: ../../images/tool/vp_main.png
.. |systemmanager| image:: ../../images/tool/vp_sysmanager.png
.. |usermanager| image:: ../../images/tool/vp_usermanager.png
.. |adduserform| image:: ../../images/tool/vp_adduserform.png
.. |adduserresult| image:: ../../images/tool/vp_adduserresult.png
.. |updateuserform| image:: ../../images/tool/vp_updateuserform.png
.. |updateuserresult| image:: ../../images/tool/vp_updateuserresult.png
.. |systemconfig| image:: ../../images/tool/vp_systemconfig.png
.. |nodemanager| image:: ../../images/tool/vp_nodemanager.png
.. |addnode| image:: ../../images/tool/vp_addnode.png
.. |nodepermission| image:: ../../images/tool/vp_nodepermission.png
.. |nodepop| image:: ../../images/tool/vp_nodeop.png
.. |cnslist| image:: ../../images/tool/vp_cnslist.png
.. |cnsregister| image:: ../../images/tool/vp_cnsregister.png
.. |cnsresult| image:: ../../images/tool/vp_cnsresult.png
.. |block| image:: ../../images/tool/vp_block.png
.. |tx| image:: ../../images/tool/vp_tx.png
.. |txdetail| image:: ../../images/tool/vp_txdetail.png
.. |contract| image:: ../../images/tool/vp_contract.png
.. |contractdetail| image:: ../../images/tool/vp_contractdetail.png
.. |deploy| image:: ../../images/tool/vp_deploy.png
