## Descripter

要使用 descriptor，descriptor 需儲存為 class variable 在別的 class 中:

{% highlight Python %}
class Ten:
    def __get__(self, obj, objtype=None):
        return 10

class A:
    x = 5                   # 一般的 class attribute
    y = Ten()               # Descriptor 實例


a = A()                     # 建立類別 A 的實例
a.x                         # 一般的 attribute lookup
a.y                         # Descriptor lookup

{% endhighlight %}

{:note}
在尋訪 a.x attribute 時，點運算子會在 class dictionary 找到 'x': 5。在尋訪 a.y 時，點運算子會藉由識別其 __get__ method 找到 descriptor 實例，recognized by its  method. 呼叫該 method 會回傳 10。注意 10 不是存在 class dictionary 或是 instance dictionary，而是呼叫時即時產生。

{:note}
descriptors 一個常見的用途是針對 instance data 做存取管理(managing access)。descriptor 會指派到 class dictionary 中的一個 public attribute，然而實際上的資料是儲存在 instance dictionary 的一個 private attribute。當 public attribute 被存取時，descriptor 的 __get__() 與 __set__() method 會被觸發

{% highlight Python %}
import logging

logging.basicConfig(level=logging.INFO)

class LoggedAgeAccess:
    
    def __init__(self):
        print('LoggedAgeAccess __init__')
    
    def __get__(self, obj, objtype=None):
        print('LoggedAgeAccess __get__')
        value = obj._age
        logging.warning('Accessing %r giving %r', 'age', value)
        return value

    def __set__(self, obj, value):
        print('LoggedAgeAccess __set__')
        logging.warning('Updating %r to %r', 'age', value)
        obj._age = value

class Person:

    age = LoggedAgeAccess()             # Descriptor instance

    def __init__(self, name, age):
        print('Person __init__')
        self.name = name                # Regular instance attribute        
        self.age = age                  # Calls __set__()

    def birthday(self):
        self.age += 1                   # Calls both __get__() and __set__()


mary = Person('Mary M', 30) 
# LoggedAgeAccess __init__
# Person __init__
# LoggedAgeAccess __set__ 
# INFO:root:Updating 'age' to 30
{% endhighlight %}

這裡因為呼叫 self.age = age 會觸發 __set__，因而顯示 LoggedAgeAccess __set__

{% highlight Python %}
vars(mary)  
# {'name': 'Mary M', '_age': 30}
{% endhighlight %}

注意這裡，實際上的資料是存在 _age 這個 private attribute 中。vars() 會回傳 __dict__ attribute，所以等同於呼叫 mary.__dict__

以下是 intance dictionary
{% highlight Python %}
mary.__dict__
# {'name': 'Mary M', '_age': 30}
{% endhighlight %}

以下是 class dictionary
{% highlight Python %}
Person.__dict__ 
# {..., 
	'age': <__main__.LoggedAgeAccess object at 0x000002AD8009F820>, 
	'ten': 10, 
	'__init__': <function Person.__init__ at 0x000002AD8030D2D0>, 
	'birthday': <function Person.birthday at 0x000002AD8030D360>, 
	'__dict__': <attribute '__dict__' of 'Person' objects>, 
	...}

{% endhighlight %}

{:note}
問題來了，為什麼 __dict__ 裡面沒有 age ? 

根據定義
object.__dict__ : A dictionary or other mapping object used to store an object's (writable) attributes，我推測這裡的 writable attribute 指的是 instance attribute，因為這裡的 age 是 class attribute，所以 mary.__dict__ 沒有 age

{% highlight Python %}
dir(mary)
# [..., '_age', 'age', 'birthday', 'name', 'ten' ...]
{% endhighlight %}

## object.__set_name__

object.__set_name__(self, owner, name)
Automatically called at the time the owning class owner is created. The object has been assigned to name in that class。在 owner 這個 class 被建立時自動呼叫，在該 class 中 object 被指派為 name

class A:
    x = C()  # 自動呼叫: x.__set_name__(A, 'x')
	
If the class variable is assigned after the class is created, __set_name__() will not be called automatically. If needed, __set_name__() can be called directly:

class A:
   pass

c = C()
A.x = c                  # The hook is not called
c.__set_name__(A, 'x')   # Manually invoke the hook


###################

# 上述範例有一個是 private name _age 是寫死在 LoggedAgeAccess 類別中。這表示每個 instance 只能有一個 logged attribute 而且 name 是固定的，我們可以用以下寫法來修正
    
class LoggedAccess:
	
    def __init__(self):
        print('LoggedAgeAccess init')
	
    def __set_name__(self, owner, name):
        print('__set_name__ called', owner, name)
        self.public_name = name
        self.private_name = '_' + name

    def __get__(self, obj, objtype=None):
        print('vars(self)', vars(self))
        value = getattr(obj, self.private_name) # 假設 self.private_name == "foo"，等同 obj.foo
        logging.warning('Accessing %r giving %r', self.public_name, value)
        return value

    def __set__(self, obj, value):
        logging.warning('Updating %r to %r', self.public_name, value)
        setattr(obj, self.private_name, value)

class Person:
    
    name = LoggedAccess()                # First descriptor instance
    
    age = LoggedAccess()                 # Second descriptor instance

    def __init__(self, name, age):
        print('*** __init__ ***')
        self.name = name                 # Calls the first descriptor
        self.age = age                   # Calls the second descriptor

    def birthday(self):
        self.age += 1
        
    def elaborate(self):
        print(self.name)

# 建立類別 Person 時，會呼叫 __set_name__         
# __set_name__ called <class '__main__.Person'> name
# __set_name__ called <class '__main__.Person'> age        

# 我們可以看到 class dictionary 中的 name 有 public_name, private_name
print(type(Person.__dict__['name'])) # <class '__main__.LoggedAccess'>           
print(vars(Person.__dict__['name'])) # {'public_name': 'name', 'private_name': '_name'}		
print(vars(vars(Person)['name']))    # {'public_name': 'name', 'private_name': '_name'}

# 我們可以看到 class dictionary 中的 age 有 public_name, private_name
print(vars(Person.__dict__['age'])) # {'public_name': 'age', 'private_name': '_age'}

pete = Person('Peter P', 10)
# WARNING:root:Updating 'name' to 'Peter P'
# WARNING:root:Updating 'age' to 10

kate = Person('Catherine C', 20)
# WARNING:root:Updating 'name' to 'Catherine C'
# WARNING:root:Updating 'age' to 20

# instance 只會包含 private 的 names
print(vars(pete)) # {'_name': 'Peter P', '_age': 10}

pete.name = 'Gunn'

print(pete.__dict__['']) # {'_name': 'Peter P', '_age': 10}
print(pete._name)   # Peter P
print(pete.name)    # Peter P
# WARNING:root:Accessing 'name' giving 'Peter P' 
# Peter P
print(vars(Person))

pete.elaborate()


# mary = Person('Mary M', 30)
# lisa = Person('Lisa L', 20)
# print(mary.__dict__)
# print(lisa.__dict__)
# print(Person.__dict__)

# print(dir(mary))



###################




descriptor 的協議如下

__get__(self, obj, type=None) -> object
__set__(self, obj, value) -> None
__delete__(self, obj) -> None
__set_name__(self, owner, name)

只實作 __get__ 稱為 non-data descriptor，實作 __set__() 或 __delete__() 稱為 data descriptor

{% highlight Python %}
# descriptors.py
class Verbose_attribute():
    def __get__(self, obj, type=None) -> object:
        print("accessing the attribute to get the value")
        return 42
    def __set__(self, obj, value) -> None:
        print("accessing the attribute to set the value")
        raise AttributeError("Cannot change the value")

class Foo():
    attribute1 = Verbose_attribute()

my_foo_object = Foo()
x = my_foo_object.attribute1
print(x)
{% endhighlight %}

## descriptor 與 property 的關係

property 實際上就是 descriptor

用 property 可以寫成下面
{% highlight Python %}
 # property_decorator.py
class Foo():
    @property
    def attribute1(self) -> object:
        print("accessing the attribute to get the value")
        return 42

    @attribute1.setter
    def attribute1(self, value) -> None:
        print("accessing the attribute to set the value")
        raise AttributeError("Cannot change the value")
{% endhighlight %}

等同於

{% highlight Python %}
# property_function.py
class Foo():
    def getter(self) -> object:
        print("accessing the attribute to get the value")
        return 42

    def setter(self, value) -> None:
        print("accessing the attribute to set the value")
        raise AttributeError("Cannot change the value")

    attribute1 = property(getter, setter)
{% endhighlight %}

property() 回傳一個實作 descriptor protocol 的 property object。

## Methods 與 Functions 的 Python Descriptors 

背後將呼叫 obj.method(*args) 變成 method(obj, *args) 是在 function object 的 .__get__() 實作中，這也是一個 non-data descriptor。特別來說，當 function object 實作 .__get__() 當我們使用 dot notation 存取時會回傳 bound method。

{% highlight Python %}
import types

class Function(object):
    ...
    def __get__(self, obj, objtype=None):
        "Simulate func_descr_get() in Objects/funcobject.c"
        if obj is None:
            return self
        return types.MethodType(self, obj)
{% endhighlight %}
