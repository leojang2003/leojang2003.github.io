---
layout: post
title: Loop Techniques
subtitle: 
tags: [dict, enumerate(), sorted(), zip(), append(), reversed(), list]
comments: true
---

### 走訪 dict 

走訪 dict 可以使用 items ( )

```python
knights = {'gallahad': 'the pure', 'robin': 'the brave'}

for k, v in knights.items():
    print(k, v)
	
# gallahad the pure
# robin the brave

```
<br/>

### 走訪序列(sequence)

走訪序列時，可以使用 enumerate ( ) 同時取得<b>索引</b>跟值

```python

for i, v in enumerate(['tic', 'tac', 'toe']):
    print(i, v)

# 0 tic
# 1 tac
# 2 toe

```

<br/>

### 同時走訪多個序列

同時走訪多個序列可以使用 <mark><b>zip ( )</b></mark>

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

### 反向走訪序列

反向走訪序列可以使用 reversed ( )

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

### 排序後走訪序列

將一個序列排序後走訪，可以使用 sorted ( ) 會回傳一個排序後的新的序列

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

### 走訪序列時若要變更序列的內容

有時候在走訪序列的時候有變更的需求，較好的做法是直接 append ( ) 到一個新的序列

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
