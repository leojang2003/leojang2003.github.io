---
layout: post
title: attribute & dot operator
subtitle: Python
tags: [function, method, MethodType, __getattribute__, __getattr__, bound, bound method, __self__, __func__, __dict__, instance dictionary, class dictionary, mro, method resolution order, class function, class attribute, instance attribute, descriptor, data descriptor, non-data descriptor, AttributeError]
comments: true
---
<br/>
### object.\_\_getattribute__ ( self , name )

官網定義如下

> Called unconditionally to implement attribute accesses for instances of the class. If the class also defines \_\_getattr__(), the latter will not be called unless \_\_getattribute__() either calls it explicitly or raises an AttributeError. This method should return the (computed) attribute value or raise an AttributeError exception. In order to avoid infinite recursion in this method, its implementation should always call the base class method with the same name to access any attributes it needs, for example, object.\_\_getattribute__(self, name).

<br/>

### 點運算子

假設我們定義了一個類別如下

```python
class Test():
    
    def __init__(self):
        self._point = None
    
    @property
    def point(self):        
        return self._point
        
    @point.setter
    def point(self, value):        
        self._point = value
    
    def __getattribute__(self, name):        
        print(f'[__getattribute__] getting the attribute name: {name}')
        return object.__getattribute__(self, name)    
```

我們可以透過物件或類別使用點運算子去存取屬性 ( attribute access )，這裡的屬性可以是 class attribute、instance attribute 或是 class function。透過類別使用點運子存取屬性，等同於存取 class dictionary 的值。另外可以從以下得知。透過類別存取屬性並不會觸發類別自訂的 \_\_getattribute__ 方法。

```python
Test.point
# <property object at 0x000002546FFF0720>

Test.__dict__['point']
# <property object at 0x000002546FFF0720>
```

相反的，透過物件使用點運子存取屬性，會觸發 class 自訂的 \_\_getattribute__ 方法，這是與上述不同的地方。

```python
t = Test()
t.point
# [__getattribute__] getting the attribute name: point
# [__getattribute__] getting the attribute name: _point
```

接下來探討的是透過物件使用點運算子存取屬性時，背後 Python 是怎麼處理的。

<br/>

### 取得 Attribute

在 Python 中透過物件取得屬性不論結果成功或失敗，都一定會呼叫 \_\_getattribute__ 方法，換句話說 \_\_getattribute__ 方法在對物件做屬性存取時會被無條件地呼叫。

\_\_getattribute__ 會回傳計算後的屬性值，或拋出 AttributeError 異常。為了避免在這個方法中產生無限迴圈，自訂的 \_\_getattribute__ 方法最後應記得呼叫內建 'object' 類別的 \_\_getattribute__ 方法，object.\_\_getattribute__ ( self , name )。

如果類別也定義了 \_\_getattr__ 方法，除非 \_\_getattribute__ 方法明確的呼叫 \_\_getattr__ 方法或是引發 AttributeError 異常，否則 \_\_getattr__ 方法將不會被呼叫。

{:.note}
\_\_getattribute__ 只有對物件做屬性存取時會呼叫。

```python
class Person:

    num_of_persons = 0

    def __init__(self, name):
        self.name = name

    def shout(self):
        print(f"Hey! I'm {self.name}.")
        
    def __getattribute__(self, name):
        print(f'getting the attribute name: {name}')
        return super().__getattribute__(name)
    
    def __getattr__(self, name):
        print(f'this attribute doesn\'t exist: {name}')
        raise IOError()

p = Person('John')

p.name
# getting the attribute name: name
# 'John'

p.name1
# getting the attribute name: name1
# this attribute doesn't exist: name1
#
# ... exception stack trace
# AttributeError:
```

我們可以看到程式並沒有明確呼叫 \_\_getattr__ 方法，但 \_\_getattr__ 方法被呼叫的原因在於因為存取不存在的屬性時，\_\_getattribute__ 方法會拋出 AttributeError，而 Python hook 在接到 AttributeError 後呼叫了 \_\_getattr__ 方法。以下使用 Python 模擬 hook 方法。

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

在對 \_\_getattribute__ 方法有了基本的認識後，接下來我們來探討 function、method 與 \_\_getattribute__ 方法之間的關係。

<br/>

### Class Function

首先，我們可以說 function 存在於 class dictionary 中，透過類別存取 function 時，回傳的是一個 function 物件。

```python
Person.shout
# <function __main__.Person.shout(self)>

Person.__dict__['shout']
# <function __main__.Person.shout(self)>
```

但是透過物件存取相同名稱的方法時，回傳的卻是一個 bound method。

```python
p.shout
# <bound method Person.shout of <__main__.Person object at 0x1037d3a60>>
```

當我們透過物件的觀點，這些 function 叫做 method，將類別的 function 轉換為 method 的這個過程叫做 bounding，得到的結果就是一個 bound method。究竟它 bound 到什麼東西? 答案是，當我們呼叫一個 <mark>物件的 function 時，Python 將該物件的參考 bound 到回傳的 method 中</mark>。所以，被 bound 就是物件本身，但這是怎麼發生的?

在 class 主體被直譯時。在這個過程中，會發生許多事情，例如定義 class  namespace ( class dictionary )、將 atttribute value 加到 namespace ( 將 class attribute 加入 class dictionary )、定義 class function 並將其綁定到其名稱。現在，當這些 function 被定義時，function 物件會存在於 class dictionary 中。

首先我們知道因 Python 內建的 'function' 類別有實作 \_\_get__ 方法，因此我們定義的所有 function 因繼承的關係也都是一個 descriptor 物件。

可參考 <a href="../2022-09-04-descriptor/">Descriptor</a>

所謂的 descriptor 物件，是指實作了一組預定義的雙底線方法的類別的 instance。一旦這些方法被實作，就可以說這個類別的物件遵循特定的協定 ( Protocol )，因此以預期的方式運作。預定義的雙底線方法分別是 \_\_get__、\_\_set__ 和 \_\_delete__。

根據實作方法的不同可以分為 Data descriptor 和 Non-data descriptor。Data descriptor 實作了 \_\_get__、\_\_set__ 和/或 \_\_delete__ 這些方法，而 Non-data descriptor 僅實作了 \_\_get__ 方法。我們熟悉的 'function'  類別就僅實作了 \_\_get__ 方法，可以說 function 就是 Non-data descriptor。

在知道 function 其實是 descriptor 之後，這又代表了什麼呢 ? 因為 function 是 descriptor 物件，當我們透過物件呼叫 function 時，\_\_getattribute__ 方法會識別該 function 是一個 descriptor，因此 \_\_getattribute__ 會呼叫 function 類別的 \_\_get__ 方法，將我們的 function 轉變成 method。

<br/>

### \_\_getattribute__

在解釋 function 與 method 的轉變前，首先我們來看 \_\_getattribute__ 究竟是如何取得我們要的屬性的。 

物件一旦使用點運算子來做 attribute lookup 時，會呼叫 \_\_getattribute__，因為我們知道類別可能有繼承自多個父類別，Python 會從物件的 MRO ( Method Resolution Order 的縮寫 ) 中找到 name，接下來依照以下流程解析此 name 是屬於 data descriptor、instance attribute、non-data descriptor 還是 class attribute 中的哪一個，解析成功則停止過程並回傳值 : 

1. 回傳該 name 的 data descriptor ( class 層級 )，或

2. 回傳該 name 的 instance attribute ( instance 層級 )，或

3. 回傳該 name 的 non-data descriptor ( class 層級 )，或

4. 回傳該 name 的 class attribute ( class 層級 )，或

5. 引發 AttributeError 並呼叫 \_\_getattr__ 方法

<br/>

以下使用 Python 模擬 \_\_getattribute__ 方法

```python
def find_name_in_mro(cls, name, default):
    
    for base in cls.__mro__:        
        if name in vars(base):            
            return vars(base)[name]
    return default


def object_getattribute(obj, name):
    
    # 模擬 Objects/object.c 的PyObject_GenericGetAttr()
    
    # 建立後續使用的預設 vanilla 物件
    null = object()

    
    # obj 是呼叫屬性的物件
    objtype = type(obj) 

       
    # 這裡 name 變數代表可以是 class function、instance attribute、class attribute 的 name。在此我們試著找到它並儲存一個參考指向它。因為我們傳入的是一個 type，所以 find_name_in_mro 是從 MRO 中的 class attribute ( 或 class function) 中找出符合的 name。若是沒有的話，則回傳 vanilla 物件。   
    cls_var = find_name_in_mro(objtype, name, null)

    
    # 這裡我們檢查這個 class attribute 的 type 是否是有實作 __get__ 方法，如果有的話，則它至少是個 Non-data descriptor。如果沒有 __get__ 方法的話，則回傳 vanilla 物件。    
    descr_get = getattr(type(cls_var), '__get__', null)

   
    # 如果 descr_get 不是預設的 vanilla 物件，接著我們檢查 cls_var 物件的 type 是否有 __set__ 或 __get__ 如果有的話，則 cls_var 是一個 data descriptor，執行 descr_get 並回傳結果。
    if descr_get is not null:
        if (hasattr(type(cls_var), '__set__')
            or hasattr(type(cls_var), '__delete__')):
            return descr_get(cls_var, obj, objtype)

    
    # 檢查 name 是否在 instance dictionary，有的話回傳其值。
    if hasattr(obj, '__dict__') and name in vars(obj):
        return vars(obj)[name]

    
    # 檢查 name 是否參考到 Non-data descriptor，有的話執行 descr_get 並回傳結果。   
    if descr_get is not null:
        return descr_get(cls_var, obj, objtype)

    
    # 檢查是否參考到 class attribute ，有的話回傳其值。   
    if cls_var is not null:
        return cls_var

    
    # 拋出 AttriuteError Exception，此時 __getattr__ 會被呼叫    
    raise AttributeError(name)
```

<br/>

### function 與 method

在了解了 \_\_getattribute__ 的運作方式後，記得之前我們提到        

> 當我們透過物件呼叫 function 時，\_\_getattribute__ 方法會識別該 function 是一個 descriptor，因此 \_\_getattribute__ 會呼叫 function 類別的 \_\_get__ 方法，將我們的 function 轉變成 method

現在我們深入探討 Python 是怎麼做到的，以下使用 Python 模擬 function 類別 : 

```python
class Function:
    ...
    def __init__(self, func, *args, **kwargs):
        ...
        self.func = func

    def __get__(self, owner, objtype=None):
        if obj is None:
            return self
        return MethodType(self, owner)

    def __call__(self, *args, **kwargs):
        ...
        return self.func(*args, **kwargs)
```

當 \_\_get__ 方法被呼叫時，會將物件的參考與 function 的參考傳入 MethodType 建構子中，回傳這個 MethodType 物件。

以下是使用 Python 模擬 MethodType 類別，我們可以看到 MethodType 使用屬性 \_\_func__ 儲存原始的 function 物件，使用屬性 \_\_self__ 儲存物件的參考，MethodType 也實作了 callable 協議，當我們呼叫 method 時，MethodType 會將物件的參考放到 \_\_func__ 的第一個參數，這就解釋了為什麼當我們使用物件呼叫方法時，第一個參數物件會被自動入的原因了。

```python
class MethodType:

    def __init__(self, func, obj):
        self.__func__ = func
        self.__self__ = obj

    def __call__(self, *args, **kwargs):
        func = self.__func__
        obj = self.__self__
        return func(obj, *args, **kwargs)
```

總結一下，我們可以想像每個 function  物件都被包裝在一個 Non-data descriptor 中。這表示這個 wrapper 物件定義了 \_\_get__ 的方法。這個方法的作用是返回一個新的 callable ( 可以將其視為一個新的 function )，其中第一個參數是我們正在執行「點運算子」的物件的參考。我們可以將它視為一個新的 function。

因為它是一個 callable。我們現在知道實質上，這個新的 function 就是 MethodType 的物件。而 MethodType 物件則將呼叫 method 的物件的參考'與 function 物件一起「包裝」起來。

我們也可以從 method 的 \_\_func__ 與 \_\_self__，分別取得原先 function 的參考與物件的參考如下

```python
print(p.shout.__self__)
# <__main__.Person object at 0x000001BF72D2F760>
print(p.shout.__func__)
# <function Person.shout at 0x000001BF72D1AC20>
```

最後，我們現在應該清楚 function 和 method 的區別，以及透過物件的點運算子存取 function 時如何被 bound 成 method。如果我們仔細思考，我們可以用以下方式呼叫 function ，不過通常是不會這樣呼叫：

```python
p = Person('Leo')

Person.shout(p)
# Hey! I'm Leo.
```

<br/>

### 範例

再舉幾個屬性解析的例子，以便更容易理解「點運算子」是如何工作的。

<br/>

#### instance dictionary
<br/>
```python
p.name
```
<br/>
1. 呼叫 \_\_getattribute__ 傳入參數 p 與 name

2. objtype 是 Person

3. descr_get 是 null 因為 Person 的 class dictionary 沒有 name

4. 因為沒有 descr_get，因此我們跳過了第一個 if 區塊

5. name 存在於 instance dictionary 中，因此我們獲得值

<br/>

#### class function
<br/>
```python
p.shout('Hey')
```
<br/>
1. 呼叫 \_\_getattribute__ 傳入參數 p 與 shout

2. objtype 是 Person

3. Person.shout 實際上被包裹在一個 non-data descriptor，此 wrapper 有實作 __get__ 方法，descr_get 存放這個 \_\_get__ 的參考

4. 此 wrapper 物件為 non-data descriptor，因此我們跳過了第一個 if 區塊

5. shout 也不存在 instance dictionary 因為它是 class 定義的一部分，因此我們跳過了第二個 if 區塊

6. shout 是一個 non-data descriptor，在第三個 if 區塊會回傳 \_\_get__() 方法

<br/>

### instance dictionary 與 class dictionary

以下我們試著用自訂的 find_name_in_mro 與 object_getattribute 來模擬呼叫 \_\_getattribute__ 時會發生的事，這之後會有發現一件事。

```python
def find_name_in_mro(cls, name, default):
    
    print('[mro] {cls}.__mro__ =', cls.__mro__)
    
    for base in cls.__mro__:
        print(f'[mro] vars({base}) =', vars(base).keys())
        if name in vars(base):
            print(f'!!! Found {name} in {vars(base).keys()}')
            return vars(base)[name]
    return default
    
class Obj():
    
    def object_getattribute(obj, name):

        print("[Obj] name =", name)

        null = object()
        objtype = type(obj)
        cls_var = find_name_in_mro(objtype, name, null)
        
        print('[Obj] cls_var =', cls_var)
        print('[Obj] type(cls_var) =', type(cls_var))
        
        descr_get = getattr(type(cls_var), '__get__', null)
        
        print('[Obj] descr_get = ', descr_get)
        
        if descr_get is not null:
            print('[Obj] descr_get NOT null')
            if (hasattr(type(cls_var), '__set__') or hasattr(type(cls_var), '__delete__')):
                print(f'return descr_get(cls_var={cls_var}, obj={obj}, objtype={objtype})')
                return descr_get(cls_var, obj, objtype)     # data descriptor
        else:
            print('[Obj] descr_get IS null ')
        
        #print('[Obj] vars(obj) =', vars(obj))
        
        print(' ')
        print('STEP 2')
        print(' ')
        
        if hasattr(obj, '__dict__') and name in vars(obj):            
            print(f'return vars(obj)[{name}]')            
            return vars(obj)[name]
        
        print(' ')
        print('STEP 3')
        print(' ')
        
        if descr_get is not null:            
            return descr_get(cls_var, obj, objtype)
        
        print(' ')
        print('STEP 4')
        print(' ')
        
        if cls_var is not null:
            return cls_var

        raise AttributeError(name)

class Test():
    
    age = 20
    
    def __init__(self):
        self._point = None
    
    @property
    def point(self):
        print('[Test] getter')
        return self._point
        
    @point.setter
    def point(self, value):        
        self._point = value
    
    def __getattribute__(self, name):
        print(' ')
        print(f'[Test] getting the attribute name: {name}')
        return Obj.object_getattribute(self, name)
```
<br/>
#### 存取 instance attribute 

定義完後，執行以下程式

```python
t = Test()
t.nice = 'car'
t.nice

# [Test] getting the attribute name: nice
# [Obj] name = nice
# [mro] {cls}.__mro__ = (<class '__main__.Test'>, <class 'object'>)     
# [mro] vars(<class '__main__.Test'>) = dict_keys(
#   ['__module__', 'age', 
#   '__init__', 'point', 
#   '__getattribute__', 
#   '__dict__', 
#   '__weakref__', 
#   '__doc__'])
# [mro] vars(<class 'object'>) = dict_keys([ ... ])     
# [Obj] cls_var = <object object at 0x000002149A1C41C0>
# [Obj] type(cls_var) = <class 'object'>
# [Obj] descr_get =  <object object at 0x000002149A1C41C0>
# [Obj] descr_get IS null 
```

當我們透過物件 t 存取屬性 nice 時，會呼叫 Test 類別的 \_\_getattribute__ 方法，我們在 \_\_getattribute__ 最後一行呼叫我們自訂的 object_getattribute 方法，用來模擬 object.\_\_getattribute__ 方法。

在 object_getattribute 中，我們將 Test 類別傳入 find_name_in_mro 方法，此方法會從 Test.\_\_mro__ 中找到是否有這個名稱 nice。根據 MRO 的順序，使用語法為 vars ( base ) 依序檢視每個類別的 class dictionary 是否有 nice。我們知道 vars ( base ) 等同於 base.\_\_dict__，因為 nice 是 instance attribute，因此 find_name_in_mro 從 class namespace 中是找不到 nice 的，因此 find_name_in_mro 會回傳 vanilla 物件。

根據回傳的物件，我們試圖取得物件是否有 \_\_get__ 方法，因內建的 'object' 物件沒有 \_\_get__ 方法，因此又將 vanilla 物件回傳。此時我們確定 nice 不是一個 descriptor 物件。

接下來進入第二個 if 區塊，試圖從 instance dictionary 中檢查是否有此屬性，因為 hasattr 與 vars 都會試圖存取 \_\_dict__ 屬性，所以我們看到程式會呼叫 \_\_getattribute__ 方法 2 次，並且在 Test 物件中都有找到 \_\_dict__ 如下。

```python
if hasattr(obj, '__dict__') and name in vars(obj):            
    print(f'return vars(obj)[{name}]')            
    return vars(obj)[name]
```

執行結果如下

```python
# STEP 2
 
# [Test] getting the attribute name: __dict__
# [Obj] name = __dict__
# [mro] {cls}.__mro__ = (<class '__main__.Test'>, <class 'object'>)     
# [mro] vars(<class '__main__.Test'>) = dict_keys(['__module__', 'age', 
# '__init__', 'point', '__getattribute__', '__dict__', '__weakref__', '__doc__'])
# !!! Found __dict__ in dict_keys(['__module__', 'age', '__init__', 'point', '__getattribute__', '__dict__', '__weakref__', '__doc__'])       
# [Obj] cls_var = <attribute '__dict__' of 'Test' objects>
# [Obj] type(cls_var) = <class 'getset_descriptor'>
# [Obj] descr_get =  <slot wrapper '__get__' of 'getset_descriptor' objects>
# [Obj] descr_get NOT null
# return descr_get(
#    cls_var=<attribute '__dict__' of 'Test' objects>, 
#    obj=<__main__.Test object at 0x000002149A2DF9D0>, 
#    objtype=<class '__main__.Test'>)


# [Test] getting the attribute name: __dict__
#
# .... 同上
#
# return descr_get(
#   cls_var=<attribute '__dict__' of 'Test' objects>, 
#   obj=<__main__.Test object at 0x000002149A2DF9D0>, 
#   objtype=<class '__main__.Test'>)

# return vars(obj)[nice]
```

因 hasattr 與 name in vars ( obj ): 皆為 True，所以最後會執行 return vars ( obj )[ name ] 時再呼叫一次 vars ( obj )，如下。

```python 
# [Test] getting the attribute name: __dict__
#
# .... 同上
#
# return descr_get(
#    cls_var=<attribute '__dict__' of 'Test' objects>, 
#    obj=<__main__.Test object at 0x000002149A2DF9D0>, 
#    objtype=<class '__main__.Test'>)
```

<br/>

#### 一個觀察，instance dictionary 是類別的一個 attribute

從上面可以觀察到，所謂的 instance dictionary，也就是 instance 的 \_\_dict__，並不是 instance 的 attribute，而是 class 的 attribute，我們執行以下程式可以看出，class dictionary 中有一個屬性是 \_\_dict__，內容是 Test 物件的屬性 \_\_dict__

```python
from pprint import pprint
pprint(Test.__dict__)

# mappingproxy({
# '__dict__': <attribute '__dict__' of 'Test' objects>,
# '__doc__': None,
# '__getattribute__': <functionTest.__getattribute__at0x0000025045A19090>,
# '__init__': <function Test.__init__ at 0x000002504551AA70>,
# '__module__': '__main__',
# '__weakref__': <attribute '__weakref__' of 'Test' objects>,
# 'age': 20,
# 'point': <property object at 0x000002504552B470>,
# 'run': <function Test.run at 0x0000025045A19120>
# })
```

從下面的也可以看到，當我們試圖要找到 Test 物件的 \_\_dict__ 時

```python
# STEP 2 
# [Test] getting the attribute name: __dict__
# [Obj] name = __dict__
```

我們其實是在 find_name_in_mro 時，在 Test 類別的 \_\_dict__ 中找到物件的 \_\_dict__ 屬性，如下所示。

```python
# [mro] {cls}.__mro__ = (<class '__main__.Test'>, <class 'object'>)     
# [mro] vars(<class '__main__.Test'>) = dict_keys(['__module__', 'age', 
# '__init__', 'point', '__getattribute__', '__dict__', '__weakref__', '__doc__'])
# !!! Found __dict__ in dict_keys(['__module__', 'age', '__init__', 'point', '__getattribute__', '__dict__', '__weakref__', '__doc__'])
```

這個 \_\_dict__ 也是內建的 <class 'getset_descriptor'> 的物件，因此每當我們透過物件存取 \_\_dict__ 時，類別的屬性 \_\_dict__ 會回傳 \_\_get__ 方法的結果，如下所示。

```python
# [Obj] cls_var = <attribute '__dict__' of 'Test' objects>
# [Obj] type(cls_var) = <class 'getset_descriptor'>
# [Obj] descr_get =  <slot wrapper '__get__' of 'getset_descriptor' objects>
# [Obj] descr_get NOT null
# return descr_get(
#   cls_var=<attribute '__dict__' of 'Test' objects>, 
#   obj=<__main__.Test object at 0x000002149A2DF9D0>, 
#   objtype=<class '__main__.Test'>)
```

我們可以執行以下程式，明確呼叫類別的 \_\_dict__ 屬性的 \_\_get__ 方法，也可以得到物件的 instance dictionary。

```python
t = Test()
t.nice = 'car'

Test.__dict__['__dict__'].__get__(t)
# {'_point': None, 'nice': 'car'}
```

{:.note}
instance dictionary ( \_\_dict__ ) 是 class dictionary 中的一個屬性。

<br/>

#### 存取 class function 的流程

若我們新增一個方法 run 並透過物件呼叫它 

```python    
class Test():

...

    def run(self, count):
        print(f'run {count} times')
...

t = Test()
t.run(10)

# Test] getting the attribute name: run
# [Obj] name = run
# [mro] {cls}.__mro__ = (<class '__main__.Test'>, <class 'object'>)     
# [mro] vars(<class '__main__.Test'>) = dict_keys([
#   '__module__',
#   'age', 
#   '__init__', 
#   'point', 
#   '__getattribute__', 
#   'run', 
#   '__dict__', 
#   '__weakref__', 
#   '__doc__'])
# !!! Found run in dict_keys(['__module__', 'age', '__init__', 'point', 
# '__getattribute__', 'run', '__dict__', '__weakref__', '__doc__'])     
# [Obj] cls_var = <function Test.run at 0x0000017925BCA8C0>
# [Obj] type(cls_var) = <class 'function'>
# [Obj] descr_get =  <slot wrapper '__get__' of 'function' objects>     
# [Obj] descr_get NOT null
```

當我們透過物件存取 run 時，會呼叫 Test 類別的 \_\_getattribute__ 方法，因為 run 是 class attribute，因此 find_name_in_mro 從 class namespace 中會找到 run 的，因此 find_name_in_mro 會回傳一個 function 物件。

```python
# cls_var = <function Test.run at 0x0000017925BCA8C0>
```

根據回傳的物件，我們試圖取得物件是否有 \_\_get__ 方法，因 function 物件有 \_\_get__ 方法，因此將 function 物件的 \_\_get__ 指派給 descr_get。因 function 不是 data descriptor，因此進入下一個 if 區塊

```python
# [Obj] type(cls_var) = <class 'function'>
# [Obj] descr_get =  <slot wrapper '__get__' of 'function' objects>  
```

接下來進入第二個 if 區塊，因 instance dictionary 沒有 run 屬性，因而進入第三個 if 區塊並執行 descr_get 方法回傳。

```python
# STEP 2  
#  
# [Test] getting the attribute name: __dict__
#  ...
# return descr_get(
#    cls_var=<attribute '__dict__' of 'Test' objects>, 
#    obj=<__main__.Test object at 0x0000017925BDF940>, 
#    objtype=<class '__main__.Test'>)
# 
# [Test] getting the attribute name: __dict__
# ...
# return descr_get(
#   cls_var=<attribute '__dict__' of 'Test' objects>,
#   obj=<__main__.Test object at 0x0000017925BDF940>, 
#   objtype=<class '__main__.Test'>)

# STEP 3

# run 10 times

```
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
