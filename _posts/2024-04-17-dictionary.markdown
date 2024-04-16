---
layout: post
title: Dictionary
subtitle: 
tags: [dict]
comments: true
---

### 如何建立 dict

使用 { } 

```python

# 第一種方式
x = {}

x["a"] = 1
x["b"] = 2

x # {'a': 1, 'b': 2}

```

使用 dict ( ) 

```python

x = dict()

x["a"] = 3
x["b"] = 4

x # {'a': 3, 'b': 4}

```

使用 dict ( **kwargs ) 

```python

dict(a = 3, b = 4)
# {'a': 3, 'b': 4}

```

使用 dict ( mapping , **kwargs ) 
<br class="new">
第一個參數是 mapping 物件

```python

dict({'a' : 3, 'b' : 4})
# {'a': 3, 'b': 4}

dict({'a' : 3, 'b' : 4}, c = 5)
# {'a': 3, 'b': 4, 'c': 5}

dict({'a' : 3, 'b' : 4, 'c' : 5}, c = 6)
# {'a': 3, 'b': 4, 'c': 6}

```

使用 dict ( iterable , **kwargs )
<br class="new">
第一個參數是 iterable 物件，iterable 物件的每個元素都必須是長度為 2 的 iterable。

```python

dict([('foo', 100), ('bar', 200)])
# {'foo': 100, 'bar': 200}

dict([('foo', 100), ('bar', 200)], a = 3) 
# {'foo': 100, 'bar': 200, 'a': 3}

dict([])
# {}

dict([['foo', 100], ['bar', 200]]) 
# {'foo': 100, 'bar': 200}

```

Dictionary 是由 key : value 組成，key 必須是 immutable 的物件，如果是只包含 strings 或是 numbers 的 tuple 也可以當 key，如果 tuple 包含 mutable 的物件，則不能當作 key，另外 list 也不能當作 key，會出現 TypeError: unhashable type: 'list'

{:.note}
immutable 物件包含 int, float, complex, bool, str, bytes, tuple, frozenset

```python

dict = {(1,2): 1, (1,4): 2, (1,2): 6} # 也可以用 tuple 當作 dict 的 key

dict {(1,2): 6, (1,4): 2} # 這邊可以看到舊的 key 被新的 key 取代了

```

補充說明如何將 int 轉成 byte，可以使用 to_bytes ( )

```python

# byte 可以表示 0 ~ 255

(15).to_bytes(4, byteorder='big') # b'\x00\x00\x00\x0f'
(16).to_bytes(4, byteorder='big') # b'\x00\x00\x00\x10'

(255).to_bytes(4, byteorder='big') #b'\x00\x00\x01\xff'
(256).to_bytes(4, byteorder='big') #b'\x00\x00\x01\x00'

```

<br/>

### 刪除 dict 的 key / value

刪除 dict 的物件

```python

dict = {(1,2): 1, (1,4): 2, (1,2): 6}
del dict[1,2] # 或是 del dict[(1,2)] 也行

dict # {(1,4): 2}

```

### in / not in

dict 是否包含 key 也可以使用 in / not in

```python

dict = {"c": 1, "b": 2, "a": 3}

'c' in dict # True
'd' in dict # False
'd' not in dict # True

```
<br/>

### Dictionary Comprehension

建立 Dictionary Comprehension，如同 set comprehension，都是使用{ }，但是使用的是 : 

```python

power = {x : x**2 for x in (2, 4, 6)}
# {2: 4, 4: 16, 6: 36}

```
<br>

### list ( d )

取得所有 dict 的key，可以使用list ( d )，依照插入的順序產生 list，可以使用sorted ( )來做排序

```python

dict = {"c": 1, "b": 2, "a": 3}

list(dict) # ['c', 'b', 'a']

sorted(list(dict)) # ['a', 'b', 'c']

```

### len ( d )

取得字典 d 的長度

<br class="new">

### iter ( d )

回傳字典的 key 的 iterator，等同 iter ( d.keys ( ) )。

```python

d = dict(a = 1, b = 2, c = 3)

q = iter(d) 
# <dict_keyiterator object at 0x000001EB821640E0>

next(q) # 'a'
next(q) # 'b'
next(q) # 'c'

```

### clear ( )

```python

d = dict(a = 1, b = 2, c = 3)

d.clear() # {}

```

### d.fromkeys ( )

fromkeys() 是一個 class method 回傳一個新的 dictionary，值預設為 None. 

```python

d = dict(a = 1, b = 2, c = 3)

d.fromkeys(d)
# {'a': None, 'b': None, 'c': None}

# All of the values refer to just a single instance, so it generally doesn’t make sense for value to be a mutable object such as an empty list. To get distinct values, use a dict comprehension instead.

import random
{key : random.randint(0, 10) for key, value in d.fromkeys(d).items()}

# {'a': 5, 'b': 2, 'c': 4}

```

### get ( key , 'default value' )

如果 key 有在字典的話取得 key 的 value，沒有的話回傳預設值，沒設定預設值回傳 None，永遠不會拋出 KeyError

```python

dict = {'a':1, 'b':2}

dict.get('c', 'missing')
# 'missing'

dict.get('c')
# None

```

### items ( )

回傳字典的 (key, value)

```python

dict = {'a':1, 'b':2}

dict.items()
# dict_items([('a', 1), ('b', 2)])

```

### keys ( )

回傳字典的 (key, value)

```python

dict = {'a':1, 'b':2}

dict.keys()
# dict_keys(['a', 'b'])

```

### pop ( key [ , default ] )

如果 key 有在字典的話，移除並回傳 key 的 value，沒有的話回傳預設值，沒設定預設值拋出 KeyError

```python

dict = {'a':1, 'b':2}

dict.pop('a') 
# 1

dict.pop('a')
# Traceback (most recent call last):
#   File "<stdin>", line 1, in <module>
# KeyError: 'a'

```

### popitem ( )

從字典移除並回傳一個 (key, value) 配對，回傳遵照 LIFO 的順序

```python

dict = {}

dict[1] = 'a'
dict[2] = 'c'

dict.popitem() # (2, 'c')

dict.popitem() # (1, 'a')

dict.popitem()
# Traceback (most recent call last):
#   File "<stdin>", line 1, in <module>
# KeyError: 'popitem(): dictionary is empty'

```

### reversed ( d )

回傳一個字典的 key 的 reversed iterator

```python

dict = {}

dict[1] = 'a'
dict[2] = 'b'
dict[3] = 'c'

i = reversed(dict)

next(i) # 3
next(i) # 2
next(i) # 1

```

### setdefault ( key [ , default ] )

如果字典有 key，則回傳其值，如果沒有插入 default 值並回傳 default 值。default 預設為 None。

```python

dict
# {1: 'a', 2: 'b', 3: 'c'}

dict.setdefault(1)
# 'a'

dict.setdefault(4)

dict
# {1: 'a', 2: 'b', 3: 'c', 4: None}

dict.setdefault(5, 'e')
# 'e'

dict
# {1: 'a', 2: 'b', 3: 'c', 4: None, 5: 'e'}

```

### update ( [ other ] )
使用 other 的 key/value 來更新字典。回傳 None。update 也接受 key/value pair 的 iterable。也可以使用 keyword argument 更新。

```python

dict
# {1: 'a', 2: 'b', 3: 'c', 4: None, 5: 'e'}

dict
# {1: 'a', 2: 'b', 3: 'c', 4: None, 5: 'e'}

dict.update({1:'A',2:'B',1:'AA'})

dict
# {1: 'AA', 2: 'B', 3: 'c', 4: None, 5: 'e'}

dict.update(red=1, blue=2)
# {1: 'AA', 2: 'B', 3: 'c', 4: None, 5: 'e', 'red': 1, 'blue': 2}

```

### values ( )

```python

dict.values()
# dict_values(['AA', 'B', 'c', None, 'e', 3, 2])

```

### copy ( )

dict 可以使用copy ( )來做 copy，copy() 生出一個全新的 dict，修改 copy 出來的 dict 不會影響原本的 dict。

<br class="new">

### update ( )

使用一個 dict 來update ( )另一個 dict 的內容，這裡就是更新 dict1，用 dict2 的內容去更新。 

```python

dict1.update(dict2)

```

### 總結
- {: .arrow}使用 {}, dict() 可以建立一個空的 dict
- {: .arrow}搭配 keyword argument 可以不用使用冒號初始化 dict
- {: .arrow}list(dict) 可以將 key 轉成一個 list
- {: .arrow}dict 只能接受 immutable 的物件作為 key
- {: .arrow}key 可以使用 in / not in 檢查

<br/>
<br/>
<br/>
