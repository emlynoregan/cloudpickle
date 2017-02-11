# yccloudpickle
This repo is a fork of cloudpickle. It adds the Y-Combinator based technique for modifying recursive inner functions to make them serializable, detailed in these articles:

* [Serialising Functions in Python](https://medium.com/@emlynoregan/serialising-all-the-functions-in-python-cd880a63b591#.f7a367n0b)
* [Digging deeper into Recursive Inner Functions in Python](https://medium.com/@emlynoregan/digging-deeper-into-recursive-inner-functions-in-python-8ec6c5b1cbb#.ox04w1k7o)
* [Manually rewriting recursive inner functions to be serialisable in Python](https://medium.com/@emlynoregan/manually-rewriting-recursive-inner-functions-to-be-serialisable-in-python-83895792b176#.9g3ibfgi3)
* [Automatically Serialising Recursive Inner Functions in Python using the Y Combinator](https://medium.com/@emlynoregan/automatically-serialising-recursive-inner-functions-in-python-using-the-y-combinator-fc5d37e50b29#.of57pdbqb)

You can install it using 


    pip install cloudpickle

and use it like this:


    import yccloudpickle
    
    ser = yccloudpickle.dumps(lambda x: x+1)
    
ie: exactly like cloudpickle, but called yccloudpickle everywhere.

The rest of this readme is from the [source cloudpickle library](https://github.com/cloudpipe/cloudpickle).

# cloudpickle 

`cloudpickle` makes it possible to serialize Python constructs not supported
by the default `pickle` module from the Python standard library.

`cloudpickle` is especially useful for cluster computing where Python
expressions are shipped over the network to execute on remote hosts, possibly
close to the data.

Among other things, `cloudpickle` supports pickling for lambda expressions,
functions and classes defined interactively in the `__main__` module.


Installation
------------

The latest release of `cloudpickle` is available from
[pypi](https://pypi.python.org/pypi/cloudpickle):

    pip install cloudpickle


Examples
--------

Pickling a lambda expression:

```python
>>> import cloudpickle
>>> squared = lambda x: x ** 2
>>> pickled_lambda = cloudpickle.dumps(squared)

>>> import pickle
>>> new_squared = pickle.loads(pickled_lambda)
>>> new_squared(2)
4
```

Pickling a function interactively defined in a Python shell session
(in the `__main__` module):

```python
>>> CONSTANT = 42
>>> def my_function(data):
...    return data + CONSTANT
...
>>> pickled_function = cloudpickle.dumps(my_function)
>>> pickle.loads(pickled_function)(43)
85
```

Running the tests
-----------------

- With `tox`, to test run the tests for all the supported versions of
  Python and PyPy:

      pip install tox
      tox

  or alternatively for a specific environment:

      tox -e py27


- With `py.test` to only run the tests for your current version of
  Python:

      pip install -r dev-requirements.txt
      PYTHONPATH='.:tests' py.test


History
-------

`cloudpickle` was initially developed by [picloud.com](http://web.archive.org/web/20140721022102/http://blog.picloud.com/2013/11/17/picloud-has-joined-dropbox/) and shipped as part of
the client SDK.

A copy of `cloudpickle.py` was included as part of PySpark, the Python
interface to [Apache Spark](https://spark.apache.org/). Davies Liu, Josh
Rosen, Thom Neale and other Apache Spark developers improved it significantly,
most notably to add support for PyPy and Python 3.

The aim of the `cloudpickle` project is to make that work available to a wider
audience outside of the Spark ecosystem and to make it easier to improve it
further notably with the help of a dedicated non-regression test suite.
