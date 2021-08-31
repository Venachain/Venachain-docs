========
智能合约
========

1. 安装 CDT 工具 
================

platone-CDT是WebAssembly(WASM)工具链和platone平台智能合约开发工具集。

具体安装与使用的教程，参照 \ `CDT 工具(Contract Development Toolkit) <../tool/cdt.html>`__\ 。

2. 合约开发
===========

2.1. cpp合约开发
^^^^^^^^^^^^^^^^

本文档通过一系列代码示例讲解合约的各个功能，用户可以通过学习这些例子来深入理解如何编写一个应用合约。

2.1.1. Contract类
-----------------

Contract类是有bcwasm库提供的合约基类，用户开发的合约必须派生于该类。Contract类中定义了一个\ ``init()``\ 虚函数，用户合约需要实现该\ ``init()``\ 函数，该函数在合约首次发布时执行，仅调用一次，该方法作用类似于solidity合约中的构造函数。

.. warning:: ``init()``\ 方法必须实现为\ ``public``\ 类型，这样合约在部署时才能够调用该函数来初始化合约数据。

.. code:: cpp

   #include <bcwasm/bcwasm.hpp>
   namespace my_namespcase {
       class my_contract : public bcwasm::Contract
       {
           public:
           my_contract(){}
           /// 实现父类: bcwasm::Contract 的虚函数
           /// 该函数在合约首次发布时执行，仅调用一次
           void init() 
           {
               /* 做一些初始化操作 */
           }
       };
   }

2.1.2. 合约外部方法
-------------------

合约外部方法指的是合约可以在外部调用的接口，功能类似与solidity合约中的\ ``public``\ 类型方法,在bcwasm库中是通过\ ``BCWASM_ABI``\ 宏来定义外部方法。通过\ ``BCWASM_ABI``\ 声明的方法，可以在合约外部通过rpc消息调用、也可以被其他合约调用。

.. code:: cpp

   #include <bcwasm/bcwasm.hpp>
   namespace my_namespcase {
       class my_contract : public bcwasm::Contract
       {
           public:
           my_contract(){}
           /// 实现父类: bcwasm::Contract 的虚函数
           /// 该函数在合约首次发布时执行，仅调用一次
           void init() 
           {
               /* 做一些初始化操作 */
           }
           void setData(char * data){
               /* 修改合约数据 */
           }
       };
   }
   // 外部方法
   BCWASM_ABI(my_namespcase::my_contract:setData)

2.1.3. 链上存储接口
-------------------

Wasm合约内置库为数据的持久化提供了\ ``setState()``\ 方法，可以通过调用\ ``bcwasm::setState()``\ 函数实现数据持久化，相应地查询调用\ ``bcwasm::getState()``\ 。

下方的示例合约中，有两个接口供外部调用\ ``setData(char* data)``\ 和\ ``getData()``\ 。这两个方法分别调用了\ ``bcwasm::setState()``\ 、\ ``bcwasm::getState()``\ ，以实现数据的上链持久化和查询。

.. code:: cpp

   #include <bcwasm/bcwasm.hpp>
   namespace my_namespcase {
       class my_contract : public bcwasm::Contract
       {
           public:
           void init(){}
           void setData(char * data){
               std::string m_data(data);
               bcwasm::setState("DataKey", m_data);
           }
           const char* getData() const{
               std::string m_data;
               bcwasm::getState("DataKey", m_data);
               return m_data.c_str();
           }
       };
   }
   // 外部方法
   BCWASM_ABI(my_namespcase::my_contract, setData)
   BCWASM_ABI(my_namespcase::my_contract, getData)

2.1.4. Const方法
----------------

合约中的\ ``const``\ 类型方法提供对合约状态的只读操作，该类型声明的函数不能够修改合约数据，一般用来查询合约的链上数据。下方代码中，\ ``getData()``\ 即为const方法，用于查询数据。

.. code:: cpp

   const char* getData() const{
       std::string m_data;
       bcwasm::getState("DataKey", m_data);
       // 读取合约数据并返回
       return m_data.c_str();
   }

2.1.5. Struct & Map
-------------------

Struct 结构体
>>>>>>>>>>>>>

结构体语法规则与C++一致，但是如果用于需要将结构数据上链持久化，则需要在结构体中使用\ ``BCWASM_SERIALIZE``\ 宏，该宏为结构体类型提供了序列化/反序列化的方式。

下方的合约示例中，定义了一个\ ``Student_t``\ 结构体类型，并通过合约接口\ ``setData()``\ 将数据持久化到链上，然后可以通过\ ``getData()``\ 方法查询数据。

.. code:: cpp

   #include <bcwasm/bcwasm.hpp>
   namespace my_namespcase {
       struct Student_t
       {
           std::string name;       // 姓名
           int64_t age;            // 年龄
           BCWASM_SERIALIZE(Student_t, (name)(age));
       };
       class my_contract : public bcwasm::Contract
       {
           public:
           void init(){}
           void setData(char * name, int64_t age){
               Student_t stu;
               stu.name = std::string (name);
               stu.age = age;
               bcwasm::setState("DataKey", stu);
           }
           const char* getData() const{
               Student_t stu;
               bcwasm::getState("DataKey", stu);
               std::stringstream ret;
               ret << "stu.name: " << stu.name << ", stu.age: " << stu.age;
               // 读取合约数据并返回
               return ret.str().c_str();
           }
       };
   }
   // 外部方法
   BCWASM_ABI(my_namespcase::my_contract, setData)
   BCWASM_ABI(my_namespcase::my_contract, getData)

Map
>>>

bcwasm中提供了map类型的封装，定义map结构时，需要指定map的名称、key的类型、value的类型。

.. code:: cpp

   char mapName[] = "students";
   bcwasm::db::Map<mapName, std::string, Student_t> students;

map结构支持如下的几种api：

-  ``find(key)``: 根据key查找value
-  ``insert(key, value)``:
   当map中还没有以key为索引的内容时，插入以key为索引的value
-  ``update(key, value)``\ ：
   当map中已经存在以key为索引的内容时，更新key对应的value

下方的示例合约中定义了一个map用于保存学生的姓名、年龄信息，以学生姓名为key作为索引，其中\ ``setData``\ 方法输入学生的姓名、年龄，\ ``getData``\ 方法根据姓名查询学生的年龄。

.. code:: cpp

   #include <bcwasm/bcwasm.hpp>
   namespace my_namespcase {
       struct Student_t
       {
           std::string name;       // 姓名
           int64_t age;            // 年龄
           BCWASM_SERIALIZE(Student_t, (name)(age));
       };
       // 定义一个map，保存学生姓名、年龄信息，以学生姓名为key作为索引
       char mapName[] = "students";
       bcwasm::db::Map<mapName, std::string, Student_t> students;

       class my_contract : public bcwasm::Contract
       {
           public:
           void init(){}
           void setData(char * name, int64_t age){
               Student_t stu;
               stu.name = std::string (name);
               stu.age = age;
               Student_t *stu_p = students.find(std::string(name));
               if (stu_p == nullptr){
                   students.insert(stu.name, stu);
               } else{
                   students.update(stu.name, stu);
               }
           }
           const char* getData(char* name) const{
               Student_t *stu = students.find(std::string(name));
               if (stu == nullptr){
                   return (std::string("no such student")).c_str();
               }else{         
                   std::stringstream ret;
                   ret << "stu.name: " << stu->name << ", stu.age: " << stu->age;
                   return ret.str().c_str();
               }
           }
       };
   }
   // 外部方法
   BCWASM_ABI(my_namespcase::my_contract, setData)
   BCWASM_ABI(my_namespcase::my_contract, getData)

2.1.6 Event
-----------

Event允许我们方便地使用PlatONE的日志基础设施。我们可以在dapp中监听Event，当合约中产生Event时，会使相关参数被存储到交易的Log中。这些Log与地址相关联，被写入区块链中，可以通过交易Receipt查询某个交易所产生的Event。

宏\ ``BCWASM_EVENT``\ 和\ ``BCWASM_EMIT_EVENT``\ 提供了对合约Event的直接支持，使用方法如下：

.. code:: cpp

   /// 定义Event.
   /// BCWASM_EVENT(eventName,arguments...)
   BCWASM_EVENT(setData,const char *,const int64_t)
   /// 触发Event
   BCWASM_EMIT_EVENT(setData,name,age);

我们在示例合约中加入Event事件，每次调用\ ``setData()``\ 时，触发Event事件，示例合约代码如下所示：

.. code:: cpp

   #include <bcwasm/bcwasm.hpp>
   namespace my_namespcase {
       struct Student_t
       {
           std::string name;       // 姓名
           int64_t age;            // 年龄
           BCWASM_SERIALIZE(Student_t, (name)(age));
       };
       // 定义一个map，保存学生姓名、年龄信息，以学生姓名为key作为索引
       char mapName[] = "students";
       bcwasm::db::Map<mapName, std::string, Student_t> students;

       class my_contract : public bcwasm::Contract
       {
           public:
           void init(){}
           // 定义Event
           BCWASM_EVENT(setData,const char*,int64_t)

           void setData(char * name, int64_t age){
               Student_t stu;
               stu.name = std::string(name);
               stu.age = age;
               Student_t *stu_p = students.find(std::string(name));
               if (stu_p == nullptr){
                   students.insert(stu.name, stu);
               } else{
                   students.update(stu.name, stu);
               }
               /// 触发Event
               BCWASM_EMIT_EVENT(setData,name,age);
           }
           const char* getData(char * name) const{
               Student_t *stu = students.find(std::string(name));
               if (stu == nullptr){
                   return (std::string("no such student")).c_str();
               }else{         
                   std::stringstream ret;
                   ret << "stu.name: " << stu->name << ", stu.age: " << stu->age;
                   return ret.str().c_str();
               }
           }
       };
   }
   // 外部方法
   BCWASM_ABI(my_namespcase::my_contract, setData)
   BCWASM_ABI(my_namespcase::my_contract, getData)

加入Event后我们再次调用\ ``setData``\ 合约，然后查询交易的Receipt，如下所示。

.. code:: java

   {
     blockHash: "0xd3324a86bb4c2a9f99592ea16c02bddae6ced421c0170a07f781fb9dfa7b1d8c",
     blockNumber: 77,
     contractAddress: null,
     cumulativeGasUsed: 449872,
     from: "0x61eaf416482341e706ff048f20491cf280bc29d6",
     gasUsed: 449872,
     logs: [{
         address: "0x07894a9f9edffe4b73eb8928f76ee2993039e4d7",
         blockHash: "0xd3324a86bb4c2a9f99592ea16c02bddae6ced421c0170a07f781fb9dfa7b1d8c",
         blockNumber: 77,
         data: "0xc785676578696e1c",
         logIndex: 0,
         removed: false,
         topics: ["0xd20950ab1def1a5df286475bfce09dc88d9dcba71bab52f01965650b43a7ca8e"],
         transactionHash: "0xa4735b9dbf93f0f8d7831f893270ff8a42244141455ed308fd985b90ee9bc3f5",
         transactionIndex: 0
     }],
     logsBloom: "0x00000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000000800000008000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000",
     status: "0x1",
     to: "0x07894a9f9edffe4b73eb8928f76ee2993039e4d7",
     transactionHash: "0xa4735b9dbf93f0f8d7831f893270ff8a42244141455ed308fd985b90ee9bc3f5",
     transactionIndex: 0
   }

在Receipt的logs字段中是我们通过Event产生的数据，其中主要字段的含义为：

-  address: 产生该Event的合约地址
-  blockHash: 产生该Event的交易所在区块哈希
-  blockNumber: 产生该Event的交易所在区块号
-  data: Event参数的Rlp编码，在上面合约示例中为[name,age]的RLP编码
-  topics:
   Event名称的哈希值，在上面合约示例为字符串\ ``"setData"``\ 的哈希值

2.1.7. 跨合约调用
-----------------

bcwasm库提供了类\ ``DeployedContract``\ 用于跨合约调用，当需要在合约中调用其他合约时，首先使用目标合约地址初始化一个\ ``DeployedContract``\ 示例，然后调用对应的方法，如下所示：

.. code:: cpp

   // 调用目的地址： "0x07894a9f9edffe4b73eb8928f76ee2993039e4d7"
   // 调用的方法： setData(name,age)
   bcwasm::DeployedContract regManagerContract("0x07894a9f9edffe4b73eb8928f76ee2993039e4d7");
   char name[]= "name";
   int64_t age = 18;
   regManagerContract.call("setData", name, age);

``DeployedContract``\ 提供如下几种调用方式：

.. code:: cpp

   // 无需返回值调用
   void call("funcName", arguments...);
   void delegateCall("funcName", arguments...);

   // string类型返回值
   std::string callString("funcName", arguments...)
   std::string delegateCallString("funcName", arguments...)

   // Int64类型返回值
   int64_t callInt64("funcName", arguments...)
   int64_t delegateCallInt64("funcName", arguments...)

``call()``\ 与\ ``delegateCall()``\ 都可以用于调用合约，但是在被调目标合约的视角来看是有区别的，使用\ ``call()``\ 时被调合约看到的调用者\ ``caller``\ 是发起该调用的合约，而当使用\ ``delegateCall()``\ 时，发起调用的合约直接将自身的\ ``caller``\ 传递给目标合约。比如在如下的两个例子中，第一种情况下，ContractB看到的\ ``caller``\ 是ContractA的地址；而在第二中情况中，ContractB看到的\ ``caller``\ 是user的地址。

.. code:: bash

   1. user ----> ContractA --call()--> ContractB
   2. user ----> ContractA --delegateCall()--> ContractB

2.1.8. 初始化方法中注册cns合约
------------------------------

PlatONE在系统合约中提供了CNS服务功能，可以将合约注册至系统合约中，以实现使用合约名称版本调用合约而无需使用地址。可以在合约的初始化方法\ ``init()``\ 中直接将合约注册到系统合约中，以便使用CNS合约的便捷功能。

通过在\ ``init()``\ 方法中调用cnsManager合约的\ ``cnsRegisterFromInit(name,version)``\ 方法就可以实现，需要注意合约版本必须是\ ``"x.x.x.x"``\ 的格式。

.. code:: cpp

   void init()
   {
       DeployedContract reg("0x0000000000000000000000000000000000000011");
       reg.call("cnsRegisterFromInit", "name", "1.0.0.0");
   }

2.1.9. hash()
-------------

bcwasm库提供了与以太坊一致的哈希方法\ ``sha3()``\ ，使用方式如下所示：

.. code:: cpp

   std::string  msg = "hello";
   bcwasm::h256 hash = bcwasm::sha3(msg);

2.1.10. ecrecover()
-------------------

``ecrecover()``\ 函数提供了根据原文哈希和签名恢复出签名人地址的功能，使用方法如下：

.. code:: cpp

   // 针对字符串"hello"的签名
   std::string  sig = "4949eb47832d8a90c8c94b57de49d11b031fcd6d6dcb18c198103d2d431e2edf07be6c3056fe054ad6d1f62a24a509426a1c76687708ab684ad609ae879399fa00";
   // 签名原文
   std::string  msg = "hello";
   // 首先求出签名原文的哈希
   bcwasm::h256 hash = bcwasm::sha3(msg);
   // 通过ecrecover恢复出签名人地址
   bcwasm::h160 addr = bcwasm::ecrecover(hash.data(), bcwasm::fromHex(sig).data());

2.1.11. caller()、origin()和address()
-------------------------------------

-  caller()：返回caller信息，假如合约A通过call()方法调用合约B，那么caller就是合约A的地址
-  origin()：返回调用发起人的地址，不管合约间的调用情况如何，该函数始终返回最初交易发送者的地址
-  address()：返回当前合约的地址

2.1.12. 其他注意事项
--------------------

1) 当前合约对外接口仅支持以下数据类型：

.. code:: cpp

   char/char* /const char*/char[]  
   unsigned __int128/__int128/uint128_t  
   unsigned long long/uint64_t  
   unsigned long/uint32_t  
   unsigned short/uint16_t  
   unsigned char/uint8_t  
   long long/int64_t  
   long/int32_t/int  
   short/int16_t  
   char/int8_t  
   void  

2) platone合约库对u32和定长数组bytesN未定义，目前可以分别用uint32_t和char[]数组代替。

3) 在实现合约对外接口的查询方法是时，若函数返回的是字符串（比如通过调用string的c_str()方法），需要在该函数内部新申请（malloc）一段内存，并将该字符串copy到这段新的内存，由于该内存是由BCWasm虚拟机统一管理，故不存在内存泄露问题。返回字符串类型时，可以使用\ ``RETURN_CHARARRAY``\ 宏实现，该宏定义如下

.. code:: cpp

   #define RETURN_CHARARRAY(src，size) \
   do \
   { \
       char *buf = (char *)malloc(size); \
       memset(buf，0，size); \
       strcpy(buf，src); \
       return buf; \
   } \
   while(0)

4) wasm合约内置库中的u256类型转换字符串类型需要进行如下调用：

.. code:: cpp

   u256value.convert_to<std::string>()
   
   
3. 合约编译和部署
=================

主要包括以下内容: 
   - Wasm合约开发环境配置教程; 
   - Wasm合约编译; 
   - Wasm合约部署及调用;

方便用户对Wasm合约进行初步的了解，并快速掌握Wasm合约开发技巧.

3.1. 配置Wasm合约开发环境
^^^^^^^^^^^^^^^^^^^^^^^^^

获取最新稳定版的WASM合约发布包，并进行解压。

.. code:: bash

   wget https://github.com/PlatONEnterprise/PlatONE-Go/releases/download/v0.9.0/BCWasm_linux_release.v0.9.0.tar.gz
   tar -xzvf BCWasm_linux_release.v0.9.0.tar.gz 
   # 进入BCWasm目录，然后定义路径：
   cd BCWasm
   export CONTRACTSSPACE=${PWD}

在\ ``${CONTRACTSSPACE}``\ 目录下包含了合约编译所需的材料，合约源码目录为\ ``${CONTRACTSSPACE}/appContract``

获取PlatONE最新稳定版的发布包，并进行解压。进入解压获得的PlatONE_linux目录，然后定义路径：

.. code:: bash

   wget https://github.com/PlatONEnterprise/PlatONE-Go/releases/download/v0.9.0/PlatONE_linux_v0.9.0.tar.gz
   tar -xzvf PlatONE_linux_v0.9.0.tar.gz
   cd PlatONE_linux_v0.9.0
   export BIN_PATH=${PWD}/bin

.. warning:: 请确保 ``${BIN_PATH}`` 设置在当前的工作Shell，以让该环境变量生效。

3.2. 构建项目及编译合约
^^^^^^^^^^^^^^^^^^^^^^^

.. code:: bash

   cd ${CONTRACTSSPACE}
   # 1. 构建用户合约
   ./script/autoprojectForApp.sh  . my_contract
   # 2. 构建工程
   ./script/autoprojectForApp.sh  .
   # 3. 编译合约
   cd build
   make my_contract

3.2.1. 构建项目
---------------

第一步时，会在\ ``${CONTRACTSSPACE}/appContract``\ 下构建新的合约目录\ ``my_contract``\ 。

第二步时，会在\ ``${CONTRACTSSPACE}``\ 下创建\ ``build``\ ，并编译合约。

此时，\ ``build``\ 目录下的结构如下所示

.. code:: bash

   ├── appContract
   │   ├── appDemo
   │   │   ├── appDemo.bc
   │   │   ├── appDemo.cpp.abi.json ## 合约的abi文件
   │   │   ├── appDemo.cpp.bc
   │   │   ├── appDemo.s
   │   │   ├── appDemo.wasm         ## 合约的wasm字节码
   │   │   ├── appDemo.wast
   │   │   ├── CMakeFiles
   │   │   ├── cmake_install.cmake
   │   │   └── Makefile
   │   ├── CMakeFiles
   │   ├── cmake_install.cmake
   │   ├── Makefile
   │   └── my_contract
   │       ├── CMakeFiles
   │       ├── cmake_install.cmake
   │       ├── Makefile
   │       ├── my_contract.cpp.abi.json ## 合约的abi文件
   │       ├── my_contract.bc
   │       ├── my_contract.cpp.bc
   │       ├── my_contract.s
   │       ├── my_contract.wasm
   │       └── my_contract.wast
   ├── CMakeCache.txt
   ├── CMakeFiles
   ├── cmake_install.cmake
   └── Makefile

合约文件会通过脚本\ ``autoprojectForApp.sh``\ 默认生成如下的源码，可以在此基础上编写自己的合约。

.. code:: cpp

   //auto create contract
   #include <stdlib.h>
   #include <string.h>
   #include <string>
   #include <bcwasm/bcwasm.hpp>
   namespace demo {
     class my_contract : public bcwasm::Contract {
       public:
       my_contract(){}

       /// 实现父类: bcwasm::Contract 的虚函数
       /// 该函数在合约首次发布时执行，仅调用一次
       void init() 
       {
           bcwasm::println("init success...");
       }
       /// 定义Event.
       /// BCWASM_EVENT(eventName, arguments...)
       BCWASM_EVENT(setName, const char *)
       
       public:
       void setName(const char *msg) {
           // 定义状态变量
           bcwasm::setState("NAME_KEY",std::string(msg));
           // 日志输出
           // 事件返回
           BCWASM_EMIT_EVENT(setName,"std::string(msg)");
       }
       const char* getName() const {
           std::string value;
           bcwasm::getState("NAME_KEY", value);
           // 读取合约数据并返回
           return value.c_str();
       }
     };
   }
   //此处定义的函数会生成ABI文件供外部调用: 
   BCWASM_ABI(demo::my_contract, setName)
   BCWASM_ABI(demo::my_contract, getName)

3.2.2. 编译合约
---------------

进入\ ``build``\ 目录并编译该合约

.. code:: bash

   cd ${CONTRACTSSPACE}/build
   make my_contract

生成文件如下

.. code:: bash

   ├build/ 
   ├-appContact/
   │   └── my_contract
   │       ├── CMakeFiles
   │       ├── cmake_install.cmake
   │       ├── Makefile
   │       ├── my_contract.cpp.abi.json ## 合约的abi文件
   │       ├── my_contract.bc
   │       ├── my_contract.cpp.bc
   │       ├── my_contract.s
   │       ├── my_contract.wasm
   │       └── my_contract.wast

主要文件简介：

-  .bc 连接标准库后的LLVM字节码文件，含所有依赖库；
-  .json 合约对应的接口描述文件；
-  .s 汇编文件；
-  .wasm 合约编译后的二进制文件，字节码指令集；
-  .wast 合约编译后的可被认为读懂的指令文件；
-  .cpp.bc ；LLVM 字节码文件，仅包含合约代码本身的；

其中，发布合约时需要用到的文件为my_contract.wast和my_contract.cpp.abi.json。

3.3. 部署及调用合约
^^^^^^^^^^^^^^^^^^^

3.3.1. 生成合约部署账户
-----------------------

.. code:: bash

   cd ${CONTRACTSSPACE}/build
   ../script/create_accout.sh --ip 127.0.0.1 --rpc_port 6791

该命令会创建一个新的账户，创建时需要输入一个账户密码，方便后续解锁。

除了新生成一个账户，该命令还会在当前目录创建一个\ ``config.json``\ 文件，该文件在调用合约时会用到。该文件记录节点的ip端口以及发送交易的账号。

运行结果如下：

.. code:: console

   nodeid: 127.0.0.1
   rpc_port: 6791

   ###########################################
   ####       Create an account           ####
   ###########################################

   Input account passphrase.
   passphrase: your_phrase
   --output{"jsonrpc":"2.0"，"id":1，"result":"0x08e7988e60ab5aa49d1f7aa9435ac91b9fcf772c"} --output
   New account: 0x08e7988e60ab5aa49d1f7aa9435ac91b9fcf772c
   {"jsonrpc":"2.0"，"id":1，"result":true}
   127.0.0.1:6791

    Create config.json for contract-deploy

   {
     "url":"http://127.0.0.1:6791"，
     "gas":"0x0"，
     "gasPrice":"0x0"，
     "from":"0x08e7988e60ab5aa49d1f7aa9435ac91b9fcf772c"
   }

配置文件字段简述：

-  ``url`` PlatONE节点开放的JSON-RPC地址信息；
-  ``from`` 发布合约者的账户地址。

长时间没用使用账户发送过交易，或者节点重启后，账户会被锁定，不能发送交易。需要使用如下命令，重新为账户解锁。

.. code:: bash

   ../script/unlock_account.sh \
                --account 0x08e7988e60ab5aa49d1f7aa9435ac91b9fcf772c \
               --phrase "your_phrase" \
               --ip 127.0.0.1 \
               --rpc_port 6791

3.3.2. 部署合约
---------------

.. code:: bash

   cd ${CONTRACTSSPACE}/build
   
   # ctool是用来部署合约及发送交易的工具。
   cp  ${BIN_PATH}/ctool  .
   
   # 部署合约
   ./ctool deploy --abi appContract/my_contract/my_contract.cpp.abi.json --code appContract/my_contract/my_contract.wasm --config ./config.json

结果:

.. code:: console

   trasaction hash: 0x2f3a868d8ceb60804a830e5fc35611e3ae22ccb6ef298830acd84aba41f6dd99
   contract address: 0x2ee8d0545ebd20f9a992ff54cb0f21a153539206

ctool工具使用方式主要有以下两种：

.. code:: bash

   # 合约部署
   ./ctool \
       deploy \
       --abi   contract.cpp.abi.json \
       --code  contract.wasm \
       --config ./config.json

   # 合约调用
   ./ctool \
       invoke \
       --abi   contract.cpp.abi.json \
       --addr  "合约地址： 0x1234" \
       --config ./config.json \
       --func  "方法名字" \
       --param  "第1个参数" \
       --param  "第2个参数"                                     

-  提示1：
   如果命令行中未明确指明配置文件路径，则会在当前工作路径读取文件：config.json。
-  提示2：
   发布合约到PlatONE网络，需要连接到节点，并保证发布合约的账户已进行了解锁操作，且没有超时。执行git-bash.exe打开git-bash窗口。
-  提示3： 如果命令不能执行，请确保脚本具有执行权限，可使用命令：chmod
   +x ctool 进行授权。

3.3.3. 调用合约
---------------

调用合约的\ ``setName方法``

.. code:: bash

   #地址需要改成上节生成的地址
   ./ctool invoke --addr 0x2ee8d0545ebd20f9a992ff54cb0f21a153539206 --abi appContract/my_contract/my_contract.cpp.abi.json   --config ./config.json  --func "setName" --param "wxbc"  

.. code:: console

    request json data：[{"from":"0x32ab0a20b589f40c7e3d6ee485a2404bb7269f87"，"to":"0x2ee8d0545ebd20f9a992ff54cb0f21a153539206"，"gas":"0x0"，"gasPrice":"0x0"，"value":""，"data":"0xdb8800000000000000028c696e766f6b654e6f746966798477786263"，"txType":2}] 
    response json：{"jsonrpc":"2.0"，"id":1，"result":"0xeb3680c65b393952a07ec590cef7b19fc87877a9fbb70c8e16797a97ed4cfaeb"}

调用合约的\ ``getName``\ 方法：

.. code:: bash

   #地址需要改成上节生成的地址
   ./ctool invoke --addr 0x2ee8d0545ebd20f9a992ff54cb0f21a153539206 --abi appContract/my_contract/my_contract.cpp.abi.json   --config ./config.json  --func getName
   
.. code:: console

   request json data：[{"from":"0x32ab0a20b589f40c7e3d6ee485a2404bb7269f87"，"to":"0x2ee8d0545ebd20f9a992ff54cb0f21a153539206"，"gas":"0x0"，"gasPrice":"0x0"，"value":""，"data":"0xd1880000000000000002876765744e616d65"，"txType":2}，"latest"] 
   response json：{"jsonrpc":"2.0"，"id":1，"result":"0x000000000000000000000000000000000000000000000000000000000000002000000000000000000000000000000000000000000000000000000000000000047778626300000000000000000000000000000000000000000000000000000000"}   
   result: wxbc

3.4. 通过console与节点交互
^^^^^^^^^^^^^^^^^^^^^^^^^^

3.4.1. 节点交互
---------------

.. code:: bash

   cd ${BIN_PATH}
   ./platone attach http://127.0.0.1:6791
   
.. code:: console

   Welcome to the PlatONE JavaScript console!

   instance: PlatONEnetwork/platone/v0.2.0-stable-1b13ff73/linux-amd64/go1.12.4
   coinbase: 0x938c231429f5ab34244618fe0dc7380e319b470e
   at block: 1557 (Tue，18 Jun 2019 16:29:12 CST)
    datadir: /home/gexin/PlatONE-Workspace/chain/PlatONE_linux/data/node-1
    modules: admin:1.0 eth:1.0 net:1.0 personal:1.0 rpc:1.0 web3:1.0

   > 

通过上面命令可以与节点交互，用于查询相关信息

3.4.2. 查询交易receipt
----------------------

.. code:: bash

   #交易哈希值改成上节生成的
   > eth.getTransactionReceipt("0x2f3a868d8ceb60804a830e5fc35611e3ae22ccb6ef298830acd84aba41f6dd99")

结果如下：

.. code:: java

   {
     blockHash: "0x3f1e326f4b122efb26e9da417daff97dbb403c714d3324e8020ddce04b88617a",
     blockNumber: 236,
     contractAddress: "0x2ee8d0545ebd20f9a992ff54cb0f21a153539206",
     cumulativeGasUsed: 1996535,
     from: "0x32ab0a20b589f40c7e3d6ee485a2404bb7269f87",
     gasUsed: 1996535,
     logs: [],
     logsBloom: "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
     status: "0x1",
     to: null,
     transactionHash: "0x2f3a868d8ceb60804a830e5fc35611e3ae22ccb6ef298830acd84aba41f6dd99",
     transactionIndex: 0
   }

4. 合约部署调用演示
===================

本文旨在让用户可以快速便捷地搭建PlatONE区块链以体验其功能，用户可以根据如下步骤快速搭建一条区块链，并在链上部署合约、调用合约。

4.1. 搭建区块链
^^^^^^^^^^^^^^^

- 环境依赖参照 \ `环境依赖 <../quick/env.html>`__\ 。
- 快速搭建参照 \ `快速上手 <../quick/quick_deploy.html>`__\
- 正常部署参照 \ `正常部署 <../develop/normal_deploy.html>`__\

4.2. PlatONE账户
^^^^^^^^^^^^^^^^

4.2.1. 创建PlatONE账户
----------------------

在发布交易之前，首先用户需要创建一个账户。

.. code:: bash

   cd ${WORKSPACE}/../../cmd/SysContracts/
   ./script/create_accout.sh --ip 127.0.0.1 --rpc_port 6791

该命令会创建一个新的账户，创建时需要输入一个账户密码，方便后续解锁。
运行结果如下，账户地址记录在\ ``./config.json``\ 文件中：

.. code:: console

   nodeid: 127.0.0.1
   rpc_port: 6791

   ###########################################
   ####       Create an account           ####
   ###########################################

   Input account passphrase.
   passphrase: your_phrase
   --output{"jsonrpc":"2.0"，"id":1，"result":"0x08e7988e60ab5aa49d1f7aa9435ac91b9fcf772c"} --output
   New account: 0x08e7988e60ab5aa49d1f7aa9435ac91b9fcf772c
   {"jsonrpc":"2.0"，"id":1，"result":true}
   127.0.0.1:6791

    Create config.json for contract-deploy

   {
     "url":"http://127.0.0.1:6791"，
     "gas":"0x0"，
     "gasPrice":"0x0"，
     "from":"0x08e7988e60ab5aa49d1f7aa9435ac91b9fcf772c"
   }

4.2.2 解锁账户
--------------

首次创建账户或者是长时间没用使用账户发送过交易，账户会被锁定，不能发送交易。需要使用如下命令，重新为账户解锁。

.. code:: bash

   cd ${WORKSPACE}/../../cmd/SysContracts/
   ./script/unlock_account.sh \
               --account 0x08e7988e60ab5aa49d1f7aa9435ac91b9fcf772c \
               --phrase "your_phrase" \
               --ip 127.0.0.1 \
               --rpc_port 6791

4.3. 部署调用合约
^^^^^^^^^^^^^^^^^

4.3.1. 创建并编译用户合约
-------------------------

首先通过如下命令创建一个默认合约，该合约包含了两个基本方法\ ``setName``\ 和\ ``getName``\ ，用户可以在此基础上编写合约逻辑。

.. code:: bash

   cd ${WORKSPACE}/../../cmd/SysContracts/
   
   # 创建默认合约
   ./script/autoprojectForApp.sh  . my_contract
   
   # 编译合约
   ./script/autoprojectForApp.sh  .

生成的合约源码文件位于\ ``${WORKSPACE}/../../cmd/SysContracts/appContract/my_contract/my_contract.cpp``\ 。

my_contract.cpp：

.. code:: cpp

   #include <stdlib.h>
   #include <string.h>
   #include <string>
   #include <bcwasm/bcwasm.hpp>

   namespace demo {
   class my_contract : public bcwasm::Contract
   {
       public:
       my_contract(){}

       /// parent class implementation: bcwasm:: virtual function of Contract
       /// this function is called only once when the contract is first released
       void init()
       {
           bcwasm::println("init success...");
       }
       /// define event.
       BCWASM_EVENT(Notify, uint64_t, const char *)

       public:
       void setName(const char *msg)
       {
           // define state
           bcwasm::setState("NAME_KEY", std::string(msg));
           // return event
           BCWASM_EMIT_EVENT(Notify, 0, "Set Name.");
       }
       const char* getName() const
       {
           std::string value;
           bcwasm::getState("NAME_KEY", value);
           // read contract data and return
           return value.c_str();
       }
   };
   }
   // the function below will generate an ABI file for external calls
   BCWASM_ABI(demo::my_contract, invokeNotify)
   BCWASM_ABI(demo::my_contract, getName)

4.3.2. 部署合约
---------------

PlatONE提供了合约工具\ ``ctool``\ ，可以使用该工具来部署智能合约及发送交易。

.. code:: bash

   cd ${WORKSPACE}/../../cmd/SysContracts/build
   
   # ctool是用来部署合约及发送交易的工具
   cp ${WORKSPACE}/bin/ctool .  
   
   # 部署合约
   ./ctool deploy --abi appContract/my_contract/my_contract.cpp.abi.json --code appContract/my_contract/my_contract.wasm --config ../config.json

命令执行结果如下:

.. code:: console

   trasaction hash: 0x2f3a868d8ceb60804a830e5fc35611e3ae22ccb6ef298830acd84aba41f6dd99
   contract address: 0x2ee8d0545ebd20f9a992ff54cb0f21a153539206

4.3.3 调用合约
--------------

调用合约的\ ``setName``\ 方法

.. code:: bash

   ./ctool invoke --addr 0x2ee8d0545ebd20f9a992ff54cb0f21a153539206 --abi appContract/my_contract/my_contract.cpp.abi.json   --config ../config.json  --func "setName" --param "wxbc"  

命令执行结果如下：

.. code:: console

    request json data：[{"from":"0x32ab0a20b589f40c7e3d6ee485a2404bb7269f87"，"to":"0x2ee8d0545ebd20f9a992ff54cb0f21a153539206"，"gas":"0x0"，"gasPrice":"0x0"，"value":""，"data":"0xdb8800000000000000028c696e766f6b654e6f746966798477786263"，"txType":2}]

    response json：{"jsonrpc":"2.0"，"id":1，"result":"0xeb3680c65b393952a07ec590cef7b19fc87877a9fbb70c8e16797a97ed4cfaeb"}

调用合约的\ ``getName``\ 方法：

.. code:: bash

   ./ctool invoke --addr 0x2ee8d0545ebd20f9a992ff54cb0f21a153539206 --abi appContract/my_contract/my_contract.cpp.abi.json   --config ../config.json  --func getName

命令执行结果如下：

.. code:: console

   request json data：[{"from":"0x32ab0a20b589f40c7e3d6ee485a2404bb7269f87"，"to":"0x2ee8d0545ebd20f9a992ff54cb0f21a153539206"，"gas":"0x0"，"gasPrice":"0x0"，"value":""，"data":"0xd1880000000000000002876765744e616d65"，"txType":2}，"latest"]

   response json：{"jsonrpc":"2.0"，"id":1，"result":"0x000000000000000000000000000000000000000000000000000000000000002000000000000000000000000000000000000000000000000000000000000000047778626300000000000000000000000000000000000000000000000000000000"}

   result: wxbc

4.4. 交易查询
^^^^^^^^^^^^^

4.4.1. 通过控制台连接到节点
---------------------------

可通过以下方式进入PlatONE控制台与链进行交互。

.. code:: bash

   cd  ${WORKSPACE}/bin
   ./platone attach http://127.0.0.1:6791

命令执行的结果如下：

.. code:: console

   Welcome to the PlatONE JavaScript console!

   instance: PlatONEnetwork/platone/v0.2.0-stable-1b13ff73/linux-amd64/go1.12.4
   coinbase: 0x938c231429f5ab34244618fe0dc7380e319b470e
   at block: 1557 (Tue，18 Jun 2019 16:29:12 CST)
    datadir: /home/user/PlatONE-Workspace/chain/PlatONE_linux/data/node-1
    modules: admin:1.0 eth:1.0 net:1.0 personal:1.0 rpc:1.0 web3:1.0

   >

4.4.2. 查询交易receipt
----------------------

.. code:: bash

   > eth.getTransactionReceipt("0x2f3a868d8ceb60804a830e5fc35611e3ae22ccb6ef298830acd84aba41f6dd99")

结果如下：

.. code:: java

   {
     blockHash: "0x3f1e326f4b122efb26e9da417daff97dbb403c714d3324e8020ddce04b88617a",
     blockNumber: 236,
     contractAddress: "0x2ee8d0545ebd20f9a992ff54cb0f21a153539206",
     cumulativeGasUsed: 1996535,
     from: "0x32ab0a20b589f40c7e3d6ee485a2404bb7269f87",
     gasUsed: 1996535,
     logs: [],
     logsBloom: "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
     status: "0x1",
     to: null,
     transactionHash: "0x2f3a868d8ceb60804a830e5fc35611e3ae22ccb6ef298830acd84aba41f6dd99",
     transactionIndex: 0
   }

4.5. 停止节点并清除数据
^^^^^^^^^^^^^^^^^^^^^^^

例如停止nodeid为0的区块链节点并清除数据的命令如下：

.. code:: bash

   cd ${WORKSPACE}/scripts
   ./platonectl.sh stop -n 0
   ./platonectl.sh clear -n 0
   
5. 合约数据迁移
===============

在数据合约升级的场景，某些情况需要处理历史数据在新旧合约之间的迁移。

PlatONE支持新旧合约之间的数据迁移，以保证合约升级时原有业务数据不丢失。

.. image:: ../../images/develop/sm_migrate.png

.. code:: bash

   ctool migInvoke --addr ${destination_contract_addr} --func 'migrateFrom' --param ${source_contract_addr}  --config ${ctool.json}

参数说明：

- ``${destination_contract_addr}`` 为要迁入合约的地址

- ``${source_contract_addr}`` 为要迁移的数据源合约

示例
^^^^

我们以迁移一个带数据的CNS合约为例，来描述操作过程：

1) 部署CNS合约 A：

.. code:: bash

   ctool deploy --code ../conf/contracts/cnsManager.wasm --abi ../conf/contracts/cnsManager.cpp.abi.json --config ../conf/ctool.json

2) 部署\ ``userRegister``\ 合约，用以在CNS合约
   A中写入数据。（在用户场景中，这个步骤可选，只要保证待迁移的合约存在数据即可）

.. code:: bash

   ctool deploy --code ../conf/contracts/userRegister.wasm --abi ../conf/contracts/userRegister.cpp.abi.json --config ../conf/ctool.json

3) 部署CNS合约 B：

.. code:: bash

   ctool deploy --code ../conf/contracts/cnsManager.wasm --abi ../conf/contracts/cnsManager.cpp.abi.json --config ../conf/ctool.json

4) 执行迁移（拷贝，对原合约数据无影响）：

.. code:: bash

   ctool migInvoke --addr 0x30c9f12cae592610df1a387cfec39db0e64989e4 --func 'migrateFrom("0xe1dcc86f53fcbad47e25e391111b03afc14f1db3")' -config ../conf/ctool.json

5) 查看CNS合约 B上的数据，确认数据已得到迁移：

.. code:: bash

   ctool invoke --addr 0x30c9f12cae592610df1a387cfec39db0e64989e4 --abi ../conf/contracts/cnsManager.cpp.abi.json --config ../conf/ctool.json --func getRegisteredContracts --param 0 --param 10

6. 隐私代币合约使用指南
=======================

6.1. wasm代币合约部署与调用
^^^^^^^^^^^^^^^^^^^^^^^^^^^

6.1.1. wasm合约部署
--------------------

.. code:: bash

   ./platonecli contract deploy ./SysContracts/build/appContract/BcToken/BcToken.wasm --abi ../SysContracts/build/appContract/BcToken/BcToken.cpp.abi.json --keyfile ../../release/linux/data/node-0/keystore/UTC--2020-08-28T06-01-22.648120896Z--55b3d5f67010b314ebcdd46ef9e420da499c1596 --default

.. code:: console

   wasm合约地址：0xbc74aca12aa33c4b0f51a8dc92a00d0c3699db6f

6.1.2. wasm合约调用-查询余额
-----------------------------

.. code:: bash

   ./platonecli contract execute "合约地址" getBalance --param "发送者地址" --abi "合约abi"
   示例：
   ./platonecli contract execute 0xbc74aca12aa33c4b0f51a8dc92a00d0c3699db6f getBalance --param 0x55b3d5f67010b314ebcdd46ef9e420da499c1596 --abi ../SysContracts/build/appContract/BcToken/BcToken.cpp.abi.json

.. code:: console

   余额:100000

6.2. 隐私代币合约部署与验证
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

6.2.1. 隐私代币合约部署
----------------------------

.. code:: bash

   ./platonecli contract deploy "solidity合约二进制文件" --abi "合约abi" --vm evm --param "epoch值" --param "绑定的wasm合约地址"
   示例：
   ./platonecli contract deploy test_contracts/privacyToken_sol_PToken.bin --abi test_contracts/privacyToken_sol_PToken.abi --vm evm --param 3 --param 0xbc74aca12aa33c4b0f51a8dc92a00d0c3699db6f

.. code:: console

   隐私合约地址：0xfff4ecb6973e1078e3cd411a69d7ea9569e8767f

6.2.2. 隐私代币合约调用
-----------------------

.. code:: bash

   ./platonecli contract execute "隐私合约地址" token --abi "隐私合约abi" --vm evm
   示例：
   ./platonecli contract execute 0xfff4ecb6973e1078e3cd411a69d7ea9569e8767f token --abi test_contracts/privacyToken_sol_PToken.abi --vm evm

6.3. 隐私代币客户端
^^^^^^^^^^^^^^^^^^^^^

6.3.1. flag说明
--------------------

6.3.1.1. 配置类
>>>>>>>>>>>>>>>>>

**（1）–url** 说明：远程节点url, 格式：\ ``https://<ip>:<port>``

**（2）–contract** 说明：部署的隐私代币合约地址

**（3）–bc-owner** 说明：同时具备如下两个用途 1. 交易发送者 2.
明文/wasm代币合约的注册用户

**（4）–config** 说明：

-  将某次命令提供的\ ``--url``, ``--contract``,
   ``--bc-owner``\ 的对应值写入结构体对应字段，并序列化后存储到\ ``config.json``\ 文件中。
-  ``config.json``\ 会在可执行文件的\ **运行路径**\ 下生成
-  每次命令会默认读取\ ``config.json``\ 中对应字段的值。如果该字段也提供了对应的flag，则flag对应的值具有较高的优先级。

6.3.1.2. 常用类
>>>>>>>>>>>>>>>>>>

**（1）–value** 说明：\ ``register``, ``depoist``,
``transfer``\ 转移的代币数量

**（2）–account** 说明：

-  隐私代币合约的账户
-  ``--account``\ 对应值：对应账户文件的路径（默认：\ ``./privacyAccount.json``\ ）

**（3）–receiver** 说明：

-  接收隐私代币的账户，该账户需是已注册的有效账户，否则会报错。
-  示例：\ ``["0x1014b579b1f13fd07f4ffec5da3f7c2ca0a04dbc68579f49369874dcd2ec0e6c","0x241aac4b68d23a0303c3cb82a8ae64fda731cf3c552613088859619ce0ffa68d"]``

6.3.1.3. 其他
>>>>>>>>>>>>>>>>

**（1）–decoy-num**
说明：仅\ ``tranfser``\ 命令使用，指定匿名集的元素个数。默认为3，即匿名集有2^3=8个账户。

**（2）–o** 说明：仅\ ``register``\ 命令使用，指定输出账户文件的文件名

**（3）–l-invl && –r-invl** 说明：

-  账户拥有的隐私代币密文的猜测区间，\ ``--l-invl``\ 为左区间，\ ``--r-invl``\ 为右区间。通过限定猜测区间，减少解密密文的时间。
-  若区间提供错误，则会报错
-  ``--l-invl``\ 默认为0，\ ``--r-invl``\ 默认为2^32-1

6.3.2. 注册register
----------------------

**（1）查看register用法**

.. code:: bash

   ./client register -h

**（2）register命令**

示例：

.. code:: bash

   ## 完整命令
   ./client register --bc-owner 0x55b3d5f67010b314ebcdd46ef9e420da499c1596 --contract 0xfff4ecb6973e1078e3cd411a69d7ea9569e8767f --url http://127.0.0.1:6791 --config

   ## 省略配置类flag
   ./client register

   ## 指定输出账户文件名称
   ./client register -o privacyAccount-x.json

6.3.3. 充值deposit
---------------------

**（1）查看deposit用法**

.. code:: bash

   ./client deposit -h

**（2）deposit命令**

示例：

.. code:: bash

   ## 完整命令
   ./client deposit --account privacyAccount-1.json --value 200 --r-invl 1000 --bc-owner 0x55b3d5f67010b314ebcdd46ef9e420da499c1596 --contract 0xfff4ecb6973e1078e3cd411a69d7ea9569e8767f --url http://127.0.0.1:6791 --config

   ## 省略配置类flag
   ./client deposit --account privacyAccount-3.json --value 400

6.3.4. 赎回withdraw
-----------------------

**（1）查看withdraw用法**

.. code:: bash

   ./client withdraw -h

**（2）withdraw命令**

示例：

.. code:: bash

   ## 完整命令
   ./client withdraw --account privacyAccount-4.json --value 400 --r-invl 1000 --bc-owner 0x55b3d5f67010b314ebcdd46ef9e420da499c1596 --contract 0xfff4ecb6973e1078e3cd411a69d7ea9569e8767f --url http://127.0.0.1:6791 --config

   ## 省略配置类flag
   ./client withdraw --account privacyAccount-4.json --value 400 --r-invl 1000

6.3.5. 转账transfer
-----------------------

**（1）查看transfer用法**

.. code:: bash

   ./client transfer -h

**（2）transfer命令**

示例：

.. code:: bash

   ## 完整命令
   ./client transfer --account privacyAccount-2.json --value 200 --r-invl 1000 --receiver "["0x1014b579b1f13fd07f4ffec5da3f7c2ca0a04dbc68579f49369874dcd2ec0e6c","0x241aac4b68d23a0303c3cb82a8ae64fda731cf3c552613088859619ce0ffa68d"]" --bc-owner 0x55b3d5f67010b314ebcdd46ef9e420da499c1596 --contract 0xfff4ecb6973e1078e3cd411a69d7ea9569e8767f --url http://127.0.0.1:6791 --config

   ## 省略配置类flag
   ./client transfer --account privacyAccount-2.json --value 200 --r-invl 1000 --receiver "["0x1014b579b1f13fd07f4ffec5da3f7c2ca0a04dbc68579f49369874dcd2ec0e6c","0x241aac4b68d23a0303c3cb82a8ae64fda731cf3c552613088859619ce0ffa68d"]"

6.3.6. 查询query
--------------------

**（1）查看query用法**

.. code:: bash

   F ./client query -h

**（2）query命令**

示例：

.. code:: bash

   ## platonecli工具查询某账户对应的账户余额
   ./platonecli contract execute 0xfff4ecb6973e1078e3cd411a69d7ea9569e8767f simulateAccounts --param "[[0x20775257b284e0d7234be2860f03b930c9b7eec1a5c9d41773a2fbe499d96e94,0x21907ecf49861a11cda03defca88c38370bd89dd7456c3665d9961ab395d25e3]]" --param 20 --abi test_contracts/privacyToken_sol_PToken.abi --vm evm

   ## 客户端查询
   ./client query --account ./privacyAccount-1.json --r-invl 1000 --bc-owner 0x55b3d5f67010b314ebcdd46ef9e420da499c1596 --contract 0xfff4ecb6973e1078e3cd411a69d7ea9569e8767f --url http://127.0.0.1:6791

   ## 省略配置类flag
   ./client query --account ./privacyAccount-1.json --r-invl 1000

