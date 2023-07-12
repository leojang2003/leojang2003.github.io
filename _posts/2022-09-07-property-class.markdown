{% highlight Python %}
{% endhighlight %} 

以下是 property() 如何以 descriptor protocol 的方式實作，我們知道 property() 是個 class 但是當成 function 使用

{% highlight Python %}
class Property:
    "Emulate PyProperty_Type() in Objects/descrobject.c"

    def __init__(self, fget=None, fset=None, fdel=None, doc=None):
        print('__init__', fget)
        self.fget = fget
        self.fset = fset
        self.fdel = fdel
        if doc is None and fget is not None:
            doc = fget.__doc__
        self.__doc__ = doc
        self._name = ''

    def __set_name__(self, owner, name):
        print('__set_name__', name)
        self._name = name

    def __get__(self, obj, objtype=None):
        print('__get__')
        if obj is None:
            return self
        if self.fget is None:
            raise AttributeError(f'unreadable attribute {self._name}')
        return self.fget(obj)

    def __set__(self, obj, value):
        print('__set__')
        if self.fset is None:
            raise AttributeError(f"can't set attribute {self._name}")
        self.fset(obj, value)

    def __delete__(self, obj):
        if self.fdel is None:
            raise AttributeError(f"can't delete attribute {self._name}")
        self.fdel(obj)
    
    def getter(self, fget):
        print('getter called')
        prop = type(self)(fget, self.fset, self.fdel, self.__doc__)
        prop._name = self._name
        return prop

    def setter(self, fset):
        print('setter called')
        prop = type(self)(self.fget, fset, self.fdel, self.__doc__)
        prop._name = self._name
        return prop

    def deleter(self, fdel):
        print('deleter called')
        prop = type(self)(self.fget, self.fset, fdel, self.__doc__)
        prop._name = self._name
        return prop
{% endhighlight %} 

## __set_name__(self, owner, name) 方法
當 class 建立時，type.__new__() 會掃描 class variables，並呼叫所有 __set_name__() 的 callback

class C:
    
    def __set_name__(self, owner, name):
        print('C __set_name__ called', owner, name)

class A:
    x = C() # 自動呼叫 : x.__set_name__(A, 'x')
  
# C __set_name__ called <class '__main__.A'> x

{% highlight Python %}
import pyproperty
import inspect

class Circle:
    def __init__(self, radius):
        print('Circle __init__', fget)
        self._radius = radius

    @pyproperty.Property
    def the_rock(self):
        """The radius property."""
        print("Get radius")
        return self._radius
  
    print('---------------')
    print(vars())
    
    # the_rock = pyproperty.Property(the_rock)
    
    # @radius.setter
    # def radius(self, value):
        # print("Set radius")
        # self._radius = value

    # @radius.deleter
    # def radius(self):
        # print("Delete radius")
        # del self._radius
    
    #print(vars()) 
    
#print(inspect.getmembers(Circle))
   
# c2 = Circle(607.0)
# print('radius =' ,c2.radius)
# c2.radius = 303
# print('radius =' ,c2.radius)
# del c2.radius
{% endhighlight %} 

參考來源
https://docs.python.org/3/howto/descriptor.html#properties
