================
web3 namespace
================

web3_clientVersion
=======================

Returns the current client version.

Parameters
^^^^^^^^^^^^^^^^

none

Returns
^^^^^^^^^^^^^

``String`` - The current client version.

Example
^^^^^^^^^^^^^

.. code:: js

   // Request
   curl -X POST --data '{"jsonrpc":"2.0","method":"web3_clientVersion","params":[],"id":67}'

   // Result
   {
     "id":67,
     "jsonrpc":"2.0",
     "result": "PlatONEnetwork/platone/v0.9.9-stable/linux-amd64/go1.11.4"
   }


web3_sha3
=============

Returns Keccak-256 (*not* the standardized SHA3-256) of the given data.


Parameters
^^^^^^^^^^^^^^^^

``DATA`` - the data to convert into a SHA3 hash.

Example Parameters
^^^^^^^^^^^^^^^^^^^^^^^

.. code:: js

   params: [
     "0x74657374"  // the hex string of text message "test"
   ]


Returns
^^^^^^^^^^^^^^

``DATA`` - The SHA3 result of the given string.

Example
^^^^^^^^^^^^^^

.. code:: js

   // Request
   curl -X POST --data '{"jsonrpc":"2.0","method":"web3_sha3","params":["0x74657374"],"id":64}'

   // Result
   {
     "id":64,
     "jsonrpc": "2.0",
     "result": "0x9c22ff5f21f0b81b113e63f7db6da94fedef11b2119b4088b89664fb9a3cb658"
   }