# Venachain部署介绍

| 时间       | 修改人 | 修改事项 | Venachain版本 | 文档版本 |
| ---------- | ------ | -------- | ------------- | -------- |
| 2022.04.22 | 吴经文 | 功能整理 | 1.1           | 1.0      |

## 概念说明

### 单机与多机

- 单机部署指的是只在同一台主机上进行操作。
- 多机部署指的是支持对不在同一个局域网中的主机进行操作。
- 在 ``venachainctl.sh`` 工具中，只有 ``remote`` 功能支持多机操作，其余功能都属于单机操作功能。

### 部署机与目标机

- 在部署时，如果主机Z分别向主机A、B、C、D部署四个节点，那么主机Z即为部署机，主机A、B、C、D即为目标机。
- 部署机也可以同时是目标机。
- 在部署机上向目标机部署完节点后，可以进入目标机的部署路径，使用脚本提供的单机操作功能。

### 配置文件

- 配置文件指的是 ``deploy_node-${node-id}.conf`` 文件，所有的部署方式都基于该文件进行操作。
- 同环境部署时，配置文件通过 ``venachainctl.sh dcgen`` 生成；项目维度部署时，配置文件通过 ``venachainctl.sh remote prepare`` 生成。
- 配置文件也可以根据需要，自定义修改 ``conf/deploy.conf.template`` 文件，然后放入相应的目录下。

## 节点部署方式

### 按部署形式来分

#### 单机部署

单机部署是部署机与目标机在同一台主机上的部署方式，即用户在本地进行部署。适合用于开发测试使用。

#### 多机部署

多机部署指的是，部署机与目标机，或目标机之间在不同的主机上。使用用于部署生产环境。

### 按部署方式来分

#### 部署目录部署

- 部署目录部署指的是，使用脚本部署节点，节点的数据与配置文件都会被存放在和脚本目录同级的 ``data`` 目录下。
- 部署目录部署的局限性在于，在同一个部署目录下，每次要部署新的链，需事先将老的链清理掉。如果要在保留原有链的前提下部署新的链，就需要将脚本、二进制文件等手动复制到一个新的工作目录下，再进行部署。
- 部署目录部署方式主要是为了保留1.1版本前的部署方式，该部署方式适合开发者在项目目录下快速部署节点对开发的功能进行测试。

#### 项目目录部署

- 项目目录部署是1.1版本后提出的，将每一条链用项目名区分，链中节点的产生的文件放置在不同的以项目命名的目录下。而所有项目目录默认被放置在 ``$HOME/Venachain`` 下，且部署路径可以在部署前通过修改配置文件来进行设置。
- 项目目录部署更加方便，只需一个用于部署的工作目录就可以在不同的目录下部署多条链，管理多条链。
- 只有 ``venachainctl.sh remote`` 命令是项目目录部署类操作，其余命令均为部署目录部署类操作。

## 节点部署目录结构

### 部署目录部署

```console
${WORKSPACE}
    ├── bin
    ├── conf
    │   ├── bak                                 // 之前部署时产生的文件的备份
    │   ├── contracts
    │   ├── contracts_privacy
    │   ├── deploy.conf.template                // 配置文件模板
    │   ├── firstnode.info                      // firstnode信息
    │   ├── genesis.json                        // 链的配置信息
    │   ├── genesis.json.istanbul.template      // genesis.json文件模板
    │   ├── keyfile.account                     // keyfile对应的account
    │   ├── keyfile.json                        // 账户生成的keyfile
    │   └── keyfile.phrase                      // keyfile的密码
    ├── data
    │   └── node-0                              // 节点数据存放位置
    │       ├── deploy_node-0.conf              // 节点配置信息
    │       ├── keystore                        // 存放节点的账户
    │       ├── logs                            // 节点的日志
    │       │   ├── venachain_error.log         // 节点报错日志
    │       │   ├── venachain_log               // 节点运行日志
    │       │   └── wasm_log
    │       ├── node-0.ipc
    │       ├── node.address                    // 节点的地址
    │       ├── node.prikey                     // 节点的私钥
    │       ├── node.pubkey                     // 节点的公钥
    │       └── venachain
    └── scripts
        ├── local
        └── remote
```

### 项目目录部署

- 部署机的目录结构如下

  ```console
  ${DEPLOYMENT_PATH}
  ├── deployment_conf                           // ${DEPLOYMENT_CONF_PATH}
  │   ├── bak                                   // 之前部署的项目的配置文件备份
  │   ├── logs								
  │   │   └── prepare_log.txt                   // 当前部署机上所有项目配置文件的信息
  │   └── projects                              // 部署项目的必要数据
  │       └── ${PROJECT_NAME}                   // ${PROJECT_CONF_PATH}
  │           ├── deploy_node-0.conf            // 节点配置文件
  │           ├── global                        // 存放genesis.json等全局信息
  │           │   ├── data                      // 存放所有节点的公钥
  │           │   │   └── node-0
  │           │   │       └── node.pubkey
  │           │   ├── firstnode.info            // firstnode的信息
  │           │   ├── genesis.json              // 链的配置
  │           │   ├── keyfile.account           // firstnode的keyfile对于的account
  │           │   ├── keyfile.json              // firstnode账户生成的keyfile
  │           │   └── keyfile.phrase            // firstnode的keyfile的密码
  │           └── logs
  │               └── deploy_log.txt            // 当前项目部署过程产生的日志
  └── linux                                     // 用于执行部署的必要文件
      ├── bin
      ├── conf
      └── scripts
  ```

- 目标机 ``deploy_path`` 的目录结构如下

  ```console
  .                                             // $HOME/Venachain
  ├── bak                                       // 以前部署过的项目的conf文件备份
  └── ${DEPLOY_PATH}
      ├── bin
      ├── conf
      │   ├── bak                               // 之前部署时产生的文件的备份
      │   ├── contracts
      │   ├── contracts_privacy
      │   ├── deploy.conf.template              // 配置文件模板
      │   ├── firstnode.info                    // firstnode信息
      │   ├── genesis.json                      // 链的配置信息
      │   ├── genesis.json.istanbul.template    // genesis.json文件模板
      │   ├── keyfile.account                   // 账户生成的keyfile
      │   └── keyfile.phrase                    // keyfile的密码
      ├── data
      │   └── node-0                            // 节点数据存放位置
      │       ├── deploy_node-0.conf            // 节点配置信息
      │       ├── keystore                      // 存放节点的账户
      │       ├── logs                          // 节点的日志
      │       │   ├── venachain_error.log       // 节点报错日志
      │       │   ├── venachain_log             // 节点运行日志
      │       │   └── wasm_log
      │       ├── node-0.ipc
      │       ├── node.address                  // 节点的地址
      │       ├── node.prikey                   // 节点的私钥
      │       ├── node.pubkey                   // 节点的公钥
      │       └── venachain
      └── scripts
          ├── local
          └── remote
  ```

