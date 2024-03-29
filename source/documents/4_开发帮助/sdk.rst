============
SDK
============

Venachain暴露了一系列RPC接口供外部调用，上层业务程序可以SDK来调用这些接口，以完成上层业务与链的交互。开发者只需要根据自身业务程序的要求，用SDK提供的API进行编程，即可实现对区块链的操作。

目前，SDK接口可实现的功能包括（但不限于）：

- 合约编译、部署、查询
- 交易发送、上链通知、参数解析、回执解析
- 链状态查询、链参数设置
- 链参数管理（管理员）
- 组员管理
- 权限设置

目前Venachain提供了JAVA-SDK：

.. toctree::
   :maxdepth: 1

   sdk/javasdk.rst
   sdk/gosdk.rst