====================
JAVA-SDK API 文档
====================

Hash
========

sha3
^^^^^^

**String sha3(String hexInput)**

计算输入字符串的sha3。

.. code:: java

    public class Demo {
        public static void main(String[] args) {
            //将合约文件转换为十六进制字符串
            byte[] dataBytes = Files.readBytes(new File("/home/username/example.wasm"));
            String binData = Hex.toHexString(dataBytes);

            //计算合约的sha3
            String sha3Value = Hash.sha3(binData);

            //验证结果
            System.out.println(sha3Value);
        }
    }

sha256
^^^^^^^^^^

**String sha256(byte[] input)**

计算输入的sha256。

.. code:: java

    public class Demo {
        public static void main(String[] args) {
            //读取合约文件
            byte[] dataBytes = Files.readBytes(new File("/home/username/example.wasm"));

            //计算合约的sha256
            String sha256Value = Hash.sha256(dataBytes);

            //验证结果
            System.out.println(sha256Value);
        }
    }

合约交互
==========

deploy
^^^^^^^^

**RemoteCall\<ContractType> deploy(Web3j web3j, Credentials credentials, String contractBinary, ContractGasProvider contractGasProvider)**

部署合约。

.. code:: java

    class Config {
        public static final String KEYFILE = "/home/username/Venachain/data/keystore/keyfile.json";
        public static final String PASSPHRASE = "passphrase";
        
        public static final String CONTRACTBINARY = "/home/username/example.wasm";
        public static final String URL = "http://127.0.0.1:6789";
    }

    public class Demo {
        public static void main(String[] args) {
            //建立连接
            Web3j web3 = Web3j.build(new HttpService(Config.URL));  // defaults to http://localhost:8545/
            
            //加载钱包
            //密钥账户，keyfile.json为venakey工具生成的账户文件，参照《Venachain密钥工具文档》
            Credentials credentials = WalletUtils.loadCredentials(Config.PASSPHRASE, Config.KEYFILE);
            
            //将合约文件转换为十六进制字符串
            byte[] dataBytes = Files.readBytes(new File(Config.CONTRACTBINARY));
            String contractBinary = Hex.toHexString(dataBytes);
            
            //部署合约
            YourContract contract = YourContract.deploy(web3j, credentials, contractBinary, new DefaultWasmGasProvider()).send();
                
            //调用demo合约的setName方法，参数输入字符串"Venachain"
            TransactionReceipt ret = demo.setName("Venachain").send();
            System.out.println("Transaction Hash: " + ret.getTransactionHash());

            //调用demo合约的getName方法
            System.out.println("getName: " + demo.getName().send());
        }
    }

load
^^^^^^

**ContractType load(String contractBinary, String contractAddressOrName, Web3j web3j, Credentials credentials, ContractGasProvider contractGasProvider)**

加载现有合约。在合约部署后，客户端可以通过合约地址或名称进行调用。

.. code:: java

    class Config {
        public static final String KEYFILE = "/home/username/Venachain/data/keystore/keyfile.json";
        public static final String PASSPHRASE = "passphrase";
        
        public static final String CONTRACTBINARY = "/home/username/example.wasm";
        public static final String URL = "http://127.0.0.1:6789";
    }

    public class Demo {
        public static void main(String[] args) {
            //建立连接
            Web3j web3 = Web3j.build(new HttpService(Config.URL));  // defaults to http://localhost:8545/
            
            //加载钱包
            Credentials credentials = WalletUtils.loadCredentials(Config.PASSPHRASE, Config.KEYFILE);
            
            //将合约文件转换为十六进制字符串
            byte[] dataBytes = Files.readBytes(new File(Config.CONTRACTBINARY));
            String contractBinary = Hex.toHexString(dataBytes);
            
            //加载合约
            YourContract contract = YourContract.load(contractBinary, "contract address or name", web3j, credentials, new DefaultWasmGasProvider()); 
            
            //调用demo合约的setName方法，参数输入字符串"Venachain"
            TransactionReceipt ret = demo.setName("Venachain").send();
            System.out.println("Transaction Hash: " + ret.getTransactionHash());

            //调用demo合约的getName方法
            System.out.println("getName: " + demo.getName().send());
        }
    }

subscribe
^^^^^^^^^^^

**Subscription subscribe(Observer\<T> observer)**

订阅区块（block）。在新区块产生时，client可以得到节点的区块数据推送。

.. code:: java

    public class Demo {
        public static void main(String[] args) {
            //建立连接
            Web3j web3j = Web3j.build(new HttpService("http://127.0.0.1:6789"));
            
            //订阅区块
            web3j.blockObservable(false).subscribe(block -> {
                System.out.println(block.getBlock().getNumber());
            });
        }
    }

订阅事件（event）。在合约中可以自定义事件，client通过订阅事件的方式来获悉合约调用中所触发的事件。例如，对于如下合约：

.. code:: java

    BCWASM_EVENT(setName,const char *) // event定义

    void setName(const char *msg)
    {
        bcwasm::setState("NAME_KEY", int, std::string(msg)); // 定义状态变量
        
        BCWASM_EMIT_EVENT(setName, 2020, "std::string(msg)"); // 日志输出 事件返回
    }

在Java合约框架中会生成与setName事件相关数据结构与接口，在服务层可以通过JavaSDK，监听该事件，示例代码如下：

.. code:: java

    class Config {
        public static final String KEYFILE = "/home/username/Venachain/data/keystore/keyfile.json";
        public static final String PASSPHRASE = "passphrase";
        
        public static final String CONTRACTBINARY = "/home/username/example.wasm";
        public static final String URL = "http://127.0.0.1:6789";
    }

    public static void main(String[] args) {
        //建立连接
        Web3j web3j = Web3j.build(new HttpService(Config.URL));

        try {
            //加载钱包
            Credentials credentials = WalletUtils.loadCredentials(Config.PASSPHRASE, Config.KEYFILE);
            
            String contractAddress = "0x1d7f2695b43be56f52f24baa199420f8c10ac1d3";
            String eventHash = Hash.sha3String("setName");
            
            //可自定义选取监听的区块范围，此处选取从最早到最新的全部区块 ealiest -> latest
            EthFilter filter = new EthFilter(DefaultBlockParameterName.EARLIEST, DefaultBlockParameterName.LATEST, contractAddress).addSingleTopic(eventHash);

            //订阅事件
            manager.setNameEventObservable(filter).subscribe(
                r -> {
                    System.out.println("\nlog: " + r.getLog());
                    System.out.println("\ncode: " + r.getCode());
                    System.out.println("\nmsg: " + r.getMsg());
                }
            );
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

.. note:: Filter实例化的输入，第三个是合约的地址，第四个是Topic的哈希值（SHA-3），返回结果中log的Data字段是事件值的rlp编码。

此外还可以通过调用合约方法返回的交易回执来获取事件。

.. code:: java

    public static void main(String[] args) {
        //建立连接
        Web3j web3j = Web3j.build(new HttpService(Config.URL));

        try {
            //加载钱包
            Credentials credentials = WalletUtils.loadCredentials(Config.PASSPHRASE, Config.KEYFILE);

            //加载合约
            YourContract contract = YourContract.load(contractBinary, "contract address or name", web3j, credentials, new DefaultWasmGasProvider()); 
            
            //调用合约中的 setName() 方法
            TransactionReceipt receipt = contract.setName("new contract name").send();

            //通过交易回执获取事件
            //EventResponse 供用户自行定义，封装事件的log，返回码，错误信息等
            List<EventResponse> respList = contract.getSetNameEvents(receipt);

            for (EventResponse resp : respList) {
                System.out.println("\nlog: " + resp.getLog());
                System.out.println("\ntopic: " + resp.getLog().getTopics());
                System.out.println("\ncode: " + resp.getCode());
                System.out.println("\nmsg: " + resp.getMsg());
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

unsubscribe
^^^^^^^^^^^^^^

**void unsubscribe()**

清除订阅。

.. code:: java

    public class Demo {
        public static void main(String[] args) {
            //建立连接
            Web3j web3j = Web3j.build(new HttpService("http://127.0.0.1:6789"));
            
            //订阅区块
            Subscription subs = web3j.blockObservable(false).subscribe();
            
            //清除订阅
            subs.unsubscribe();
        }
    }

交易相关
=========

ethSign
^^^^^^^^^

**Request<?, EthSign> ethSign(String address, String sha3HashOfDataToSign)**

对交易进行签名。

.. code:: java

    public class Demo {
        public static void main(String[] args) {
            BigInteger value = Convert.toWei("1", Convert.Unit.ETHER).toBigInteger();
            
            //创建交易，输入:nonce, gas price, gas limit, to (address), value
            RawTransaction rawTransaction = RawTransaction.createEtherTransaction(
                            BigInteger.valueOf(1048587), BigInteger.valueOf(500000), BigInteger.valueOf(500000),
                            "0x9C98E381Edc5Fe1Ac514935F3Cc3eDAA764cf004",
                            value);
            byte[] encoded = TransactionEncoder.encode(rawTransaction);
            byte[] hashed = Hash.sha3(encoded);

            //对交易进行签名
            EthSign ethSign = web3j.ethSign(ALICE.getAddress(), Numeric.toHexString(hashed)).sendAsync().get();

            //验证结果
            String signature = ethSign.getSignature();
        }
    }

ethSendRawTransaction
^^^^^^^^^^^^^^^^^^^^^^^

**Request<?, org.web3j.protocol.core.methods.response.EthSendTransaction> ethSendRawTransaction(String signedTransactionData)**

将已经签名的交易发送出去。

.. code:: java

    public class Demo {
        static final Credentials ALICE = Credentials.create(
            "",  // 32 byte hex value
            "0x"  // 64 byte hex value
        );
        static final Credentials BOB = Credentials.create(
            "",  // 32 byte hex value
            "0x"  // 64 byte hex value
        );
        static final int SLEEP_DURATION = 15000;
        static final int ATTEMPTS = 40;
        
        public static void main(String[] args) {
            //创建交易
            BigInteger nonce = getNonce(ALICE.getAddress());
            RawTransaction rawTransaction = createEtherTransaction(nonce, BOB.getAddress());

            //签名
            byte[] signedMessage = TransactionEncoder.signMessage(rawTransaction, ALICE);
            String hexValue = Numeric.toHexString(signedMessage);

            //发送交易
            EthSendTransaction ethSendTransaction = web3j.ethSendRawTransaction(hexValue).sendAsync().get();
            
            //获取交易回执
            String transactionHash = ethSendTransaction.getTransactionHash();

            Optional<TransactionReceipt> transactionReceiptOptional = getTransactionReceipt(transactionHash, SLEEP_DURATION, ATTEMPTS);

            if (transactionReceiptOptional.isPresent()) {
                TransactionReceipt transactionReceipt transactionReceiptOptional.get();
            } else {
                fail("Transaction receipt not generated after " + ATTEMPTS + " attempts");
            }
        }
    }

ethGetBlockReceipts
^^^^^^^^^^^^^^^^^^^^^

**Request<?, org.web3j.protocol.core.methods.response.EthGetBlockReceipts> ethGetBlockReceipts(DefaultBlockParameter defaultBlockParameter)**

获取区块上的所有交易的回执。

.. code:: java

    public class Demo {
        public static void main(String[] args) {
            // 建立连接
            Web3j web3j = Web3j.build(new HttpService("http://127.0.0.1:6791"));

            // 选择获取的区块号，此处为1
            DefaultBlockParameter blockNum = DefaultBlockParameter.valueOf(new BigInteger("1"));

            // 获取1号区块中所有交易的回执
            Optional<List<TransactionReceipt>> blockReceipts = web3j.ethGetBlockReceipts(blockNum).send().getAllReceipts();
            if (blockReceipts.isPresent()) {
                List<TransactionReceipt> rets = blockReceipts.get();

                for (TransactionReceipt tr : rets) {
                    System.out.println(tr);

                    System.out.println(tr.getLogs());
                    System.out.println(tr.getStatus());
                    System.out.println(tr.getTransactionHash());
                }
            }
        }
    }

getTransactionHash
^^^^^^^^^^^^^^^^^^^^^

**String getTransactionHash()**

获取部署合约返回的哈希值。

.. code:: java

    public static void main(String[] args) {
        TransactionReceipt receipt = yourSmartContract.setName("Venachain").send();
        String txHash = ret.getTransactionHash();

        System.out.println("Transaction Hash: " + txHash);
        
        //根据交易哈希获取交易的回执 
        EthGetTransactionReceipt response = web3j.ethGetTransactionReceipt("0x...").send();
        TransactionReceipt receipt = response.getTransactionReceipt();
        String txHash = ret.getTransactionHash();

        System.out.println("Transaction Hash: " + txHash);
        
        //后续可以根据receipt获取event数据
        List<YourSmartContract.SetNameEventResponse> eventParams = yourSmartContract.getSetNameEvents(receipt);

        System.out.println(eventParams.get(0).param1); // Event中第一个参数
        System.out.println(eventParams.get(0).param2); // Event中第二个参数
    }

getTransaction
^^^^^^^^^^^^^^^^

**Transaction getTransaction()**

根据交易哈希获取交易内容，例如Raw，Gas信息等。

.. code:: java

    public static void main(String[] args) {
        EthTransaction response = web3j.ethGetTransactionByHash("0x...").send();
        Transaction tx = response.getTransaction();

        System.out.println(tx.getRaw());
        System.out.println(tx.getGas());
        
        // ... tx.getXXXXX()
    }

账户操作
==========

personalNewAccount
^^^^^^^^^^^^^^^^^^^^^

**Request<?, PersonalNewAccount> personalNewAccount(String password)**

创建由节点控制的账户并返回账户地址。

.. code:: java

    public class Demo {
        public static void main(String[] args) {
            //建立连接
            Admin admin = Admin.build(new HttpService("http://127.0.0.1:6789"));
            
            //发送rpc创建账户
            PersonalNewAccount response = admin.personalNewAccount("password").send();

            String accountId = response.getAccountId();      
        }
    }

personalListAccounts
^^^^^^^^^^^^^^^^^^^^^^^^^

**Request<?, PersonalListAccounts> personalListAccounts()**

得到由节点控制的账户列表。

.. code:: java

    public class Demo {
        public static void main(String[] args) {
            //建立连接
            Admin admin = Admin.build(new HttpService("http://127.0.0.1:6789"));
            
            //发送rpc
            PersonalListAccounts response = admin.personalListAccounts().send();

            //获取账户列表
            List<String> accountIds = response.getAccountIds();
        }
    }

personalUnlockAccount
^^^^^^^^^^^^^^^^^^^^^^^^

**Request<?, PersonalUnlockAccount> personalUnlockAccount(String accountId, String password, BigInteger duration)**

解锁节点保存的账户

.. code:: java

    public class Demo {
        public static void main(String[] args) {
            //建立连接
            Admin admin = Admin.build(new HttpService("http://127.0.0.1:6789"));
            
            //发送rpc
            PersonalUnlockAccount response = admin.personalUnlockAccount("accountId", "password", 600).send();

            //获取结果
            Boolean result = response.accountUnlocked();
        }
    }

其他
=====

getAddress
^^^^^^^^^^^^^

**String getAddress(String publicKey)**

根据公钥计算地址。

.. code:: java

    public static void main(String[] args) {
        String address = Keys.getAddress("public key");
        System.out.println(address);
    }

getAccounts
^^^^^^^^^^^^^^

**List\<String> getAccounts()**

获取由节点控制的账户列表。

.. code:: java

    public static void main(String[] args) {
        EthAccounts response = web3j.ethAccounts().send();
        List<String> accountsList = response.getAccounts();
    }

getBlockNumber
^^^^^^^^^^^^^^^^^

**BigInteger getBlockNumber()**

获取当前最新区块高度。

.. code:: java

    public static void main(String[] args) {
        EthBlockNumber response = web3j.ethBlockNumber().send();
        System.out.println(response.getBlockNumber());
    }