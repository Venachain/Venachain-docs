=============
部署指南
=============

系统部署涉及多个组件的协调配合，此文档详细说明部署流程。关于系统的设计可浏览 :ref:`系统架构设计 <vp-structure>`

节点部署
=============

在部署可视化平台前，必须有至少一个节点正在运行。节点部署方式可以参考 :ref:`正常部署 <normal-deploy>` 。

.. note:: 可视化平台只支持PlatONE 1.0及以后的版本。


1. PlatONE REST Server部署
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``PlatONE REST Server`` 是提供与区块链交互服务的统一接口层，并提供账户密钥管理功能。

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

服务启动后，默认监听端口7000，会读取 ``./config.json`` 获取默认配置（如果有），内容如下:

.. code:: json

   {
     "url":"节点IP:节点RPC端口",
     "keystore":"DEFAULT_KEYSTORE_FILE",
     "account":"0xDEFAULT_ACCOUNT"
   }

其中， ``keystore`` 和 ``account`` 是区块链管理员账户的 ``密钥文件路径`` 和 ``账户地址`` ，可以在 ``节点data目录/keystore`` 下找到，
文件名形如 ``UTC--2020-09-11T11-50-43.133274056Z--8d4d2ed9ca6c6279bab46be1624cf7adbab89e18``

在 ``PlatONE REST Server`` 启动目录下新建 ``keystore`` 文件夹，将管理员密钥文件拷贝过去，现在管理员密钥就在本服务的控制下啦，请接着将相关信息写进 ``config.json`` 并启动服务。

- :ref:`platonecli说明文档 <tool-platonecli>`

2. PlatONE Data Manager部署
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

2.1. 数据库启动
--------------------

``PlatONE Data Manager`` 负责持续从链上获取信息，并格式化为我们想要的数据格式，然后将其存储到DB中（目前仅支持mongodb），并为前端提供数据接口。

部署需要先安装 ``mongodb`` ，在当前目录配置文件中写入必要信息，然后启动。

文件结构：

.. code:: console

   .
   ├── config.toml
   └── datamanager

启动方式：

.. code:: bash

   ./datamanager

服务启动后默认监听端口8000

2.2. 数据库配置
---------------------

**创建数据库用户**


创建用户例子如下。登录mongo客户端。然后运行如下命令，用来创建用户。

.. code:: bash

   use admin
   db.createUser(
        {
            user:"root",
            pwd:"root",
            roles:[{role:"root",db:"admin"}]
       }
   )

**创建数据库**

.. code:: bash

   use data-mamager

2.3. 配置文件
-----------------

.. code:: console

   [http]
   ip = "127.0.0.1" #监听ip
   port = 7000 #监听端口
   debug = true #是否开启debug模式

   [log]
   # trace < debug < info < warn < error < fatal < panic
   level = "debug"
   # std | file
   output = "std"
   filepath = "./data-manager.log"

   [db]
   ip = "127.0.0.1"
   port = "27017"
   username = "root"
   password = "root"
   dbname = "data-manager"

   [sync]
   interval = 5 #同步区块链数据的周期间隔时间。单位：秒
   # 访问哪些区块链节点的rpc接口。可以设置多个接口，同步程序回在每次同步时都随机选取一个。
   urls = [
       "http://10.250.122.10:6791"
   ]

   [sync-tx-count]
   when="00:00:30" # 什么时候统计昨天的交易总量，并记录到数据库
   try_times=1 # 如果统计失败，可以重试的次数

   [chain]
   id=300 # 同步的区块链的链ID
   node_rest_server="http://10.250.122.10:8000" # 区块链rest-api接口地址
   node_rpc_address="http://10.250.122.10:6791" # 统计指定区块链rest-api程序访问哪个区块链节点

3. PlatONE API Server部署
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``PlatONE API Server`` 负责为前端可视化页面提供后端接口服务。

文件结构：

.. code:: console

   .
   ├── config.toml
   ├── apiserver
   └── keys
       └── ca.cert

其中 ``config.toml`` 是配置文件， ``ca.cert`` 是访问 ``platone-monitor(下述)`` 的证书文件。

启动方式：

.. code:: bash

   ## 在当前目录配置文件中填入必要信息，然后
   ./apiserver

服务启动后，默认监听端口9999；初次启动时，需要初始化一个系统管理员账号，用GET方式访问初始化端口:

.. code:: bash

   curl http://localhost:9999/init?Name=admin&Password=admin&Address=0x8d4d2Ed9cA6c6279BaB46Be1624cF7ADbAB89E18&Passphrase=0

其中 ``Name`` 为账号， ``Password`` 为登录密码， ``Address`` 为管理员账户地址（PlatONE REST Server部署时配置过）， ``Passphrase`` 为解锁私钥文件的密码（默认是0）

4. 系统前端部署
^^^^^^^^^^^^^^^^^^^^

系统前端用vue框架开发，可以build出来用nginx等web服务器部署，或者直接用vue的serve服务启动部署，本文描述后者的部署方式。

1) 安装node和npm

2) 获取源代码

.. code:: bash

   git clone https://git-c.i.wxblockchain.com/PlatONE/src/node/platone-manager/platone-frontend.git

3) 编辑 ``src/config.js`` ，将 ``dataUrl`` 设置为 ``PlatONE Data Manager`` 的接口地址、将 ``apiServerUrl`` 设置为 ``PlatONE API Server`` 的接口地址。

4) 启动前端服务

.. code:: bash

   npm install
   npm run prod

前端默认端口为8080

5. PlatONE Monitor部署
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``PlatONE Monitor`` 与节点部署在同一台服务器，对外提供本地节点部署、启动、停止等服务，即在每一个节点服务器上，都需要部署一个 ``PlatONE Monitor`` 服务。

文件结构：

.. code:: console

   .
   ├── config
   │   └── config.toml
   ├── keys
   │   ├── service.key
   │   └── service.pem
   └── monitor

其中 ``config.toml`` 是服务配置文件、 ``keys`` 下面的文件是TLS加密所需的证书，由前述 ``PlatONE API Server`` 中的 ``ca.cert`` 签发。

启动方式：

.. code:: bash

   ## 在当前目录配置文件中填入必要信息，然后
   ./monitor