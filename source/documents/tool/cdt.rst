=======================
智能合约开发工具 (CDT)
=======================

**platone-CDT**\ 是WebAssembly(WASM)工具链和\ **platone**\ 平台智能合约开发工具集.

1. 编译
=======

1.1. 编译要求
^^^^^^^^^^^^^

-  GCC 7.4+ 或 Clang 7.0+
-  CMake 3.17+
-  Git
-  Python

1.2. 编译方法
^^^^^^^^^^^^^

Ubuntu
------

以下编译步骤在ubuntu 18.04下操作.

-  安装依赖

.. code:: sh

   sudo apt install build-essential cmake libz-dev libtinfo-dev

-  获取源码

   git clone https://git-c.i.wxblockchain.com/PlatONE/src/node/PlatONE-CDT.git


-  执行编译

.. code:: sh

   cd platone-CDT
   mkdir build && cd build
   cmake .. 
   make && make install

Windows
-------

Windows下编译需要先安装\ `MinGW-W64
GCC-8.1.0 <https://sourceforge.net/projects/mingw-w64/files/Toolchains%20targetting%20Win64/Personal%20Builds/mingw-builds/8.1.0/threads-posix/sjlj/x86_64-8.1.0-release-posix-sjlj-rt_v6-rev0.7z>`__,
且安装路径不能含有空格(即: 不能安装在”Program Files”或”Program
Files(x86)目录”), 否则编译失败.

-  获取源码

.. code:: bash

   git clone https://git-c.i.wxblockchain.com/PlatONE/src/node/PlatONE-CDT.git

-  执行编译

.. code:: bash

   cd platone-CDT
   mkdir build && cd build
   cmake -G "MinGW Makefiles" .. -DCMAKE_INSTALL_PREFIX="C:/platone.cdt" -DCMAKE_MAKE_PROGRAM=mingw32-make
   mingw32-make && mingw32-make install

2. 使用
=======

在使用platone-CDT之前必须将platone-CDT编译生成的执行文件路径加到PATH环境变量中.

2.1. 单文件项目
^^^^^^^^^^^^^^^

-  初始化项目

.. code:: sh

   platone-init -project=example -bare

-  编译WASM文件

.. code:: sh

   cd example
   platone-cpp -o example.wasm example.cpp -abigen

2.2. CMake项目
^^^^^^^^^^^^^^

-  初始化项目

.. code:: sh

   platone-init -project=cmake_example 

-  编译

   -  Linux

      .. code:: bash

         cd cmake_example/build
         cmake ..

   -  Windows>\ **编译依赖:**>+ `MinGW-W64
      GCC-8.1.0 <https://sourceforge.net/projects/mingw-w64/files/Toolchains%20targetting%20Win64/Personal%20Builds/mingw-builds/8.1.0/threads-posix/sjlj/x86_64-8.1.0-release-posix-sjlj-rt_v6-rev0.7z>`__>+
      CMake 3.5 or higher

      .. code:: bash

         cd cmake_example/build
         cmake .. -G "MinGW Makefiles" -Dplatone_CDT_ROOT=<cdt_install_dir>

3. 故障处理
===========

.. code:: console

   platone-init: /lib64/libstdc++.so.6: version `GLIBCXX_3.4.20' not found (required by platone-init)
   platone-init: /lib64/libstdc++.so.6: version `CXXABI_1.3.9' not found (required by platone-init)
   platone-init: /lib64/libstdc++.so.6: version `GLIBCXX_3.4.21' not found (required by platone-init)

gcc&g++版本太低导致，请升级版本

4. License
==========

GNU General Public License v3.0, see
`LICENSE <https://github.com/platonenetwork/platone-CDT/blob/master/LICENSE>`__.