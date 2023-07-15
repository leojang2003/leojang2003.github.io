# __qualname__

The qualified name of the class, function, method, descriptor, or generator instance.
New in version 3.3.

```python
import builtins

author = 'Mike'

def test_func():
    pass

class A:
    
    jula = 'Lisa'

    def __init__(self):
        pass

    def a_run(self, distance):           
        self.shoes = 'Nike'
        shirt = 'Adidas'
        print(author)   # Mike James
        print(A.author) # Lisa Holmes
        #print(a_run.__qualname__)
        print(distance)
        #print(globals().keys())
        print(locals().keys())


tom = A()
print(__name__)                 # __main__
print(__qualname__)             # name '__qualname__' is not defined
print(test_func.__name__)       # test_func
print(test_func.__qualname__)   # test_func
print(tom.a_run.__name__)       # a_run
print(tom.a_run.__qualname__)   # A.a_run
```


