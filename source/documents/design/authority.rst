========
权限模型
========

根据系统中的不同实体对象，Venachain将权限管理进行了模块化的拆分。针对系统中用户账户、节点和智能合约这三类实体，分别设计了用户角色管理模块、节点管理模块和合约防火墙模块来进行权限的控制和管理。

-  角色管理模块：用于管理用户在链上的权限

-  节点管理模块：用于管理区块链网络中的节点

-  合约防火墙模块：实现接口级别的合约访问权限


.. toctree::
   :hidden:
   
   authority/node
   authority/role
   authority/firewall
