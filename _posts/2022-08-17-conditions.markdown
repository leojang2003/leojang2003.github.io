### in / not in 比較運算子

```python
s = set('abcdef') # 初始化一個set

print ('x' in s) # False
print ('x' not in s) # True

```
<br/>
### is / is not 比較運算子

```python
class Node:

    def __init__(self, val)
        self.val = val
		
node = Node(10)
node2 = node

print(node is node2) # True
print(node is not node2) # False

```
<br/>
### and / or / not 的優先順序

and, or, not 的優先序是低於一般運算子(+-*/)，3者之間的優先序為 not > and > or，所以 A and not B or C 等同於 (A and (not B)) or C。and / or 是所謂的 short-circuit 運算子，以 A and B and C 為例，假設 A and B 為 False，A and C 為 True，則在評估 A and B 為 False 時就會結束，不會評估繼續評估 C。

<br/>
### 將比較的結果或是其他 boolean expression 指派到變數

```python
string1, string2, string3 = '', 'Trondheim', 'Hammer Dance'
non_null = string1 or string2 or string3
non_null # 'Trondheim'
```

{:.note}
注意這裡的 string1, string2, string3 = '', 'Trondheim', 'Hammer Dance' 這樣一個多重指派(assignment)，其實就是將 tuple 做 unpack 的動作，右側的 tuple 的括號省略了，string1, string2, string3 也可以寫成('', 'Trondheim', 'Hammer Dance')

### walrus 運算子

walrus 運算子可以在 expression 裡做 assignment，可用於複雜的運算式中，如果想要看部分運算式的值時，可以將部分運算式使用 := 指派給一個變數，後續就可以讀取這個變數

```python
sum = 20 + (3+4*2) - 9
# 假設我們想要取得 3+4*2 的值，則我們可以使用 :=
sum = 20 + (mid := 3+4*2) - 9 

sum # 22
mid # 11
```

再舉一個例子
```python
import random
import time

while x:=random.randint(0,3)
    print(x)
    time.sleep(5)
```
<br/>
### 比較是可以串連的

```python
a < b == c tests # a 是否小於 b 且 b 是否等於 c
```
<br/>
### 物件比較

sequence 物件可以與具有相同 sequence type 的其他物件進行比較。比較方式使用 lexicographical ordering：首先比較前兩項，如果它們不同，則確定比較的結果；如果它們相等，則比較接下來的兩項，依此類推，直到任一 sequence 用完。

如果要比較的兩個 sequence 是相同類型的序列，則以遞歸方式進行 lexicographical 比較。
如果兩個 sequence 的所有項比較相等，則認為序列相等。
如果一個 sequence 是另一個 sequence 的初始子序列，則較短的 sequence 是較小的 sequence。

```python
(1, 2, 3)              < [1, 2, 4]

(1, 2, 3)              < (1, 2, 4)
[1, 2, 3]              < [1, 2, 4]
'ABC' < 'C' < 'Pascal' < 'Python'
(1, 2, 3, 4)           < (1, 2, 4) 
(1, 2)                 < (1, 2, -1) 
(1, 2, 3)             == (1.0, 2.0, 3.0)
(1, 2, ('aa', 'ab'))   < (1, 2, ('abc', 'a'), 4)
```

{:.note}
dict 沒有支援比較，set 比較請用內建方法 union() 聯集、intersection() 交集、difference() 差集，a.difference(b) 與 b.difference(a) 是不同的、symmetric_difference()  等同聯集減掉交集

<br/>
<br/>
<br/>
