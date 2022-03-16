============
Graces 安装
============

环境准备
============

硬件环境准备
^^^^^^^^^^^^

- 主  机：Intel Xeon E5-2650或以上。
- 内  存：16G或以上。
- 硬  盘：32GB或以上。
- 图形卡：VGA/DVI。

软件环境准备
^^^^^^^^^^^^

- OS	Linux 64bit
- Golang 1.14.4 或以上
- Venachain v1.0.0 或以上
- MongoDB 4.2.8 或以上
- npm 6.14.13 或以上
- nodjs v14.17.1 或以上
- Graces v1.0.0

安装步骤
==========

1. 下载、安装和配置 Golang
^^^^^^^^^^^^^^^^^^^^^^^^^^^

2. 下载、安装和启动 MongoDB
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

3. 下载和编译 Venachain
^^^^^^^^^^^^^^^^^^^^^^^^^

Venachain仓库地址：https://github.com/Venachain/Venachain

安装方法请见 :ref:`Venachain安装教程 <deploy-env>`

4. 下载 Graces V1.0.0
^^^^^^^^^^^^^^^^^^^^^^^^^^

下载后端源码：https://github.com/Venachain/graces-server/releases/tag/v1.0.0

下载前端源码：https://github.com/Venachain/graces-web/releases/tag/v1.0.0

下载完成后对项目文件进行解压，将后端目录重命名为 ``graces-server`` ，将前端目录重命名为 ``graces-web`` 。

5. 配置 Graces 前端
^^^^^^^^^^^^^^^^^^^^^^

进到 graces-web 目录下，修改 ``.env.development`` 文件中 base api 下的 ``localhost:9999`` 为自己机器的 IP 端口号或域名端口号，如果不修改则默认使用 ``localhost:9999`` 。

.. code:: console

   # base api
   VUE_APP_BASE_API = 'http://localhost:9999/api'
   VUE_APP_BASE_WS = 'ws://localhost:9999/api'

6. 配置 Graces 后端
^^^^^^^^^^^^^^^^^^^^^^^^

1) 进到 graces-server 目录下。

2) 在 ``go.mod`` 文件中配置好对 Venachain 的依赖，修改为自己下载好且编译好后的 Venachain 路径.

.. code:: console

    replace (
        github.com/Venachain/Venachain => ${Venachain路径}
    )

3) 在 ``config.toml`` 文件中配置 Graces 运行的 IP 地址和端口号，本示例前端运行在 http://localhost:8080 中。

.. note:: cors 的值必须是 Graces 前端的运行地址

.. code:: toml

    [http]
    ip = "127.0.0.1"
    port = "9999"
    # mode 必须是 "release"、"debug"、"test" 中的一个
    mode = "debug"
    # cors 跨域资源共享白名单
    cors = "http://localhost:8080"

4) 在 ``config.toml`` 文件中配置 Graces 所需的 MongoDB 信息，如有需要则修改为自己的 MongoDB 配置。

.. code:: toml

    [db]
    ip = "127.0.0.1"
    port = "27017"
    username = "username"
    password = "password"
    dbname = "graces"
    timeout = 10

7. 启动 Graces
^^^^^^^^^^^^^^^^^

1) 启动 Graces 后端

进到 graces-server 目录下，执行以下命令

.. code:: bash

    go build -o graces
    nohup ./graces > ./graces.log 2>&1 &

2) 启动 Graces 前端

进到 graces-web 目录下，执行以下命令

.. code:: bash

    npm install
    npm run dev

8. 访问 Graces
^^^^^^^^^^^^^^^^

开打浏览器，输入 http://localhost:8080 便可以进到 Graces 主页面。

.. figure:: ../../../images/tool/graces/index.png