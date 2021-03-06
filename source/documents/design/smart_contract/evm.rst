.. _evm:

===========
EVM虚拟机
===========

EVM原理
===========

以太坊上的虚拟机称之为EVM，是一个基于栈的虚拟机。现在有很多种以太坊智能合约语言，只要合约语言可以通过编译器编译成符合EVM虚拟机要求的特定指令集，就可以编写以太坊智能合约。现在最流行以太坊智能合约语言是Solidity。Solidity语言是一门静态语言，其编译器是solc。通过编译，Solidity语言变成特定格式的字节码，可以通过发送交易的方式可以把得到的字节码发送并存储到区块链上。

EVM 不是基于寄存器的，而是基于栈的，因此所有的计算都在一个被称为栈（stack） 的区域执行。栈最大有1024个元素，每个元素长度是一个字（256位）。对栈的访问只限于其顶端，限制方式为: 允许拷贝最顶端的16个元素中的一个到栈顶，或者是交换栈顶元素和下面16个元素中的一个。所有其他操作都只能取最顶的两个（或一个，或更多，取决于具体的操作）元素，运算后，把结果压入栈顶。当然可以把栈上的元素放到存储或内存中。但是无法只访问栈上指定深度的那个元素，除非先从栈顶移除其他元素。

指令集类型
^^^^^^^^^^^^^^

由于操作码被限制在一个字节以内，所以EVM指令集最多只能容纳256条指令。目前EVM已经定义了约142条指令，还有100多条指令可供以后扩展。这142条指令包括以下四大类: 

-  基本操作码：算术运算指令，比较操作指令，按位运算指令，栈，跳转指令；

-  密码学计算操作码：密码学计算指令；例如：keccak256（好像只有这一个）

-  存储相关操作码：memory，storage操作指令；例如：mstor, sstore, sload, mload等等
   
-  与区块链的直接接口操作码：call, blockhash, log, caller, timestamp等。

内存
^^^^^^^^

合约会试图为每一次消息调用获取一块被重新擦拭干净的内存实例。
内存是线性的，可按字节级寻址，但读的长度被限制为256位，而写的长度可以是8位或256位。当访问（无论是读还是写）之前从未访问过的内存字（word）时（无论是偏移到该字内的任何位置），内存将按字进行扩展（每个字是256位）。扩容也将消耗一定的gas。
随着内存使用量的增长，其费用也会增高（以平方级别）。在初始化内存时是不会进行预先分配任何内存空间的，而是虚拟机会在执行每个一个指令之前先计算一下执行这个指令现在的内存是否够用，如果不够用的话，就用以下命令进行扩容。

.. code:: go

   // Resize resizes the memory to size
   func (m *Memory) Resize(size uint64) {
       if uint64(m.Len()) < size {
           m.store = append(m.store, make([]byte, size-uint64(m.Len()))...)
       }
   }

部署合约
^^^^^^^^^^

Solidity源码编译成的字节码至少包含两个部分。第一部分的 ``.code`` 包含了一些智能合约初始化的代码，比如构造函数，state variable（全局变量）的赋值等操作。在部署合约时，会调用这部分代码进行初始化合约，并把返回的 ``runtime bytecode`` 永久存储到storage中。区块链浏览器，如Etherscan，默认是无法看到这部分的代码的。

调用合约
^^^^^^^^^^^^^

函数签名是一个4bytes的hash值，用来唯一标识智能合约中的函数。它是通过sha3(“functionName(type1,type2)”)，取前4bytes得到的。也就是说该函数签名只与函数名，形式参数类型有关。

从.data开始，是智能合约的runtime bytecode，也就是在区块链上保存的合约的bytecode。

这部分字节码的开头是整个合约的所有可调用函数的函数签名，在调用合约的函数时，首先通过calldata操作码读取调用函数的函数签名的前四个字节，然后EVM是从头开始线性的往下依次加载每个函数签名，并进行比较，如果函数签名一致的话，则通过jumpi指令跳转到相应的函数进行操作。在EVM中，回退函数是唯一一个未命名的函数，如果遍历完所有的函数签名也没有匹配的函数的话，则会调用回退函数从而退出整个调用过程。

案例
^^^^^^^

下面我们按照以下源码进行分析。

.. code:: bash

   pragma solidity ^0.5.11;
   contract SimpleStorage {
       uint storedData;

       function set(uint x) public {
           storedData = x;
       }

       function get() public view returns (uint) {
           return storedData;
       }
   }

源码编译后得到的二进制数据如下： 

.. code:: console

   608060405234801561001057600080fd5b5060c68061001f6000396000f3fe6080604052348015600f57600080fd5b506004361060325760003560e01c806360fe47b11460375780636d4ce63c146062575b600080fd5b606060048036036020811015604b57600080fd5b8101908080359060200190929190505050607e565b005b60686088565b6040518082815260200191505060405180910390f35b8060008190555050565b6000805490509056fea265627a7a72315820f7616ca7610ee51eb34eb9619c012a95b32e296d4fcdefb15c4c6051175c683964736f6c634300050b0032

把以上代码作为交易的data部署到链上，但是作为合约code存储到链上的数据，是以上数据的子集，我们称之为Runtime
ByteCode如下所示：

.. code:: console

   6080604052348015600f57600080fd5b506004361060325760003560e01c806360fe47b11460375780636d4ce63c146062575b600080fd5b606060048036036020811015604b57600080fd5b8101908080359060200190929190505050607e565b005b60686088565b6040518082815260200191505060405180910390f35b8060008190555050565b6000805490509056fea265627a7a72315820f7616ca7610ee51eb34eb9619c012a95b32e296d4fcdefb15c4c6051175c683964736f6c634300050b0032

下面我们按照源码的汇编表示来进行具体分析。

.. code:: bash

   .code
     PUSH 80           contract SimpleStorage {\n    ...
     PUSH 40           contract SimpleStorage {\n    ...
     MSTORE            contract SimpleStorage {\n    ...
     CALLVALUE             contract SimpleStorage {\n    ...
     DUP1          olidity ^
     ISZERO            a 
     PUSH [tag] 1          a 
     JUMPI             a 
     PUSH 0            a
     DUP1          n
     REVERT            .11;\ncontrac
   tag 1           a 
     JUMPDEST          a 
     POP           contract SimpleStorage {\n    ...
     PUSH #[$] 0000000000000000000000000000000000000000000000000000000000000000            contract SimpleStorage {\n    ...
     DUP1          contract SimpleStorage {\n    ...
     PUSH [$] 0000000000000000000000000000000000000000000000000000000000000000         contract SimpleStorage {\n    ...
     PUSH 0            contract SimpleStorage {\n    ...
     CODECOPY          contract SimpleStorage {\n    ...
     PUSH 0            contract SimpleStorage {\n    ...
     RETURN            contract SimpleStorage {\n    ...
   .data
     0:
       .code
         PUSH 80           contract SimpleStorage {\n    ...
         PUSH 40           contract SimpleStorage {\n    ...
         MSTORE            contract SimpleStorage {\n    ...
         CALLVALUE             contract SimpleStorage {\n    ...
         DUP1          olidity ^
         ISZERO            a 
         PUSH [tag] 1          a 
         JUMPI             a 
         PUSH 0            a
         DUP1          n
         REVERT            .11;\ncontrac
       tag 1           a 
         JUMPDEST          a 
         POP           contract SimpleStorage {\n    ...
         PUSH 4            contract SimpleStorage {\n    ...
         CALLDATASIZE          contract SimpleStorage {\n    ...
         LT            contract SimpleStorage {\n    ...
         PUSH [tag] 2          contract SimpleStorage {\n    ...
         JUMPI             contract SimpleStorage {\n    ...
         PUSH 0            contract SimpleStorage {\n    ...
         CALLDATALOAD          contract SimpleStorage {\n    ...
         PUSH E0           contract SimpleStorage {\n    ...
         SHR           contract SimpleStorage {\n    ...
         DUP1          contract SimpleStorage {\n    ...
         PUSH 60FE47B1         contract SimpleStorage {\n    ...
         EQ            contract SimpleStorage {\n    ...
         PUSH [tag] 3          contract SimpleStorage {\n    ...
         JUMPI             contract SimpleStorage {\n    ...
         DUP1          contract SimpleStorage {\n    ...
         PUSH 6D4CE63C         contract SimpleStorage {\n    ...
         EQ            contract SimpleStorage {\n    ...
         PUSH [tag] 4          contract SimpleStorage {\n    ...
         JUMPI             contract SimpleStorage {\n    ...
       tag 2           contract SimpleStorage {\n    ...
         JUMPDEST          contract SimpleStorage {\n    ...
         PUSH 0            contract SimpleStorage {\n    ...
         DUP1          contract SimpleStorage {\n    ...
         REVERT            contract SimpleStorage {\n    ...
       tag 3           function set(uint x) public {\...
         JUMPDEST          function set(uint x) public {\...
         PUSH [tag] 5          function set(uint x) public {\...
         ....
         JUMPI             ag
         PUSH 0            r
         DUP1          o
         REVERT            5.11;\ncontra
       tag 6           ag
         JUMPDEST          ag
         .....
         PUSH [tag] 7          function set(uint x) public {\...
         JUMP [in]         function set(uint x) public {\...
       tag 5           function set(uint x) public {\...
         JUMPDEST          function set(uint x) public {\...
         STOP          function set(uint x) public {\...
       tag 4           function get() public view ret...
         JUMPDEST          function get() public view ret...
         PUSH [tag] 8          function get() public view ret...
         PUSH [tag] 9          function get() public view ret...
         JUMP [in]         function get() public view ret...
       tag 8           function get() public view ret...
         JUMPDEST          function get() public view ret...
         .....
         RETURN            function get() public view ret...
       tag 7           function set(uint x) public {\...
         JUMPDEST          function set(uint x) public {\...
         .....
         JUMP [out]            function set(uint x) public {\...
       tag 9           function get() public view ret...
         JUMPDEST          function get() public view ret...
         ......
         JUMP [out]            function get() public view ret...
       .data

在开始处标识 ``.code`` 的部分就是我们前面说智能合约部署时进行初始化的代码。在EVM中0x40地址是一个被预留的地址，称之为“空内存地址”: 即内存中我们可以用来存储东西的地方，保证没有人会覆盖它（除非我们犯了错误）。而0x00到0x40之间的内存是用来保存计算哈希值，这个对于映射和其他类型的动态数据是必需的。

1) 要调用get()方法，需要根据sha3(“get()”)得到前4个字节，即函数签名6d4ce63c。
2) 在.code中的tag1，CALLDATASIZE会获取交易传入的参数长度，LT指令来比较是否小于4个字节，如果小于4个字节，则会跳转到tag2，整个合约运行完毕。这里的执行就是回退函数。
3) 如果不小于4个字节，则会继续执行CALLDATALOAD指令，CALLDATALOAD会把参数内容压入栈顶。
4) 然后在逻辑右移0xE0（224）位，原因是为了凑足256位。
5) 然后通过EQ指令，对比栈顶的两个数据是否一致，如果一直的话，跳转到相应的tag。如果不一致的话，继续向下执行下面的指令。
6) 找到了6d4ce63c函数签名的tag4，执行其代码。

在二进制的开头部分通常是用来判断一个函数是否是payable的。比如CALLVALUE指令会得到transacation是否发了eth，如果发了eth，ISZERO的结果就会是false，因此不会执行跳转。从这里可以看出来，对一个合约地址不可以同时进行转账和调用合约两项事情。

Venachain对EVM支持情况说明
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Venachain支持以太坊Byzantine的协议，后续更新的evm协议暂不支持（比如2019年更新的Constantinople、Istanbul等）。

在以太坊Byzantium版本之后，目前有以下几个版本，其中新增的字节码Venachain暂不支持

-  Constantinople(2019.1.16更新) Opcodes ``create2``, ``extcodehash``, ``shl``, ``shr`` and ``sar`` are available in assembly.

-  Petersburg(2019.2.28更新) The compiler behaves the same way as with constantinople.

-  Istanbul (2019.12.7更新) Opcodes ``chainid`` and ``selfbalance`` are available in assembly.

目前Venachain对solidity版本没有要求，0.4.x～0.6.x都可以使用，但是编译solidity合约时候需要明确指定EVM版本为Byzantium，因为目前

如果合约中涉及到Byzantium版本EVM不支持的功能，底层链也不会支持，变现为合约执行时gas耗尽。

参考资料
^^^^^^^^^^^^

1) https://solidity.readthedocs.io/en/v0.5.12/
2) http://remix.ethereum.org/#optimize=false&evmVersion=null&version=soljson-v0.5.11+commit.c082d0b4.js&appVersion=0.7.7
3) https://blog.csdn.net/Programmer_CJC/article/details/80218649
4) https://blog.csdn.net/notjusttech/article/details/80363911
5) https://arvanaghi.com/blog/reversing-ethereum-smart-contracts/
6) https://blog.trustlook.com/understand-evm-bytecode-part-1/
7) https://www.ratingtoken.net/news/41b22c70febd11e8a867795a7618abd3
8) http://qyuan.top/2019/09/12/evm/