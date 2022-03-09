=============================================
Wasm合约开发工具 (Venahcain-CDT)
=============================================

**Venachain-CDT** 是WebAssembly(WASM)工具链和 **Venachain** 平台智能合约开发工具集。主要功能如下：

- 创建C++合约模版；
- 将C++合约编译为wasm格式文件，并生成对应合约的ABI文件。

安装
=====

有两种安装方式：

- 方式一：使用已编译版本。将本项目的release对应操作系统版本下的压缩包解压至 ``/usr/local/`` 下，或将解压目录加入到PATH环境变量中即可。
- 方式二：手动编译安装。按照下文 :ref:`编译步骤 <venachain-cdt-build>` 进行手动编译安装。

.. note:: 由于方式二的编译时间可能较长，推荐使用方式一。

.. _venachain-cdt-build:

编译
=====

编译要求
^^^^^^^^

- gcc&g++ 7.4+ 或 Clang 7.0+
- CMake 3.17+
- Git
- Python
- 若在windows环境编译，则要求：
    
  + 安装 `MinGW-W64 GCC-8.1.0 <https://sourceforge.net/projects/mingw-w64/files/Toolchains%20targetting%20Win64/Personal%20Builds/mingw-builds/8.1.0/threads-posix/sjlj/x86_64-8.1.0-release-posix-sjlj-rt_v6-rev0.7z>`__ , 且安装路径不能含有空格(即: 不能安装在 ``Program Files`` 或 ``Program Files(x86)`` 目录), 否则可能导致编译失败.
  + CMake 3.5+

Linux
^^^^^^

**安装依赖**

- Ubuntu

.. code:: bash

    sudo apt install build-essential cmake libz-dev libtinfo-dev libncurses5-dev

- CentOS

.. code:: bash

    sudo yum install gcc g++ make ncurses-devel zlib-devel

**获取源码**

.. code:: bash

    git clone https://git-c.i.wxblockchain.com/vena/src/Venachain-CDT.git

**执行编译**

.. code:: bash

    cd Venachain-CDT
    mkdir build && cd build
    cmake .. 
    make && make install

MacOS
^^^^^^^

**获取源码**

.. code:: bash

    git clone https://git-c.i.wxblockchain.com/vena/src/Venachain-CDT.git

**执行编译**

.. code:: bash

    cd Venachain-CDT
    mkdir build && cd build
    cmake ..
    make && make install

Windows
^^^^^^^^^

**获取源码**

.. code:: bash

    git clone https://git-c.i.wxblockchain.com/vena/src/Venachain-CDT.git

**执行编译**

.. code:: bash

    cd Venachain-CDT
    mkdir build && cd build
    cmake -G "MinGW Makefiles" .. -DCMAKE_INSTALL_PREFIX="C:/venachain.cdt" -DCMAKE_MAKE_PROGRAM=mingw32-make
    mingw32-make && mingw32-make install

使用
=====

在使用Venachain-CDT之前须将Venachain-CDT编译生成的执行文件路径加到PATH环境变量中。

C++合约项目分为两种类型：

- 单文件项目：即合约项目中只包含一个C++合约文件。
- 多文件项目：若合约文件涉及引用其他多个C++文件，则需要使用该类型。该类型项目采用 ``makefile`` 方式将合约编译为wasm文件。采用该种方式编译合约时，需将本项目下的 ``release`` 目录中的内容解压到 ``/usr/local`` 目录下，或采用上述 :ref:`手动编译安装 <venachain-cdt-build>` 的方式安装Venachain-CDT。

单文件项目
^^^^^^^^^^

**初始化项目**

.. code:: bash

    venachain-init -project=example -bare

**编译WASM文件**

.. code:: bash

    cd example
    venachain-cpp -o example.wasm example.cpp -abigen

多文件项目
^^^^^^^^^^

**初始化项目**

.. code :: bash

    venachain-init -project=cmake_example 

**编译WASM文件**

- Linux

.. code:: bash

    cd cmake_example/build
    cmake ..
    make

- MacOS

.. code:: bash

    cd cmake_example/build
    cmake ..
    make

- Windows

.. code:: bash

    cd cmake_example/build
    cmake .. -G "MinGW Makefiles" -Dvenachain_CDT_ROOT=<cdt_install_dir>    

Q&A
=====

1) 遇到以下问题：

.. code:: console

    venachain-init: /lib64/libstdc++.so.6: version `GLIBCXX_3.4.20' not found (required by venachain-init)
    venachain-init: /lib64/libstdc++.so.6: version `CXXABI_1.3.9' not found (required by venachain-init)
    venachain-init: /lib64/libstdc++.so.6: version `GLIBCXX_3.4.21' not found (required by venachain-init)

**原因与解决方法**：gcc和g++版本太低导致，请升级版本

2) 遇到以下问题：

.. code:: console

    can not find libtinfo/ can not fond libncurses

**原因与解决方法**：首先确认有没有安装 ``libncurses`` ，在 ``/usr/lib64/`` 下：

- 如果没有，执行 

.. code:: bash

    yum -y install libncurses.so.5

- 若有，则执行 

.. code:: bash

    ln -s libncurses.so  libncurses.so.5.9``

3) MacOS系统版本比较高时，需要升级boost。

例如 MacOS Big Sur 版本 11.6.1， ``CMakeModules/BoostExternalProject.txt`` 的 ``ExternalProject_Add`` 模块的内容改为：

.. code:: console

    ExternalProject_Add(
        boost
        URL https://sourceforge.net/projects/boost/files/boost/1.77.0/boost_1_77_0.tar.bz2/download
        URL_MD5 09dc857466718f27237144c6f2432d86
        CONFIGURE_COMMAND ./bootstrap.sh --with-toolset=clang --with-libraries=filesystem,system,log,thread,random,exception
        BUILD_COMMAND ./b2 link=static ${BUILD_COMMAND_EXTRA}
        BUILD_IN_SOURCE 1
        INSTALL_COMMAND ""
        BUILD_ALWAYS 1
    )

4) 编译项目时，出现 ``boost_1_69_0.tar.ba2`` 下载慢或者下载失败的情况。

**解决方法**：先停止当前 ``make`` 流程，然后手动在 `官网 <https://boostorg.jfrog.io/artifactory/main/release/1.69.0/source/>`__ 下载 ``boost_1_69_0.tar.ba2`` ，放到项目 ``build/thirdparty/Download/boost/`` 目录下,然后继续执行 ``make`` 。