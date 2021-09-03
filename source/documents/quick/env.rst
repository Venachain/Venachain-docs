========
环境依赖
========

1. 系统要求
===========

本文档提供PlatONE在Linux环境下安装方式,在编译安装PlatONE之前,
需确保系统已经安装了如下环境:

-  gcc 7.3+
-  cmake 3.10+
-  go 1.11.4+ （需开启go mod 模式）

下表是单节点的配置要求，您可根据实际业务需求，合理配置机器资源。

==== ========= =========
资源 最低配置  推荐配置
==== ========= =========
CPU  1核1.5GHZ 4核2.4GHZ
内存 1GB       8GB
带宽 1Mb       10Mb
==== ========= =========

**操作系统要求**

-  CentOS 7.2+
-  Ubuntu16.04+
-  macOS 10.14+

2. 源码下载及编译
=================

首先用户需要从github下载PlatONE源码并编译

.. code:: bash

   # 获取PlatONE源码
   git clone --recursive https://github.com/PlatONEnterprise/PlatONE-Go.git
   export WORKSPACE=${PWD}/PlatONE-Go/release/linux
   # 编译PlatONE
   cd PlatONE-Go
   make all
   cd ..

编译完成后，会在release目录下生成搭链所需的材料。

如果编译失败，请确保您正确安装了所需的环境，然后重新尝试.