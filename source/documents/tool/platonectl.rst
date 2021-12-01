.. _tool-platonectl:

=============================
节点部署工具 (platonectl.sh)
=============================

platonectl可用的命令。

.. code:: console

       描述
           PlatONE脚本部署
       用法:
           platonectl.sh <command> [command options] [arguments...]
       命令:
           init                             初始化节点。 请先设置genesis
           one                              完全启动一个节点
           four                             完全启动四个节点
           start                            尝试启动指定的节点
           stop                             尝试停止指定的节点
           restart                          尝试重新启动指定的节点
           console                          启动交互式JavaScript环境
           deploysys                        部署系统合约
           updatesys                        正常节点更新到共识节点
           addnode                          将正常节点添加到系统合约中
           clear                            清除所有节点数据
           unlock                           解锁节点帐户
           get                              显示系统合约中的所有节点
           setupgen                         创建genesis.json与节点私钥
           status                           显示节点状态
           createacc                        创建帐户
           version                          查看版本

.. toctree::
   :hidden:
   
   platonectl/main
   platonectl/console

