# lotus_eaters
A simple Python throttling lib relying on the token bucket algorithm - Redis Backend
========

The throttle library provides a robust and versatile throttling mechanism for Python.

It can be used in multiple environments, from a single thread to a distributed
server cluster.


It supports Python versions 2.6 to 3.3.


Usage
-----

In order to perform throttling tasks, simply use the :meth:`~throttle.throttle`
function:

.. code-block:: python

    import throttle

    def some_fun(uid):
        if not throttle.check(key=uid, rate=30, size=10):
            raise ThrottleError()
        # Do the job

    # Or with a custom Redis client,
    throttle(key=key, rate=1, capacity=5, storage=BaseStorage(client=Redis(host='localhost', port=6379, db=220, password=None)), amount=3)


Algorithm
---------

throttle uses the "token bucket" algorithm: for each key, a virtual bucket
exists.

Whenever a new request gets in, the algorithm performs the following actions:

- Test if adding the request's cost to the bucket would exceed its capacity;
  in that case, return False
- Otherwise, add the request's cost to the bucket, and return True

Simultaneously, the bucket's current value is decremented at the chosen rate.


This allows for temporary bursts and average computations.


Installing
----------

From pip (https://pypi.python.org/pypi/lotus_eaters):

.. code-block:: sh

    $ pip install lotus_eaters


From Github:

.. code-block:: sh

    $ git clone git://github.com/mpetyx/lotus_eaters.git