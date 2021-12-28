====================
合约开发(solidity)
====================

环境准备
========

Solidity合约开发和部署有两种方式：

1）Remix 是一个基于 Web 浏览器的 IDE，它可以让你编写 Solidity 智能合约，然后部署并运行该智能合约。在Remix网站上在线编写solidity代码并在线编译和部署到venachain节点上。 

2）下载solidity的编译器solc，编写完solidity源码后再使用solc编译成二进制文件，再手动部署到venachain节点上。

Ubutu系统下安装solc：

.. code:: bash

   sudo add-apt-repository ppa:ethereum/ethereum 
   sudo apt-get update 
   sudo apt-get install solc

`Remix网址 <https://remix.ethereum.org/>`__ |
`Remix离线版本 <https://github.com/ethereum/browser-solidity/tree/gh-pages>`__ |
`其他环境下solc安装方式 <https://solidity-cn.readthedocs.io/zh/develop/installing-solidity.html>`__ \

编写solidity合约
================

下面是一个简单solidity智能合约代码的例子：

.. code:: solidity

   pragma solidity ^0.4.0;

   contract SimpleStorage {     
       uint storedData; 

       function set(uint x) public {         
           storedData = x;     
       }     

       function get() public view returns (uint) {         
           return storedData;     
       } 
   }

-  第一行说明源代码使用Solidity版本0.4.0写的，并且使用0.4.0以上版本运行也没问题。这是为了确保合约不会在新的编译器版本中突然行为异常。
-  Solidity中合约的含义就是一组代码（函数)和数据（状态），它们位于以太坊区块链的一个特定地址上。代码行 ``uint storedData;`` 声明一个类型为 uint(256位无符号整数）的状态变量。你可以认为它是数据库里的一个位置，可以通过调用管理数据库代码的函数进行查询和变更。

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