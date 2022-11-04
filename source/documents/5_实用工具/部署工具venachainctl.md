# 部署工具venachainctl

## 0. 阅读指南

在使用 ``venachainctl.sh`` 工具前，请先阅读完本章 ``0.阅读指南`` 。**如果存在文档也无法提供帮助解决的问题，或者文档有地方存在异议，请及时联系文档最后一次的修改人。**

### 文档使用方法

- 关于Venachain的下载与编译请见 [**文档《Venachain编译》**](../6_深入使用指南/Venachain编译.md) 。

- 关于Venachain部署中涉及的一些名词解释与部署环境的目录结构，请见 [**文档《Venachain部署介绍》**](../2_区块链部署/Venachain部署介绍.md) 。
- 关于脚本提供功能的使用方法，直接看 **操作示例** 部分即可；
- 如果在操作过程中遇到问题与疑惑，那么可以通过 **注意事项** 进行排查。

#### 功能分类

- 在单机上操作时，查看 ``1.venachainctl.sh`` 提供的功能；
- 多机一键部署查看 ``2.venachainctl.sh remote deploy`` 。

#### 指令参数介绍

关于指令中每个参数的作用以及默认值，请查看指令 ``--help`` 中的英文解释：
- 如果没有给出 ``default`` 就代表该参数没有默认值。
- 如果参数解释中有 ``must be specified`` ，那么代表该参数是必须给定的。
- 给定选项选择的都会有 ``"AAA" and "BBB" are supported`` 的提示，参数值必须从给出的可选选项中进行选择。

#### 注意事项

- 在 **注意事项** 中，每一个目录第一次出现都会给出完整的相对路径或绝对的方式来表示，相对路径是与 ``venachainctl.sh`` 相对的路径。
- 如果该目录在文中再次出现，那么会直接使用引用表示（ ``${}`` ）的方法。因此在阅读时，如果需要知道引用表示目录的绝对路径，那么可以在前文中找到。

## 1. venachainctl.sh

所有与部署相关的功能的入口。

```console
DESCRIPTION
    The deployment script for venachain
USAGE:
    venachainctl.sh <command> [command options] [arguments...]
COMMANDS
    one                      start a node completely (default account password: 0)
    four                     start four node completely (default account password: 0)
    setupgen                 create the genesis.json and compile sys contract
    init                     initialize node. please setup genesis first
    start                    try to start the specified node
    deploysys                deploy the system contract
    addnode                  add normal node to system contract
    updatesys                update node type
    dcgen                    generate deploy conf file
    keygen                   generate key pair
    gengen                   generate genesis.json file
    addadmin                 add super admin role and chain admin role
    delete                   try to delete the specified node
    stop                     try to stop the specified node
    restart                  try to restart the specified node
    clear                    try to stop and clear the node data
    createacc                create account
    unlock                   unlock node account
    console                  start an interactive JavaScript environment
    remote                   remote deploy
    status                   show all node status
    get                      display all nodes in the system contract
    version                  show venachain release version
```

**接口说明**

- 指令 ``one`` - ``updatesys`` 是兼容老脚本的单机部署与分布部署的命令。
- 指令 ``dcgen`` - ``addadmin`` 是新添加的分布部署相关的命令，有新增的功能也有从老脚本逻辑中提取出来的功能。
- 指令 ``delete`` - ``clear`` 是单机上和清理链相关的命令。
- 指令 ``create`` 、 ``unlock`` 是账户操作的命令。
- 指令 ``console`` 是进入控制台的命令。
- 指令 ``remote`` 是远程一键部署的命令。
- 指令 ``status`` - ``version`` 是查看节点信息相关的命令。


### one

在单机上部署一个单节点的链。**部署后的目录结构请看** [**文档《Venachain部署介绍》**](../2_区块链部署/Venachain部署介绍.md) 。 

```console
one OPTIONS
    --help, -h                   show help
```

**操作示例**

```bash
./venachainctl.sh one
```

**注意事项**

- 如果是要在当前 ``${WORKSPACE}`` 部署新的链，那么在使用前先使用 ``clear`` 指令清理下。
- 固定值： ``nodeid`` 为 ``0`` ， ``ip_addr`` 为 ``127.0.0.1`` ， ``rpc_port`` 为 ``6791`` ， ``p2p_port`` 为 ``16791`` ， ``ws_port`` 为 ``26791`` 。如果上述端口被占用，那么无法完成部署。

### four

在单机上部署一个四节点的链。**部署后的目录结构请看** [**文档《Venachain部署介绍》**](../2_区块链部署/Venachain部署介绍.md) 。 

```console
four OPTIONS
    --help, -h                   show help
```

**操作示例**

```bash
./venachainctl.sh four
```

**注意事项**

- 如果是要在当前 ``${WORKSPACE}`` 部署新的链，那么在使用前先使用 ``clear`` 指令清理下。
- 固定值： ``nodeid`` 为 ``0`` ~ ``3`` ， ``ip_addr`` 为 ``127.0.0.1`` ， ``rpc_port`` 为 ``6791`` ~ ``6794``， ``p2p_port`` 为 ``16791`` ~ ``16794`` ， ``ws_port`` 为 ``26791`` ~ ``26794`` 。如果上述端口被占用，那么无法完成部署。

### setupgen

第一个节点生成密钥并生成创世信息。

```console
setupgen OPTIONS
    --nodeid, -n                 the first node id (default: 0)
    --interpreter, -i            select virtual machine interpreter
                                 "wasm", "evm" and "all" are supported (default: all)
    --validatorNodes, -v         set the genesis validatorNodes
                                 (default: the first node enode code)
    --ip                         the first node ip (default: 127.0.0.1)
    --p2p_port, -p2p             the first node p2p_port (default: 16791)
    --auto                       will read exit the deploy conf, node key and skip ip check
    --help, -h                   show help
```

**操作示例**

```bash
## 快速执行
./venachainctl.sh setupgen --auto true

## 自定义参数执行
./venachainctl.sh setupgen --nodeid 1 --ip 10.10.10.10 --p2p_port 16792 --interpreter all --auto true
```

**注意事项**

- 如果使用了参数 ``--p2p_port`` ，那么它的值会在完成配置文件的生成后，被保存到配置文件中。

- 如果设置了 ``validatorNodes`` ，那么 ``genesis.json`` 中的 ``firstValidatorNode`` 字段会将它作为值；否则根据 ``ip`` 和 ``p2p`` 以及节点的公钥生成 ``enode://${node_pubkey}@${IP_ADDR}:${P2P_PORT}`` 作为该字段的值。

- 如果在 ``../conf`` 下已存在 ``genesis.json`` ，那么将它备份至 ``../conf/bak`` 下并生成新的。

- 如果使用了 ``--auto`` 参数:

   - 如果在 ``../data/node-${node-id}`` 中存在 ``deploy_node-${node-id}.conf`` 配置文件，那么直接读取，否则直接生成新的配置文件。
   - 如果在 ``../data/node-${node-id}`` 下同时存在 ``node.prikey`` 、 ``node.pubkey`` 、 ``node.address`` 这三个密钥文件时，会自动读取它们，否则会将已存在的密钥文件备份至 ``../data/node-${node-id}/bak`` 下，并生成新的密钥。**（老脚本即使在三个密钥文件都存在的情况，也会直接覆盖旧密钥生成新密钥）**。
   - 在密钥成功生成后保存 ``ip`` 的值到配置文件中。

- 如果没有使用 ``--auto`` 参数：

   - 会先询问是否生成配置文件：

     1. 假如选择 ``y`` ，且在 ``../data/node-${node-id}`` 中存在 ``deploy_node-${node-id}.conf`` 配置文件，那么会询问是否要覆盖。如果选择 ``y`` ，会将已存在的密钥文件备份至 ``../data/node-${node-id}/bak`` 下，并生成新的密钥。

     2. 所有交互完成后，如果存在配置文件，会读取配置文件，否则终止流程。

        ```console
        [local/generate-deployconf] Do You What To Create a deploy conf ? Yes or No(y/n):
        y
        [local/generate-deployconf] Deploy conf already exists, overwrite it? Yes or No(y/n):
        y
        ```

   - 会询问是否生成密钥：

     1. 如果选择 ``y`` ，那么会检测是否已有密钥文件。只要存在一个密钥文件，就会询问是否要备份旧的密钥并生成新的密钥。如果选择 ``y`` ，会将已存在的密钥文件备份至 ``../data/node-${node-id}/bak`` 下，并生成新的密钥；

     2. 交互完后，如果三个密钥文件都存在，那么继续流程，否则终止流程。（不会检测公私钥是否匹配）。

        ```console
        [local/generate-key] Do You What To Create a new node key ? Yes or No(y/n):
        y
        [local/generate-key] Node key already exists, overwrite it? Yes or No(y/n):
        y
        ```

   - 会检查输入的 ``ip`` 参数的合法性（简单检查是否是 ``0-999.0-999.0-999.0-999`` 的格式），如果不合法则要求再次输入。通过检测后，保存 ``ip`` 的值到配置文件中。

### init

初始化节点。**除了 ``firstnode`` ，其余节点都从本步骤开始进行在单机上的分步部署**。

```console
init OPTIONS
    --nodeid, -n                 set node id (default: 0)
    --ip                         set node ip (default: 127.0.0.1)
    --rpc_port, -rpc             set node rpc port (default: 6791)
    --p2p_port, -p2p             set node p2p port (default: 16791)
    --ws_port, -ws               set node ws port (default: 26791)
    --auto                       will read exit the deploy conf and node key 
    --help, -h                   show help
```

**操作示例**

```bash
## 快速执行
./venachainctl.sh init --auto true

## 自定义参数执行
./venachainctl.sh init --nodeid 1 --ip 10.10.10.10 --rpc_port 6792 --p2p_port 16792 --ws_port 26792  --auto true
```

**注意事项**

- 如果使用了参数 ``--ip`` 、 ``--rpc_port`` 、 ``--p2p_port`` 、 ``--ws_port`` ，那么它们的值会被保存到配置文件中。

- 如果使用了 ``--auto`` 参数:

   - 如果在 ``../data/node-${node-id}`` 中存在 ``deploy_node-${node-id}.conf`` 配置文件，那么直接读取，否则直接生成新的配置文件。
   - 如果在 ``../data/node-${node-id}`` 下同时存在 ``node.prikey`` 、 ``node.pubkey`` 、 ``node.address`` 这三个密钥文件时，会自动读取它们，否则会将已存在的密钥文件备份至 ``../data/node-${node-id}/bak`` 下，并生成新的密钥。**（老脚本只要发现有node.pubkey就跳过生成，新脚本要保证三个文件都存在才跳过）**。
   - 在密钥成功生成后保存 ``ip`` 的值到配置文件中。

- 如果没有使用 ``--auto`` 参数（**firstnode谨慎使用，可能会导致与genesis中信息不一致的情况**）：

   - 会先询问是否生成配置文件：

     1. 假如选择 ``y`` ，且在 ``../data/node-${node-id}`` 中存在 ``deploy_node-${node-id}.conf`` 配置文件，那么会询问是否要覆盖。如果选择 ``y`` ，会将已存在的密钥文件备份至 ``../data/node-${node-id}/bak`` 下，并生成新的密钥。

     2. 所有交互完成后，如果存在配置文件，会读取配置文件，否则终止流程。

        ```console
        [local/generate-deployconf] Do You What To Create a deploy conf ? Yes or No(y/n):
        y
        [local/generate-deployconf] Deploy conf already exists, overwrite it? Yes or No(y/n):
        y
        ```

   - 会询问是否生成密钥：

     1. 如果选择 ``y`` ，那么会检测是否已有密钥文件。只要存在一个密钥文件，就会询问是否要备份旧的密钥并生成新的密钥。如果选择 ``y`` ，会将已存在的密钥文件备份至 ``../data/node-${node-id}/bak`` 下，并生成新的密钥。

     2. 交互完后，如果三个密钥文件都存在，那么继续流程，否则终止流程。（不会检测公私钥是否匹配）。

        ```console
        [local/generate-key] Do You What To Create a new node key ? Yes or No(y/n):
        y
        [local/generate-key] Node key already exists, overwrite it? Yes or No(y/n):
        y
        ```

### start

根据启动参数来启动节点。

```console
start OPTIONS
    --nodeid, -n                 start the specified node, must be specified
    --bootnodes, -b              connect to the specified bootnodes node
                                 (default: the first in the suggestObserverNodes in genesis.json)
    --logsize, -s                Log block size (default: 67108864)
    --logdir, -d                 log dir (default: ../data/node_dir/logs/)
    --extraoptions, -e           extra venachain command options when venachain starts
                                 (default: --debug)
    --txcount, -c                max tx count in a block (default: 1000)
    --tx_global_slots, -tgs      max tx count in txpool (default: 4096)
    --lightmode, -l              select lightnode mode
                                 "lightnode" and "lightserver" are supported
    --dbtype                     select database type
                                 "leveldb" and "pebbledb" are supported (default: leveldb)
    --all, -a                    start all nodes
    --help, -h                   show help
```

**操作示例**

```bash
## 启动单个节点
./venachainctl.sh start --nodeid 0

## 启动所有节点
./venachainctl.sh start --all

## 自定义参数执行
./venachainctl.sh start --nodeid 1 --logdir "/opt/venachain/logs" --extraoptions "--debug --verbosity 4" --lightmode lightserver
```

**注意事项**

- 如果节点已经启动，那么不会执行启动命令。
- 如果使用了参数 ``--bootnodes`` 、 ``--logsize`` 、 ``--logdir`` 、 ``--extraoptions`` 、``--txcount`` 、 ``--tx_global_slots`` 、 ``--lightmode`` 、 ``--dbtype`` ，那么它们的值会被保存到配置文件中。如果前一次启动失败，那么要注意在 ``../data/node-${NODE_ID}/deploy_node-${NODE_ID}.conf`` 已经保存上了前一次传入的参数。
- 如果节点启动失败，可以查看 ``${log_dir}/venachain_log`` 下的日志或 ``${log_dir}/venachain_error.log`` 来排查问题。
- ``--logdir`` 参数需要加上 ``""`` ，且使用绝对路径。
- ``venachain`` 的其他参数，都可以加在 ``--extraoptions`` 中，参数也需要加上 ``""`` 。
- 如果要启动轻节点，那么设置参数为 ``--lightmode lightnode`` ；如果要启动轻节点服务，那么设置参数为 ``--lightmode lightserver`` 。

### deploysys

firstnode完成权限设置，加入链并成为链上的共识节点。

```console
deploysys OPTIONS
    --nodeid, -n                 the specified node id (default: 0)
    --auto                       will use the default node password: 0
                                 to create the account and also to unlock the account
    --help, -h                   show help
```

**操作示例**

```bash
./venachainctl.sh deploysys --auto true
```

**注意事项**

- 不使用 ``--auto`` 会询问要创建账户使用的密码。

   ```console
   [local/create-account] Please input account passphrase.
   123
   ```

- 生成账户后会解锁账户。

- ``firstnode`` 在执行完本操作后完成部署。

### addnode

将节点添加到链上。

```console
addnode OPTIONS
    --nodeid, -n                 the specified node id, must be specified
    --desc                       the specified node desc
    --ip                         the specified node ip
                                 If the node specified by nodeid is local,
                                 then you do not need to specify this option (default: 127.0.0.1)
    --rpc_port, -rpc             the specified node rpc_port
                                 If the node specified by nodeid is local,
                                 then you do not need to specify this option (default: 6791)
    --p2p_port, -p2p             the specified node p2p_port
                                 If the node specified by nodeid is local,
                                 then you do not need to specify this option (default: 16791)
    --pubkey                     the specified node pubkey
                                 If the node specified by nodeid is local,
                                 then you do not need to specify this option (default: content of file ${NODE_DIR}/node.pubkey)
    --type                       select specified node type in "2" & "3"
                                 "2" is observer, "3" is lightnode (default: 2)
    --help, -h                   show help
```

**操作示例**

```bash
## 快速执行
./venachainctl.sh addnode --nodeid 0

## 自定义参数执行
./venachainctl.sh addnode --nodeid 1 --desc node1 --ip 10.10.10.10 --rpc_port 6792 --p2p_port 16792 --pubkey b9b5b10c9685b340701a72ce78e471314edcba62f3b208305d6f1f6e50700f3cd8a5b811d70f2a925a79f75c28c8b29c14779c9ff0931b089a1b4e3c5659f310 --type 3
```

**注意事项**

- 参数 ``--desc`` 的值中不能有空格。
- 如果要添加为轻节点，设置参数 ``--type 3`` 。
- 要由 ``firstnode`` 来完成将其他节点加入到区块链。

### updatesys

更新节点的类型，可以更新为共识节点或观察者节点。

```console
updatesys OPTIONS
    --nodeid, -n                 the specified node id, must be specified
    --content, -c                update content 
                                 "consensus" and "observer" are supported (default: consensus)
    --help, -h                   show help
```

**操作示例**

```bash
## 默认更新为共识节点
./venachainctl.sh updatesys --nodeid 0 

## 更新为观察者节点
./venachainctl.sh updatesys --nodeid 0 --content observer
```

**注意事项**

- 通过 ``./venachainctl.sh get`` 可以查看节点类型， ``type:1`` 为共识节点， ``type:2`` 为观察者节点， ``type:3`` 为轻节点。
- 要由 ``firstnode`` 来完成更新节点类型。

### dcgen

自动生成配置文件。

```console
dcgen OPTIONS
    --nodeid, -n                 the node's name, must be specified
    --auto                       will read exist file
    --help, -h                   show help
```

**操作示例**

```bash
./venachainctl.sh dcgen --nodeid 0 --auto
```

**注意事项**

- 会在 ``../data/node-${NODE_ID}`` 目录下生成 ``deploy_node-${NODE_ID}.conf`` 。

- 会检查本机端口占用情况，自动选择空闲端口作为 ``rpc`` 、 ``p2p`` 、 ``ws`` 使用的端口号。

- 如果使用 ``--auto`` ，那么在已存在配置文件的情况下，会直接读取配置。

- 如果没有使用 ``--auto`` ，那么首先会询问是否新建配置文件。如果选择 ``y`` ，那么会检查是否已存在配置文件，假如已存在，那么会询问是否覆盖该配置文件。

   ```console
   [local/generate-deployconf] Do You What To Create a deploy conf ? Yes or No(y/n):
   y
   [local/generate-deployconf] Deploy conf already exists, overwrite it? Yes or No(y/n):
   y
   ```

- 备份的配置文件会放置到 ``../data/node-${NODE_ID}/bak`` 下。

### keygen

用于生成密钥对。

```console
keygen OPTIONS
   --nodeid, -n                 the specified node name, must be specified
   --auto                       will read exit node key
   --help, -h                   show help
```

**操作示例**

```bash
./venachainctl.sh keygen --nodeid 0 --auto
```

**注意事项**

- 会生成 ``node.pubkey`` 、 ``node.prikey`` 、 ``node.address`` 并放置在 ``../data/node-${NODE_ID}`` 目录下。

- 如果选择 ``--auto`` ，且三个密钥文件都存在，那么会直接读取；否则备份已有的密钥文件并生成新的密钥文件。

- 如果不选择 ``--auto`` ，那么会询问是否要生成新的密钥。如果选择 ``y`` ，会检查是否存在任意一个密钥文件，如果存在则会询问是否要覆盖。

   ```console
   [local/generate-key] Do You What To Create a new node key ? Yes or No(y/n):
   y
   [local/generate-key] Node key already exists, overwrite it? Yes or No(y/n):
   y
   ```

- 备份的密钥文件会放置到 ``../data/node-${NODE_ID}/bak`` 下。

### gengen

生成 ``genesis.json`` 文件。

```console
gengen OPTIONS
   --nodeid, -n                 the first node id, must be specified
   --interpreter, -i            select virtual machine interpreter, must be specified
                                "wasm", "evm" and "all" are supported
   --validatorNodes, -v         set the genesis validatorNodes
                                (default: the first node enode code)
   --ip                         the first node ip
   --p2p_port, -p2p             the first node p2p_port
   --help, -h                   show help
```

**操作示例**

```bash
## 快速执行
./venachainctl.sh gengen --nodeid 0

## 自定义参数执行
./venachainctl.sh gengen --nodeid 0 --interpreter all 
```

**注意事项**

- 如果在 ``../conf`` 下已存在 ``genesis.json`` ，那么它会被备份至 ``../conf/bak`` 下并生成新的。
- 如果设置了 ``validatorNodes`` ，那么 ``genesis.json`` 中的 ``firstValidatorNode`` 字段会将它作为值；否则根据 ``ip`` 和 ``p2p`` 以及节点的公钥生成 ``enode://${node_pubkey}@${IP_ADDR}:${P2P_PORT}`` 作为该字段的值。

### addadmin

授予节点系统管理员与链管理员权限。

```console
addadmin OPTIONS
    --nodeid, -n                the specified node name, must be specified
    --help, -h                  show help
```

**操作示例**

```bash
./venachainctl.sh addadmin --nodeid 0
```

**注意事项**

- 如果节点已经有相应的权限，那么会跳过执行。
- 会在 ``../conf`` 下生成 ``firstnode.info`` 文件。

### delete

将节点从链上删除。

```console
delete OPTIONS
    --nodeid, -n                the specified node id, must be specified
    --help, -h                  show help
```

**操作示例**

```bash
./venachainctl.sh delete --nodeid 3
```

**注意事项**

- 在执行前，要保证 ``firstnode`` 处于运行状态。
- 尽量保证被删除节点处于运行状态，否则会有删除失败的情况发生。
- 被删除的节点，其 ``status`` 会变为 ``2`` ，删除的操作不可逆。节点的 ``status`` 可以通过 ``get`` 指令查看。
- 如果 ``firstnode`` 被删除，那么链将会无法正常运作。

### stop

停止在 ``../data`` 目录下，正在运行的节点。

```console
stop OPTIONS
   --nodeid, -n                 stop the specified node
   --all, -a                    stop all node
   --help, -h                   show help
```

**操作示例**

```bash
## 指定某个节点
./venachainctl.sh stop --nodeid 0

## 全部节点
./venachainctl.sh stop --all
```

### restart

重新启动在 ``../data`` 目录下的节点。先停止节点，然后再启动节点。

```
restart OPTIONS
   --nodeid, -n                 restart the specified node
   --all, -a                    restart all node
   --help, -h                   show help
```

**操作示例**

```bash
## 指定某个节点
./venachainctl.sh restart --nodeid 0

## 全部节点
./venachainctl.sh restart --all
```

**注意事项**

- 没有使用 ``--all`` ，那么如果节点处于不可用（disable状态）时，会直接启动节点。
- 使用了 ``--all`` ，那么只会对正在运行（enable状态）的节点生效，没有启动的节点不会被启动。

### clear

停止在 ``../data`` 目录下的节点，并清除数据。

```console
clear OPTIONS
   --nodeid, -n                 clear specified node data
   --all, -a                    clear all nodes data
   --help, -h                   show help
```

**操作示例**

```bash
## 指定某个节点
./venachainctl.sh clear --nodeid 0

## 全部节点
./venachainctl.sh clear --all
```

**注意事项**

- 假如是清除指定节点，那么会删除该节点的 ``../data/node-${nodeid}`` 目录。
- 假如时清除所有节点，那么会删除整个 ``../data`` 目录，并将 ``../conf`` 目录下的 ``genesis.json`` 、  ``firstnode.info`` 、 ``keyfile.json`` 、 ``keyfile.phrase`` 、 ``keyfile.account`` 备份至 ``../conf/bak`` 目录下，并将这5个文件从 ``../conf`` 中移除。

### createacc

创建账户。

```console
createacc OPTIONS
    --nodeid, -n                the specified node name, must be specified
    --create_keyfile, -ck       will create keyfile
    --auto                      will use the default node password: 0
                                to create the account and also to unlock the account
    --help, -h                  show help
```

**操作示例**

```bash
## 生成账户
./venachainctl.sh createacc --nodeid 0 

## 生成账户，并保存keyfile到本地
./venachainctl.sh createacc --nodeid 0 --create_keyfile

## 自动生成密码为0的账户，并保存keyfile到本地
./venachainctl.sh createacc --nodeid 0 --create_keyfile --auto
```

**注意事项**

- 操作后会在 ``../data/node-${nodeid}`` 下生成 ``UTC--*`` 文件。
- 使用 ``--create_keyfile`` ，会使用 ``../data/node-${nodeid}/keystore`` 下最新生成的 ``UTC--*`` 文件来，在 ``../conf`` 下生成三个 ``keyfile`` 文件。
- 使用 ``--auto`` 指令，会自动使用 ``0`` 作为账户的密码来生成账户。
- 生成账户后会解锁账户。

### unlock

解锁账户。

```console
unlock OPTIONS
   --nodeid, -n                 unlock account on specified node
   --account                    unlock certain account
   --help, -h                   show help
```

**操作示例**

```bash
## 一个节点可能有多个账户，本操作会将最早生成的账户解锁
./venachainctl.sh unlock --nodeid 0

## 指定账户解锁
./venachainctl.sh unlock --nodeid 0 --account 0x50ddfc6e619a3e94d087d667460fe38fdded2950
```

### console

进入JavaScript控制台。

```console
console OPTIONS
   --opennodeid , -n            open the specified node console
                                set the node id here
   --closenodeid, -c            stop the specified node console
                                set the node id here
   --closeall                   stop all node console
   --help, -h                   show help
```

**操作示例**

```bash
## 进入控制台，exit退出
./venachainctl.sh console --nodeid 0

## 杀死指定控制台进程
./venachainctl.sh console --closenodeid 0

## 杀死所有控制台进程
./venachainctl.sh console --closeall
```

**注意事项**

- 当使用exit退出控制台后，控制台进程就会被删除，不需要再手动执行一次 ``--closenodeid`` 命令。

### status

获取本机上部署的在 ``../data``下的节点的运行情况以及节点信息

```console
status OPTIONS                  show all node status
   --nodeid, -n                 show the specified node status info
   --all, -a                    show all nodes status info
   --help, -h                   show help
```

**操作示例**

```bash
## 指定某个节点
./venachainctl.sh status --nodeid 0

## 所有节点
./venachainctl.sh status --all
```

### get

获取加入区块链的节点信息。

**操作示例**

```bash
./venachainctl.sh get
```

**注意事项**

- 使用前要确保链上已经有节点完成部署。

## 2. venachainctl.sh remote

```console
remote OPTIONS
   deploy               deploy nodes
   prepare              generate directory structure and deployment conf file
   transfer             transfer necessary file to target node
   init                 initialize the target node
   start                start the target node
   clear                clear the target node
   --help, -h           show help
```

**接口说明**

- **命令需在操作机使用。**
- **如果部署的节点不在操作机上，那么必须提前做好操作机对各目标机的免密登录**。
- **部署相关的名词解释和部署环境的目录结构请看** [**文档《Venachain部署介绍》**](../2_区块链部署/Venachain部署介绍.md) 。

### remote deploy

一键多机部署。

```console
remote deploy OPTIONS:
    --project, -p               the project name, must be specified
    --interpreter, -i           Select virtual machine interpreter
                                "wasm", "evm" and "all" are supported (default: all)
    --validatorNodes, -v        set the genesis validatorNodes (default: the first node enode code)
    --mode, -m                  the specified deploy mode
                                "conf", "one", "four" are supported (default: conf)
                                "conf": deploy node by exist node deploy conf file
                                "one": deploy a one-node chain locally
                                "four": deploy a four-nodes chain locally
    --node, -n                  the node name, only used in conf mode
                                use "," to seperate the name of node
    --address, -addr            the specified node address, only used in conf mode
                                deploy conf file will be generated automatically
    --all, -a                   deploy all nodes
    --help, -h                  show help
```

**操作示例**

```bash
## 单机部署单节点
./venachainctl.sh remote deploy --mode one --project test

## 单机部署四节点
./venachainctl.sh remote deploy --mode four --project test

## 根据配置一键多机部署
## --mode conf 可省略
## 根据地址，新建配置文件部署
./venachainctl.sh remote deploy --mode conf --project test --address root@10.10.10.10,user@10.10.10.11

## 根据节点名称，读取已有配置文件部署
# 单个节点
./venachainctl.sh remote deploy --mode conf --project test --node 0
# 多个节点
./venachainctl.sh remote deploy --mode conf --project test --node 1,2
# 所有节点
./venachainctl.sh remote deploy --mode conf --project test --all
```

**注意事项**

- 支持 ``--interpreter`` 和 ``--validatorNodes`` 参数，它们只有在部署的节点是 ``firstnode`` 时才会生效。

- 参数 ``--addr`` 的值以 ``${USER_NAME}@${IP_ADDR}`` 的形式表示

- 在 ``one`` 和 ``four`` 模式下，如果已存在 ``../../../deployment_conf/${PROJECT_NAME}`` ( ``${PROJECT_CONF_PATH}`` ) 目录，那么会询问是否要覆盖已有项目。如果选择 ``y`` ，那么会停止已有项目的节点并进行备份和清除数据，然后依次部署节点；否则停止执行。

   ```console
   [remote/deploy] ${DEPLOYMENT_CONF_PATH}/projects/test has already been existed, do you want to overwrite it?
   n
   ```

- 在 ``conf`` 模式中，有两种部署方式。如果同时使用了 ``--address`` 和 ``--node`` 或 ``--all`` 参数时，默认选择使用 ``--address`` 方式。

   - 使用 ``--address`` 方式时，如果操作机已存在 ``${PROJECT_CONF_PATH}`` ，那么会询问是否覆盖。如果选择 ``y`` ，那么会停止已有项目的节点并进行备份和清除数据，然后依次新建配置文件并部署节点。否则，会询问是否要在已有项目目录下新建配置文件进行部署。如果选择 ``y`` ，那么节点会以新节点加入已有链的形式进行部署，否则停止执行。

     ```console
     [remote/prepare] ${DEPLOYMENT_CONF_PATH}/projects/test already exists, do you want to cover it? Yes or No(y/n): 
     n
     [remote/prepare] Do you mean you want to create new conf file in exist path? Yes or No(y/n): 
     y
     ```

   - 使用 ``--node`` 或 ``--all`` 方式时，会读取 ``${PROJECT_CONF_PATH}`` 下的配置文件，并根据 ``${PROJECT_CONF_PATH}/logs/deploy_log.txt`` 日志内容进行部署。同时使用了 ``--node`` 和 ``--all`` 参数，会默认使用 ``--all`` 进行部署。

- 关于备份与清除的文件处理的具体内容可参考 ``remote prepare --cover`` 。

- 如果节点已经被启动，那么再次根据已有配置部署，节点会被重启。

### remote prepare

生成操作机上的目录结构并生成项目的配置文件。

```console
remote prepare OPTIONS:

    --project, -p               the specified project name, must be specified
    --address, -addr            nodes' addresses, must be specified
    --cover                     backup the project directory if exists
    --help, -h                  show help
```

**操作示例**

```bash
## 生成单个配置文件
./venachainctl.sh remote prepare --project test --address root@10.10.10.10

## 生成多个配置文件
./venachainctl.sh remote prepare --project test --address root@10.10.10.10,user@10.10.10.11,user@10.10.10.11
```

**注意事项**

- 会新建2个目录。其中， ``../../../deployment_conf`` ( ``${DEPLOYMENT_CONF_PATH}`` ) 作为操作机主要的工作目录； ``../../../deployment_conf/${PROJECT_NAME}`` ( ``${PROJECT_CONF_PATH}`` ) 作为项目目录。

- 如果操作机与目标机无法连通，或者无法免密登录目标机，那么会终止执行。本机不会做该检测。

- 参数 ``--addr`` 的值以 ``${USER_NAME}@${IP_ADDR}`` 的形式表示。

- 生成配置文件时，对于参数 ``deploy_path`` ，会使用 ``$HOME/Venachain/${PROJECT_NAME}`` 。

- 生成的配置文件会放在 ``${PROJECT_CONF_PATH}`` 目录下。如果要自定义配置文件，手动生成的配置文件同样也需要放入该目录下。

- 自动生成的配置文件相关信息会立即被写入 ``${DEPLOYMENT_CONF_PATH}/logs/prepare_log.txt`` ；手动添加的配置文件，在下次执行 ``prepare`` 命令时也会被写入 ``${DEPLOYMENT_CONF_PATH}/logs/prepare_log.txt`` 。该日志文件记录了操作机目录下所有项目下配置文件的概要信息。

- 使用参数 ``--cover`` ，会在已存在相同项目名的目录的情况下，直接进行备份覆盖操作。

- 不使用参数 ``--cover`` ，在已存在相同项目名的目录的情况下，会首先问是否要覆盖已有项目。如果选择 ``y`` ，那么会备份已有项目并生成新的项目，覆盖会根据当前项目的配置文件完成节点的停止、备份以及清理；否则询问是否要在已有项目下新增配置文件。关于目标机上的备份与清除的文件处理可参考 ``remote clear --mode clean --all`` 。清理完成后，操作机上 ``${PROJECT_CONF_PATH}`` 会被打上时间戳并移至 ``${DEPLOYMENT_CONF_PATH}/bak`` 下。

   ```console
   [remote/prepare] /home/wujingwen/workspace/go/src/venachain/release/deployment_conf/projects/test already exists, do you want to cover it? Yes or No(y/n): 
   n
   [remote/prepare] Do you mean you want to create new conf file in exist path? Yes or No(y/n): 
   y
   ```

- 生成配置文件会自动探测并避开目标机已占用的端口，以及 ``prepare_log.txt`` 中记录的已占用的端口。

### remote transfer

向目标机传输部署需要的文件。

```console
remote transfer OPTIONS:
    --project, -p               the specified project name, must be specified
    --node, -n                  the specified node name
                                use "," to seperate the name of node
    --all, -a                   transfer files to all nodes
    --help, -h                  show help
```

**操作示例**

```bash
## 单节点执行
./venachainctl.sh remote transfer --project test --node 0

## 多节点执行
./venachainctl.sh remote transfer --project test --node 1,2

## 所有节点执行
./venachainctl.sh remote transfer --project test --all
```

**注意事项**

- 在执行前，必须确保 ``${PROJECT_CONF_PATH}/deploy_node-${NODE_ID}.conf`` 中 ``deploy_path`` 、 ``user_name`` 、 ``ip_addr`` 、 ``p2p_port`` 不能为空。

- 如果操作机与目标机无法连通，或者无法免密登录目标机，那么会终止执行。本机不会做该检测。

- 同时使用了 ``--node`` 和 ``--all`` 参数，会默认使用 ``--all`` 。

- 会新建两个目录。其中，目录 ``${PROJECT_CONF_PATH}/global`` 用于存放全局信息，如 ``genesis.json`` 、 ``firstnode.info`` 等；目录 ``${PROJECT_CONF_PATH}/logs`` 用于存放项目部署过程中产生的日志信息。 

- 会将 ``../conf`` 、 ``../scripts`` 、 ``../bin`` 以及 ``${PROJECT_CONF_PATH}/deploy_node-${NODE_ID}.conf`` 传输到目标机。

- 各项文件传输完成的日志会被记录到 ``${PROJECT_CONF_PATH}/logs/deploy_log.txt`` 中。如果传输过程中因出现错误而中断，再次执行相同的指令可以跳过已完成传输的文件。

- 同一台目标机上的同一个项目，目录  ``../conf`` 、 ``../scripts`` 、 ``../bin`` 只会被传送一次。其中任意一个已经在 ``deploy_log.txt`` 里记录过完成传输，就不会再进行传输。因此，如果手动将目标机上的以上任意目录活目录中的文件删除，那么应当手动将关联的输出日志手动删除。


### remote init

完成启动节点前的所有初始化操作。

```console
remote init OPTIONS:
    --project, -p               the specified project name, must be specified
    --interpreter, -i           Select virtual machine interpreter, must be specified for new project
                                "wasm", "evm" and "all" are supported
    --validatorNodes, -v        set the genesis validatorNodes (default: the first node enode code)
    --node, -n                  the specified node name
                                use "," to seperate the name of node
    --all, -a                   init all nodes
    --help, -h                  show help
```

**操作示例**

```bash
## 单节点执行
./venachainctl.sh remote init --project test --node 0

## 多节点执行
./venachainctl.sh remote init --project test --node 1,2

## 所有节点执行
./venachainctl.sh remote init --project test --all
```

**注意事项**

- 在执行前，必须确保 ``${PROJECT_CONF_PATH}/deploy_node-${NODE_ID}.conf`` 中 ``deploy_path`` 、 ``user_name`` 、 ``ip_addr`` 、 ``rpc_port`` 、 ``p2p_port`` 不能为空。
- 如果操作机与目标机无法连通，或者无法免密登录目标机，那么会终止执行。本机不会做该检测。
- 同时使用了 ``--node`` 和 ``--all`` 参数，会默认使用 ``--all`` 。
- 如果是新项目，还没有生成 ``genesis.json`` ，那么必须设置 ``--interpreter`` 。
- 目标机上生成的 ``node.pubkey`` 会被传输至操作机的 ``${PROJECT_CONF_PATH}/global/data/node-${NODE_ID}/`` 目录下。
- 只要操作机不存在 ``${PROJECT_CONF_PATH}/global/genesis.json`` ，就会默认当前节点为 ``firstnode`` 。
- 如果是 ``firstnode`` ，目标机上生成的 ``genesis.json`` 会被传输至操作机的 ``${PROJECT_CONF_PATH}/global`` 目录下。
- 如果不是 ``firstnode`` ，那么会先将操作机上的 ``${PROJECT_CONF_PATH}/global/genesis.json`` 传输至目标机的 ``${DEPLOY_PATH}/conf`` 下。
- 初始化阶段各项执行完成的日志会被记录到 ``${PROJECT_CONF_PATH}/logs/deploy_log.txt`` 中。如果执行命令的过程中因出现错误而中断，再次执行相同指令可以跳过已完成执行的命令。所有命令执行成功后，根据日志只会执行一次，如果手动对密钥或genesis文件进行了删除，那么也应当删除对应的日志内容。
- 第一次部署该节点，即没有相关执行完成的日志（比如密钥生成、genesis生成）时，但目标节点的三个密钥文件都存在（可能某些需求场景需要用指定密钥手动放入），那么会直接读取；否则会备份已有的密钥文件并生成新的密钥文件。而如果已存在 ``genesis.json`` ，那么会被直接备份原文件并生成新文件。

### remote start

完成节点的启动、加入链与成为共识节点。

```console
remote start OPTIONS:
    --project, -p               the specified project name, must be specified
    --node, -n                  the specified node name
                                use "," to seperate the name of node
    --all, -a                   start all nodes
    --help, -h                  show help
```

**操作示例**

```bash
## 单节点执行
./venachainctl.sh remote start --project test --node 0

## 多节点执行
./venachainctl.sh remote start --project test --node 1,2

## 所有节点执行
./venachainctl.sh remote start --project test --all
```

**注意事项**

- 在执行前，必须确保 ``${PROJECT_CONF_PATH}/deploy_node-${NODE_ID}.conf`` 中 ``deploy_path`` 、 ``user_name`` 、 ``ip_addr`` 、 ``rpc_port`` 不能为空。
- 如果操作机与目标机无法连通，或者无法免密登录目标机，那么会终止执行。本机不会做该检测。
- 同时使用了 ``--node`` 和 ``--all`` 参数，会默认使用 ``--all`` 。
- 只要操作机不存在 ``${PROJECT_CONF_PATH}/global/genesis.json`` ，就会默认当前节点为 ``firstnode`` 。
- 部署 ``firstnode`` 时，生成的 ``keyfile`` 文件与 ``firstnode.info`` 会从目标机传输到操作机的 ``${PROJECT_CONF_PATH}/global`` 下。
- 部署非 ``firstnode`` 时， ``firstnode.info`` 会被传输至宿主机的 ``${DEPLOY_PATH}/conf`` 下；当前部署节点的 ``node.pubkey`` 与配置文件会被传输至 ``firstnode`` 所在目标机的 ``${DEPLOY_PATH}/data/node-${NODE_ID}`` 下。其中， ``${NODE_ID}`` 是当前被部署的节点名，而不是 ``firstnode`` 的节点名。
- 本阶段各项执行完成的日志会被记录到 ``${PROJECT_CONF_PATH}/logs/deploy_log.txt`` 中。如果在执行命令的过程中因出现错误而中断，再次执行相同的指令可以跳过已完成执行的命令（节点启动除外）。
- 如果节点已经启动，那么再次执行执行本命令，节点会被重启。

### remote clear

远程清洁。

```console
remote clear OPTIONS:
    --project, -p               the specified project name, must be specified
    --node, -n                  the specified node name
                                use "," to seperate the name of node
    --mode, -m                  the specified execute mode
                                "deep", "delete", "stop", "restart", "clean" are supported (default: deep)
                                "deep": will do "delete" and "clean" actions
                                "delete": will delete the node from chain
                                "stop": will stop the node
                                "restart": will restart the node
                                "clean": will stop the node and remove the files, configuration files will be backed up
    --all, -a                   clear all nodes
    --help, -h                  show help
```

**操作示例**

```bash
## 链上删除节点
./venachainctl.sh remote clear --project test --mode delete --node 1

## 停止节点
./venachainctl.sh remote clear --project test --mode stop --node 1

## 重启节点
./venachainctl.sh remote clear --project test --mode restart --node 1

## 停止节点，并删除节点文件
./venachainctl.sh remote clear --project test --mode clean --node 1

## 链上删除节点，停止节点，并删除节点文件
# --mode deep 可省略
./venachainctl.sh remote clear --project test --mode deep --node 1

## 多种执行方式
# 单节点执行
./venachainctl.sh remote clear --project test --node 0

# 多节点执行
./venachainctl.sh remote clear --project test --node 1,2

# 所有节点执行
./venachainctl.sh remote clear --project test --all
```

**注意事项**

- 在执行前，必须确保 ``${PROJECT_CONF_PATH}/deploy_node-${NODE_ID}.conf`` 中 ``deploy_path`` 、 ``user_name`` 、 ``ip_addr`` 、 ``p2p_port`` 不能为空。
- 如果操作机与目标机无法连通，或者无法免密登录目标机，那么会终止执行。本机不会做该检测。
- 同时使用了 ``--node`` 和 ``--all`` 参数，会默认使用 ``--all`` 。
- ``delete`` 操作：
   - 不允许对 ``firstnode`` 进行该操作。如果有需求需要从链上删除 ``firstnode`` ，可到目标机使用 ``vcl`` 或 ``venachainctl.sh delete`` 进行删除。使用 ``--all`` 会跳过对 ``firstnode`` 的删除。
   - 要求 ``${PROJECT_CONF_PATH}/global/firstnode.info`` 存在，且 ``user_name`` 、 ``ip_addr`` 、 ``rpc_port`` 字段不能为空。
   - ``firstnode`` 必须在正常运行。
- ``stop`` 操作会杀死节点的进程 。
- ``clean`` 操作：
   - 首先执行 ``stop`` 操作，然后对节点部署所在目录中的文件进行清理。
   - 只有配置文件会被备份至 ``"${DEPLOY_PATH}/../bak/${PROJECT_NAME}"`` 目录下，其余在 ``${NODE_DIR}`` 下的文件都会被删除。
   - 节点在部署过程中向 ``deploy_log.txt`` 里写入的日志内容会被删除。
   - 如果节点所在目标机的部署路径下，所有文件都已被删除，或路径下存在的所有 ``${NODE_DIR}`` 中都没有 ``node.prikey`` 文件，那么该目标机部署路径下的 ``scripts`` 、 ``data`` 、 ``bin`` 目录会被删除； ``conf`` 目录会被备份至 ``"${DEPLOY_PATH}/../bak/${PROJECT_NAME}"`` 目录下。
   - 如果某个节点的部署过程还没有执行到密钥生成步骤，仅完成了配置文件的传输，就停止了部署。而同目录下其他节点都完成了 ``clean`` 或 ``deep`` 操作，那么该节点也会被一起清理掉。并且，它被清理的配置文件不会被备份。
   - 在项目中的所有节点被清除后：
     -  ``"${DEPLOY_PATH}/../bak/${PROJECT_NAME}"`` 会被打上时间戳。
     - 操作机上的 ``${PROJECT_CONF_PATH}`` 下的 ``global`` 和 ``logs`` 会被删除，如果需要备份可以提前手动进行操作。
     - ``${PROJECT_CONF_PATH}`` 目录以及目录下的配置文件不会删除是为了方便下次以使用 ``remote deploy --node`` 命令进行同样配置的部署；如果想要覆盖重新部署，那么可以使用 ``remote deploy --addr`` ；如果想要彻底删除，可以直接手动进行操作。
- ``deep`` 操作会依次执行 ``delete`` 和 ``clean`` 。
