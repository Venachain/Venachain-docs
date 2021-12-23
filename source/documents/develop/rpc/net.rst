================
net namespace
================

net_listening
=================

Returns ``true`` if client is actively listening for network
connections.


Parameters
^^^^^^^^^^^^^^^

none


Returns
^^^^^^^^^^^

``Boolean`` - ``true`` when listening, otherwise ``false``.


Example
^^^^^^^^^^^^

.. code:: js

   // Request
   curl -X POST --data '{"jsonrpc":"2.0","method":"net_listening","params":[],"id":67}'

   // Result
   {
     "id":67,
     "jsonrpc":"2.0",
     "result":true
   }

net_peerCount
====================

Returns number of peers currently connected to the client.


Parameters
^^^^^^^^^^^^^^^^

none


Returns
^^^^^^^^^^^^

``QUANTITY`` - integer of the number of connected peers.


Example
^^^^^^^^^^^

``./venachaincil.sh four``

.. code:: js

   // Request
   curl -X POST --data '{"jsonrpc":"2.0","method":"net_peerCount","params":[],"id":74}'

   // Result
   {
     "id":74,
     "jsonrpc": "2.0",
     "result": "0x3"   // 3个节点
   }
