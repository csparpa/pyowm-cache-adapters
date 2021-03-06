pyowm-cache-adapters
====================

A set of simple draft adapters allowing the PyOWM library to leverage external cache systems.
The code relies on the Python bindings for the adapted system.

Supported cache systems
-----------------------
* [Memcached](http://memcached.org/)
* [Redis](http://redis.io)
* [MongoDB](http://www.mongodb.org/)

More to come...

License
-------
[MIT](https://github.com/csparpa/pyowm-cache-adapters/blob/master/LICENSE) license

Installation
------------
1. Of course, you need to install the PyOWM library before :-) Use [pip](https://pypi.python.org/pypi/pip)
2. Install the Python bindings for the cache system/s you need to adapt:
    * [python-memcached](https://pypi.python.org/pypi/python-memcached)
    * [redis-py](https://pypi.python.org/pypi/redis/)
    * [pymongo](https://pypi.python.org/pypi/pymongo/)
3. Download the adapters package
4. Place it somewhere you can reach via your `PYTHONPATH` variable (the PyOWM installation folder is OK)

Wiring
------
There are two ways...

### The easy one
1. Wire the adapters into a configuration module that you setup and put in a location reachable from your `PYTHONPATH`. Eg: say your config file is `mypyowmconfig/myconfig.py` and `mypyowmconfig` is somewhere in your `PYTHONPATH`:
    ```
    # Content of your own PyOWM config module
    from pyowm-cache-adapters import redis_adapter  # This is where you installed the adapters
    cache = nullcache.RedisAdapter()
    # [...] (other config values)
    ```
    
2. Instantiate the library injecting a reference to your own config module. Eg:

   ```
   import pyowm
   owm = pyowm.OWM(None, None, 'mypyowmconfig.myconfig')  # This loads the config values from your module
   ```

   More about PyOWM config module on the [PyOWM](https://github.com/csparpa/pyowm) wiki.

### The difficult one

1. Find the `configurationXX.py` file which is specific for the web API version you are currently using. This will likely be in `/usr/local/lib/python2.6/dist-packages/pyowm/webapi25/config25.py` for Unix-like envs or in `C:\Python27\Lib\site-packages\pyowm\webapi25\config25.py` for Windows envs.

2. Open the file and spot this line:
    ```python
    # Cache provider to be used
    cache = NullCache()
    ```
   and change it accordingly to the adapter you need. In example, for Memcached:
   ```python
   # Cache provider to be used
   # cache = NullCache()  # <--- comment/remove this line
   from pyowm_cache_adapter.memcached_adapter import MemcachedAdapter
   cache = MemcachedAdapter( [... parameters ...] )
   ```
   (be careful not to miss the `import` statement)

Modifying
---------
The provided adapters are just intended to be drafts: modify them accordingly
to your application-specific needs.

There's only one thing that you must bear in mind: each adapter must subclass the
the `pyowm.abstractions.owmcache.OWMCache` abstract class for it to work into the PyOWM library.

Bugs
----
Please signal any bugs you may encounter.
