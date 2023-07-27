---
layout: post
title: Error And Exceptions
subtitle: 
tags: []
comments: true
---

如果沒有 exception，則 except 區塊不會執行，若是有 exception 且 exception name 一致，則會執行 except 區塊，若是 exception 沒有處理，則會傳到外層的 try，如果外層也沒處理，則變成未處理的 exception

```python
while True:
    try:
        x = int(input("Please enter a number: "))
        break
    except ValueError:
        print("Oops!  That was no valid number.  Try again...")
```

可以有多個 except，但只有一個會執行

也可以使用 tuple 

```python
except (RuntimeError, TypeError, NameError):
    pass
```

except 子句如果有吻合 exception 的話就會執行，如果 except 後的 exception 是丟出的 exception 的父類別時，則此 exception 子句會執行。反過來說，如果 except 後的 exception 是丟出的 exception 的子類別時，則不會執行 exception 子句。

{: .note}
父可以接子，子不可以接父

```python
class B(Exception):
    pass

class C(B):
    pass

class D(C):
    pass

# Exception 
#    |
#    B
#    |
#    C
#    |
#    D
	
for cls in [B, C, D]:
    try:
		print(issubclass(cls, Exception)) # 是否為子類別
		print(type(x:=cls()))
        raise x
    except D:
        print("D")
    except C:
        print("C")
    except B:
        print("B")

# True
# <class '__main__.B'>
# B

# True
# <class '__main__.C'>
# C

# True
# <class '__main__.D'>
# D
```

{:.note}
raise 後面可以接一個 exception 物件，或是一個 Exception 類別，如果是後者，則背後會建立該類別的物件

如果 except 順序倒過來，因為 C D 皆為 B 的子類別，所以只會執行 except B:

{: .note}
因為父可以接子，所以 B 可以接 C D

```python
class B(Exception):
    pass

class C(B):
    pass

class D(C):
    pass

for cls in [B, C, D]:
    try:
		print(issubclass(cls, Exception)) # 是否為子類別
		print(type(x:=cls()))
        raise x
	except B:
        print("B")
	except C:
        print("C")
    except D:
        print("D")

# True
# <class '__main__.B'>
# B

# True
# <class '__main__.C'>
# B

# True
# <class '__main__.D'>
# B
```

所有的 exception 都是繼承自 BaseException，所以可以使用該類別作為一個 wildcard。

```python
import sys

try:
    f = open('myfile.txt')
    s = f.readline()
    i = int(s.strip())
except OSError as err:
    print("OS error: {0}".format(err))
except ValueError:
    print("Could not convert data to an integer.")
except BaseException as err:
    print(f"Unexpected {err=}, {type(err)=}")
    raise
```

<br/>

### try ... except ... else

try … except 可以接 else，當 try 沒有異常時會執行

```python
py sample.py myfile.txt

for arg in sys.argv[1:]:
    try:
        f = open(arg, 'r')
    except OSError:
        print('cannot open', arg)
    else:
        print(arg, 'has', len(f.readlines()), 'lines')
        f.close()
		
# myfile.txt has 1 lines		
```

<br/>

### except ... as ...

```python
try:
    raise Exception('spam', 'eggs')
except Exception as inst:# 綁定 exception 物件到 inst
    print(type(inst))    # <class 'Exception'>
    print(inst.args)     # 參數儲存在 .args
    print(inst)          # 因為 exception 有定義 __str__()，所以可以直接印出參數
                         # 但此方法可能會被子類別覆寫
    x, y = inst.args     # unpack args
    print('x =', x)
    print('y =', y)
...
<class 'Exception'>
('spam', 'eggs')
('spam', 'eggs')
x = spam
y = eggs
```

<br/>

### 繼承 Exception 並覆寫 __str__() 方法

```python
class SubException(Exception):
		
	def __init__(self, *args):
		self.list = [x for x in args]
			
	# 覆寫 __str__() 方法
	def __str__(self):		
		return '***' + repr(self.list) + '***'

try:
    raise SubException('spam', 'eggs')
except Exception as inst:
    
    print(inst)          
	# SubException 覆寫 __str__() 方法，print() 顯示如下
	# ***['spam', 'eggs']***
```	

如果要知道是否有特定的 exception 但不想處理，則可以直接 raise

```python
try:
    raise NameError('HiThere')
except NameError:
    print('An exception flew by!')
    raise
```

### Exception Chaining

```python
# exc must be exception instance or None.
raise RuntimeError from exc
```

```python
def func():
    raise ConnectionError

try:
    func()
except ConnectionError as exc:
    raise RuntimeError('Failed to open database') from exc

Traceback (most recent call last):
  File "<stdin>", line 2, in <module>
  File "<stdin>", line 2, in func
ConnectionError

The above exception was the direct cause of the following exception:

Traceback (most recent call last):
  File "<stdin>", line 4, in <module>
RuntimeError: Failed to open database
```

在 except 和 finally 出現的 exception 會自動 exception chaining，若要關閉這樣的行為可以使用
```python
try:
    open('database.sqlite')
except OSError:
    raise RuntimeError from None

Traceback (most recent call last):
  File "<stdin>", line 4, in <module>
RuntimeError
```

## finally
如果有 finally 子句，則 finally 子句將作為 try 語句完成之前的最後一個任務執行。無論 try 語句是否產生e exception，finally 子句都會運行。以下幾點討論了發生異常時更複雜的情況：

如果在 try 子句執行期間發生異常，則該異常可以由 except 子句處理。如果異常未由 except 子句處理，則在 finally 子句執行後重新引發異常。

在執行 except 或 else 子句期間可能會發生異常。同樣，在 finally 子句執行後重新引發異常。

如果 finally 子句包含 return 語句，則返回值將是 finally 子句的 return 語句中的值，而不是 try 子句的 return 語句中的值。

```python
def bool_return():
    try:
        return True
    finally:
        return False
bool_return() # False
```

如果 finally 子句執行 break、continue 或 return 語句，則不會重新引發異常。如果 try 語句到達 break、continue 或 return 語句，finally 子句將在 break、continue 或 return 語句執行之前執行。

```python
def bool_return():
    try:
        raise ZeroDivisionError
    finally:
        return 'finally'
bool_return() # 'finally'
```
	
```python
def raise_exception():
	for i in range(5):
		if i == 3:
			try:
				raise Exception('i=3 exception')
			except ZeroDivisionError:
				print('except')
			else:
				print('else')
			finally:
				print('finally')
				break

raise_exception()
# finally
# 因為 finally 有 break 所以不會出現 exception
```		
