The str() 方法會回傳一個可讀的 reprensentation，repr() 會回傳 interpreter 可讀的 reprensentation。若是沒有特別的 representation，str() 與 repr() 回傳相同的東西，如 lists/dictionaries，而 Strings就有不同的 representations

```python
s = 'Hello, world.'
str(s)
'Hello, world.'
repr(s)
"'Hello, world.'"
str(1/7)
'0.14285714285714285'
x = 10 * 3.25
y = 200 * 200
s = 'The value of x is ' + repr(x) + ', and y is ' + repr(y) + '...'
print(s)
# The value of x is 32.5, and y is 40000...
# str 的 repr() 會加上 '' 並顯示反斜線:
... hello = 'hello, world\n'
print(repr(hello)) # 'hello, world\n'
print(str(hello))  # hello, world 

# repr() 參數可以是 Python 物件:
... repr((x, y, ('spam', 'eggs')))
"(32.5, 40000, ('spam', 'eggs'))"
```

## Formatted string literals 
簡稱 f-string，可以將 Python expression 包入 string 中

小數點後三位，類似str.format()的語法
```python
import math
print(f'The value of pi is approximately {math.pi:.3f}.')
# The value of pi is approximately 3.142.
```

設定最小數字寬
```python
table = {'Sjoerd': 4127, 'Jack': 4098, 'Dcab': 7678}
for name, phone in table.items():
    print(f'{name:10} ==> {phone:10d}')
# Sjoerd     ==>       4127
# Jack       ==>       4098
# Dcab       ==>       7678
```

## str.rjust()
str.rjust(n) 在寬度n靠右左補空白。還有類似的方法 str.ljust() 和 str.center()。這些方法不寫任何東西，它們只是返回一個新的字串。如果輸入字串太長，他們不會截斷它，而是原樣返回

```python
for x in range(10):
    print(repr(x*x*x).rjust(3))

# 靠右左補空白，長度3

  0
  1
  8
 27
 64
125
216
343
512
729
```

```python
for x in range(10):
    print(repr(str(x*x*x)).rjust(5))

# 靠右左補空白，長度5

 '0'
 '1'
 '8'
'27'
'64'
'125'
'216'
'343'
'512'
'729'
```

{:note}
string x 傳入 repr() 時，會在左右加上單引號，回傳 'x'

## str.rfill()

靠右左補0

```python
'12'.zfill(5)               # '00012'
'a'.zfill(5)                # '0000a'
'-3.14'.zfill(7)            # '-003.14'
'3.14159265359'.zfill(5)    # '3.14159265359'
```

## 讀寫檔案
```python
f = open('workfile', 'w', encoding="utf-8")
```

'w' 寫入
'r' 讀取(預設)
'a' 附加
'r+' 讀寫

建議加上 with，好處是會自動關閉檔案
```python
with open('workfile', encoding="utf-8") as f:
    read_data = f.read()

# We can check that the file has been automatically closed.
f.closed
```

如果沒用 with，則需手動加上 f.close

## 讀檔案 read()
```python
f.read()
# 'This is the entire file.\n'
f.read()
# '' 結束

```

## 讀檔案 readline()
```python
f.readline()
# 'This is the first line of the file.\n'
f.readline()
# 'Second line of the file\n'
f.readline()
# ''
```

## 讀檔案使用 for
```python
for line in f:
    print(line, end='')
# This is the first line of the file.
# Second line of the file
```

## 讀取所有行數並儲存到 list
```python
list(f) 或是 f.readlines()
```

## f.write()
```python
f.write('This is a test\n') # 回傳15 寫入的char數
```

其他型別要寫入需要先轉型成 str
```python
value = ('the answer', 42)
s = str(value)  # convert the tuple to string
f.write(s) # 18
```

## json

import json
```python
with open('file.txt', 'w') as f:

    x = [1, 'simple', 'list']
    print(json.dumps(x)) # '[1, "simple", "list"]'

    dict = { "age":20, "height":176, "born":{"country": "usa", "city":"dallas"}}
    json.dump(dict, f)

with open('file.txt', 'r') as f:
    file = json.load(f)
    print (file) # {'age': 20, 'height': 176, 'born': {'country': 'usa', 'city': 'dallas'}}
```

