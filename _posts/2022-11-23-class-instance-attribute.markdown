---
layout: post
title: Instance Attribute / Class Attribute
subtitle: 
tags: []
comments: true
---

### Class Attribute

instance attribute 是只屬於單一一個物件的 Python 變數。此變數僅能從物件的 scope 存取，通常是在 _ _ init _ _ 方法內定義。class attribute 是屬於類別的 Python 變數，可在多個物件中分享，通常定義於 _ _ init _ _ 之外。

```python

class Example:
    
    class_attr = 0
        
    def __init__(self, instance_attr):
        self.instance_attr = instance_attr
        
foo = Example(1)

bar = Example(2)

foo.instance_attr # 1

bar.instance_attr # 2

Example.class_attr # 0

foo.class_attr # 0

bar.class_attr # 0

```
{:.note}
注意到 class attribute 可以透過 class property 或是 instance property 存取，如 foo.class_attr。

<br/>

在這背後就是 namespace 的關係，Python 的 class 與 instance 有不同的 namespace，Example.__dict_ 為 class 的 namespace，而 foo.__dict__ 為 instance 的 namespace。<mark>When you access an attribute (instance or class attribute) as a property of an object using dot convention</mark>，Python 會先找 object 的 namespace，再來找 class 的 namespace，如果還是找不到的話，則拋出 AttributeError

<br/>

注意我們透過 foo 物件變更 class_attr，並不會影響 bar 物件存取 class_attri 的值，這是怎麼回事? 

```python

# 透過物件 foo 將 class_attr 改成 5
foo.class_attr = 5

foo.class_attr
# 5

bar.class_attr 
# 0
# 不影響 bar 的 class_attr

```

{:.note}
Python 神奇特性

實際上透過物件變更 class_attr，會在 instance namespace 加入一個同名的 instance attribute，我們可以從下看到。

```python

foo.__dict__
# {'instance_attr': 1}

foo.class_attr = 5

foo.__dict__
# {'instance_attr': 1, 'class_attr': 5}
# foo 多了一個名為 class_attr 的 instance attribute

bar.__dict__
# {'instance_attr': 2}

bar.class_attr 
# 0
# 不影響 bar 的 class_attr

```

若 class attribute 的值為 immutable 物件，以上的規則必成立。但若 class attribute 的值為 mutable 物件，則有不一樣的效果。

```python

class Example:
    
    class_attr = []
        
    def __init__(self, instance_attr):
        self.instance_attr = instance_attr
        
foo = Example(1)
bar = Example(2)

# 此時 Example.class_attr = foo.class_attr = bar.class_attr = []

```

若是透過 foo 物件來 append 元素到 class_attr，則不會新增 class_attr 到 instance namespace。如下

```python

foo.class_attr.append(2)

print(foo.__dict__)
# {'instance_attr': 1}
# 與 immutable 物件不同，mutable 物件不會在 instance namespace 新增 instance attribute

print(foo.class_attr)
# {'instance_attr': 1} 

print(bar.__dict__)
# {'instance_attr': 2}

print(bar.class_attr)
# [2]

```

但如果是指派一個新的 list 如 foo.class_attr = []，則如同 mutable 物件，instance namespace 會新增名為 class_attr 的 instance attribute 如下。

```python

class Example:
    
    class_attr = [1]
        
    def __init__(self, instance_attr):
        self.instance_attr = instance_attr
        
foo = Example(1)
bar = Example(2)


foo.__dict__
# {'instance_attr': 1}

foo.class_attr = [2]
# 變更 class_attr 的值

foo.__dict__
# {'instance_attr': 1, 'class_attr': [2]}
# foo 的 instance namespace 新增了 instance attribute

foo.class_attr
# [2]

bar.__dict__
# {'instance_attr': 2}

bar.class_attr
# [1]
# 同一開始的例子，bar 的 class attribute 沒有變更

```

<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
