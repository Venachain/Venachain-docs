====================
合约开发(solidity)
====================

简介
========
Solidity是一种智能合约高级语言，它的语法接近于Javascript，运行在Ethereum虚拟机（EVM）之上。

环境准备
========

Solidity合约开发和部署有3种方式，可以选择其中一种：

.. note:: 推荐第一种方式，采用Remix部署和调用solidity合约更加方便。

Remix方式
^^^^^^^^^^^

Remix是一个基于 Web 浏览器的 IDE，它可以让你编写 Solidity 智能合约，然后部署并运行该智能合约。在Remix网站上在线编写solidity代码并在线编译和部署到Venachain节点上。

`Remix网址 <https://remix.ethereum.org/>`__ |
`Remix离线版本 <https://github.com/ethereum/browser-solidity/tree/gh-pages>`__

vcl方式
^^^^^^^^^^

下载solidity的编译器solc，编写完solidity源码后再使用solc编译成二进制文件，再手动部署到Venachain节点上。

Ubutu系统下安装solc：

.. code:: bash

    sudo add-apt-repository ppa:ethereum/ethereum 
    sudo apt-get update 
    sudo apt-get install solc

`其他环境下solc安装方式 <https://solidity-cn.readthedocs.io/zh/develop/installing-solidity.html>`__ 

truffer方式
^^^^^^^^^^^^^^

环境要求：

- node & npm：node版本要求 `12.22.0 <https://nodejs.org/download/release/v12.22.0/>`__

.. _evm-contract-truffle-build1:

**安装方式一：直接安装**

1. 通过npm安装：

.. code:: bash

    npm install -g truffle 

2. 初始化空白模版：

.. code:: bash

    # filepath 为可选项，即合约生成文件存放的路径
    truffle init [filepath] 

.. _evm-contract-truffle-build2:

**安装方式二：源码编译**

1. 下载truffle项目源码： `源码链接 <https://github.com/trufflesuite/truffle/tree/v5.5.0>`__

2. 编译项目：

.. code:: bash

    # 安装yarn
    npm install -g yarn

    # 在truffle项目根目录下执行编译
    yarn

3. 初始化空白模版：

.. code:: bash

    # 在truffle项目根目录下执行。其中，filepath 为可选项，即合约生成文件存放的路径
    yarn run truffle init [filepath]
    
生成的合约项目文件目录结构如下：

.. code:: console

    |-contracts/ ：存放合约
            |—Migrations.sol：通常不会修改，管理和更新已部署合约的状态
    |-migrations/：存放编译和部署合约的脚本,脚本前面会有数字编号，执行的时候会按照数字按顺序执行
            |--1_initial_migration.js：用于部署Migrations.sol
    |-test/：存放测试合约或者DAPP的测试用例
    |-truffle-config.js:该文件用于配制truffle项目，例如区块链客户端

可在生成的空白模版上修改合约。

编写solidity合约
================

下面是一个简单solidity智能合约代码的例子：

.. code:: solidity

    pragma solidity^0.5.0;

    contract createfunc{
        uint public a;
        constructor(uint _a)public{
            a=_a;
        }
        function get()public view returns(uint){
            return a;
        }
        function set(uint _a) public{
            a=_a;
        }
    }

-  第一行说明源代码使用Solidity版本0.5.0写的，并且使用0.5.0以上版本运行也没问题。这是为了确保合约不会在新的编译器版本中突然行为异常。
-  Solidity中合约的含义就是一组代码（函数)和数据（状态），它们位于以太坊区块链的一个特定地址上。 代码行 ``uint storedData`` ;声明一个类型为 uint (256位无符号整数）的状态变量。 你可以认为它是数据库里的一个位置，可以通过调用管理数据库代码的函数进行查询和变更。
-  ``constructor`` 标记合约的构造函数。

由于Solidity是一个静态类型的语言，所以编译时需明确指定变量的类型（包括本地变量或状态变量），Solidity编程语言提供了一些基本类型(elementary types)可以用来组合成复杂类型。

solidity值类型包括：

-  布尔(Booleans)
-  整型(Integer)
-  地址(Address)
-  定长字节数组(fixed byte arrays)
-  有理数和整型(Rational and Integer Literals，String literals)
-  枚举类型(Enums)
-  函数(Function Types)

复杂类型，占用空间较大的。在拷贝时占用空间较大。所以考虑通过引用传递。常见的引用类型有：

-  不定长字节数组（bytes）
-  字符串（string）
-  数组（Array）
-  结构体（Struts）

更多solidity语法参见：`solidy语法 <https://www.tryblockchain.org/>`__

