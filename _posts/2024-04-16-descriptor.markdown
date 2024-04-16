---
layout: post
title: Descriptor
subtitle: 
tags: [property, descriptor, first-class object, data descriptor, non-data descriptor]
comments: true
---

可參考 <a href="../2022-11-23-class-instance-attribute/">類別與物件的屬性</a>
可參考 <a href="../2024-01-18-dot-operator/">點運算子</a>

### object.\_\_get__ ( self , owner , ownerType = None )

首先 <i>\_\_get__</i> 方法官網定義如下

> Called to get the attribute of the owner class (class attribute access) or of an instance of that class (instance attribute access). The optional owner argument is the owner class, while instance is the instance that the attribute was accessed through, or None when the attribute is accessed through the owner.

透過類別或物件去存取 <i>class attribute</i> 時，都會觸發 <i>class attribute</i> 的類別的 <i>\_\_get__</i> 方法，須注意不是所以有類別都有實作 <i>\_\_get__</i>。

<br/>

#### 透過 class 存取 class attribute

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
從上面可以看出，透過類別不論存取的屬性是否為 descriptor，都不會觸發類別的 <i>\_\_getattribute__ ( )</i>，實際上被呼叫的是 <i><b>type.\_\_getattribute__ ( )</b></i>

<br/>
