# Singleton Patterns / Metaclasses

In general, it makes sense to use a metaclass to implement a singleton. A singleton is special because its instance is created only once, 
and a metaclass is the way you customize the creation of a class, allowing it to behave differenly than a normal class. Using a metaclass
gives you more control in case you need to customize the singleton class definitions in other ways.

Your singletons won't need multiple inheritance (because the metaclass is not a base class), but for subclasses of the created class that
use multiple inheritance, you need to make sure the singleton class is the first / leftmost one with a metaclass that redefines __call__ This is very unlikely to be an issue.
The instance dict is not in the instance's namespace so it won't accidentally overwrite it.

If you define a metaclass but do not explicitly inherit from type, Python will fall back to the default base class for normal classes, which is object.
However, for metaclasses to work correctly, they must inherit from type.

If Singleton does not inherit from type, it won't function as a valid metaclass. Instead, it will just be a regular class inheriting from object.
This is why you explicitly inherit type when creating a metaclass.

__call__ => __new__ => __init__

```
class Singleton(type):
    _instances = {}
    def __call__(cls, *args, **kwargs):
        if cls not in cls._instances:
            cls._instances[cls] = super(Singleton, cls).__call__(*args, **kwargs)
        return cls._instances[cls]

class Logger(metaclass=Singleton):
  def log(self, msg):
    print(msg)
```
