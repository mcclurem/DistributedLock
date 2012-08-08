python-distributed-lock
=======================

Python distributed lock (currently only with memcached)

How to configure
------------------------

In your setup file (in django, settings.py), configure locking:

```python
import distributedlock
distributedlock.DEFAULT_MEMCACHED_CLIENT = memcache.Client(['127.0.0.1:11211'])
distributedlock.DEFAULT_TIMEOUT=60
distributedlock.DEFAULT_BLOCKING=False
```

If you setting up memcached in Django, you can use it abstraction of memcached.

```python
from django.core.cache import cache
distributedlock.DEFAULT_MEMCACHED_CLIENT = cache
```

You can configure this settings in each lock, as parameter.


How to use
------------------------

Using minimal configuration, as decorator:

```python
from distributedlock import syncronize
@syncronize()
def hello_world():
    print 'running'
```

Or as `with` block:

```python
from distributedlock import syncronize
... my code before
with syncronize('hello'):
    print 'running'
... my code after
```

You can use with conventional threading.Lock (only in process locking)

```python
from distributedlock import syncronize
import threading
with syncronize('hello', lock=threading.Lock())
    print 'running'
```

Arguments
------------------------

```python
def syncronize(key, lock=None, blocking=None)
```

  * key: name of key in memcached. Avoid long names, because memcached supports only 255 characters in key. Using decorator
  key name will be class name + method name if not specified.
  * lock: If you desire use another lock strategy, like `threading.Lock()` or `threading.RLock()`. defaults to `distributedlock.memcachedlock.MemcachedLock`
  * blocking: If another process has lock, wait until have a lock or abort immediately, raising `LockNotAcquiredError`. Defaults to `distributedlock.DEFAULT_BLOCKING`
  


