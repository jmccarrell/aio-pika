.. _aio-pika: https://github.com/mosquito/aio-pika


Patterns and helpers
++++++++++++++++++++

.. note:: Available since `aio-pika>=1.7.0`

`aio-pika`_ includes some useful patterns for creating distributed systems.


.. _patterns-worker:

Master/Worker
~~~~~~~~~~~~~

Helper class that implements the Master/Worker pattern.
This approach balances tasks between multiple workers when there is no need to
return a result to the master.

The master creates the tasks:

.. literalinclude:: examples/master.py
   :language: python


The worker treats each message as a request for a unit of work:

.. literalinclude:: examples/worker.py
   :language: python

In this example, one or more workers must be created before the master.

.. _patterns-rpc:

RPC
~~~

Helper class that implements the Remote Procedure Call pattern.
Here we use the named queue ``multiply`` to communicate.
There may be one or more rpc callers writing messages into the queue.

When the workers have a long computation to complete, this pattern makes sense.

.. literalinclude:: examples/rpc-caller.py
   :language: python


The workers create the queue ``multiply``.  Thus again, in this example, one or
more workers must be created before the master.

.. literalinclude:: examples/rpc-callee.py
   :language: python

Serialization
~~~~~~~~~~~~~

Both Master/Worker and the RPC helper classes uses pickle as the serialization
format by default.
Here we show overriding methods :func:`aio_pika.patterns.base.serialize` and
:func:`aio_pika.patterns.base.deserialize` to implement a simple json
serializer, and then a json serializer with compression.

.. literalinclude:: examples/extend-patterns.py
   :language: python
