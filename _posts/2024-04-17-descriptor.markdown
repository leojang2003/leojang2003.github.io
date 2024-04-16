---
layout: post
title: Descriptor
subtitle: 
tags: [property, descriptor, first-class object, data descriptor, non-data descriptor]
comments: true
---

可參考 <a href="../2024-04-17-class-instance-attribute/">類別與物件的屬性</a>
可參考 <a href="../2024-04-17-dot-operator/">點運算子</a>

### object.\_\_get__ ( self , owner , ownerType = None )

首先 \_\_get__ 方法官網定義如下

> Called to get the attribute of the owner class (class attribute access) or of an instance of that class (instance attribute access). The optional owner argument is the owner class, while instance is the instance that the attribute was accessed through, or None when the attribute is accessed through the owner.

透過類別或物件去存取 class attribute 時，都會觸發 class attribute 的類別的 \_\_get__ 方法，須注意不是所以有類別都有實作 \_\_get__。

<br/>

#### 透過 class 存取 class attribute

舉例來說下面的類別 A，類別 A 有一個 class attribute ( 定義在 class 層級的 attribute ) x 與 y。若是呼叫 A.y，傳入 \_\_get__ ( ) 的 owner 會是 None，而 ownerType 會是 A。若是呼叫 A.x，因為 int 物件沒有 \_\_get__ ( ) 方法，所以不會觸發 \_\_get__ ( )。

```python
class Ten:

    def __get__(self, owner, ownerType=None):                
        return 10
        
class A:

    x = 5       # 一般的類別屬性
    y = Ten()   # descriptor 物件
        
    def __getattribute__(self, name):
        print('A.__getattribute__')
        return object.__getattribute__(self, name)

A.y
# self =  <__main__.Ten object at ...>
# owner =  None , 
# ownerType =  <class '__main__.A'>

A.x
# 沒有任何輸出，不會呼叫 __getattribute__
```
{:.note}
從上面可以看出，透過類別不論存取的屬性是否為 descriptor，都不會觸發類別的 \_\_getattribute__ ( )，實際上被呼叫的是 <b>type.\_\_getattribute__ ( )</b>

<br/>

#### 透過 object 存取 class attribute

若是透過物件存取 class attribute，傳入 \_\_get__ ( ) 方法的 owner 會是 A 物件，而 ownerType 同樣會是 A。

```python
a = A

a.y
# self =  <__main__.Ten object at ...>
# owner =  <__main__.A object at ...>
# ownerType =  <class '__main__.A'>

a.x
# A.__getattribute__
```

{:.note}
a.y 會試著先從 instance dictionary 中找出 y ( a.\_\_dict__ [ 'y' ] )，如果沒有的話則從 class dictionary 尋找是否有 y ( type ( a ).\_\_dict__ [ 'y' ] )。以這裡的範例而言，雖然也叫做 instance attribute access，但實際上是透過<b>物件</b>存取的是 class attribute

<br/>

#### 存取 instance attribute 不會觸發 \_\_get__ ( )

如果是加到 instance dictionary 的話，則呼叫 instance attribute 時不會自動呼 叫 \_\_get__ ( ) 方法。下面我們新增一個 Ten 物件，加入 a 的 instance dictionary，但存取這個 instance attribute 不會觸發 \_\_get__ ( )。

```python
t = Ten()

a.t = t

a.__dict__
# A.__getattribute__
# {'t': <__main__.Ten object at 0x000001D52BB9BB20>}

a.t
# A.__getattribute__
# <__main__.Ten object at 0x000001D52BB9BB20>

a.t.__get__(A)
# A.__getattribute__
# 10
```

{:.note}
從上述可以知道，只有呼叫 class attribute 時才會自動觸發 \_\_get__

<br/>

### 一個簡單只固定回傳值的 descriptor

回來正題，到底什麼是 descriptor ? 簡單來說，descriptor 物件必須以 <b>class attribute</b> 的方式儲存在類別中。

```python
class Ten:
    def __get__(self, owner, ownerType=None):
        return 10

class A:
    x = 5                   # 一般的類別屬性
    y = Ten()               # descriptor 實例


a = A()                     # 建立類別 A 的實例
a.x                         # 存取一般的實例屬性
a.y                         # 存取 descriptor 
```

存取 a.x 時，點運算子透過 \_\_getattribute__ ( ) 在 class dictionary 中找到 'x' : 5。存取  a.y 時，點運算子會藉由識別其 \_\_get__ ( ) 方法找到 descriptor 物件，呼叫該方法會回傳 10。注意 10 不是存在 class dictionary 或是 instance dictionary 中，而是呼叫時即時產生。通常如果是常數的話是不需要使用 descriptor 的，在這裡僅做為示範使用。

<br/>

### Managed Attribute

descriptor 一個常見的用途是針對 instance data 做存取管理 ( managing access )。descriptor 會指派到 class dictionary 中的一個 <b>public</b> attribute，然而實際上的資料是儲存在 instance dictionary 中的一個 <b>private</b> attribute。當 public attribute 被存取時，descriptor 的 \_\_get__ ( ) 與 \_\_set__ ( ) 方法會被觸發存取 private attribute。

```python
import logging

logging.basicConfig(level=logging.INFO)

class Log:
    
    def __init__(self):
        pass
    
    def __get__(self, obj, objtype=None):
        print('Log __get__')
        value = obj._age
        logging.warning('Accessing %r giving %r', 'age', value)
        return value

    def __set__(self, obj, value):
        print('Log __set__')
        logging.warning('Updating %r to %r', 'age', value)
        obj._age = value

class Person:
	
    age = Log()

	type(age) # <class '__main__.LoggedAgeAccess'>

    def __init__(self, name, age):
        print('Person __init__')
        self.name = name                # 一般實例屬性        
        self.age = age                  # 呼叫 __set__()

    def birthday(self):
        self.age += 1                   # 同時呼叫 __get__() 與 __set__()


mary = Person('Mary M', 30) 
# Log __set__ 
# INFO:root:Updating 'age' to 30
```
Log 是個 descriptor 類別，我們在 Person 類別有個名為 age 的 public class attribute，是個 descriptor 實例，當載入 Person 類別時，首先會初始化 age，因此會呼叫 Log 的 \_\_init__ ( ) 方法。當建立 Person 物件時，會呼叫 Person 的 \_\_init__ ( ) 方法，指派 name 到 instance dictionary，並將 30 指派到 self.age 這個 descriptor 物件。

<br/>

### 點運算子的存取優先順序

注意這行 self.age = age，初次看會覺得奇怪，為什麼不是在 instance dictionary 新增名為 age 的 instance attribute ? 

關鍵在於這個點運算子，我們知道如果是<b>透過物件呼叫點運算子，則會無條件呼叫物件的 \_\_getattribute__  ( ) 方法</b>，這個方法會依照以下順序走訪，data descriptor > instance dictionary > Non-data descriptor > class dictionary > 最後都找不到則拋出 AttributeError 並呼叫 \_\_getattr__ ( )。

{:.note}
Non-data descriptor 一般用在 method。只實作 \_\_get__ ( ) 方法的是 Non-data descriptor。

<br/>

### 詳細檢視 \_\_getattribute__

Python 版本的 object_getattribute ( ) 相當於以下程式 ...

```python
def find_name_in_mro(cls, name, default):

    "模擬 Objects/typeobject.c 的 _PyType_Lookup()"

    for base in cls.__mro__:
        if name in vars(base):
            return vars(base)[name]
    return default

def object_getattribute(obj, name):

    "模擬 PyObject_GenericGetAttr() in Objects/object.c"

    null = object()
    objtype = type(obj)
    cls_var = find_name_in_mro(objtype, name, null)
    descr_get = getattr(type(cls_var), '__get__', null)

    if descr_get is not null:
        if (hasattr(type(cls_var), '__set__')
            or hasattr(type(cls_var), '__delete__')):
            return descr_get(cls_var, obj, objtype)     # data descriptor

    if hasattr(obj, '__dict__') and name in vars(obj):
        return vars(obj)[name]                          # instance variable

    if descr_get is not null:
        return descr_get(cls_var, obj, objtype)         # non-data descriptor

    if cls_var is not null:
        return cls_var                                  # class variable

    raise AttributeError(name)
```

注意到 \_\_getattribute__ ( ) 方法沒有呼叫 \_\_getattr__ ( ) 方法，直接呼叫 \_\_getattribute__ ( ) 或是 super ( ).\_\_getattribute__ 會完全跳過 \_\_getattr__ ( )，實際上是點運子與 <b>getattr ( )</b> 方法負責呼叫 \_\_getattr__ ( ) 當 \_\_getattribute__ ( ) 拋出 AttributeError 時。

{:.note}
舉例來說 getattr ( x , 'foobar' ) 等同於 x.foobar

```python
def getattr_hook(obj, name):

    "模擬 Objects/typeobject.c 的 slot_tp_getattr_hook()"

    try:
        return obj.__getattribute__(name)
    except AttributeError:
        if not hasattr(type(obj), '__getattr__'):
            raise
    return type(obj).__getattr__(obj, name)             # __getattr__
```

回到我們的例子，self.age 開始依序找尋，首先有找到名為 age 的 data descriptor，在程式試圖要指派值給 descriptor 時，會呼叫 data descriptor 的 \_\_set__ ( ) 方法，\_\_set__ ( ) 會在 Person 物件新增一個名為 _age 的 instance attribute。

透過 descriptor，我們將實際的值存放在物件的 _age 下，但對外僅提供 age 這個 public attribute，這就是所謂的 managed attribute。目前我們的 _age 屬性是寫死的，後續會作調整。

如果 a.x 找到一個 descriptor，則是會呼叫 desc.\_\_get__ ( a , type ( a ) )。

```python
vars(mary)  
# {'name': 'Mary M', '_age': 30}
```

注意這裡，實際上的資料是存在 _age 這個 private attribute 中。vars ( ) 會回傳 \_\_dict__ 屬性，所以等同於呼叫 mary.\_\_dict__，以下是 instance dictionary。

```python
mary.__dict__
# {'name': 'Mary M', '_age': 30}
```

以下是 class dictionary

```python
Person.__dict__ 
# {..., 
	'age': <__main__.Log object at 0x000002AD8009F820>, 
	'ten': 10, 
	'__init__': <function Person.__init__ at 0x000002AD8030D2D0>, 
	'birthday': <function Person.birthday at 0x000002AD8030D360>, 
	'__dict__': <attribute '__dict__' of 'Person' objects>, 
	...}
```

總結一下，instance dictionary 裡面只有 name 和 _age，沒有 age，而 class dictionary 裡有 age。

```python
dir(mary)
# [..., '_age', 'age', 'birthday', 'name', 'ten' ...]
```
<br/>

### object.\_\_set_name__ ( self , owner , name )

剛剛提到我們在 descriptor 的 \_\_set__ ( ) 方法中寫死 _age 變數是有問題的，這樣我們無法新增其它的 managed attribute。要改善這個問題前，首先來認識 \_\_set_name__ ( )，官網定義如下 :

> When a class is created, type.\_\_new__() scans the class variables and makes callbacks to those with a \_\_set_name__() hook. Automatically called at the time the owning class owner is created. The object has been assigned to name in that class。

當類別被建立( interpreted )時，type.\_\_new__ ( ) 會檢視類別的所有 <b>class attribute</b>，若 class attribute 的類別有定義 \_\_set_name__ ( ) 的話，則自動執行他們的 \_\_set_name__ ( ) ，據測試 \_\_set_name__ ( ) 會在 owner 類別主體載入完之後才執行，而不是 descriptor 物件被建立時就執行。

```python
class A:
    x = C() 
# ''類別 A 載入後'''，自動呼叫 x.__set_name__(A, 'x')
```

在 owner 這個類別被建立時自動呼叫 \_\_set_name__ ( ) 方法，這裡的建立指的是 owner 類別定義被載入後，不是 descriptor 類別被初始化後。object 會被指派 name。

```python
class C:
    pass

class A:
    x = C()  # C 沒有定義 __set_name__，沒執行 __set_name__	
```

下面當類別有定義 \_\_set_name__ ( )，但方法內容為空，可以看到沒有指派 name，我們可以得知雖然 \_\_set_name__ ( ) 會自動被呼叫，但指派名稱這件事必須手動在 \_\_set_name__ ( ) 中實作。

```python
class C:
    def __set_name__(self, owner, name):
        pass

class A:
    x = C()  
	print(c.__dict__) # {}

# 自動呼叫: x.__set_name__(A, 'x')

A.c.__dict__  # {}
```

我們必須明確定義 \_\_set_name__ ( ) 方法的內容，下面是最簡單的例子

```python
class C:
    def __set_name__(self, owner, name):
        self.name = name

class A:
    x = C()  
    y = C()

	x.__dict__ # {} 這時候還沒執行 __set_name__
    y.__dict__ # {} 這時候還沒執行 __set_name__

# 自動呼叫: x.__set_name__(A, 'x')
# 自動呼叫: y.__set_name__(A, 'y')

A.x.__dict__  # {'name': 'x'}
A.y.__dict__  # {'name': 'y'}
```

如果在類別建立後才指派到類別變數，則 \_\_set_name__ ( ) 不會自動呼叫，如需要需直接呼叫。

```python
class A:
   pass

c = C()
A.x = c                  # 此時不會呼叫 __set_name__
c.__set_name__(A, 'x')   # 需手動呼叫 __set_name__
```

<br/>

### 使用 \_\_set_name__ ( ) 改寫範例

回到剛剛的問題，一開始範例有一個是 private 名稱 _age 是寫死在 Log 類別中。這表示每個實例只能有一個 logged attribute 而且 name 是固定的，我們可以用以下寫法來修正，新增 \_\_set_name__ ( ) 並使用 <b>getattr ( )</b> 改寫 \_\_set__ 與 \_\_get__ 。
    
```python
class LoggedAccess:
	
    def __init__(self):
        print('Log init')
	
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
    
    name = LoggedAccess()                # 第一個 descriptor 實例
    
    age = LoggedAccess()                 # 第二個 descriptor 實例

    def __init__(self, name, age):
        print('*** __init__ ***')
        self.name = name                 # 呼叫第一個 descriptor
        self.age = age                   # 呼叫第二個 descriptor

    def birthday(self):
        self.age += 1
    
# 建立類別 Person 時，會呼叫 __set_name__         
# __set_name__ called <class '__main__.Person'> name
# __set_name__ called <class '__main__.Person'> age        

vars(Person.__dict__['name']) 
# {'public_name': 'name', 'private_name': '_name'}	
# 我們可以看到 class dictionary 中的 name 有 public_name, private_name

vars(Person.__dict__['age'])
# 我們可以看到 class dictionary 中的 age 有 public_name, private_name
# {'public_name': 'age', 'private_name': '_age'}
```

如果我們初始化 Person 類別，self.name = name 會呼叫第一個 descriptor 物件的 \_\_set__ ( ) 方法，因為 self.private_name 是 _name，所以 setattr ( obj , self.private_name , value ) 等同於執行 setattr ( obj, '_name', 'Peter P' )。

同樣的 self.age = age 會呼叫第二個 descriptor 物件的 \_\_set__ ( ) 方法，因為 self.private_name 是  _age ，所以 setattr ( obj , self.private_name , value ) 等同於執行 setattr ( obj , '_age' , 10 )。

```python
pete = Person('Peter P', 10)
# WARNING:root:Updating 'name' to 'Peter P'
# WARNING:root:Updating 'age' to 10

kate = Person('Catherine C', 20)
# WARNING:root:Updating 'name' to 'Catherine C'
# WARNING:root:Updating 'age' to 20

# 確認實例只會包含私有的名稱
vars(pete) # {'_name': 'Peter P', '_age': 10}

pete.name    
# WARNING:root:Accessing 'name' giving 'Peter P' 
# Peter P
```

<br/>

### Descriptor 協議

任何定義了 <b>\_\_get__ ( )</b>、<b>\_\_set__ ( )</b> 或 <b>\_\_delete__ ( )</b> 方法的物件就是 descriptor。descriptor 也可以有 \_\_set_name__ ( ) 方法，但就算該類別不是 descriptor，\_\_set_name__ ( ) 還是會被自動呼叫。我們整理 descriptor 的協議如下，

\_\_get__ ( self , obj , type = None ) -> object <br class="new">
\_\_set__ ( self , obj , value ) -> None <br class="new">
\_\_delete__ ( self , obj ) -> None <br class="new">
\_\_set_name__ ( self , owner , name ) <br class="new">

只實作 \_\_get__ 稱為 <b>Non-data descriptor</b>，另外實作 \_\_set__ ( ) 與/或 \_\_delete__ ( ) 稱為 <b>Data descriptor<b/>。<br class="new">

如果使用  vars ( some_class ) [ descriptor_name ] 的語法，僅會回傳 descriptor 物件，不會執行。<b>descriptor 只能在作為 class attribute 時有用，作為 instance attribute 時沒用。</b>

<br/>

### 為什麼需要 descriptor

descriptor 主要是在提供，當透過物件存取 attribute 時，提供一個 hook 去允許儲存在 class attribute 中的物件 ( descriptor 物件 )，控制值的存取。

> The main motivation for descriptors is to provide a hook allowing objects stored in class variables to control what happens during attribute lookup.

一般來說，owner 類別，也就是呼叫類別通常會決定屬性 lookup 時該怎麼做。descriptor 反轉了控制的走向，讓被呼叫的屬性自己決定怎麼做。

> Traditionally, the calling class controls what happens during lookup. Descriptors invert that relationship and allow the data being looked-up to have a say in the matter.

descriptor 在 Python 中視無所不在，包含 function 怎麼變成 method，另外 classmethod ( )、staticmethod ( )、property ( )、functools.cached_property ( ) 都是 descriptor 的應用。

<br/>

### descriptor 的範例

現在我們建立一個實作 descriptor 協議的 ABC Validator再讓

```python
# 建立一個 ABC 名為 Validator 的類別

from abc import ABC, abstractmethod

class Validator(ABC):

    def __set_name__(self, owner, name):
        self.private_name = '_' + name

    def __get__(self, obj, objtype=None):
        return getattr(obj, self.private_name)

    def __set__(self, obj, value):
        self.validate(value) # 設定值之前先呼叫驗證
        setattr(obj, self.private_name, value)

    @abstractmethod
    def validate(self, value):
        pass
```

在 \_\_set__ ( ) 方法設定值之前，先呼叫 validate ( ) 來驗證值。

```Python
class OneOf(Validator):

    def __init__(self, *options):
        self.options = set(options)

    def validate(self, value):
        if value not in self.options:
            raise ValueError(f'Expected {value!r} to be one of {self.options!r}')
            # !r 表示為 repr()
```

!r 表示為 repr()

```Python
class Number(Validator):

    def __init__(self, minvalue=None, maxvalue=None):
        self.minvalue = minvalue
        self.maxvalue = maxvalue

    def validate(self, value):
        if not isinstance(value, (int, float)):
            raise TypeError(f'Expected {value!r} to be an int or float')
        if self.minvalue is not None and value < self.minvalue:
            raise ValueError(
                f'Expected {value!r} to be at least {self.minvalue!r}'
            )
        if self.maxvalue is not None and value > self.maxvalue:
            raise ValueError(
                f'Expected {value!r} to be no more than {self.maxvalue!r}'
            )
```

```Python
class String(Validator):

    def __init__(self, minsize=None, maxsize=None, predicate=None):
        self.minsize = minsize
        self.maxsize = maxsize
        self.predicate = predicate

    def validate(self, value):
        if not isinstance(value, str):
            raise TypeError(f'Expected {value!r} to be an str')
        if self.minsize is not None and len(value) < self.minsize:
            raise ValueError(
                f'Expected {value!r} to be no smaller than {self.minsize!r}'
            )
        if self.maxsize is not None and len(value) > self.maxsize:
            raise ValueError(
                f'Expected {value!r} to be no bigger than {self.maxsize!r}'
            )
        if self.predicate is not None and not self.predicate(value):
            raise ValueError(
                f'Expected {self.predicate} to be true for {value!r}'
            )

# name, kind, quantity 都是 descriptor, 因為這些類別都繼承 descriptor 類別 Validator
class Component:
    
    # str.isupper 為檢查是否都是大寫
    name = String(minsize=3, maxsize=10, predicate=str.isupper)
    kind = OneOf('wood', 'metal', 'plastic')
    quantity = Number(minvalue=0)

    def __init__(self, name, kind, quantity):
        self.name = name
        self.kind = kind
        self.quantity = quantity

tom = Component('TOM', 'wood', 20)
```
<br/>

### 整理呼叫 descriptor 的方式

以下我們整理各種呼叫 descriptor 的方式 : 

<br/>

#### 從實例呼叫 descriptor

如果 a.x 時，呼叫 object.\_\_getattribute__ ( ) 發現 x 是 descriptor，則使用 desc.\_\_get__ ( a , type ( a ) ) 方式呼叫。

<br/>

#### 從類別呼叫 descriptor

使用 A.x 會呼叫 type.\_\_getattribute__ ( )。步驟與 object.\_\_getattribute__ ( ) 類似，但從 instance dictionary 尋找被取代為尋找 class 的 method resolution order。

如果有找到 descriptor，則呼叫 desc.\_\_get__ ( None , A )。

<br/>

#### 從 super ( ) 呼叫 descriptor

super 的點運算子 lookup 邏輯看的是 super ( ) 回傳的物件的 \_\_getattribute__ ( )。
關於 super ( ) 請參考 <a href="../2022-08-30-inheritance/">繼承</a>

<br/>

### descriptor 重點記憶

- descriptor 的機制嵌入在 object、type、super ( ) 的 \_\_getattribute__ ( ) 方法中。

- 只有 \_\_getattribute__ ( ) 會呼叫 descriptor。

- 類別從 object、type、super ( ) 繼承此機制。

- 覆寫 \_\_getattribute__ ( ) 會防止自動呼叫 descriptor，因為 descriptor 的邏輯都在此方法中。

- object.\_\_getattribute__ ( ) 與 type.\_\_getattribute__ ( ) 會使用不同的方式呼叫 \_\_get__ ( )。前者呼叫時會包含實例並有可能包含類別，後者呼叫時則是放 None 與類別。

- Data descriptor 永遠會覆寫 instance dictionary，data descriptor 優先權是高於 instance dictionary 的。

- Non-Data descriptor 可能被 instance dictionary 覆寫，instance dictionary 的優先權是高於 non-data descriptor 的。

<br/>

### 裝飾器 Decorator

在介紹 descriptor 與 property 的關係前，要來先介紹裝飾器。在 Python 中，function 是 <b>first-class object</b>，這表示如同其他物件，function 可以當成參數傳遞。

<br/>

#### First-Class Object

```python

# 簡單的範例

def call(name):
    print('call', name)

def yell(name):
    print('yell', name)

def notify_tom(action):
    return action('Tom')

notify_tom(call)
# call Tom

notify_tom(yell)
# yell Tom

```

#### 內部函式 Inner Function

再來看以下的範例，我們可以看到 inner function 在 outer function 被呼叫前是沒有定義的，只有在呼叫 parent ( ) 後才會成為 parent ( ) 的 local 變數。

```python

def parent():       

    def child1():
        print('child1')

    def child2():
        print('child2')

    print(type(child1))

    child1()
    child2()

```

#### 從 function 回傳 function

我們也可以從 function 回傳一個 function 的參考

```python

def parent(num):
    def first_child():
        return "Hi, I am Emma"

    def second_child():
        return "Call me Liam"

    if num == 1:
        return first_child
    else:
        return second_child

first = parent(1)
second = parent(2)

first
# <function parent.<locals>.first_child at 0x7f599f1e2e18>

second
# <function parent.<locals>.second_child at 0x7f599dad5268>

first()
# 'Hi, I am Emma'

```

#### 一個簡單的裝飾器

```python

def decorator(func):
    def wrapper():
        print("Something is happening before the function is called.")
        func()
        print("Something is happening after the function is called.")
    return wrapper

def call():
    print("Test")

call = decorator(call)

call()
# Something is happening before the function is called.
# Test
# Something is happening after the function is called.

```

Python 的裝飾器可以使用 syntax sugar 改寫如下，實際上 @decorator 等同於 call = decorator ( call )。

```python

@decorator
def call():
    print("Test")

call()
# Something is happening before the function is called.
# Test
# Something is happening after the function is called.

```

#### 接受參數的裝飾器

```python

def decorator(func):
    def wrapper(*args, **kwargs):
        print('decorating')
        func(*args, **kwargs)
    return wrapper

```

#### 回傳被裝飾的函數的值

設定裝飾器回傳值，在 wrapper 新增 return，奇妙的是，加入這行 return，func 並不會執行兩次。

```python

def decorator(func):
    def wrapper(*args, **kwargs):
        print('decorating')
        func(*args, **kwargs)
        return func(*args, **kwargs)
    return wrapper

@decorator
def call():
    print('call')    

```

#### @functools.wraps

加上 @functools.wraps 可以增加辨識度避免混淆

```python

call
# <function decorator.<locals>.wrapper at 0x000002496F844280>
call.__name__
# wrapper

def decorator(func):
    @functools.wraps(func) # 新增這行
    def wrapper(*args, **kwargs):
        print('decorating')
        func(*args, **kwargs)
        return func(*args, **kwargs)
    return wrapper

call
# <function call at 0x000002CB09BE4280>
call.__name__
# call

```
### descriptor 與 property 的關係

呼叫 property ( ) 就是一個簡潔的方式來建立一個 data descriptor。property 的定義如下，property ( ) 回傳一個實作 descriptor 協議的 property 物件。

{:.note}
property ( fget = None , fset = None , fdel = None , doc = None ) -> property

我們先來看一下還沒有使用裝飾器前如何使用 property ( )，可以看到我們定義了一個 managed attribute attribute1 如下，只要是對 Foo 物件的 attribute1 做存取都會觸發 getter / setter 方法。

```python

# property_function.py

class Foo():
    def getter(self) -> object:
        print("accessing the attribute to get the value")
        return 42

    def setter(self, value) -> None:
        print("accessing the attribute to set the value")
        raise AttributeError("Cannot change the value")

    attribute1 = property(getter, setter)
```
以下寫法是使用裝飾器 @property 的寫法，等同於以上程式

```python

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

```

關於 property ( ) 詳細內容請參考<a href="../2022-09-07-property/">property</a>

### method 與 function

function 原先儲存在 class dictionary 中，在呼叫時被轉變成 method。method 與一般 function 的差異在於物件被綑綁到第一個參數了。

可以使用 types.MethodType 手動產生 method，Python 版本的 types.MethodType 相當於以下程式

```python

import types

class MethodType:

    "模擬 Objects/classobject.c 的 PyMethod_Type"

    # func 就是要呼叫的 function
    # obj 就是 owner 物件
    def __init__(self, func, obj):
        self.__func__ = func 
        self.__self__ = obj

    def __call__(self, *args, **kwargs):
        func = self.__func__
        obj = self.__self__
        return func(obj, *args, **kwargs)

```

呼叫方式如下，可以看到回傳一個 MethodType 物件，這個物件的 \_\_self__就是 Temp 物件，\_\_func__ 就是 function 物件 test。

```python

class Temp():
    
    def test(self, num):
        print('test')

t = Temp()

t
# <__main__.Temp object at 0x0000026E5AB93C70>

x = MethodType(Temp.test, t)
# <__main__.MethodType object at 0x0000026E5AB93C40>

x.__self__
# <__main__.Temp object at 0x0000026E5AB93C70>

x.__func__
# <function Temp.test at 0x0000026E5AB8A4D0>

```

為了支援自動建立 method，function 有 \_\_get__ ( ) 方法，在透過物件存取 attribute ( function ) 時觸發 \_\_get__ ( ) 綁定 method。這表示 function 是 non-data descriptor，對物件做點運算子 lookup 時回傳 bound method。

Python 版本的 function 類別相當於以下程式

```python
        
class Function(object):
    
    def __get__(self, owner, objtype=None):

        "模擬 Objects/funcobject.c 的 func_descr_get()"        

        if owner is None:
            return self
        return types.MethodType(self, owner)
        
```

用以下來舉例，我們知道 descriptor 物件必須以 class attribute 的方式儲存在 owner 的類別中 ( 也就是 class dictionary 中 )，在這裡的 f 就是一個 function 物件，存在於 owner 類別的 class dictionary 中，我們可以從 D.\_\_dict__ 可以看到 ( 這裡與上述 descriptor 範例不同的地方在於之前都是顯示 < \_\_main__.描述器類別名 object at ... > )。

```python

class D:
    def f(self, x):
         return x

D.__dict__
# {..., 'f': <function D.f at 0x0000026CE3AB8A60>, ...}

```

這個函式有一個 qualified name

```python

D.f.__qualname__
# D.f

```

我們知道透過 class dictionary 存取 attribute 不會呼叫 \_\_get__ ( )，只會回傳 function 

```python

D.__dict__['f']
# <function D.f at 0x00C45070>

```

從類別使用點運算子會呼叫 \_\_get__( )，但同樣只會回傳 function 

```python

D.f
# <function D.f at 0x00C45070>

```

有趣的地方在於透過實例呼叫點運算子，會呼叫 \_\_get__( ) 並回傳 bound method 物件。

```python

d = D()
d.f
# <bound method D.f of <__main__.D object at 0x00B18C90>>

```

從內部看，bound method 儲存了底下的 function 與物件實例。

```python

d.f.__func__
# <function D.f at 0x00C45070>

d.f.__self__
# <__main__.D object at 0x1012e1f98>

```

總結來說，function 有 \_\_get__( ) 方法，當透過 attribute 存取時，會轉換成 method。non-data descriptor 將 obj.f ( *args ) 呼叫變成 f ( obj , *args )。呼叫 cls.f ( *args ) 變成 f ( *args )。

<br/>

### Static method

Static method 會不做修改地回傳底下的 function。呼叫 c.f 或 C.f 等同於 object.\_\_getattribute__ ( c , 'f' ) 或 object.\_\_getattribute__ ( C , 'f' )。因此，此函式變成從物件或類別存取時都相同。

{:.note}
Static method 適合不需要參考 self 變數的方法。

```python
class E:
    @staticmethod
    def f(x):
        return x * 10

E.f(3)
# 30

E().f(3)
# 30
```
Python 版本的 @staticmethod 相當於以下程式，在這裡的 \_\_get__不管是透過物件還是類別呼叫，回傳的固定都是 function

```python
class StaticMethod:

    "模擬 Objects/funcobject.c 的 PyStaticMethod_Type()"

    def __init__(self, f):        
        self.f = f

    def __get__(self, obj, objtype=None):
        return self.f

    def __call__(self, *args, **kwds):
        return self.f(*args, **kwds)
```

我們可以看到當載入 Sample 類別定義時，也就是還沒建立 Sample 物件時，已經會呼叫 StaticMethod 的 \_\_init__ 方法，並將 test 方法傳入 f。因此不論我們是透過物件或類別的方式呼叫 test 方法，傳回的結果都是一樣的。

```python
class Sample:
    
    @StaticMethod
    def test(name, age):
        print(f'{name} is {age} years old')
        
t = Sample()

t.test
# <function Sample.test at 0x0000019EC84EA560>

Sample.test
# <function Sample.test at 0x0000019EC84EA560>

t.test('Lisa', 20) 
# Lisa is 20 years old

# 因為 @StaticMethod 沒有將實例附加到第一個參數，因此呼叫時所有的參數都必須傳入
```

{:.note}
定義為 @staticmethod 的方法，呼叫時必須傳入所有的參數，不論是透過類別呼叫或物件呼叫。

<br/>

### Class method

與 static method 不同，class method 在呼叫函式前將 <b>class reference</b> 附加到參數清單。不論呼叫者為物件或類別。

```python
class Test:

    @classmethod
    def clsmethod(cls, x):
        return cls.__name__, x

Test.clsmethod
# <bound method Test.clsmethod of <class '__main__.Test'>>
```

可以看到連呼叫 Test.clsmethod 都回傳 bound method 了

```python
Test.clsmethod(3)
# ('Test', 3)

t = Test()

t.clsmethod
# <bound method Test.clsmethod of <class '__main__.Test'>>

t.clsmethod(3)
#('Test', 3)
```

當只需要 class 參考 而不需要 instance data 時，這就是 class method 的用途。class method 的一個用途就是建立備用的 class 建構子。舉例來說，dict.fromkeys ( ) 會從 keys 建立一個新的 dictionary，Python 版本的 fromkeys 相當於以下程式，可以看到 fromkeys 只需要 cls 這個 class reference 來建立一個新的物件
用法如上。

```python
class Dict(dict):

    @classmethod
    def fromkeys(cls, iterable, value=None):

        "模擬 Objects/dictobject.c 的 dict_fromkeys()"

        d = cls()
        for key in iterable:
            d[key] = value
        return d

d = Dict.fromkeys('abracadabra')

type(d) is Dict
# True

d
#{'a': None, 'b': None, 'r': None, 'c': None, 'd': None}
```
<br/>
Python 版本的 @classmethod 相當於以下程式

```python
class ClassMethod:

    "模擬 Objects/funcobject.c 的 PyClassMethod_Type()"

    def __init__(self, f):
        self.f = f

    def __get__(self, owner, ownerType=None):

        if ownerType is None:
            ownerType = type(owner)
        
        # 為了連鎖裝飾器用
        if hasattr(type(self.f), '__get__'):
            return self.f.__get__(ownerType, ownerType)

        return MethodType(self.f, ownerType)
```
假設在只有一個裝飾器 @classmethod 的情況下，f 是 function 物件 則不需要第二段的 if 區塊，只需將 f 與 'function[ class 傳入 MethodType 建構子就會回傳一個 bound method。

但在考慮多個裝飾器的情況下，像是下面範例多一個 @property 裝飾器時，此時 f 會是 property 物件，使用 property 物件傳入 MethodType 是錯誤的，我們真正要的是呼叫 property 物件的 \_\_get__ 方法。

因此在 ClassMethod 的 \_\_get__ 方法中，第二個條件，如果 type ( self. f ) 是 descriptor，則回傳 self.f.\_\_get__( ownerType , ownerType )，這行會呼叫 \_\_get__ 回傳，這樣就能支援 <b>chain decorator</b>，如下方的類別 G ( Python 3.9 後支援 )。

<br/>

####  class method 範例

```python

class Test():
    
    @ClassMethod
    def clsmethod(cls, x):
        return cls.__name__, x

    def __getattribute__(self, name):
       print('Test.__getattribute__')
       return object.__getattribute__(self, name) 

Test.clsmethod
# bound method Test.clsmethod of <class '__main__.Test'>>
```

首先呼叫 Test.clsmethod 如預期，Test 的 \_\_getattribute__ 沒有呼叫，因呼叫的是 type.\_\_getattribute__，傳入 \_\_get__ 方法的參數分別是 self = <\_\_main__.ClassMethod object at 0x000001F0E948EEF0>， owner = None，ownerType = <class '\_\_main__.Test'>

```python
Test.clsmethod(3)
# ('Test', 3)
```

再來 t.clsmethod 如預期，Test 的 \_\_getattribute__ 有呼叫，傳入 \_\_get__ 方法的參數分別是 self = <\_\_main__.ClassMethod object at 0x000001F0E948EEF0>，owner = <\_\_main__.Test object at 0x0000024575D94130>，ownerType = <class '\_\_main__.Test'>

```python
t = Test()

t.clsmethod
# Test.__getattribute__
# <bound method Test.clsmethod of <class '__main__.Test'>>

t.clsmethod(3)
# Test.__getattribute__
# ('Test', 3)
```

####  classmethod 的連鎖裝飾器範例

```python
class G:

    @classmethod
    @property
    def __doc__(cls):
        return f'A doc for {cls.__name__!r}'

G.__doc__
# A doc for 'G'
```

<br/>

### Member object 與 \_\_slots__

當類別定義 \_\_slots__ 時，它會使用固定長度陣列取代 instance dictionary，這有幾個用途

1. 避免 attribute name 指派錯誤，只允許指派 \_\_slots__ 中的 attribute

```python

class Vehicle:
    __slots__ = ('id_number', 'make', 'model')

auto = Vehicle()
auto.id_nubmer = 'VYE483814LQEX'
# Traceback (most recent call last):
#    ...
# AttributeError: 'Vehicle' object has no attribute 'id_nubmer'

```

2. 當 descriptor 存取 \_\_slots__ 中的 private attribute 時，建立 immutable object :

```python

class Immutable:

    __slots__ = ('_dept', '_name')          # Replace the instance dictionary

    def __init__(self, dept, name):
        self._dept = dept                   # Store to private attribute
        self._name = name                   # Store to private attribute

    @property                               # Read-only descriptor
    def dept(self):
        return self._dept

    @property
    def name(self):                         # Read-only descriptor
        return self._name

mark = Immutable('Botany', 'Mark Watney')
mark.dept
# 'Botany'

mark.dept = 'Space Pirate'
# Traceback (most recent call last):
#     ...
# AttributeError: can't set attribute

mark.location = 'Mars'
# Traceback (most recent call last):
#     ...
# AttributeError: 'Immutable' object has no attribute 'location'

```

3. 節省記憶體

4. 增加速度

5. Blocks tools 像是 functools.cached_property() 會需要 instance dictionary 以正常運作:

```python
from functools import cached_property

class CP:
    __slots__ = ()                          # Eliminates the instance dict

    @cached_property                        # Requires an instance dict
    def pi(self):
        return 4 * sum((-1.0)**n / (2.0*n + 1.0)
                       for n in reversed(range(100_000)))

CP().pi
# Traceback (most recent call last):
#   ...
# TypeError: No '__dict__' attribute on 'CP' instance to cache 'pi' property.

```

<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
