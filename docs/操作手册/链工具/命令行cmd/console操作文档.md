# 多链console操作文档
在PlatONE多链架构下，节点多个链组共享节点和账户公私钥，分别维护各自的账本，groupid为0的链组被称为主链，主链上会部署链组管理系统合约，所有其他链组创建后必须在主链链组管理合约中注册，
某些链组级别的操作依赖主链，因此多链架构下必须保证主链账本正常运行。

## 0. 目录结构
多链架构下，区块链目录结构如下所示
```
.
|-linux
  |-bin ......................................可执行目录文件
  |  |-ctool
  |  |-platone
  |-conf.....................................配置文件目录
  |  |-config.json........................node0配置文件
  |  |-config_1.json....................node1配置文件（单机多节点情况下）
  |-data.....................................数据目录
  |  |-node_0.............................节点1数据目录
  |  |  |-group_0.........................链组0账本数据
  |  |  |  |-config.toml
  |  |  |  |-genesis.json
  |  |  |  |-logs
  |  |  |  |-platone
  |  |  |-group_1.........................链组1账本数据
  |  |  |  |-config.toml
  |  |  |  |-genesis.json
  |  |  |  |-logs
  |  |  |  |-platone
  |  |-node_1.............................节点1数据目录
  |  |  |-group_0
  |  |  |  |-config.toml
  |  |  |  |-genesis.json
  |  |  |  |-logs
  |  |  |  |-platone
```

## 1.  全局变量
本小节描述console启动时传递给console的全局变量。
**示例**
``` shell
./console.py --config path/to/config --bin path/to/bin --datadir path/to/datadir
```
**配置文件 config.json**
- console启动时 通过`--config`指定配置文件位置。
- 若不指定位置，默认寻找`../conf/config.json`作为配置文件，若不存在，则在默认位置按照默认配置生成config.json。

``` json
{
    //多链信息
    "groups":[
        {
        "id":"0",  // 链ID
        "p2pPort":"", // 链p2p端口，默认16790+id
        "rpcPort":"",  //链rpc端口，默认6790+id
        "wsPort":"",  //链ws端口,默认3790+id
        "dashboardPort":"", //链dashboard端口，默认1090+id 
        "bootstrapNodes":"", //链启动后主动连接的节点
        "url":"", // rpc通信地址 http://ip:rpcPort
        "status":1 // 1=默认启动此链；0=不启动
        }
    ],

    //发交易用
    "gas":"0x0",
    "gasPrice":"0x0",
    "from":"",

    // 用于存放区块数据的路径，默认../data/node_0
    "datadir":""
}
```
**可执行文件存放目录**
- console启动时，通过`--bin`指定PlatONE可执行文件存放的目录。
- 默认值`../bin`

**区块链数据存放目录**
- console启动时，通过`--datadir`指定PlatONE区块链数据存放目录。
- 默认值`../data/node_0`
  
**是否开启交互界面**
- console启动时，默认会开启一个交互式console供用户使用。
- 可以在启动console时直接输入执行命令和参数，并在末尾添加`--direct`，console会直接执行命令后退出。
``` shell
./console.py group create --groupid 1 --direct
```
## 2. 用户操作

### 2.1 链组

#### 2.1.1 单节点单链组启动
* **描述**：从头初始化并启动一个单节点，会自动生成账户、节点公私钥、genesis文件、初始化创世区块并启动节点。
* **参数**：
    ``` 
    --groupid                   指定链组id，默认0
    --chainid                    指定链组chainid，默认300 + groupid
    --ip                               指定节点ip，默认 127.0.0.1，若要与其他节点组网通信，必须指定真实的网络ip。
    --port                          此链组中节点p2p端口，默认 16790 + groupid
    --rpcport                   此链组中节点rpc端口，默认 6790 + groupid
    --wsport                    此链组中节点websocket端口，默认 3790 + groupid
    --dashport               此链组中节点dashboard端口，默认 1090 + groupid

    --password               生成账户时加密账户，此密码用于日后锁定和解锁账户，默认0
    ```
* **示例**
    ``` shell
    one --ip 10.200.26.69 --password  123456
    ```

#### 2.1.2 创建链组 group create
* **描述**：创建一个链组。
* **参数**：
    ```
    --groupi                      指定链组id，默认0
    --chainid                    指定链组chainid，默认300 + groupid
    --ip                               指定节点ip，默认 127.0.0.1，若要与其他节点组网通信，必须指定真实的网络ip。
    --port                          此链组中节点p2p端口，默认 16790 + groupid
    --rpcport                   此链组中节点rpc端口，默认 6790 + groupid
    --wsport                    此链组中节点websocket端口，默认 3790 + groupid
    --dashport                此链组中节点dashboard端口，默认 1090 + groupid

    --password              账户密码，用于解锁账户以便创建链组，默认0
    ```
* **示例**
    ``` shell
    group create --groupid 1 --password 123456  --ip 10.200.26.69 
    ```

#### 2.1.3 切换链组 switch
* **描述**：存在多个链组时，用于在不同链组间切换
* **参数**：
    ```
    链组id
    ```
* **示例**：
    ``` shell
    switch 1
    ```

#### 2.1.4 添加链组节点准入 group add
* **描述**：允许一个节点加入当前链组，执行此命令前，请先切换至目标链组。
* **参数**：
    ```
    --enode                      待添加节点的enode，格式为enode://pubkey@ip:port
    --pubkey                   待添加节点的pubkey，若指定则会覆盖enode中的pubkey
    --ip                              待添加节点的ip，若指定则会覆盖enode中的ip
    --port                         待添加节点的p2p端口，若指定则会覆盖enode中的port
    --name                      节点名称，默认pubkey的前50个字符
    --type                        节点在群组中的角色，0=观察者节点；1=共识节点，默认为0
    --desc                        节点描述信息，非必须
    --rpcport                  节点rpc端口，非必须

    --password             用于解锁账户，执行准入交易，默认"0"
    ```
* **示例**
    ``` shell
    switch 1
    group add --enode enode://1f8fa99baace67b994945279f173b285c98cccdd080376b8a08691439b78c9df9514bc3367ebd78b5b53b1b52b990585bdef36523f63c67343d33c3337205713@10.200.65.37:16791 --password 123456
    ```

#### 2.1.5 加入链组 group join
* **描述**：主动加入一个链组
* **参数**：
    ```
    --groupid                   指定链组id，默认0
    --chainid                    指定链组chainid，默认300 + groupid
    --ip                               指定节点ip，默认 127.0.0.1，用于配置rpc地址，可以不指定
    --port                          此链组中节点p2p端口，默认 16790 + groupid
    --rpcport                   此链组中节点rpc端口，默认 6790 + groupid
    --wsport                    此链组中节点websocket端口，默认 3790 + groupid
    --dashport                此链组中节点dashboard端口，默认 1090 + groupid

    --creator_enode    groupid非0时不必指定/为0时必须指定，创建者的enode，用于初始化genesis.json，格式为enode://pubkey@ip:port
    --bootNodes           groupid非0时不必指定/为0时默认等于creator_enode，节点启动时主动连接的目标节点，多个enode用逗号分割
    --password              groupid为0时，创建账户时指定密码，默认0

    ```
* **示例**
    1. 加入主链链组：
    ``` shell
    group join --creator_enode enode://1f8fa99baace67b994945279f173b285c98cccdd080376b8a08691439b78c9df9514bc3367ebd78b5b53b1b52b990585bdef36523f63c67343d33c3337205713@10.200.65.37:16791 --password 123456
    ```
    1. 加入主链后，加入链组1：
    ``` shell
    group join --groupid 1
    ```

#### 2.1.6 离开链组 group leave
* **描述**：主动离开一个链组，无法离开链组0，此命令不会删除已同步账本。
* **参数**：
    ```
    --groupid                   指定链组id，默认0
    ```
* **示例**
    ``` shell
    group leave --groupid 1
    ```

### 2.2 其他命令
#### 2.2.1 启动 start
* **描述**：启动一个或多个链组，若指定groupid，则启动单个链组，否则启动所有链组
* **参数**：
    ```
   链组id
    ```
* **示例**
    ``` shell
    start  1
    ```

#### 2.2.2 停止 stop
* **描述**：停止一个或多个链组，若指定groupid，则停止单个链组，否则停止所有链组
* **参数**：
    ```
   链组id
    ```
* **示例**
    ``` shell
    stop  1
    ```

#### 2.2.3 创建账户 createacc
* **描述**：创建账户，所有链组共享账户
* **参数**：
    ```
    --password               指定密码，默认为0
    ```
* **示例**
    ``` shell
    createacc  --password 123456
    ```

#### 2.2.4 解锁账户 unlock
* **描述**：为当前链组解锁账户
* **参数**：
    ```
    --account                  指定账号，默认为config文件中的from字段配置的账户
    --password              指定密码，默认为0
    ```
* **示例**
    ``` shell
    unlock --account 0x3431952248809829f790c33f5411d0b56e58079c --password 123456
    ```

#### 2.2.5 启动链组交互式命令行 console
* **描述**：启动与当前链组进行交互的javascript命令行
* **参数**：
    ```
    无
    ```
* **示例**
    ``` shell
    console
    ```

#### 2.2.6 调用ctool与链组内进行交互 ctool
* **描述**：通过调用ctool与链组进行其他交互
* **参数**：
    ```
    与直接调用ctool无异
    ```
* **示例**
    ``` shell
    ctool invoke -addr "0xFC43e7f481b9d3F75CcfFc8D23eAC522E96dE570" -func "transfer("a",b,c) " -abi "D:\\resource\\temp\\contractc.cpp.abi.json" 
    ```

#### 2.2.7 快速搭建一个4节点网络 four
* **描述**：搭建一个4节点网络，其中node0和node1会额外建立一个链组group1
* **参数**：
    ```
        --password              指定密码，默认为0
        --ip                              指定ip，默认127.0.0.1 若要与非本机节点组网通信，必须指定真实的网络ip。
    ```
* **示例**
    ``` shell
    four --password 123456 
    ```
    此命令搭建一个4节点网络，其中node0和node1会额外组建一个链组group1，网络搭建完成后，console配置默认指向node0。若要与其他节点交互，请重启console并指定对应的配置文件。
    ``` shell
    ./console.py --config ../conf/config_1.json
    ```------
<font Size=1>Edit by PlatONE Team 2020-07-29</font>

