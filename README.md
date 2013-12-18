pyowm-cache-adapters
====================

A set of simple Python adapters allowing the PyOWM library to leverage external cache systems.

Supported cache systems
-----------------------
* [Memcached](http://memcached.org/)
* [Redis](http://redis.io)
* [MongoDB](http://www.mongodb.org/)

More to come...

License
-------
[MIT](https://github.com/csparpa/pyowm/blob/master/LICENSE) license

Adapters installation
---------------------
Of course, install the PyOWM library before :)
Do it with [pip](https://pypi.python.org/pypi/pip)

1. Download the adapters you wish to plug into PyOWM
2. Place them into a folder reachable via your `PYTHONPATH` variable (the PyOWM installation folder is OK)

Adapters wiring
---------------
1. Find the `configurationXX.py` file which is specific for the web API version you are currently using. This will likely be in `/usr/local/lib/python2.6/dist-packages/pyowm/webapi25/config25.py` for Unix-like envs or in `C:\Python27\Lib\site-packages\pyowm\webapi25\config25.py` for Windows envs.

2. Open the file and spot this line:

```python
# Cache provider to be used
cache = NullCache()
```

and change it accordingly to the adapter you need, eg: Memcached adapter:

```python
# Cache provider to be used
# cache = NullCache()  # <--- comment/remove this line
from pyowm_memcached_adapter import MemcachedAdapter
cache = MemcachedAdapter( [... parameters ...] )
```

(be careful not to miss the `import` statement)
