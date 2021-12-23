=============
RPC接口文档
=============

`JSON <http://json.org/>`__ is a lightweight data-interchange format. It
can represent numbers, strings, ordered sequences of values, and
collections of name/value pairs.

`JSON-RPC <http://www.jsonrpc.org/specification>`__ is a stateless,
light-weight remote procedure call (RPC) protocol. Primarily this
specification defines several data structures and the rules around their
processing. It is transport agnostic in that the concepts can be used
within the same process, over sockets, over HTTP, or in many various
message passing environments. It uses JSON (`RFC
4627 <http://www.ietf.org/rfc/rfc4627.txt>`__) as data format.

1） JavaScript API

To talk to an Venachain node from inside a JavaScript application use the
`web3.js <https://github.com/ethereum/web3.js>`__ library, which gives a
convenient interface for the RPC methods. See the `JavaScript
API <https://github.com/ethereum/wiki/wiki/JavaScript-API>`__ for more.

2） JSON-RPC Endpoint

Default JSON-RPC endpoints:

====== =====================
Client URL
====== =====================
Go     http://localhost:6790
====== =====================

You can start the HTTP JSON-RPC with the ``--rpc`` flag

.. code:: bash

   venachain --rpc

change the default port (8545) and listing address (localhost) with:

.. code:: bash

   venachain --rpc --rpcaddr <ip> --rpcport <portnumber>

If accessing the RPC from a browser, CORS will need to be enabled with
the appropriate domain set. Otherwise, JavaScript calls are limit by the
same-origin policy and requests will fail:

.. code:: bash

   venachain --rpc --rpccorsdomain "http://localhost:3000"

The JSON RPC can also be started from the `venachain
console <https://github.com/ethereum/go-ethereum/wiki/JavaScript-Console>`__
using the ``admin.startRPC(addr, port)`` command.

3） JSON-RPC support

============== ==========
\              venachain-Go
============== ==========
JSON-RPC 1.0   
JSON-RPC 2.0   ✓
Batch requests ✓
HTTP           ✓
IPC            ✓
WS             ✓
============== ==========

4） HEX value encoding

At present there are two key datatypes that are passed over JSON:
unformatted byte arrays and quantities. Both are passed with a hex
encoding, however with different requirements to formatting:

When encoding **QUANTITIES** (integers, numbers): encode as hex, prefix
with “0x”, the most compact representation (slight exception: zero
should be represented as “0x0”). Examples:

-  0x41 (65 in decimal)
-  0x400 (1024 in decimal)
-  WRONG: 0x (should always have at least one digit - zero is “0x0”)
-  WRONG: 0x0400 (no leading zeroes allowed)
-  WRONG: ff (must be prefixed 0x)

When encoding **UNFORMATTED DATA** (byte arrays, account addresses,
hashes, bytecode arrays): encode as hex, prefix with “0x”, two hex
digits per byte. Examples:

-  0x41 (size 1, “A”)
-  0x004200 (size 3, “\\0B\0”)
-  0x (size 0, ““)
-  WRONG: 0xf0f0f (must be even number of digits)
-  WRONG: 004200 (must be prefixed 0x)

Currently
`cpp-ethereum <https://github.com/ethereum/cpp-ethereum>`__,\ `go-ethereum <https://github.com/ethereum/go-ethereum>`__
and `parity <https://github.com/paritytech/parity>`__ provide JSON-RPC
communication over http and IPC (unix socket Linux and OSX/named pipes
on Windows). Version 1.4 of go-ethereum, version 1.6 of Parity and
version 0.8 of Pantheon onwards have websocket support.

.. _rpc-dbp:

5） The default block parameter

The following methods have an extra default block parameter:

-  :ref:`eth_getBalance <eth-getbalance>`
-  :ref:`eth_getCode <eth-getCode>`
-  :ref:`eth_getTransactionCount <eth-getTransactionCount>`
-  :ref:`eth_getStorageAt <eth-getStorageAt>`
-  :ref:`eth_call <eth-call>`

When requests are made that act on the state of ethereum, the last
default block parameter determines the height of the block.

The following options are possible for the defaultBlock parameter:

-  ``HEX String`` an integer block number
-  ``String "earliest"`` for the earliest/genesis block
-  ``String "latest"`` for the latest mined block
-  ``String "pending"`` for the pending state/transactions

6） Curl Examples Explained

The curl options below might return a response where the node complains
about the content type, this is because the –data option sets the
content type to application/x-www-form-urlencoded . If your node does
complain, manually set the header by placing -H “Content-Type:
application/json” at the start of the call.

The examples also do not include the URL/IP & port combination which
must be the last argument given to curl e.x. 127.0.0.1:8545


.. toctree::
   :hidden:
   
   rpc/web3
   rpc/net
   rpc/eth
   rpc/admin
   rpc/personal
   rpc/txpool






