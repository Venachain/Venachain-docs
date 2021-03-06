=========
命令详情
=========


init
========

**用法**

.. code:: console

   init OPTIONS
       --nodeid, -n                 set node id (default=0)
       --ip                         set node ip (default=127.0.0.1)
       --rpc_port                   set node rpc port (default=6791)
       --p2p_port                   set node p2p port (default=16791)
       --ws_port                    set node ws port (default=26791)
       --auto                       auto=true: will no prompt to create
                                    the node key and init (default: false)
       --help, -h                   show help

**示例**
   
.. code:: bash
   
   venachainctl.sh init -n 1 \
                 --ip 127.0.0.1 \
                 --rpc_port 6792 \
                 --p2p_port 16792 \
                 --ws_port 26792 \
   
   ## or
   venachainctl.sh init --auto true

one
=======

**用法**

.. code:: bash

   ./venachainctl.sh one

**说明**

-  创建genesis.json
-  初始化第一个节点
-  启动第一个节点
-  部署系统合约

   +  创建账户并解锁
   +  升级账户权限
   +  将第一个节点添加到区块链
   +  将第一个节点更新为共识节点

four
========

**用法**

.. code:: bash

   ./venachainctl.sh four

**说明**

-  创建 genesis.json
-  初始化第一个节点和其他三个节点
-  启动第一个节点
-  部署系统合约

   +  创建账户并解锁
   +  升级账户权限
   +  将第一个节点添加到区块链
   +  将第一个节点更新为共识节点
   
-  启动其他三个节点
-  将其他三个节点添加到区块链
-  更新其他三个节点类型为共识节点

start
===========

.. code:: console

   描述
       Venachain脚本部署
   用法:
       venachainctl.sh <command> [command options] [arguments...]
   命令
       start OPTIONS
           --nodeid, -n                 启动指定的节点
           --bootnodes, -b              连接到指定的bootnodes节点
                                        默认值是observeNodes中的第一个enode在genesis.json
           --logsize, -s                日志块大小（默认值:67108864）
           --logdir, -d                 log dir (默认值位置:../data/node_dir/logs/)
                                        使用绝对路径
           --extraoptions, -e           venachain命令启动时, 额外需要设置的命令行参数.
                                        (默认值: --debug)
           --txcount, -c                区块中最大交易数量 (默认值: 1000)
           --all, -a                    启动所有节点
           --help, -h                   显示帮助

stop
=========

.. code:: console

   描述
       Venachain脚本部署
   用法
       venachainctl.sh <command> [command options] [arguments...]
   命令
       stop OPTIONS
           --nodeid, -n                 停止指定的节点
           --all, -a                    停止所有节点
           --help, -h                   显示帮助

restart
============

.. code:: console

   描述
       Venachain脚本部署
   用法
       venachainctl.sh <command> [command options] [arguments...]
   命令
       restart OPTIONS
           --nodeid, -n                 重新启动指定的节点
           --all, -a                    重启所有节点
           --help, -h                   显示帮助

console
===========

.. code:: console

   描述
       Venachain脚本部署
   用法
       venachainctl.sh <command> [command options] [arguments...]
   示例
       ./venachain console -n 0

详情请见 :ref:`venachainctl console 使用介绍 <venachainctl-console>`

deploysys
==============

.. code:: console

   描述
       Venachain脚本部署
   用法
       venachainctl.sh <command> [command options] [arguments...]
   命令
       deploysys OPTIONS
           --nodeid, -n                 指定的节点标识（默认值:0）
           --auto                       auto=true: 将使用默认节点密码:0
                                        创建帐户，并解锁帐户（默认值:false）
           --help, -h                   显示帮助

updatesys
==============

.. code:: console

   描述
       Venachain脚本部署
   用法
       venachainctl.sh <command> [command options] [arguments...]
   命令
       updatesys OPTIONS
           --nodeid, -n                 指定的节点ID
           --content, -c                更新内容 (默认值:'{“type”:1}'）
                                        注意参数格式
           --help, -h                   显示帮助

addnode
===========

.. code:: console

   描述
       Venachain脚本部署
   用法
       venachainctl.sh <command> [command options] [arguments...]
   命令
       addnode OPTIONS
           --nodeid, -n                 指定的节点ID，必须指定
           --desc                       指定的节点desc
           --p2p_port                   指定的节点p2p_port
                                        如果nodeid指定的节点是本地的，
                                        那么不需要指定这个选项。
           --rpc_port                   指定的节点rpc_port
                                        如果nodeid指定的节点是本地的，
                                        那么不需要指定这个选项。
           --ip                         指定的节点ip
                                        如果nodeid指定的节点是本地的，
                                        那么不需要指定这个选项。
           --pubkey                     指定的节点pubkey
                                        如果nodeid指定的节点是本地的，
                                        那么不需要指定这个选项。
           --account                    指定的节点帐户
                                        如果nodeid指定的节点是本地的，
                                        那么不需要指定这个选项。
           --help, -h                   显示帮助

clear
=========

.. code:: console

   描述
       Venachain脚本部署
   用法
       venachainctl.sh <command> [command options] [arguments...]
   命令
       clear OPTIONS
           --nodeid, -n                 清除指定的节点数据
           --all, -a                    清除所有节点数据
           --help, -h                   显示帮助

unlock
=========

.. code:: console

   描述
       Venachain脚本部署
   用法
       venachainctl.sh <command> [command options] [arguments...]
   命令
       unlock OPTIONS
           --nodeid, -n                 解锁指定的节点帐户
           --help, -h                   显示帮助

get
======

从NodeManager系统合同中获取所有节点

**示例**

.. code:: bash

   ./venachainctl.sh get

setupgen
============

.. code:: console

   描述
       Venachain脚本部署
   用法
       venachainctl.sh <command> [command options] [arguments...]
   命令
       setupgen OPTIONS
           --nodeid, -n                 第一个节点id（默认值:0）
           --ip                         第一个节点ip（默认值:127.0.0.1）
           --p2p_port                   第一个节点p2p_port（默认值:16791）
           --auto                       auto=true: 将自动创建新的节点密钥并将自动创建
                                        不再编译系统合约（默认= false）
           --observeNodes, -o           设置genesis observeNodes
                                       （默认值是第一个节点的enode代码）
           --validatorNodes, -v         设置genesis validatorNodes
                                       （默认值是第一个节点的enode代码）
           --interpreter, -i            选择虚拟机解释器:wasm, evm, all (default: wasm)
           --help, -h                   显示帮助

status
==========

.. code:: console

   描述
       Venachain脚本部署
   用法
       venachainctl.sh <command> [command options] [arguments...]
   命令
       status OPTIONS                   显示所有节点状态
           --nodeid, -n                 显示指定的节点状态信息
           --all, -a                    显示所有节点状态信息
           --help, -h                   显示帮助

createacc
=============

.. code:: console

   描述
       Venachain脚本部署
   用法
       venachainctl.sh <command> [command options] [arguments...]
   命令
       createacc OPTIONS
           --nodeid, -n                 为指定节点创建帐户
           --help, -h                   显示帮助