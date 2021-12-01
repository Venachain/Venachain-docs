.. _cli-sysconfig:

==================================
系统参数操作 sysconfig
==================================

针对系统参数的相关操作

系统参数设置 sysconfig set
===============================

**描述**

系统参数设置。

**参数**

- 可选参数:

.. code:: bash

      --block-gaslimit : the gas limit of the block
      --tx-gaslimit : the gas limit of transactions
      --tx-use-gas : if transactions use gas, 'use-gas' for transactions use gas, 'not-use' for not
      --audit-con : approve the deployed contracts, 'audit' for allowing contracts audit, 'not-audit' for not
      --check-perm : check the sender permission when deploying contracts, 'with-perm' for checking permission, 'without-perm' for not
      --empty-block : consensus produces empty block, 'allow-empty' for allowing to produce empty block, 'notallow-empty' for not
      --gas-contract : register the gas contract by contract name
      --vrf-params : modify vrf parameters

**操作**

.. code:: bash

      #注意一次只能操作一个系统参数
      ./platonecli sysconfig set  --audit-con audit  --keyfile ../conf/keyfile.json

**输出结果**

以audit-con为例

.. code:: json

      {
        "status": "Operation Succeeded",
        "logs": [
            "Event IsApproveDeployedContract: 0 param set successful. "
        ],
        "blockNumber": 200,
        "GasUsed": 103352,
        "From": "0x8d4d2ed9ca6c6279bab46be1624cf7adbab89e18",
        "To": "0x1000000000000000000000000000000000000004",
        "TxHash": ""
      }

**操作**

.. code:: bash

      ./platonecli sysconfig set --vrf-params '{"electionEpoch":7,"nextElectionBlock":0,"validatorCount":3}' --keyfile ../conf/keyfile.json

系统参数获取 sysconfig get
===============================

**描述**

系统参数获取。

**参数**

- 可选参数:

.. code:: bash

      --block-gaslimit : the gas limit of the block
      --tx-gaslimit : the gas limit of transactions
      --tx-use-gas : if transactions use gas, 'use-gas' for transactions use gas, 'not-use' for not
      --audit-con : approve the deployed contracts, 'audit' for allowing contracts audit, 'not-audit' for not
      --check-perm : check the sender permission when deploying contracts, 'with-perm' for checking permission, 'without-perm' for not
      --empty-block : consensus produces empty block, 'allow-empty' for allowing to produce empty block, 'notallow-empty' for not
      --gas-contract : register the gas contract by contract name
      --vrf-param : get vrf parameters

**操作**

.. code:: bash

      #注意一次只能获取一个系统参数
      ./platonecli sysconfig get  --audit-con  --keyfile ../conf/keyfile.json

**输出结果**

.. code:: console

      # 以audit-con为例
      result:IsApproveDeployedContract: audit

**操作**

.. code:: bash

      ./platonecli sysconfig get --vrf-params  --keyfile ../conf/keyfile.json