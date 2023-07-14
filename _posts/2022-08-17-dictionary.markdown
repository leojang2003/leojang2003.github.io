### 如何建立 dict

兩種方式可以建立 dict，使用 {} 或是 dict() 
```python
# 第一種方式
dict= {}

dict["a"] = 1
dict["b"] = 2

dict # {'a': 1, 'b': 2}

# 第二種方式
dict2= dict()

dict2["a"] = 3
dict2["b"] = 4

dict2 # {'a': 3, 'b': 4}

```
<br/>
### dict 可接受參數

使用 dict() 來建立一個新的 dict，傳入一個 tuple 的 list

```python

l = [('a', 1), ('b', 2), ('c', 3)]

d = dict(l)
l 
#{'a': 1, 'b': 2, 'c': 3}
```

也可以傳入一個包含 tuple 的 tuple

```python
t = (('a', 4), ('b', 5), ('c', 6))

d = dict(t)
d 
#{'a': 4, 'b': 5, 'c': 6}
```
<br/>
### 搭配 keyword argument
使用 dict() 可以搭配 keyword argument，比較簡單

```python
dict(sape=4139, guido=4127, jack=4098)
{'sape': 4139, 'guido': 4127, 'jack': 4098}
```

Dictionary 是由 key:value 組成，key 必須是 immutable 的物件，如果是只包含 strings 或是 numbers 的 tuple 也可以當 key，如果 tuple 包含 mutable 的物件，則不能當作 key，另外 list 也不能當作 key，會出現 TypeError: unhashable type: 'list'

{:.note}
immutable 物件包含 int, float, complex, bool, str, bytes, tuple

```python
dict = {(1,2): 1, (1,4): 2, (1,2): 6} # 也可以用 tuple 當作 dict 的 key

dict {(1,2): 6, (1,4): 2} # 這邊可以看到舊的 key 被新的 key 取代了

```

補充說明如何將 int 轉成 byte，可以使用 to_bytes
```python
# byte 可以表示 0 ~ 255
(255).to_bytes(4, byteorder='big') #b'\x00\x00\x01\xff'
(256).to_bytes(4, byteorder='big') #b'\x00\x00\x01\x00'
```
<br/>


<br/>
### 刪除 dict 的 key/value
刪除 dict 的物件
```python
dict = {(1,2): 1, (1,4): 2, (1,2): 6}
del dict[1,2] # 或是 del dict[(1,2)] 也行

dict # {(1,4): 2}
```

取得所有 dict 的 key，可以使用 list(dict)，依照插入的順序產生list，可以使用sorted()來做排序
```python
dict = {"c": 1, "b": 2, "a": 3}

list(dict) # ['c', 'b', 'a']

sorted(list(dict)) # ['c', 'b', 'a'] 排序

```

dict 是否包含 key 也可以使用 in / not in
```python
dict = {"c": 1, "b": 2, "a": 3}

'c' in dict # True
'd' in dict # False
'd' not in dict # True

```
<br/>
### Dictionary Comprehension
建立 Dictionary Comprehension，如同 set comprehension，都是使用{}，但是使用的是冒號
```python
power = {x: x**2 for x in (2, 4, 6)}
# {2: 4, 4: 16, 6: 36}
```
<br>

### 總結
- {:.arrow}使用 {}, dict() 可以建立一個空的 dict
- {:.arrow}搭配 keyword argument 可以不用使用冒號初始化 dict
- {:.arrow}list(dict) 可以將 key 轉成一個 list
- {:.arrow}dict 只能接受 immutable 的物件作為 key
- {:.arrow}key 可以使用 in / not in 檢查





