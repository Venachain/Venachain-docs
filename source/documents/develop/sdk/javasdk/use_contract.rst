=====================
JAVA-SDK 合约交互
=====================

为了方便在java项目中调用链上合约，需要首先生成合约对应的java类，在项目中创建合约类实例后，便可以调用合约。

合约骨架生成
=============

合约编写
^^^^^^^^^

编写合约(以demo为例)，编写合约的步骤请参阅 :ref:`Wasm合约开发 <smart-contract-wasm-cpp-develop>` 。

.. code:: cpp

    #include <stdlib.h>
    #include <string.h>
    #include <string>
    #include <bcwasm/bcwasm.hpp>

    namespace demo {
        class FirstDemo : public bcwasm::Contract
        {
            public:
                FirstDemo(){}

                // 实现父类: bcwasm::Contract 的虚函数
                // 该函数在合约首次发布时执行，仅调用一次
                void init()
                {
                    bcwasm::println("init success...");
                }
            public:
                void setName(const char *msg)
                {
                    // 定义状态变量
                    bcwasm::setState("NAME_KEY", std::string(msg));
                }

                const char* getName() const
                {
                    std::string value;
                    bcwasm::getState("NAME_KEY", value);
                    // 读取合约数据并返回
                    return value.c_str();
                }
        };
    }
    // 此处定义的函数会生成ABI文件供外部调用
    BCWASM_ABI(demo::FirstDemo, setName)
    BCWASM_ABI(demo::FirstDemo, getName)

合约编译后会产生 ``demo.cpp.abi.json`` 和 ``demo.wasm`` ，在生成java合约代码时需要用到这两个文件。

骨架生成
^^^^^^^^^

使用合约骨架生成工具生成java合约骨架:

.. code:: bash

    cd client_sdk_java_v1.0.0/java-sdk/bin
    ./client-sdk wasm generate --javaTypes      \
            </path/to/demo.wasm>                \
            </path/to/demo.cpp.abi.json>        \
            -o </path/to/src/main/java>         \
            -p <com.your.organisation.name>     \
            -t wasm

.. note:: 

    - 把尖括号内的内容替换成自己的内容。
    - 运行后会生成合约对应的java类。
    - java类中包含了合约中的方法，方便在应用层中调用合约。

合约操作
=========

部署合约
^^^^^^^^^

.. code:: java

    //optional
    class NodeConfiguration {
        public static final String WALLETSOURCE = "/home/username/Work/Venachain/data/keystore/keyfile.json";
        public static final String DEMOBIN = "/home/user/Work/client-sdk-0.4.1/contract/firstdemo.wasm";
    }

    //建立连接
    Web3j web3j = Web3j.build(new HttpService("http://127.0.0.1:6791"));

    //加载钱包
    Credentials credentials = WalletUtils.loadCredentials("<wallet password>", NodeConfiguration.WALLETSOURCE);

    //部署合约  
    byte[] dataBytes = Files.readBytes(new File(NodeConfiguration.DEMOBIN));
    String binData = Hex.toHexString(dataBytes);
    Firstdemo demo = Firstdemo.deploy(web3j, credentials, binData, new DefaultWasmGasProvider()).send();

加载合约
^^^^^^^^^

.. code:: java

    //optional
    class NodeConfiguration {
        public static final String WALLETSOURCE = "/home/username/Work/Venachain/data/keystore/keyfile.json";
        public static final String DEMOBIN = "/home/user/Work/client-sdk-0.4.0/contract/firstdemo.wasm";
    }

    //建立连接
    Web3j web3j = Web3j.build(new HttpService("http://127.0.0.1:6791"));

    //加载钱包
    Credentials credentials = WalletUtils.loadCredentials("<wallet password>", NodeConfiguration.WALLETSOURCE);

    //加载合约
    byte[] dataBytes = Files.readBytes(new File(NodeConfiguration.DEMOBIN));
    String binData = Hex.toHexString(dataBytes);
    Firstdemo contract = Firstdemo.load(binData, "<contract address>", web3j, credentials, new DefaultWasmGasProvider());

调用合约示例
^^^^^^^^^^^^^

在合约部署后，客户端可以通过合约地址进行合约调用。

合约地址
---------

.. code:: java

    public  static void main(String args[]) {

        Web3j web3j = Web3j.build(new HttpService("http://127.0.0.1:6791"));

        try {
            // 密钥账户，keyfile.json为venakey工具生成的账户文件，参照《Venachain密钥工具文档》
            Credentials credentials = WalletUtils.loadCredentials("1", "/home/wxuser/keyfile.json");

            // 合约数据
            byte[] dataBytes = Files.readBytes(new File("/home/user/Venachain-Workspace-0.2/contracts/build/appContract/demo/demo.wasm"));
            String binData = Hex.toHexString(dataBytes);

            // 加载合约
            Demo demo = Demo.load(binData, "0x1d7f2695b43be56f52f24baa199420f8c10ac1d3", web3j, credentials, new DefaultWasmGasProvider());

            // 调用demo合约的setName方法，参数输入字符串"Venachain"
            TransactionReceipt ret = demo.setName("Venachain").send();
            System.out.println("Transaction Hash: "+ret.getTransactionHash());

            // 调用demo合约的getName方法
            System.out.println("getName: " +  demo.getName().send());

        }catch (Exception e){
            System.out.println(e);
        }
    }

合约名称
----------

.. code:: java

    public static void main(String[] args) {
        try {
            Web3j web3j = Web3j.build(new HttpService("http://127.0.0.1:6791"));
            Credentials credentials = WalletUtils.loadCredentials("1", "/home/wxuser/keyfile.json");
            byte[] dataBytes = Files.readBytes(new File("/home/user/Venachain-Workspace-0.2/contracts/build/appContract/demo/demo.wasm"));
            String binData = Hex.toHexString(dataBytes);
            // load contract
            CnsManager cns = CnsManager.load(null, "0x0000000000000000000000000000000000000011", web3j, credentials, new DefaultWasmGasProvider());
            TransactionReceipt r = cns.cnsRegister("demo", "1.0.0.0", "0x1d7f2695b43be56f52f24baa199420f8c10ac1d3").send();
            if (r.isStatusOK()){
                Demo d = Demo.load(null, "demo", web3j, c, new DefaultWasmGasProvider());
                d.setName("cns").send();
                System.out.println(d.getName().send());
            }

        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            System.out.println("Done...");
        }
    }

订阅事件
============

订阅区块
^^^^^^^^^^

在新区块产生时，client可以得到节点的区块数据推送。

.. code:: java

    Subscription sub = web3j.blockObservable(false).subscribe( block -> {
        System.out.println(block.getBlock().getNumber());
    });

订阅event
^^^^^^^^^^^

在合约中可以自定义事件，client通过订阅事件的方式来获悉合约调用中所触发的事件。

合约中定义如下的event，每次setName被调用时，就会触发该event。

.. code:: java

    // event定义
    BCWASM_EVENT(setName, const char *)

    void setName(const char *msg)
    {
        // 定义状态变量
        bcwasm::setState("NAME_KEY", int, std::string(msg));
        // 日志输出
        // 事件返回
        BCWASM_EMIT_EVENT(setName, 2020, "std::string(msg)");
    }

在Java合约框架中会生成与 ``setName`` 事件相关数据结构与接口，在服务层可以通过JavaSDK，监听该事件，示例代码如下：

.. code:: java

    String contractAddress = "0x1d7f2695b43be56f52f24baa199420f8c10ac1d3";
    String eventHash = Hash.sha3String("setName");

    EthFilter filter = new EthFilter(DefaultBlockParameterName.EARLIEST,DefaultBlockParameterName.LATEST,contractAddress).addSingleTopic(eventHash);

    Subscription subTx = demo.setNameEventObservable(filter).subscribe(
            r -> {
                System.out.println(r.param1);
                System.out.println(r.param2);
            }
    );

.. note:: Filter实例化的输入，第三个是合约的地址，第四个是Topic的哈希值（SHA-3），返回结果中log的Data字段是事件值的rlp编码。

Event 事件内容获取
====================

根据Receipt，获取Event事件内容

.. code:: java

    // 调用demo合约的setName方法，参数输入字符串"Venachain"
    TransactionReceipt ret = demo.setName("Venachain").send();
    System.out.println("Transaction Hash: "+ret.getTransactionHash());

    // 根据receipt获取event数据
    List<Demo.SetNameEventResponse> eventParams = demo.getSetNameEvents(ret);
    System.out.println(eventParams.get(0).param1); // Event中第一个参数
    System.out.println(eventParams.get(0).param2); // Event中第二个参数

web3 api 调用
=================

.. code:: java

    web3j.ethBlockNumber(); // 当前最新区块高度
    web3j.ethGetTransactionByHash("0x..."); // 根据交易哈希多去交易内容
    web3j.ethGetTransactionReceipt("0x..."); // 根据交易哈希获取交易的回执
