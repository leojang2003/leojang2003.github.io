
### 走訪 dict 

走訪 dict 可以使用 items()

```python
knights = {'gallahad': 'the pure', 'robin': 'the brave'}

for k, v in knights.items():
    print(k, v)
	
# gallahad the pure
# robin the brave
```
<br/>

### 走訪 sequence

走訪 sequence 時，可以使用 enumerate() 同時取得 index 跟 value

```python
for i, v in enumerate(['tic', 'tac', 'toe']):
    print(i, v)

	# 0 tic
# 1 tac
# 2 toe
```
<br/>

### 同時走訪多個 sequence

同時走訪多個 sequences 可以使用 zip()

```python
questions = ['name', 'quest', 'favorite color']
answers = ['lancelot', 'the holy grail', 'blue']
for q, a in zip(questions, answers):
    print('What is your {0}?  It is {1}.'.format(q, a))

# What is your name?  It is lancelot.
# What is your quest?  It is the holy grail.
# What is your favorite color?  It is blue.		
```
<br/>

### 反向走訪 sequence

反向走訪 sequences 可以使用 reversed()

```python
for i in reversed(range(1, 10, 2)):
    print(i)
	
# 9
# 7
# 5
# 3
# 1
```
<br/>

### 排序後走訪 sequence

將一個 sequence 排序後走訪，可以使用 sorted() 會回傳一個排序後的 new sequence

```python

basket = ['apple', 'orange', 'apple', 'pear', 'orange', 'banana']
for i in sorted(basket):
    print(i)

# apple
# apple
# banana
# orange
# orange
# pear	
```
<br/>

### 走訪 sequence 時若要變更 sequence 的內容

有時候在走訪 sequence 的時候有變更的需求，較好的做法是直接 append() 到一個新的 sequence

```python

import math
raw_data = [56.2, float('NaN'), 51.7, 55.3, 52.5, float('NaN'), 47.8]
filtered_data = []
for value in raw_data:
    if not math.isnan(value):
        filtered_data.append(value)

filtered_data # [56.2, 51.7, 55.3, 52.5, 47.8]		
```

接續上述，也可以使用 list comprehension 方式如下

```python
filtered_data = [ value for value in raw_data if not math.isnan(value) ]
```

<br/>
<br/>
<br/>
