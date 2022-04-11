===========================
WASM和EVM智能合约互调用
===========================

Wasm-Call-Solidity
======================

当在Wasm合约中, 需要调用solidity合约的函数, 只需要在Wasm正常的合约互调用方法中, 稍微改变传递的参数即可。

如此C++合约为例：

.. code:: cpp

    void callSolTest()
    {
        bcwasm::DeployedContract address("0x9bf684bceb5dbd644f9517e503a8cde558cdbdbe");
        int64_t res = address.callInt64("getTestNum", "int64(213)", "string(TestTestTest)");
        bcwasm::println(res);
    }

在C++合约中， 如需调用其他合约函数使用的方式为：``DeployedContract.callInt64(funcName, param1, param2 ...)`` ，当在调用Solidity合约时，唯一的区别在传递的参数param。以上述案例为参照，说明如下：

- ``"getTestNum"``：调用的目的合约函数的函数名。
- ``"int64(213)"``：调用目的函数 ``getTestNum`` 所需要传递的参数，必须全为字符串。 ``int64()`` 为参数的真正类型， ``213`` 为参数的值。参数类型，请在合约对应abi中进行查找。

Solidity-Call-Wasm
========================

在Solidity中调用Wasm中的函数，跟Solidity调用其他Solidity的区别在于Call传递的参数。案例如下：

.. code:: solidity

    function callWasmTest() public{
        bool ok;
        bytes memory res;
        (ok, res) = address(0xDD57d4cb459C2fcb57E1E17fc7090FE9cC622eb1).call(bytes('{"func_name": "getName", "func_params": ["int64(100)"]}'));
      	string memory name = string(res);
    }

参数说明：

``'{"func_name": "getName", "func_params": ["int64(100)"]}'``

此参数为json字符串，其中各字段说明如下：

- ``func_name``：为需要调用的Wasm合约中函数的函数名。
- ``func_params`` ：为需要调用的Wasm合约中函数的参数。其中，参数同样使用 ``"参数类型（参数值）"`` 的方式方式表示，如 ``"int64(100)"``。参数类型，请在合约对应abi中进行查找。

返回值类型说明
===================

Wasm与Solidity合约进行互调用时，所支持的调用的函数返回值只支持2种：

- ``int64``
- ``string``

其他说明
============

**版本说明**

目前支持的solidity版本为0.5.2版本。