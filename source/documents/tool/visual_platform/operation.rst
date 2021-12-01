==============
使用说明
==============

用户登录
=============

登录
^^^^^^^^^^^^

登陆网址http://10.250.122.10:8080/login 进入用户登陆界面

.. figure:: ../../../images/tool/visual_platform/login.png

系统部署初需要制定超级管理员用户名密码以及关联到链上链创建者的账户，这里用户名和密码均为admin

主页
^^^^^^^^^^

主页展示信息如下： |main|

用户管理
=============

添加用户
^^^^^^^^^^^

1) 点击左侧导航栏，选择“系统管理”，进入系统管理页面： |systemmanager|

2) 在系统管理页面右上部分为用户管理模块，该模块如下： |usermanager|

3) 点击右上角添加按钮，跳出添加用户所需填写的相关信息： |adduserform|

4) 点击确认提交信息，提交后用户被正确添加： |adduserresult|

更新用户
^^^^^^^^^^^

1) 在用户管理页面点击想要更改信息的用户名，进入用户信息更改页面：|updateuserform|

2) 可以修改用户密码及权限，如图所示，更改后点击确认，并得到结果：|updateuserresult|

PlatONE系统参数设置
========================

1) 点击左侧导航栏，选择“系统管理”，进入系统管理页面： |systemmanager|

2) 在系统管理页面左上部分为PlatONE系统参数设置模块，该模块如下：|systemconfig|

3) 可在该模块修改系统参数设置并点击确认进行提交

.. note:: ``TxGasLimit`` 需小于 ``BlockGasLimit``

节点管理
===============

部署节点
^^^^^^^^^^^^^^

1) 点击左侧导航栏，选择“系统管理”，进入系统管理页面： |systemmanager|

2) 在系统管理页面左下部分为节点管理模块，该模块如下： |nodemanager|

3) 点击右上角部署新节点，弹出节点部署窗口，可输入如下节点各类信息：|addnode|

添加节点准入
^^^^^^^^^^^^^^^^

1) 点击右上角添加节点准入，弹出节点准入信息窗口 |nodepermission|

2) 输入节点信息后点击确认

节点管理（开启/关闭/重启）
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

点击某个节点，可针对该节点进行启动、停止和重启操作 |nodepop|

CNS管理
==============

CNS列表
^^^^^^^^^^^^^

1) 点击左侧导航栏，选择“系统管理”，进入系统管理页面： |systemmanager|

2) 在系统管理页面右下部分为CNS管理模块，下图为CNS列表页面： |cnslist|

CNS注册
^^^^^^^^^^

1) 点击页面右上角添加新的CNS注册信息，弹出CNS注册窗口： |cnsregister|

2) 注册成功后页面显示新的CNS信息： |cnsresult|

区块浏览
============

区块浏览
^^^^^^^^^^^^^^

1) 点击左侧导航栏，选择“区块浏览”，进入区块浏览页面： |systemmanager|

2) 区块浏览页面包含区块高度、出块节点、gas消耗等多维度的区块信息，页面如下：|block|

3) 点击某条区块信息可进入该区块交易的详情页面，在页面上侧还提供区块查询功能

交易浏览
===========

1) 点击左侧导航栏，选择“交易浏览”，进入交易浏览页面： |systemmanager|

2) 交易浏览页面包含交易哈希、交易双方地址等多维度的区块信息，页面如下：|tx|

3) 点击某条交易可进入该交易的详情页面： |txdetail|

合约操作
===========

合约浏览
^^^^^^^^^^^

1) 点击左侧导航栏，选择“合约浏览”，进入合约浏览页面： |systemmanager|

2) 合约浏览页面包含等多维度的区块信息，页面如下： |contract|

3) 点击某条交易可进入该交易的详情页面： |contractdetail|

合约部署
^^^^^^^^^^^^^^^

1) 点击左上合约部署按钮，弹出合约部署窗口： |deploy|

2) 上传wasm和abi文件，点击确认部署合约

.. |main| image:: ../../../images/tool/visual_platform/main.png
.. |systemmanager| image:: ../../../images/tool/visual_platform/sysmanager.png
.. |usermanager| image:: ../../../images/tool/visual_platform/usermanager.png
.. |adduserform| image:: ../../../images/tool/visual_platform/adduserform.png
.. |adduserresult| image:: ../../../images/tool/visual_platform/adduserresult.png
.. |updateuserform| image:: ../../../images/tool/visual_platform/updateuserform.png
.. |updateuserresult| image:: ../../../images/tool/visual_platform/updateuserresult.png
.. |systemconfig| image:: ../../../images/tool/visual_platform/systemconfig.png
.. |nodemanager| image:: ../../../images/tool/visual_platform/nodemanager.png
.. |addnode| image:: ../../../images/tool/visual_platform/addnode.png
.. |nodepermission| image:: ../../../images/tool/visual_platform/nodepermission.png
.. |nodepop| image:: ../../../images/tool/visual_platform/nodeop.png
.. |cnslist| image:: ../../../images/tool/visual_platform/cnslist.png
.. |cnsregister| image:: ../../../images/tool/visual_platform/cnsregister.png
.. |cnsresult| image:: ../../../images/tool/visual_platform/cnsresult.png
.. |block| image:: ../../../images/tool/visual_platform/block.png
.. |tx| image:: ../../../images/tool/visual_platform/tx.png
.. |txdetail| image:: ../../../images/tool/visual_platform/txdetail.png
.. |contract| image:: ../../../images/tool/visual_platform/contract.png
.. |contractdetail| image:: ../../../images/tool/visual_platform/contractdetail.png
.. |deploy| image:: ../../../images/tool/visual_platform/deploy.png
