# JAVA-SDK 预编译合约文档

## 1. 用户权限


### 1.1 setSuperAdmin

+ 接口： setSuperAdmin()
+ 入参： 空
+ 返回值：TransactionReceipt
+ Event事件： 
  + setSuperAdmin(int code, string msg)
  + topic: "0x7e901ebc26b576bc08715c4d41c6a7732dd47c98452d104a8b6d4b15f0cd7cbb"
+ 说明： 链只有一个SuperAdmin，若已设置超级管理员，则该调用无效。

调用示例：

```java
public class Demo {
    public class Config {
        public static final String URL = "http://127.0.0.1:6789";

        public static final String KEYFILE = "/home/user/Venachain/build/bin/data/keystore/keyfile";
        public static final String PASSPHRASE = "passphrase";

        public static final String CONTRACTBINARY = "/home/user/Downloads/contractBinary.wasm";

        public static final String USER1NAME = "user1name";
        public static final String USER1ADDRESS = "0x123456...";
        public static final String USER2NAME = "user2name";
        public static final String USER2ADDRESS = "0x235869...";
        public static final String USER3ADDRESS = "0x354758...";
    }
    
    public static void main(String[] args) {
        //建立连接
        Web3j web3j = Web3j.build(new HttpService(Config.URL));

        try {
            //加载钱包
            Credentials credentials = WalletUtils.loadCredentials(Config.PASSPHRASE, Config.KEYFILE);

            //加载合约
            UserManagement manager = UserManagement.load(web3j, credentials, new DefaultWasmGasProvider());
            
            String eventHash = Hash.sha3String(UserManagement.FUNC_SETSUPERADMIN);
            //Filter实例化的输入，返回结果中log的Data字段是事件值的rlp编码
            EthFilter filter = new EthFilter(DefaultBlockParameterName.LATEST, DefaultBlockParameterName.PENDING, UserManagement.CONTRACT_ADDRESS).addSingleTopic(eventHash);
            
            //订阅事件，client通过订阅事件的方式来获悉合约调用中所触发的事件
            manager.setNameEventObservable(filter).subscribe(
                r -> {
                    System.out.println("\nlog: " + r.getLog());
                    System.out.println("\ncode: " + r.getCode());
                    System.out.println("\nmsg: " + r.getMsg());
                }
            );
            
            //调用系统合约 UserManagement 中的 setSuperAdmin() 方法
            TransactionReceipt receipt = manager.setSuperAdmin().send();
            
            //验证结果: ["SUPER_ADMIN"]
            System.out.println(manager.getRolesByAddress(Config.USER1ADDRESS).send());
            
            //通过交易回执获取事件
            List<EventResponse> respList = manager.getSetSuperAdminEvents(receipt);
            
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
}
```

### 1.2 transferSuperAdminByAddress

+ 接口： transferSuperAdminByAddress(String address)
+ 入参： String 地址
+ 返回值：TransactionReceipt
+ Event事件： 
  + transferSuperAdminByAddress(int code, string msg)
  + topic: "0x04dc07acc5837377e61181a20a86aa29043cf4f14f9877a7e6e54b8a41dc4a8c"
+ 说明： 只有SuperAdmin可以调用该接口转让SuperAdmin身份，需要谨慎调用。

调用示例：

```java
public class Demo {
    public class Config { /*...*/ }
    
    public static void main(String[] args) {
        //建立连接
        Web3j web3j = Web3j.build(new HttpService(Config.URL));

        try {
            //加载钱包
            Credentials credentials = WalletUtils.loadCredentials(Config.PASSPHRASE, Config.KEYFILE);

            //加载合约
            UserManagement manager = UserManagement.load(web3j, credentials, new DefaultWasmGasProvider());

            //调用系统合约 UserManagement 中的 transferSuperAdminByAddress() 方法
            TransactionReceipt receipt = manager.transferSuperAdminByAddress(Config.USER2).send();

            //返回交易回执
            System.out.println(receipt);
            
            //验证结果: ["SUPER_ADMIN"]
            System.out.println(manager.getRolesByAddress(USER1ADDRESS).send());
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

### 1.3 getRolesByAddress

+ 接口名称： getRolesByAddress(String address)
+ 入参： String 地址
+ 返回值：String 角色列表
+ 说明：给定用户地址，获取用户的角色

调用示例：

```java
public class Demo {
    public class Config { /*...*/ }
    
    public static void main(String[] args) {
        //建立连接
        Web3j web3j = Web3j.build(new HttpService(Config.URL));

        try {
            //加载钱包
            Credentials credentials = WalletUtils.loadCredentials(Config.PASSPHRASE, Config.KEYFILE);

            //加载合约
            UserManagement manager = UserManagement.load(web3j, credentials, new DefaultWasmGasProvider());

            //调用系统合约 UserManagement 中的 getRolesByAddress() 方法
            System.out.println(manager.getRolesByAddress(USER1ADDRESS).send());
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

### 1.4 getRolesByName

+ 接口名称： getRolesByName(String address)
+ 入参： String 地址
+ 返回值：String 角色列表
+ 说明：给定用户名称，获取用户的角色

调用示例：

```java
public class Demo {
  // 参考 getRolesByAddress()   
}
```

### 1.5 getAddrListOfRole

+ 接口名称： getAddrListOfRole(String role)
+ 入参： String 角色
+ 返回值：String 地址列表
+ 说明：给定角色，获取所有拥有该角色的用户的地址

调用示例：

```java
public class Demo {
    public class Config { /*...*/ }
    
    public static void main(String[] args) {
        //建立连接
        Web3j web3j = Web3j.build(new HttpService(Config.URL));

        try {
            //加载钱包
            Credentials credentials = WalletUtils.loadCredentials(Config.PASSPHRASE, Config.KEYFILE);

            //加载合约
            UserManagement manager = UserManagement.load(web3j, credentials, new DefaultWasmGasProvider());

            //调用系统合约 UserManagement 中的 getAddrListOfRole() 方法
            System.out.println(manager.getAddrListOfRole("SUPER_ADMIN").send());
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

### 1.6 addUser

+ 接口名称： addUser(String userInfo)
+ 入参： String 角色信息
+ 返回值：TransactionReceipt
+ Event事件： 
  + addUser(int code, string msg)
  + topic: "0x9ccac5649883d3dcc5360b728ec01c6ca72122f7a364e6a859896afa72b21c8b"
+ 说明：将用户添加到用户管理系统中

调用示例：

```java
public class Demo {
  public class Config { /*...*/ }

  public static void main(String[] args) {
    //建立连接
    Web3j web3j = Web3j.build(new HttpService(Config.URL));

    try {
      //加载钱包
      Credentials credentials = WalletUtils.loadCredentials(Config.PASSPHRASE, Config.KEYFILE);

      //加载合约
      UserManagement manager = UserManagement.load(web3j, credentials, new DefaultWasmGasProvider());

      // 用户信息
      String user = "{\"address\":\"0x2d25d910d50f1b13e960f5823cf3027050177751\",\"name\":\"superuser\"}";
      
      //调用系统合约 UserManagement 中的 addUser() 方法
      System.out.println("\nResult for add user:\n" + manager.addUser(user).send());

      // 验证
      System.out.println("\nAll users:\n" + manager.getAllUsers().send());
    } catch (Exception e) {
      e.printStackTrace();
    }
  }
}
```

### 1.7 updateUserDescInfo

+ 接口名称： updateUserDescInfo(String address, String userInfo)
+ 入参： String 地址，用户信息 (JSON)
+ 返回值：TransactionReceipt
+ Event事件： 
  + updateUserDescInfo(int code, string msg)
  + topic: "0xfa385413c593ef913f0920d01d863405847947cc72f3d6c65894740a3fae7b61"
+ 说明：
  + ChainAdmin可以调用该接口
  + 更新方式为增量更新，比如如果传入参数中未包含相关字段，则维持原有信息不变。

调用示例：

```java
public class Demo {
    static class Config { /*...*/ }

    public static void main(String[] args) {
        Web3j web3j = Web3j.build(new HttpService(Config.URL));

        try {
            //加载钱包
            Credentials credentials = WalletUtils.loadCredentials(Config.PASSPHRASE, Config.KEYFILE);

            //加载合约
            UserManagement manager = UserManagement.load(web3j, credentials, new DefaultWasmGasProvider());

            //调用系统合约 UserManagement 中的 updateUserDescInfo() 方法
            manager.updateUserDescInfo("0xahfdalkf...,", "{\"phone\":\"1100000000\",\"email\":\"gexin@wx.com\",\"organization\":\"wxbc\"}").send();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```


### 1.8 getAllUsers

+ 接口名称： getAllUsers()
+ 入参： 空
+ 返回值：String 角色列表
+ 说明：获取全部用户具体信息

调用示例：

```java
public class Demo {
    static class Config { /*...*/ }

    public static void main(String[] args) {
        Web3j web3j = Web3j.build(new HttpService(Config.URL));

        try {
            //加载钱包
            Credentials credentials = WalletUtils.loadCredentials(Config.PASSPHRASE, Config.KEYFILE);

            //加载合约
            UserManagement manager = UserManagement.load(web3j, credentials, new DefaultWasmGasProvider());

            //调用系统合约 UserManagement 中的 getAllUsers() 方法
            System.out.println("\nResult for getting all users: \n" + manager.getAllUsers().send());
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

### 1.9 getUserByAddress

+ 接口名称： getUserByAddress(String address)
+ 入参： String 地址
+ 返回值：String 角色信息
+ 说明：给定用户地址，获取用户具体信息

调用示例：

```java
public class Demo {
    public class Config { /*...*/ }
    
    public static void main(String[] args) {
        //建立连接
        Web3j web3j = Web3j.build(new HttpService(Config.URL));

        try {
            //加载钱包
            Credentials credentials = WalletUtils.loadCredentials(Config.PASSPHRASE, Config.KEYFILE);

            //加载合约
            UserManagement manager = UserManagement.load(web3j, credentials, new DefaultWasmGasProvider());

            //调用系统合约 UserManagement 中的 getUserByAddress() 方法
            System.out.println(manager.getUserByAddress(USER1ADDRESS).send());
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

### 1.10 getUserByName

+ 接口名称： getUserByAddress(String name)
+ 入参： String 用户姓名
+ 返回值：String 角色信息
+ 说明：给定用户姓名，获取用户具体信息

调用示例：

```java
public class Demo {
    public class Config { /*...*/ }
    
    public static void main(String[] args) {
        //建立连接
        Web3j web3j = Web3j.build(new HttpService(Config.URL));

        try {
            //加载钱包
            Credentials credentials = WalletUtils.loadCredentials(Config.PASSPHRASE, Config.KEYFILE);

            //加载合约
            UserManagement manager = UserManagement.load(web3j, credentials, new DefaultWasmGasProvider());

            //调用系统合约 UserManagement 中的 getUserByName() 方法
            System.out.println(manager.getUserByName(USER1NAME).send());
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

### 1.11 addChainAdminByAddress

+ 接口： addChainAdminByAddress(String address)
+ 入参： String 地址
+ 返回值：TransactionReceipt
+ Event事件：
  + addChainAdminByAddress(int code, string msg)
  + topic: "0x8bfc2a5b39cf58956d263532fd7f80deaec7bfb37f8465f1931b6980ee43cf62"
+ 说明：只有SuperAdmin可以调用该接口

调用示例：

```java
public class Demo {
    public class Config { /*...*/ }

    public static void main(String[] args) {
        //建立连接
        Web3j web3j = Web3j.build(new HttpService(Config.URL));

        try {
            //加载钱包
            Credentials credentials = WalletUtils.loadCredentials(Config.PASSPHRASE, Config.KEYFILE);

            //加载合约
            UserManagement manager = UserManagement.load(web3j, credentials, new DefaultWasmGasProvider());

			String eventHash = Hash.sha3String(UserManagement.FUNC_ADDCHAINADMINBYADDRESS);
            //Filter实例化的输入，返回结果中log的Data字段是事件值的rlp编码
            EthFilter filter = new EthFilter(DefaultBlockParameterName.LATEST, DefaultBlockParameterName.PENDING, UserManagement.CONTRACT_ADDRESS).addSingleTopic(eventHash);

            //订阅事件，client通过订阅事件的方式来获悉合约调用中所触发的事件
            manager.addChainAdminByAddressObservable(filter).subscribe(
                    r -> {
                        System.out.println("\nlog: " + r.getLog());
                        System.out.println("\ncode: " + r.getCode());
                        System.out.println("\nmsg: " + r.getMsg());
                                      }
            );
            //调用系统合约 UserManagement 中的 addChainAdminByAddress() 方法
            TransactionReceipt receipt = manager.addChainAdminByAddress(Config.USER1).send();

            //返回交易回执
            System.out.println(receipt);

            //验证结果: ["SUPER_ADMIN", "CHAIN_ADMIN"]
            System.out.println(manager.getRolesByAddress(Config.USER1).send());
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

### 1.12 addChainAdminByName

+ 接口： addChainAdminByName(String name)
+ 入参： String 用户名
  + 用户名长度不大于128，由字母、数字、下线组成（也支持中文字符，但不建议使用中文字符作为用户名）
+ 返回值：TransactionReceipt 
+ Event事件： 
  + addChainAdminByName(int code, string msg)
  + topic: "0x7466e25f058497ccd4c4ac063be5e803e9e6d517d0a20140cbe56c3217055a85"
+ 说明： 只有SuperAdmin可以调用该接口

调用示例：

```java
public class Demo {
    public class Config { /*...*/ }

    public static void main(String[] args) {
        //建立连接
        Web3j web3j = Web3j.build(new HttpService(Config.URL));

        try {
            //加载钱包
            Credentials credentials = WalletUtils.loadCredentials(Config.PASSPHRASE, Config.KEYFILE);

            //加载合约
            UserManagement manager = UserManagement.load(web3j, credentials, new DefaultWasmGasProvider());

			String eventHash = Hash.sha3String(UserManagement.FUNC_ADDCHAINADMINBYNAME);
            //Filter实例化的输入，返回结果中log的Data字段是事件值的rlp编码
            EthFilter filter = new EthFilter(DefaultBlockParameterName.LATEST, DefaultBlockParameterName.PENDING, UserManagement.CONTRACT_ADDRESS).addSingleTopic(eventHash);

            //订阅事件，client通过订阅事件的方式来获悉合约调用中所触发的事件
            manager.addChainAdminByNameObservable(filter).subscribe(
                    r -> {
                        System.out.println("\nlog: " + r.getLog());
                        System.out.println("\ncode: " + r.getCode());
                        System.out.println("\nmsg: " + r.getMsg());
                                         }
            );
            //调用系统合约 UserManagement 中的 addChainAdminByName() 方法
            TransactionReceipt receipt = manager.addChainAdminByName(Config.USER2NAME).send();

            //返回交易回执
            System.out.println(receipt);

            //验证结果: ["CHAIN_ADMIN"]
            System.out.println(manager.getRolesByAddress(Config.USER2).send());
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

### 1.13 delChainAdminByAddress

+ 接口名称： delChainAdminByAddress(String address)
+ 入参： String 地址
+ 返回值：TransactionReceipt
+ Event事件： 
  + delChainAdminByAddress(int code, string msg)
  + topic: "0xddbe46696d8a768320bc2fc723b1d25499a1b927df40377e8cf45a88b7485659"
+ 说明： 只有SuperAdmin可以调用该接口

调用示例：

```java
public class Demo {
    public class Config { /*...*/ }

    public static void main(String[] args) {
        //建立连接
        Web3j web3j = Web3j.build(new HttpService(Config.URL));

        try {
            //加载钱包
            Credentials credentials = WalletUtils.loadCredentials(Config.PASSPHRASE, Config.KEYFILE);

            //加载合约
            UserManagement manager = UserManagement.load(web3j, credentials, new DefaultWasmGasProvider());
			String eventHash = Hash.sha3String(UserManagement.FUNC_DELCHAINADMINBYADDRESS);
            //Filter实例化的输入，返回结果中log的Data字段是事件值的rlp编码
            EthFilter filter = new EthFilter(DefaultBlockParameterName.LATEST, DefaultBlockParameterName.PENDING, UserManagement.CONTRACT_ADDRESS).addSingleTopic(eventHash);

            //订阅事件，client通过订阅事件的方式来获悉合约调用中所触发的事件
            manager.delChainAdminByAddressObservable(filter).subscribe(
                    r -> {
                        System.out.println("\nlog: " + r.getLog());
                        System.out.println("\ncode: " + r.getCode());
                        System.out.println("\nmsg: " + r.getMsg());
                                          }
            );
            //调用系统合约 UserManagement 中的 delChainAdminByAddress() 方法
            TransactionReceipt receipt = manager.delChainAdminByAddress(Config.USER1ADDRESS).send();

            //返回交易回执
            System.out.println(receipt);

            //验证结果: ["SUPER_ADMIN"]
            System.out.println(manager.getRolesByAddress(Config.USER1).send());
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

### 1.14 delChainAdminByName

+ 接口名称： delChainAdminByName(String name)
+ 入参： String 用户名
  + 用户名长度不大于128，由字母、数字、下线组成（也支持中文字符，但不建议使用中文字符作为用户名）
+ 返回值：TransactionReceipt
+ Event事件： 
  + delChainAdminByName(int code, string msg)
  + topic: "0x883bbe522eb02a648a58e15aec7df01dd3fe233fd1d658d210f01d7b6b907c83"
+ 说明： 只有SuperAdmin可以调用该接口

调用示例：

```java
public class Demo {
    public class Config { /*...*/ }

    public static void main(String[] args) {
        //建立连接
        Web3j web3j = Web3j.build(new HttpService(Config.URL));

        try {
            //加载钱包
            Credentials credentials = WalletUtils.loadCredentials(Config.PASSPHRASE, Config.KEYFILE);

            //加载合约
            UserManagement manager = UserManagement.load(web3j, credentials, new DefaultWasmGasProvider());
			String eventHash = Hash.sha3String(UserManagement.FUNC_DELCHAINADMINBYNAME);
            //Filter实例化的输入，返回结果中log的Data字段是事件值的rlp编码
            EthFilter filter = new EthFilter(DefaultBlockParameterName.LATEST, DefaultBlockParameterName.PENDING, UserManagement.CONTRACT_ADDRESS).addSingleTopic(eventHash);

            //订阅事件，client通过订阅事件的方式来获悉合约调用中所触发的事件
            manager.delChainAdminByNameObservable(filter).subscribe(
                    r -> {
                        System.out.println("\nlog: " + r.getLog());
                        System.out.println("\ncode: " + r.getCode());
                        System.out.println("\nmsg: " + r.getMsg());  
                    }
            );
            //调用系统合约 UserManagement 中的 delChainAdminByName() 方法
            TransactionReceipt receipt = manager.delChainAdminByName(Config.USER1NAME).send();

            //返回交易回执
            System.out.println(receipt);

            //验证结果: ["SUPER_ADMIN"]
            System.out.println(manager.getRolesByAddress(Config.USER1).send());
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

### 1.15 addGroupAdminByAddress

+ 说明： 只有ChainAdmin可以调用该接口，设置用户为链管理员

```java
public class Demo {
  // 参考 addChainAdminByAddress()   
}
```

### 1.16 addGroupAdminByName

+ 说明： 只有ChainAdmin可以调用该接口，设置用户为链管理员

```java
public class Demo {
  // 参考 addChainAdminByName()   
}
```

### 1.17 delGroupAdminByAddress

+ 说明： 只有ChainAdmin可以调用该接口，设置用户为链管理员

```java
public class Demo {
  // 参考 delChainAdminByAddress()   
}
```

### 1.18 delGroupAdminByName

+ 说明： 只有ChainAdmin可以调用该接口，设置用户为链管理员

```java
public class Demo {
  // 参考 delChainAdminByName()   
}
```

### 1.19 addNodeAdminByAddress

+ 说明： 只有ChainAdmin可以调用该接口，设置用户为节点管理员

```java
public class Demo {
  // 参考 addChainAdminByAddress()   
}
```

### 1.20 addNodeAdminByName

+ 说明： 只有ChainAdmin可以调用该接口，设置用户为节点管理员

```java
public class Demo {
  // 参考 addChainAdminByName()   
}
```

### 1.21 delNodeAdminByAddress

+ 说明： 只有ChainAdmin可以调用该接口，设置用户为节点管理员

```java
public class Demo {
  // 参考 delChainAdminByAddress()   
}
```

### 1.22 delNodeAdminByName

+ 说明： 只有ChainAdmin可以调用该接口，设置用户为节点管理员

```java
public class Demo {
  // 参考 delChainAdminByName()   
}
```

### 1.23 addContractAdminByAddress

+ 说明： 只有ChainAdmin可以调用该接口，设置用户为合约管理员

```java
public class Demo {
  // 参考 addChainAdminByAddress()   
}
```

### 1.24 addContractAdminByName

+ 说明： 只有ChainAdmin可以调用该接口，设置用户为合约管理员

```java
public class Demo {
  // 参考 addChainAdminByName()   
}
```

### 1.25 delContractAdminByAddress

+ 说明： 只有ChainAdmin可以调用该接口，设置用户为合约管理员

```java
public class Demo {
  // 参考 delChainAdminByAddress()   
}
```

### 1.26 delContractAdminByName

+ 说明： 只有ChainAdmin可以调用该接口，设置用户为合约管理员

```java
public class Demo {
  // 参考 delChainAdminByName()   
}
```

### 1.27 addContractDeployerByAddress

+ 说明： ChainAdmin、ContractAdmin可以调用该接口

```java
public class Demo {
  // 参考 addChainAdminByAddress()   
}
```

### 1.28 addContractDeployerByName

+ 说明： ChainAdmin、ContractAdmin可以调用该接口

```java
public class Demo {
  // 参考 addChainAdminByName()   
}
```

### 1.29 delContractDeployerByAddress

+ 说明： ChainAdmin、ContractAdmin可以调用该接口

```java
public class Demo {
  // 参考 delChainAdminByAddress()   
}
```

### 1.30 delContractDeployerByName

+ 说明： ChainAdmin、ContractAdmin可以调用该接口

```java
public class Demo {
  // 参考 delChainAdminByName()   
}
```

## 2. 参数

```{note}
限制：
* set类方法只有ChainAdmin可以调用
* get类方法都可以调用，无权限要求
```

### 2.1 setGasContractName / getGasContractName

+ 接口： setGasContractName(String name)
+ 入参： String 合约名
+ 返回值：TransactionReceipt
+ 接口： getGasContractName()
+ 入参： 空
+ 返回值：String
+ 说明： 消耗特定合约代币名称参数设置
+ 事件： 
  + 事件名：GasContractName(int code, string msg)
  + Topic: 0xb6d24fc9e837f5361659c92524d539907afff716cba6aea7900cf58588c1e471 
    + （Hash.sha3String(ParamManagement.GasContractNameKey)
  + 参数： 
    + 结果码（uint类型） 
      + 0:  成功 
      + 1:  没有权限 
      + 2:  rlp编码错误
      + 3:  无效入参
      + 4:  合约未注册
    + 结果消息（string类型）

调用示例：

```java
public class Demo {
    public class Config { /*...*/ }
    
    public static void main(String[] args) {
        //建立连接
        Web3j web3j = Web3j.build(new HttpService(Config.URL));

        try {
            //加载钱包
            Credentials credentials = WalletUtils.loadCredentials(Config.PASSPHRASE, Config.KEYFILE);
          
          // 事件topic
          String eventHash = Hash.sha3String(ParamManagement.GasContractNameKey);

          EthFilter filter = new EthFilter(DefaultBlockParameterName.LATEST, DefaultBlockParameterName.PENDING, ParamManagement.CONTRACT_ADDRESS).addSingleTopic(eventHash);
            manager.setTxGasLimitObservable(filter).subscribe(
                    r -> {
                        System.out.println("\nlog: " + r.getLog());
                        System.out.println("\ncode: " + r.getCode());
                        System.out.println("\nmsg: " + r.getMsg());
                    }
            );

            //加载合约
            ParamManagement manager = ParamManagement.load(web3j, credentials, new DefaultWasmGasProvider());

            //调用系统合约 ParamManagement 中的 setGasContractName() 方法
            manager.setGasContractName("wxbc").send();
            
            //验证结果
            System.out.println(manager.getGasContractName().send());
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

### 2.2 setIsProduceEmptyBlock / getIsProduceEmptyBlock

+ 接口： setIsProduceEmptyBlock(BigInteger value)
+ 入参： BigInteger 1表示产生空块；0表示不产生空块；其他无效
+ 返回值：TransactionReceipt
+ 接口： getIsProduceEmptyBlock()
+ 入参： 空
+ 返回值：String
+ 说明：空块产生参数设置
+ 事件：
  + 事件名：IsProduceEmptyBlock(int code, string msg)
  + Topic: 0xa9e21c0522cfd72cfaf98346cd66ec0af65b300d642923296c441a8c2c983c4d 
    + Hash.sha3String(ParamManagement.IsProduceEmptyBlockKey)
  + 参数：
    + 结果码（uint类型）
      + 0:  成功
      + 1:  没有权限
      + 2:  rlp编码错误
      + 3:  无效入参
    + 结果消息（string类型）

调用示例：

```java
public class Demo {
    // 参考 setGasContractName / getGasContractName
    // 订阅事件
    String eventHash = Hash.sha3String(ParamManagement.IsProduceEmptyBlockKey);
    EthFilter filter = new EthFilter(DefaultBlockParameterName.LATEST, DefaultBlockParameterName.PENDING, ParamManagement.setIsProduceEmptyBlockObservable(filter).subscribe(
      r -> {
        System.out.println("\nlog: " + r.getLog());
        System.out.println("\ncode: " + r.getCode());
        System.out.println("\nmsg: " + r.getMsg());
      }
    );
}
```

### 2.3 setTxGasLimit / getTxGasLimit

+ 接口： setTxGasLimit(BigInteger gasLimit)
+ 入参： BigInteger Gas限制 gas上限：2e9  gas下限：12771596 * 100
+ 返回值：TransactionReceipt
+ 接口： getTxGasLimit()
+ 入参： 空
+ 返回值：String
+ 说明：交易gas限制设置
+ 事件：
  + 事件名：TxGasLimit(int code, string msg)
  + Topic: 0x5bde5fa4722a26a37dced3593fb92e3f3cf748e22c4d4e4de2f6e72d81a4673d 
    + Hash.sha3String(ParamManagement.TxGasLimitKey)
  + 参数：
    + 结果码（uint类型）
      + 0:  成功
      + 1:  没有权限
      + 2:  rlp编码错误
      + 3:  无效入参
    + 结果消息（string类型）

调用示例：

```java
public class Demo {
    public class Config { /*...*/ }

    public static void main(String[] args) {
        //建立连接
        Web3j web3j = Web3j.build(new HttpService(Config.URL));

        try {
            //加载钱包
            Credentials credentials = WalletUtils.loadCredentials(Config.PASSPHRASE, Config.KEYFILE);

            //加载合约
            ParamManagement manager = ParamManagement.load(web3j, credentials, new DefaultWasmGasProvider());
          
          	// 订阅事件
            String eventHash = Hash.sha3String(ParamManagement.TxGasLimitKey);
            EthFilter filter = new EthFilter(DefaultBlockParameterName.LATEST, DefaultBlockParameterName.PENDING, ParamManagement.setTxGasLimitObservable(filter).subscribe(
              r -> {
                System.out.println("\nlog: " + r.getLog());
                System.out.println("\ncode: " + r.getCode());
                System.out.println("\nmsg: " + r.getMsg());
              }
            );

            //调用系统合约 ParamManagement 中的 setTxGasLimit() 方法
            manager.setTxGasLimit(new BigInteger("2000000000")).send();

            //验证结果
            System.out.println(manager.getTxGasLimit().send());
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

### 2.4 getBlockGasLimit / setBlockGasLimit

+ 接口： getBlockGasLimit()
+ 入参： BigInteger gas限制 gas上限：2e10 gas下限：12771596 * 100
+ 返回值：TransactionReceipt
+ 接口： setBlockGasLimit(BigInteger gasLimit)
+ 入参： 空
+ 返回值：String
+ 说明： 区块gas限制设置
+ 事件：
  + 事件名：BlockGasLimt(int code, string msg)
  + Topic: 0x78148ab89ff26e82fd3538bc45b5d3a8457a27c2d3c659aae6e6b12420615f73 
    + Hash.sha3String(ParamManagement.BlockGasLimitKey)
  + 参数：
    + 结果码（uint类型）
      + 0:  成功
      + 1:  没有权限
      + 2:  rlp编码错误
      + 3:  无效入参
    + 结果消息（string类型）

调用示例：

```java
public class Demo {
    // 参考 setTxGasLimit / getTxGasLimit
  	// 订阅事件
  	String eventHash = Hash.sha3String(ParamManagement.BlockGasLimitKey);
    EthFilter filter = new EthFilter(DefaultBlockParameterName.LATEST, DefaultBlockParameterName.PENDING, ParamManagement.setBlockGasLimitObservable(filter).subscribe(
      r -> {
        System.out.println("\nlog: " + r.getLog());
        System.out.println("\ncode: " + r.getCode());
        System.out.println("\nmsg: " + r.getMsg());
      }
    );
}
```

### 2.5 setCheckContractDeployPermission / getCheckContractDeployPermission

+ 接口： setCheckContractDeployPermission(BigInteger value)
+ 入参： BigInteger 1表示检查合约部署权限，用户具有相应权限才可以部署合约；0表示不检查合约部署权限，允许任意用户部署合约；其他无效
+ 返回值：TransactionReceipt
+ 接口： getCheckContractDeployPermission()
+ 入参： 空
+ 返回值：String
+ 说明：检查合约部署权限设置
+ 说明： 区块gas限制设置
+ 事件：
  + 事件名：IsCheckContractDeployPermission(int code, string msg)
  + Topic: 0x6e7dd5a4ebd11e3b3cf569ea5c6fea10966208f4a067754c17a5d5fcdf4c93de
    + Hash.sha3String(ParamManagement.IsCheckContractDeployPermissionKey)
  + 参数：
    + 结果码（uint类型）
      + 0:  成功
      + 1:  没有权限
      + 2:  rlp编码错误
      + 3:  无效入参
    + 结果消息（string类型）

调用示例：

```java
public class Demo {
    // 参考 setTxGasLimit / getTxGasLimit
  	// 订阅事件
  	String eventHash = Hash.sha3String(ParamManagement.IsCheckContractDeployPermissionKey);
    EthFilter filter = new EthFilter(DefaultBlockParameterName.LATEST, DefaultBlockParameterName.PENDING, ParamManagement.setCheckContractDeployPermissionObservable(filter).subscribe(
      r -> {
        System.out.println("\nlog: " + r.getLog());
        System.out.println("\ncode: " + r.getCode());
        System.out.println("\nmsg: " + r.getMsg());
      }
    );	
}
```


### 2.6 setIsTxUseGas / getIsTxUseGas

+ 接口： setIsTxUseGas(BigInteger value)
+ 入参： BigInteger 1表示交易消耗gas；0表示交易不消耗gas；其他无效
+ 返回值：TransactionReceipt
+ 接口： getIsTxUseGas()
+ 入参： 空
+ 返回值：String
+ 说明：交易是否消耗gas设置
+ 事件：
  + 事件名：setIsTxUseGas(int code, string msg)
  + Topic: 0x56bf34d9d39c7e0b1ac3aaaffdda121743da153952eb0a7c55b1c3b37a4dadde
    + Hash.sha3String(ParamManagement.IsTxUseGasKey)
  + 参数：
    + 结果码（uint类型）
      + 0:  成功
      + 1:  没有权限
      + 2:  rlp编码错误
      + 3:  无效入参
    + 结果消息（string类型）

调用示例：

```java
public class Demo {
    // 参考 setTxGasLimit / getTxGasLimit
  	// 订阅事件
  	String eventHash = Hash.sha3String(ParamManagement.IsTxUseGasKey);
    EthFilter filter = new EthFilter(DefaultBlockParameterName.LATEST, DefaultBlockParameterName.PENDING, ParamManagement.setIsTxUseGasObservable(filter).subscribe(
      r -> {
        System.out.println("\nlog: " + r.getLog());
        System.out.println("\ncode: " + r.getCode());
        System.out.println("\nmsg: " + r.getMsg());
      }
    );
}
```

### 2.7 setVRFParams / getVRFParams

+ 接口： setVRFParams(String vrfParams)
+ 入参： String VRF参数
+ 返回值：TransactionReceipt
+ 接口： getVRFParams()
+ 入参： 空
+ 返回值：String
+ 说明：vrf参数设置
  * electionEpoch     vrf选举区块间隔（0表示不开启vrf功能）
  * nextElectionBlock 下次进行vrf选举的区块高度（与electionEpoch是并行逻辑判断，用于主动发起vrf选举）
  * validatorCount    选举出的共识节点数量（候选节点不足时，取所有候选节点作为共识节点）
+ 说明：交易是否消耗gas设置
+ 事件：
  + Topic: 0x1754fcc737bde6a7de9f5bd6ec36edb44ec4e30dda71d84a74441f4294051684
    + Hash.sha3String(ParamManagement.VrfParamsKey)
  + 参数：
    + 结果码（uint类型）
      + 0:  成功
      + 1:  没有权限
      + 2:  rlp编码错误
      + 3:  无效入参
    + 结果消息（string类型）

调用示例：

```java
public class Demo {
    public class Config { /*...*/ }
    
    public static void main(String[] args) {
        //建立连接
        Web3j web3j = Web3j.build(new HttpService(Config.URL));

        try {
            //加载钱包
            Credentials credentials = WalletUtils.loadCredentials(Config.PASSPHRASE, Config.KEYFILE);

            //加载合约
            ParamManagement manager = ParamManagement.load(web3j, credentials, new DefaultWasmGasProvider());
          	
          	// 订阅事件
            String eventHash = Hash.sha3String(ParamManagement.VrfParamsKey);
            EthFilter filter = new EthFilter(DefaultBlockParameterName.LATEST, DefaultBlockParameterName.PENDING, ParamManagement.setVRFParamsObservable(filter).subscribe(
              r -> {
                System.out.println("\nlog: " + r.getLog());
                System.out.println("\ncode: " + r.getCode());
                System.out.println("\nmsg: " + r.getMsg());
              }
            );

            ObjectMapper mapper = new ObjectMapper();
            ObjectNode raw = mapper.createObjectNode();
            raw.put("electionEpoch", 0);
            raw.put("nextElectionBlock", 0);
            raw.put("validatorCount", 1);
            String VRFParams = mapper.writer().writeValueAsString(raw); 
            /* "{
            	 "electionEpoch": 0,
            	 "nextElectionBlock": 0,
            	 "validatorCount": 0
            	}"
            */
            
            //调用系统合约 ParamManagement 中的 setVRFParams() 方法
            manager.setVRFParams(VRFParams).send();
            
            //验证结果
            System.out.println(manager.getVRFParams().send());
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

## 3. 节点

```{note}
限制：
* 节点名称: 全网唯一，不能重复，长度不大于128，由字母、数字、下线组成（也支持中文字符，但不建议使用中文字符作为用户名）
* 设置类方法，只有NodeAdmin和ChainAdmin可以调用
```

### 数据结构说明

```C++
struct NodeInfo
{
    string name;       //[必填] 全网唯一，不能重复。所有接口均以此为主键。
    uint32 status;        //[必填] 1:正常；2：删除
    string publicKey;  //[必填] 节点公钥，全网唯一，不能重复
    uint32 p2pPort;       //[必填] p2p 通讯端口
  
    address owner;     //[可选] 申请者的地址
    string desc;       //[可选] 【注意：最大长度100个字符, 任意字符串】节点描述
    uint32 type;          //[必填] 1:共识节点; 2:观察者节点；3：轻节点
    string externalIP; //[必填] 外网 IP
    string internalIP; //[必填] 内网 IP
    uint32 rpcPort;       //[可选] rpc 通讯端口
    uint64 delayNum;      //[可选] 共识节点延迟设置的区块高度 (可选, 默认实时设置)
}
```

### 3.1 add

+ 接口名称： add(String nodeInfo)
+ 入参： String NodeInfo数据结构的JSON格式
+ 返回值：TransactionReceipt
+ 说明：添加节点
+ 事件：
  + Topic 1: 0x8cd284134f0437457b5542cb3a7da283d0c38208c497c5b4b005df47719f98a1
    + topic 1 为Hash.sha3String(NodeManagement.NotifyEventKey)的计算结果
    + 参数：
      + 结果码（int类型）
        + 0:  成功
        + 1:  没有权限
        + 2:  rlp编码错误
      + 结果消息（string类型）
  + Topic 2: 0x4f5a8bb8492337e79bdc674d6f31ac448f8017e26cc7bfe3144fb5d886fe5369
    + topic 2 为Hash.sha3String(NodeManagement.FUNC_ADD)的计算结果，执行失败会有此事件发生
    + 参数：
      + 结果码（int类型）
        + 0:  成功
        + 1:  失败
      + 结果消息（string类型）

调用示例：

```java
public class Demo {
    public class Config { /*...*/ }
    
    public static void main(String[] args) {
        //建立连接
        Web3j web3j = Web3j.build(new HttpService(Config.URL));

        try {
            //加载钱包
            Credentials credentials = WalletUtils.loadCredentials(Config.PASSPHRASE, Config.KEYFILE);

            //加载合约
            NodeManagement manager = NodeManagement.load(web3j, credentials, new DefaultWasmGasProvider());
          
          	//订阅事件
            EthFilter filter = new EthFilter(DefaultBlockParameterName.LATEST, DefaultBlockParameterName.PENDING, NodeManagement.CONTRACT_ADDRESS)
                    .addOptionalTopics(Hash.sha3String(NodeManagement.NotifyEventKey), Hash.sha3String(NodeManagement.FUNC_ADD));

          	manager.addObservable(filter).subscribe(
                    r -> {
                        System.out.println("\nlog: " + r.getLog());
                        System.out.println("\ncode: " + r.getCode());
                        System.out.println("\nmsg: " + r.getMsg());
                    }
            );

            //生成node数据
            ObjectMapper mapper = new ObjectMapper();
            ObjectNode raw = mapper.createObjectNode();
            raw.put("name", "wxbc0");
            raw.put("status", 1);
            raw.put("internalIP", "127.0.0.1");
            raw.put("externalIP", "172.182.0.123");
            raw.put("publicKey", "8d77134f633639cb1d224d96b3f01c79bd946c4d401da67c03ade9e4b05e5996ff855dda88838878eef00e318b6e072be4b5059d29d7e1739f17f531e81e07d3");
            raw.put("p2pPort", 8888);
            String nodeInfo = mapper.writer().writeValueAsString(raw);
            
            //调用系统合约 NodeManagement 中的 add() 方法
            manager.add(nodeInfo).send();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

### 3.2 update

+ 接口名称： update(String nodeInfo)
+ 入参：
  + String 节点名字
  + String 更新内容（JSON）
+ 返回值：TransactionReceipt
+ 说明：更新节点
+ 事件：
  + Topic 1: 0x8cd284134f0437457b5542cb3a7da283d0c38208c497c5b4b005df47719f98a1
    + topic 1 为Hash.sha3String(NodeManagement.NotifyEventKey)的计算结果
    + 参数：
      + 结果码（int类型）
        + 0:  成功
        + 1:  没有权限
        + 2:  rlp编码错误
      + 结果消息（string类型）
  + Topic 2: 0x5ef8d21b3c3919d0cb2b4728880495e379f8c1817d7867ff6b1360f2321f9598
    + topic 2 为Hash.sha3String(NodeManagement.FUNC_UPDATE)的计算结果，执行失败会有此事件发生
    + 参数：
      + 结果码（int类型）
        + 0:  成功
        + 1:  失败
      + 结果消息（string类型）

调用示例：

```java
public class Demo {
    public class Config { /*...*/ }
    
    public static void main(String[] args) {
        //建立连接
        Web3j web3j = Web3j.build(new HttpService(Config.URL));

        try {
            //加载钱包
            Credentials credentials = WalletUtils.loadCredentials(Config.PASSPHRASE, Config.KEYFILE);

            //加载合约
            NodeManagement manager = NodeManagement.load(web3j, credentials, new DefaultWasmGasProvider());

          	//订阅事件
            EthFilter filter = new EthFilter(DefaultBlockParameterName.LATEST, DefaultBlockParameterName.PENDING, NodeManagement.CONTRACT_ADDRESS)
                    .addOptionalTopics(Hash.sha3String(NodeManagement.NotifyEventKey), Hash.sha3String(NodeManagement.FUNC_UPDATE));

          	manager.updateObservable(filter).subscribe(
                    r -> {
                        System.out.println("\nlog: " + r.getLog());
                        System.out.println("\ncode: " + r.getCode());
                        System.out.println("\nmsg: " + r.getMsg());
                    }
            );
            
            String nodeName = "wxbc0";
            ObjectMapper mapper = new ObjectMapper();
            ObjectNode raw = mapper.createObjectNode();
            raw.put("desc", "This is the first node.");
            String nodeInfoUpdate = mapper.writer().writeValueAsString(raw);
            
            //调用系统合约 NodeManagement 中的 update() 方法
            manager.update(nodeName, nodeInfoUpdate).send();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

### 3.3 getAllNodes

+ 接口名称： getAllNodes()
+ 入参：空
+ 返回值：String
+ 说明：获取所有节点信息

调用示例：

```java
public class Demo {
    public class Config { /*...*/ }
    
    public static void main(String[] args) {
        //建立连接
        Web3j web3j = Web3j.build(new HttpService(Config.URL));

        try {
            //加载钱包
            Credentials credentials = WalletUtils.loadCredentials(Config.PASSPHRASE, Config.KEYFILE);

            //加载合约
            NodeManagement manager = NodeManagement.load(web3j, credentials, new DefaultWasmGasProvider());
            
            //调用系统合约 NodeManagement 中的 getAllNodes() 方法
            System.out.println(manager.getAllNodes().send());
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

### 3.4 validJoinNode

+ 接口名称： validJoinNode()
+ 入参：String 节点公钥
+ 返回值：Boolean
+ 说明：验证公钥是否已经存在

调用示例：

```java
public class Demo {
    public class Config { /*...*/ }
    
    public static void main(String[] args) {
        //建立连接
        Web3j web3j = Web3j.build(new HttpService(Config.URL));

        try {
            //加载钱包
            Credentials credentials = WalletUtils.loadCredentials(Config.PASSPHRASE, Config.KEYFILE);

            //加载合约
            NodeManagement manager = NodeManagement.load(web3j, credentials, new DefaultWasmGasProvider());
            
            String pubKey = "0xansdakl541212afaf...";
            //调用系统合约 NodeManagement 中的 validJoinNode() 方法
            System.out.println(manager.validJoinNode(pubKey).send());
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

### 3.5 getNodes

+ 接口名称： getNodes(String filter)
+ 入参：String NodeInfo数据结构的json格式
+ 返回值：String NodeInfo数据结构字段的查询交集
+ 说明：查询节点

调用示例：

```java
public class Demo {
    public class Config { /*...*/ }
    
    public static void main(String[] args) {
        //建立连接
        Web3j web3j = Web3j.build(new HttpService(Config.URL));

        try {
            //加载钱包
            Credentials credentials = WalletUtils.loadCredentials(Config.PASSPHRASE, Config.KEYFILE);

            //加载合约
            NodeManagement manager = NodeManagement.load(web3j, credentials, new DefaultWasmGasProvider());
            
            String nodeFilter = "{\"status\":2}";
            //调用系统合约 NodeManagement 中的 getNodes() 方法
            System.out.println(manager.getNodes(nodeFilter).send());
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

### 3.6 getDeletedEnodeNodes

+ 接口名称： getDeletedEnodeNodes()
+ 入参：空
+ 返回值：String
+ 说明：获取所有删除节点的enode

调用示例：

```java
public class Demo {
    public class Config { /*...*/ }
    
    public static void main(String[] args) {
        //建立连接
        Web3j web3j = Web3j.build(new HttpService(Config.URL));

        try {
            //加载钱包
            Credentials credentials = WalletUtils.loadCredentials(Config.PASSPHRASE, Config.KEYFILE);

            //加载合约
            NodeManagement manager = NodeManagement.load(web3j, credentials, new DefaultWasmGasProvider());
            
            String nodeFilter = "{\"status\":2}";
            //调用系统合约 NodeManagement 中的 getDeletedEnodeNodes() 方法
            System.out.println(manager.getDeletedEnodeNodes().send());
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

### 3.7 getNormalEnodeNodes

+ 接口名称： getNormalEnodeNodes()
+ 入参：空
+ 返回值：String
+ 说明：获取所有正常节点的enode

调用示例：

```java
public class Demo {
    // 参考 getDeletedEnodeNodes
}
```

### 3.8 nodesNum

+ 接口名称： nodesNum(String filter)
+ 入参：String NodeInfo数据结构的json格式
+ 返回值：BigInteger NodeInfo数据结构字段的查询交集的节点数量
+ 说明：获取符合条件的节点数量

调用示例：

```java
public class Demo {
    // 参考 getNodes
}
```

### 3.9 importOldNodesData

+ 接口名称： importOldNodesData(String data)
+ 入参：String 旧节点信息
+ 返回值：TransactionReceipt
+ 说明：导入原始节点管理合约中节点信息
+ 事件：
  + Topic 1: 0x8cd284134f0437457b5542cb3a7da283d0c38208c497c5b4b005df47719f98a1
    + topic 1 为Hash.sha3String(NodeManagement.NotifyEventKey)的计算结果
    + 参数：
      + 结果码（int类型）
        + 0:  成功
        + 1:  数据反序列化错误
      + 结果消息（string类型）
  + Topic 2: 0x21975ecc2e5d872379f39f70cf47dd10cc3c49da967603a799e2ad7019ebf125
    + topic 2 为Hash.sha3String(NodeManagement.FUNC_IMPORTOLDNODESDATA)的计算结果，执行失败会有此事件发生
    + 参数：
      + 结果码（int类型）
        + 0:  成功
        + 1:  失败
      + 结果消息（string类型）

调用示例：

```java
public class Demo {
    public class Demo {
    public class Config { /*...*/ }
    
    public static void main(String[] args) {
        //建立连接
        Web3j web3j = Web3j.build(new HttpService(Config.URL));

        try {
            //加载钱包
            Credentials credentials = WalletUtils.loadCredentials(Config.PASSPHRASE, Config.KEYFILE);

            //加载合约
            NodeManagement manager = NodeManagement.load(web3j, credentials, new DefaultWasmGasProvider());
          
          	//订阅事件
            EthFilter filter = new EthFilter(DefaultBlockParameterName.LATEST, DefaultBlockParameterName.PENDING, NodeManagement.CONTRACT_ADDRESS)
                    .addOptionalTopics(Hash.sha3String(NodeManagement.NotifyEventKey), Hash.sha3String(NodeManagement.FUNC_IMPORTOLDNODESDATA));

          	manager.importOldNodesDataObservable(filter).subscribe(
                    r -> {
                        System.out.println("\nlog: " + r.getLog());
                        System.out.println("\ncode: " + r.getCode());
                        System.out.println("\nmsg: " + r.getMsg());
                    }
            );

            ObjectMapper mapper = new ObjectMapper();
            ObjectNode raw = mapper.createObjectNode();
            raw.put("name", "wxbc0");
            raw.put("owner", Config.SUPERUSERADDRESS);
            raw.put("status", 1);
            raw.put("internalIP", "127.0.0.1");
            raw.put("externalIP", "172.182.0.123");
            raw.put("publicKey", "8d77134f633639cb1d224d96b3f01c79bd946c4d401da67c03ade9e4b05e5996ff855dda88838878eef00e318b6e072be4b5059d29d7e1739f17f531e81e07d3");
            raw.put("p2pPort", 8888);
            raw.put("type", 2);
            
          	String nodeInfo = mapper.writer().writeValueAsString(raw);
            System.out.println(nodeInfo);

            //调用系统合约 NodeManagement 中的 importOldNodesData() 方法
            TransactionReceipt ret = manager.importOldNodesData("[" + nodeInfo + "]").send();
            System.out.println(ret.getStatus());
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
}
```

## 4. CNS

### 数据结构说明

CNS主要数据结构如下：

```go
type ContractInfo struct
{
    Name    string          // 【必填】合约名称（数字下划线英文字符，长度限制：1-128位）
    Version string          // 【必填】合约版本号【注：注册时版本号需要递增添加】
    Address common.Address  // 【必填】合约地址
    Origin  common.Address  // 【非用户填写】合约部署者地址
    TimeStamp   uint64      // 【非用户填写】时间戳
}
```

```{note}
限制：

* 用户名长度不大于128，由字母、数字、下线组成（也支持中文字符，但不建议使用中文字符作为用户名）
* 版本号格式为 xx.xx.xx.xx，每个字段均为数字，每个字段数字最大为3位
```

### 权限说明

* 写操作cnsXXX: 仅合约部署者对该合约地址有操作权限
* 读操作getXXX or ifRegisteredXXX: 任意用户都有查询权限
* 导入操作importXXX: 暂未做限制


### 4.1 cnsRegister

+ 接口名称： cnsRegister(String contractName, String contractVersion, String contractAddress)
+ 入参：
  + String contractName 合约名
  + String contractVersion 合约版本
  + String contractAddress 合约地址
+ 返回值：TransactionReceipt
+ 说明：合约注册（非初始化） 
+ 事件：
  + Topic 1: 0x3e8c576b2f109e49f59952e75c0a038c00d1e57c324f7923b7911ec70f16f558
    + topic 1 为Hash.sha3String(CnsManagement.CNSNotifyEventKey)的计算结果
    + 参数：
      + 结果码（int类型）
        + 0:  成功
        + 1： 无效调用 
        + 2： 权限错误 
        + 3： 无效入参 
        + 4： 注册失败 
      + 结果消息（string类型）
  + Topic 2: 0x8cd284134f0437457b5542cb3a7da283d0c38208c497c5b4b005df47719f98a1
    + topic 2 为Hash.sha3String(CnsManagement.NotifyEventKey)的计算结果。因无法从交易结构体中获取函数名而失败时会有此事件发生。
    + 参数：
      + 结果码（int类型）
        + 0:  成功
        + 1:  失败
      + 结果消息（string类型）
  + Topic 3: 0x3e8c576b2f109e49f59952e75c0a038c00d1e57c324f7923b7911ec70f16f558
    + topic 2 为Hash.sha3String(CnsManagement.FUNC_CNSREGISTER)的计算结果。成功从交易结构体中获取函数名，但因其他原有交易失败时会有此事件发生。
    + 参数：
      + 结果码（int类型）
        + 0:  成功
        + 1:  失败
      + 结果消息（string类型）

调用示例：

```java
public class Demo {
    public class Config { /*...*/ }
    
    public static void main(String[] args) {
        //建立连接
        Web3j web3j = Web3j.build(new HttpService(Config.URL));

        try {
            //加载钱包
            Credentials credentials = WalletUtils.loadCredentials(Config.PASSPHRASE, Config.KEYFILE);

            //将合约文件转换为二进制文件
            byte[] dataBytes = Files.readBytes(new File(Config.CONTRACTBINARY));
            String binData = Hex.toHexString(dataBytes);
            
            //部署合约，成功部署后，合约地址会打印在log中
            MyContract.deploy(web3j, cred, binData, new DefaultWasmGasProvider()).send();
            
            //加载已部署合约
            MyContract.load("", "0xce4efe084fd855c0c5d096f8478595ff35ec7ff8", web3j, cred, new DefaultWasmGasProvider());
            
            //加载系统合约
            CnsManagement manager = CnsManagement.load(web3j, credentials, new DefaultWasmGasProvider());
          
          	//订阅事件
			EthFilter filter = new EthFilter(DefaultBlockParameterName.EARLIEST, DefaultBlockParameterName.LATEST, CnsManagement.CONTRACT_ADDRESS)
                    .addOptionalTopics(
                            Hash.sha3String(CnsManagement.NotifyEventKey),
                            Hash.sha3String(CnsManagement.FUNC_CNSREGISTER),
                            Hash.sha3String(CnsManagement.CNSNotifyEventKey));
           	manager.cnsRegisterObservable(filter).subscribe(
                    r -> {
                        System.out.println("\nlog: " + r.getLog());
                        System.out.println("\ncode: " + r.getCode());
                        System.out.println("\nmsg: " + r.getMsg());
                    }
            );

            //调用系统合约 CnsManagement 中的 cnsRegister() 方法
            //注册合约到CNS系统中
            manager.cnsRegister("contractName", "0.0.0.1", "0xce4efe084fd855c0c5d096f8478595ff35ec7ff8").send();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```


### 4.2 cnsRedirect

+ 接口名称： cnsRedirect(String contractName, String contractVersion)
+ 入参：
  + String contractName 合约名
  + String contractVersion 合约版本
+ 返回值：TransactionReceipt
+ 说明：重新指定cns合约名对应的合约地址（该合约地址以及对应版本需在cns中已注册）
+ 事件：
  + Topic 1: 0x3e8c576b2f109e49f59952e75c0a038c00d1e57c324f7923b7911ec70f16f558
    + topic 1 为Hash.sha3String(CnsManagement.CNSNotifyEventKey)的计算结果
    + 参数：
      + 结果码（int类型）
        + 0:  成功
        + 1： 无效调用 
        + 2： 权限错误 
        + 3： 无效入参 
        + 4： 注册失败 
      + 结果消息（string类型）
  + Topic 2: 0x8cd284134f0437457b5542cb3a7da283d0c38208c497c5b4b005df47719f98a1
    + topic 2 为Hash.sha3String(CnsManagement.NotifyEventKey)的计算结果。因无法从交易结构体中获取函数名而失败时会有此事件发生。
    + 参数：
      + 结果码（int类型）
        + 0:  成功
        + 1:  失败
      + 结果消息（string类型）
  + Topic 3: 0x6e4176a92392f1e85402ff3f952e8d0eb504eb0dc50256bdf9cc0d2b586dd5cc
    + topic 2 为Hash.sha3String(CnsManagement.FUNC_CNSREDIRECT)的计算结果。成功从交易结构体中获取函数名，但因其他原有交易失败时会有此事件发生。
    + 参数：
      + 结果码（int类型）
        + 0:  成功
        + 1:  失败
      + 结果消息（string类型）

调用示例：

```java
public class Demo {
    public class Config { /*...*/ }
    
    public static void main(String[] args) {
        //建立连接
        Web3j web3j = Web3j.build(new HttpService(Config.URL));

        try {
            //加载钱包
            Credentials credentials = WalletUtils.loadCredentials(Config.PASSPHRASE, Config.KEYFILE);
            
            //部署合约，成功部署后，合约地址会打印在log中
            MyContract.deploy(web3j, cred, binData, new DefaultWasmGasProvider()).send();

            //加载已部署合约
            MyContract.load("", "0xse5235sdgfsaf78595ff35ec7ff8", web3j, cred, new DefaultWasmGasProvider());

            //加载系统合约
            CnsManagement manager = CnsManagement.load(web3j, credentials, new DefaultWasmGasProvider());
          
          	//订阅事件
    		EthFilter filter = new EthFilter(DefaultBlockParameterName.EARLIEST, DefaultBlockParameterName.LATEST, CnsManagement.CONTRACT_ADDRESS)
                    .addOptionalTopics(
                            Hash.sha3String(CnsManagement.NotifyEventKey),
                            Hash.sha3String(CnsManagement.FUNC_CNSREDIRECT),
                            Hash.sha3String(CnsManagement.CNSNotifyEventKey));

           	manager.cnsRedirectObservable(filter).subscribe(
                    r -> {
                        System.out.println("\nlog: " + r.getLog());
                        System.out.println("\ncode: " + r.getCode());
                        System.out.println("\nmsg: " + r.getMsg());
                    }
            );

            //调用系统合约 CnsManagement 中的 cnsRegister() 方法
            //注册合约到CNS系统中
            manager.cnsRegister("contractName", "0.0.0.2", "0xse5235sdgfsaf78595ff35ec7ff8").send();
            
            //调用系统合约 CnsManagement 中的 cnsRedirect() 方法
            manager.cnsRedirect("contractName", "0.0.0.2").send();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```


### 4.3 getContractAddress

+ 接口名称： getContractAddress(String contractName, String contractVersion)
+ 入参：
  + String contractName合约名称（类型：string）
  + String contractVersion 版本号，格式：x.x.x.x 或 "latest"
+ 返回值：String 合约地址，无则为空（null） 
+ 说明：合约名称地址查询

调用示例：

```java
public class Demo {
    public class Config { /*...*/ }
    
    public static void main(String[] args) {
        //建立连接
        Web3j web3j = Web3j.build(new HttpService(Config.URL));

        try {
            //加载钱包
            Credentials credentials = WalletUtils.loadCredentials(Config.PASSPHRASE, Config.KEYFILE);

            //加载系统合约
            CnsManagement manager = CnsManagement.load(web3j, credentials, new DefaultWasmGasProvider());

            //调用系统合约 CnsManagement 中的 getContractAddress() 方法
            Stirng address = manager.getContractAddress("contractName", "latest").send();
          	if (address != null) {
              System.out.println(address);
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

### 4.4 ifRegisteredByName

+ 接口名称： ifRegisteredByName(String contractName)
+ 入参：
  + String contractName 合约名称
+ 返回值：BigInteger 
  + 未注册：0
  + 已注册：1
  + 无效入参：3

+ 说明：通过合约名查询是否注册

调用示例：

```java
public class Demo {
    public class Config { /*...*/ }
    
    public static void main(String[] args) {
        //建立连接
        Web3j web3j = Web3j.build(new HttpService(Config.URL));

        try {
            //加载钱包
            Credentials credentials = WalletUtils.loadCredentials(Config.PASSPHRASE, Config.KEYFILE);

            //加载系统合约
            CnsManagement manager = CnsManagement.load(web3j, credentials, new DefaultWasmGasProvider());

            //调用系统合约 CnsManagement 中的 ifRegisteredByName() 方法
            System.out.println(manager.ifRegisteredByName("contractName").send());
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

### 4.5 ifRegisteredByAddress

+ 接口名称： ifRegisteredByAddress(String contractAddress)
+ 入参：
  + String contractAddress 合约地址
+ 返回值：BigInteger 
  + 未注册：0
  + 已注册：1
  + 无效入参：3
+ 说明：通过合约地址查询是否注册

调用示例：

```java
public class Demo {
    // 参考 ifRegisteredByName()   
}
```

### 4.6 getRegisteredContracts

+ 接口名称： getRegisteredContracts(int startIndex, int endIndex)
+ 入参：
  + int startIndex 起始下标（从0开始）
  + int endIndex 选择范围，当值为0时代表从起始下标到尾标
+ 返回值：String
+ 说明：通过范围获取已注册合约信息

调用示例：

```java
public class Demo {
    public class Config { /*...*/ }
    
    public static void main(String[] args) {
        //建立连接
        Web3j web3j = Web3j.build(new HttpService(Config.URL));

        try {
            //加载钱包
            Credentials credentials = WalletUtils.loadCredentials(Config.PASSPHRASE, Config.KEYFILE);

            //加载系统合约
            CnsManagement manager = CnsManagement.load(web3j, credentials, new DefaultWasmGasProvider());

            //调用系统合约 CnsManagement 中的 getRegisteredContracts() 方法
            //注册合约到CNS系统中
            System.out.println(manager.getRegisteredContracts(0, 2).send());
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

### 4.7 getRegisteredContractsByName

+ 接口名称： getRegisteredContractsByName(String contractName)
+ 入参：
  + String contractName 合约名
+ 返回值：String
+ 说明：通过合约名称获取已注册合约信息

调用示例：

```java
public class Demo {
    public class Config { /*...*/ }
    
    public static void main(String[] args) {
        //建立连接
        Web3j web3j = Web3j.build(new HttpService(Config.URL));

        try {
            //加载钱包
            Credentials credentials = WalletUtils.loadCredentials(Config.PASSPHRASE, Config.KEYFILE);

            //加载系统合约
            CnsManagement manager = CnsManagement.load(web3j, credentials, new DefaultWasmGasProvider());

            //调用系统合约 CnsManagement 中的 getRegisteredContractsByName() 方法
            //注册合约到CNS系统中
            System.out.println(manager.getRegisteredContractsByName("contratName").send());
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

### 4.8 getRegisteredContractsByAddress

+ 接口名称： getRegisteredContractsByAddress(String contractAddress)
+ 入参：
  + String contractAddress 合约地址
+ 返回值：String
+ 说明：通过合约地址获取已注册合约信息

调用示例：

```java
public class Demo {
    // 参考 getRegisteredContractsByName()   
}
```

### 4.9 getRegisteredContractsByOrigin

+ 接口名称： getRegisteredContractsByOrigin(String origin)
+ 入参：
  + String origin 合约注册人地址
+ 返回值：String
+ 说明：通过注册人地址获取已注册合约信息

调用示例：

```java
public class Demo {
    // 参考 getRegisteredContractsByName()   
}
```

## 5. 合约防火墙

### 数据结构说明

防火墙主要数据结构如下：

```go
type FwStatus struct {
    ContractAddress common.Address  // 合约地址
    FwActive        bool            // 防火墙开启/关闭状态
    AcceptedList    []FwElem        // 防火墙白名单
    RejectedList    []FwElem        // 防火墙黑名单
}

type FwElem struct {
    Addr        common.Address      // 地址
    FuncName    string              // 合约方法名称
}
```

```{note}
限制：

* 设置类方法只有部署合约的人才可以操作
```

### 权限说明

* 查询接口__sys_FwStatus: 任何用户均有查询权限
* 非查询接口：仅合约部署者对该合约地址的防火墙有操作权限
* 导入导出接口：暂未做限制

### 5.1 sysFwOpen

+ 接口名称： sysFwOpen(String contractAddress)
+ 入参：
  + String contractAddress 合约地址
+ 返回值：TransactionReceipt
+ 说明：开启防火墙 
+ 事件：
  + Topic 1: 0x8cd284134f0437457b5542cb3a7da283d0c38208c497c5b4b005df47719f98a1
    + topic 1 为Hash.sha3String(FwManagement.NotifyEventKey)的计算结果
    + 参数：
      + 结果码（int类型）
        + 0:  成功
        + 1:  权限错误
        + 2:  无效入参
      + 结果消息（string类型）
  + Topic 2: 0x6619b1053f4847152c7e5c33165a19b9c6116674010cb4b50c0b402834546f4b
    + topic 2 为Hash.sha3String(FwManagement.FUNC_SYSFWOPEN)的计算结果
    + 参数：
      + 结果码（int类型）
        + 0:  成功
        + 1:  失败
      + 结果消息（string类型）

调用示例：

```java
public class Demo {
    public class Config { /*...*/ }
    
    public static void main(String[] args) {
        //建立连接
        Web3j web3j = Web3j.build(new HttpService(Config.URL));

        try {
            //加载钱包
            Credentials credentials = WalletUtils.loadCredentials(Config.PASSPHRASE, Config.KEYFILE);
      
            //加载系统合约
            FwManagement manager = FwManagement.load(web3j, credentials, new DefaultWasmGasProvider());
          
          	//订阅事件
    		 EthFilter filter = new EthFilter(DefaultBlockParameterName.LATEST, DefaultBlockParameterName.PENDING, FwManagement.CONTRACT_ADDRESS)
                    .addOptionalTopics(Hash.sha3String(FwManagement.NotifyEventKey), Hash.sha3String(FwManagement.FUNC_SYSFWOPEN));

           	manager.sysFwOpenObservable(filter).subscribe(
                    r -> {
                        System.out.println("\nlog: " + r.getLog());
                        System.out.println("\ncode: " + r.getCode());
                        System.out.println("\nmsg: " + r.getMsg());
                    }
            );

            //调用系统合约 FwManagement 中的 sysFwOpen() 方法
            manager.sysFwOpen("0xasdhaslf353253...").send();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

### 5.2 sysFwClose

+ 接口名称： sysFwClose(String contractAddress)
+ 入参：
  + String contractAddress 合约地址
+ 返回值：TransactionReceipt
+ 说明：关闭防火墙
+ 事件：
  + Topic 1: 0x8cd284134f0437457b5542cb3a7da283d0c38208c497c5b4b005df47719f98a1
    + topic 1 为Hash.sha3String(FwManagement.NotifyEventKey)的计算结果
    + 参数：
      + 结果码（int类型）
        + 0:  成功
        + 1:  权限错误
        + 2:  无效入参
      + 结果消息（string类型）
  + Topic 2: 0x8579b36523a7fa1486ded44370a790e016548500d00d82cbea1596a7dc576665
    + topic 2 为Hash.sha3String(FwManagement.FUNC_SYSFWCLOSE)的计算结果
    + 参数：
      + 结果码（int类型）
        + 0:  成功
        + 1:  失败
      + 结果消息（string类型）

调用示例：

```java
public class Demo {
    // 参考 sys_FwOpen()   
}
```

### 5.3 sysFwAdd

+ 接口名称： sysFwAdd(String contractName, String rule, String funcNames)
+ 入参：
  + String contractName 合约地址
  + String 防火墙黑(REJECT)/白(ACCEPT)名单
  + String 防火墙规则，格式：单一规则 <地址>:<合约方法>；多规则 <地址1>:<合约方法1>|<地址2>:<合约方法2>
+ 返回值：TransactionReceipt
+ 说明：指定防火墙黑白名单
+ 事件：
  + Topic 1: 0x8cd284134f0437457b5542cb3a7da283d0c38208c497c5b4b005df47719f98a1
    + topic 1 为Hash.sha3String(FwManagement.NotifyEventKey)的计算结果
    + 参数：
      + 结果码（int类型）
        + 0:  成功
        + 1:  权限错误
        + 2:  无效入数
      + 结果消息（string类型）
  + Topic 2: 0x83aa60d4e3fac26899883e984b27d75df175ba9b232677b144a91780fcc2dd38
    + topic 2 为Hash.sha3String(FwManagement.FUNC_SYSFWADD)的计算结果
    + 参数：
      + 结果码（int类型）
        + 0:  成功
        + 1:  失败
      + 结果消息（string类型）


调用示例：

```java
public class Demo {
    public class Config { /*...*/ }
    
    public static void main(String[] args) {
        //建立连接
        Web3j web3j = Web3j.build(new HttpService(Config.URL));

        try {
            //加载钱包
            Credentials credentials = WalletUtils.loadCredentials(Config.PASSPHRASE, Config.KEYFILE);
      
            //加载系统合约
            FwManagement manager = FwManagement.load(web3j, credentials, new DefaultWasmGasProvider());
          
          	//订阅事件
            EthFilter filter = new EthFilter(DefaultBlockParameterName.EARLIEST, DefaultBlockParameterName.LATEST, FwManagement.CONTRACT_ADDRESS)
                    .addOptionalTopics(Hash.sha3String(FwManagement.NotifyEventKey), Hash.sha3String(FwManagement.FUNC_SYSFWADD));
           	manager.sysFwAddObservable(filter).subscribe(
                    r -> {
                        System.out.println("\nlog: " + r.getLog());
                        System.out.println("\ncode: " + r.getCode());
                        System.out.println("\nmsg: " + r.getMsg());
                    }
            );

            //调用系统合约 FwManagement 中的 sysFwAdd() 方法
            manager.sysFwAdd("contractName", "ACCEPT", "0x33:*|*:funcName2|0x55:funcName3").send();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

### 5.4 sysFwClear

+ 接口名称： sysFwClear(String contractName, String rule)
+ 入参：
  + String contractName 合约地址
  + String 防火墙黑(REJECT)/白(ACCEPT)名单
+ 返回值：TransactionReceipt
+ 说明：清空防火墙黑白名单
+ 事件：
  + Topic 1: 0x8cd284134f0437457b5542cb3a7da283d0c38208c497c5b4b005df47719f98a1
    + topic 1 为Hash.sha3String(FwManagement.NotifyEventKey)的计算结果
    + 参数：
      + 结果码（int类型）
        + 0:  成功
        + 1:  权限错误
        + 2:  无效入数
      + 结果消息（string类型）
  + Topic 2: 0x4be88811f3b33d3da31d8208ddbb87371e77f042416d54099e311b18d347658f
    + topic 2 为Hash.sha3String(FwManagement.FUNC_SYSFWCLEAR)的计算结果
    + 参数：
      + 结果码（int类型）
        + 0:  成功
        + 1:  失败
      + 结果消息（string类型）

调用示例：

```java
public class Demo {
    // 参考 sysFwAdd()   
}
```

### 5.5 sysFwDel

+ 接口名称： sysFwDel(String contractName, String rule, String funcNames)
+ 入参：
  + String contractName 合约地址
  + String 防火墙黑(REJECT)/白(ACCEPT)名单
  + String 防火墙规则，格式：单一规则 <地址>:<合约方法>；多规则 <地址1>:<合约方法1>|<地址2>:<合约方法2>
+ 返回值：TransactionReceipt
+ 说明：删除防火墙黑白名单
+ 事件：
  + Topic 1: 0x8cd284134f0437457b5542cb3a7da283d0c38208c497c5b4b005df47719f98a1
    + topic 1 为Hash.sha3String(FwManagement.NotifyEventKey)的计算结果
    + 参数：
      + 结果码（int类型）
        + 0:  成功
        + 1:  权限错误
        + 2:  无效入数
      + 结果消息（string类型）
  + Topic 2: 0xe68ff3a3fa0884b33036c599a770e875c81a28a36457ac3c8eb9dd5ca2a2151f
    + topic 2 为Hash.sha3String(FwManagement.FUNC_SYSFWDEL)的计算结果
    + 参数：
      + 结果码（int类型）
        + 0:  成功
        + 1:  失败
      + 结果消息（string类型）

调用示例：

```java
public class Demo {
    // 参考 sysFwAdd()   
}
```

### 5.6 sysFwSet

+ 接口名称： sysFwSet(String contractName, String rule, String funcNames)
+ 入参：
  + String contractName 合约地址
  + String 防火墙黑(REJECT)/白(ACCEPT)名单
  + String 防火墙规则，格式：单一规则 <地址>:<合约方法>；多规则 <地址1>:<合约方法1>|<地址2>:<合约方法2>
+ 返回值：TransactionReceipt
+ 说明：重置防火墙黑白名单
+ 事件：
  + Topic 1: 0x8cd284134f0437457b5542cb3a7da283d0c38208c497c5b4b005df47719f98a1
    + topic 1 为Hash.sha3String(FwManagement.NotifyEventKey)的计算结果
    + 参数：
      + 结果码（int类型）
        + 0:  成功
        + 1:  权限错误
        + 2:  无效入数
      + 结果消息（string类型）
  + Topic 2: 0x7135cdbf43c8c41c86f2665c215692acf08a6d36e07e1115be48fcb06444995b
    + topic 2 为Hash.sha3String(FwManagement.FUNC_SYSFWSET)的计算结果
    + 参数：
      + 结果码（int类型）
        + 0:  成功
        + 1:  失败
      + 结果消息（string类型）

调用示例：

```java
public class Demo {
    // 参考 sysFwAdd()   
}
```

### 5.7 sysFwStatus

+ 接口名称： sysFwStatus(String contractAddress)
+ 入参：
  + String contractAddress 合约地址
+ 返回值：String
+ 说明：查询防火墙状态

调用示例：

```java
public class Demo {
    public class Config { /*...*/ }
    
    public static void main(String[] args) {
        //建立连接
        Web3j web3j = Web3j.build(new HttpService(Config.URL));

        try {
            //加载钱包
            Credentials credentials = WalletUtils.loadCredentials(Config.PASSPHRASE, Config.KEYFILE);
      
            //加载系统合约
            FwManagement manager = FwManagement.load(web3j, credentials, new DefaultWasmGasProvider());

            //调用系统合约 FwManagement 中的 sys_FwStatus() 方法
            manager.sysFwStatus("0xasdhas4235423b5j...").send();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

### 5.8 sysFwImport

+ 接口名称： sysFwImport(String contractAddress, String config)
+ 入参：
  + String contractAddress 合约地址
  + String 防火墙规则
+ 返回值：TransactionReceipt
+ 说明：导入防火墙黑白名单
+ 事件：
  + Topic 1: 0x8cd284134f0437457b5542cb3a7da283d0c38208c497c5b4b005df47719f98a1
    + topic 1 为Hash.sha3String(FwManagement.NotifyEventKey)的计算结果
    + 参数：
      + 结果码（int类型）
        + 0:  成功
        + 1:  权限错误
        + 2:  无效入数
      + 结果消息（string类型）
  + Topic 2: 0xfb7443dc0dbc85fa5c9df6aff809506bd417b93c5abfaa28d658dc5d32916daf
    + topic 2 为Hash.sha3String(FwManagement.FUNC_SYSFWIMPORT)的计算结果
    + 参数：
      + 结果码（int类型）
        + 0:  成功
        + 1:  失败
      + 结果消息（string类型）

调用示例：

```java
public class Demo {
    public class Config { /*...*/ }
    
    public static void main(String[] args) {
        //建立连接
        Web3j web3j = Web3j.build(new HttpService(Config.URL));

        try {
            //加载钱包
            Credentials credentials = WalletUtils.loadCredentials(Config.PASSPHRASE, Config.KEYFILE);
      
            //加载系统合约
            FwManagement manager = FwManagement.load(web3j, credentials, new DefaultWasmGasProvider());
          	
          	//订阅事件
            EthFilter filter = new EthFilter(DefaultBlockParameterName.LATEST, DefaultBlockParameterName.PENDING, FwManagement.CONTRACT_ADDRESS)
                    .addOptionalTopics(Hash.sha3String(FwManagement.NotifyEventKey), Hash.sha3String(FwManagement.FUNC_SYSFWIMPORT));

           	manager.sysFwImportObservable(filter).subscribe(
                    r -> {
                        System.out.println("\nlog: " + r.getLog());
                        System.out.println("\ncode: " + r.getCode());
                        System.out.println("\nmsg: " + r.getMsg());
                    }
            );

            //调用系统合约 FwManagement 中的 sysFwImport() 方法
            manager.sysFwImport("0xasdhas4235423b5j...", "{\"AcceptedList\":[{\"Addr\":\"0x16c8a21295e68f039b8406d13ee0dc6c3a481c76\",\"FuncName\":\"function1\"}],\"RejectedList\":null}").send();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

### 5.9 sysFwExport

+ 接口名称： sysFwExport(String contractAddress)
+ 入参：
  + String contractAddress 合约地址
+ 返回值：String
+ 说明：导出防火墙黑白名单

调用示例：

```java
public class Demo {
    public class Config { /*...*/ }
    
    public static void main(String[] args) {
        //建立连接
        Web3j web3j = Web3j.build(new HttpService(Config.URL));

        try {
            //加载钱包
            Credentials credentials = WalletUtils.loadCredentials(Config.PASSPHRASE, Config.KEYFILE);
      
            //加载系统合约
            FwManagement manager = FwManagement.load(web3j, credentials, new DefaultWasmGasProvider());

            //调用系统合约 FwManagement 中的 sysFwExport() 方法
            manager.sys_FwExport("0xasdhas4235423b5j...").send();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```