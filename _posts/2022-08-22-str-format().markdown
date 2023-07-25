---
layout: post
title: str.format()
subtitle: 
tags: []
comments: true
---

str.format() 的語法如下

```python
replacement_field ::=  "{" [field_name] ["!" conversion] [":" format_spec] "}"
field_name        ::=  arg_name ("." attribute_name | "[" element_index "]")*
arg_name          ::=  [identifier | digit+]
attribute_name    ::=  identifier
element_index     ::=  digit+ | index_string
index_string      ::=  <any source character except "]"> +
conversion        ::=  "r" | "s" | "a"
format_spec       ::=  <described in the next section>
```

<br/>

### [field_name]

field_name 是 optional 的，放的是要被格式化的值，可以是數字或是 keyword，如果是數字就是 positional argument，如果是 keyword，則是 keyword argument，解釋如下
```python

'first {} second {} third {}'.format(10, 20, 30) 
# 如果 positional argument 是依序傳入，則可以忽略如上
# first 10 second 20 third 30

'first {0} second {2}'.format(10, 20, 30)
# 如果 positional argument 非依序傳入，則需明確定義 index 如上
# first 10 second 30 

'first {0} second {x}'.format(10, 20, 30, x=40)
# field_name 是 keyword argument 
# first 10 second 40

# 將 dict 使用 ** 註記當成 keyword傳入
table = {'Sjoerd': 4127, 'Jack': 4098, 'Dcab': 8637678}
print('Jack: {Jack:d}; Sjoerd: {Sjoerd:d}; Dcab: {Dcab:d}'.format(**table))
Jack: 4098; Sjoerd: 4127; Dcab: 8637678
```

統整如下
```python
"First, thou shalt count to {0}"  # 參考第一個 positional argument
"Bring me a {}"                   # 參考第一個 positional argument
"From {} to {}"                   # 等同 "From {} to {}"
"My quest is {name}"              # 參考 keyword argument 'name'
"Weight in tons {0.weight}"       # 參考第一個 positional argument 的 'weight' 屬性
"Units destroyed: {players[0]}"   # keyword argument 'players' 的第一個物件
```

上述最後兩個範例用新的例子解說如下
```python
class Person:
	def __init__(self, age, name):
		self.age = age
		self.name = name

tom = Person(20, 'Tom')
tom.weapon = 'Axe'
weapons = ['Shield', 'Sword', 'Dagger']

'weapon {0.weapon}, name {0.name}'.format(tom) 
# weapon Axe, name Tom
# 第一個 positional argument 為 tom 物件，所以 {0.weapon} 就是 'Axe'

'weapon {x[2]}'.format(x=weapons) 
# {x[2]} 表示 keyword argument x 的第3個元素，也就是 'Dagger'
# weapon Dagger
```

<br/>

### ["!" conversion]

["!" conversion] 欄位會在 format 之前先做 type 目前共有3種 conversion flags '!s' 呼叫 str()， '!r' 會呼叫 repr()，'!a' 會呼叫 ascii()

```python
"Harold's a clever {0!s}"        # Calls str() on the argument first
"Bring out the holy {name!r}"    # Calls repr() on the argument first
"More {!a}"                      # Calls ascii() on the argument first
```

repr() 會回傳物件的 printable representation，如果物件有定義 _ _ repr _ _ () 則回傳自訂內容。
<br/>
ascii() 會回傳物件的 printable representation，並使用 \x, \u or \U 跳脫非 ASCII 字元

### format_spec

如上所述，

{: .box-note}
**Note:** replacement_field ::=  "{" [field_name] ["!" conversion] [":" format_spec] "}"

:冒號後面接的是 format_spec，規則如下

```python
format_spec     ::=  [[fill]align][sign][#][0][width][grouping_option][.precision][type]
fill            ::=  <any character>
align           ::=  "<" | ">" | "=" | "^"
sign            ::=  "+" | "-" | " "
width           ::=  digit+
grouping_option ::=  "_" | ","
precision       ::=  digit+
type            ::=  "b" | "c" | "d" | "e" | "E" | "f" | "F" | "g" | "G" | "n" | "o" | "s" | "x" | "X" | "%"
```
<br/>

### fill

要填入的，可以是任何的字元，需搭配其他使用如 align ... 等

```python
'{0:x>7}@'.format('apple')  
xxapple@
```

<br/>

### alignment options

'<' 靠左，右補空白(此為多數物件的預設)

大部分物件如果不特別註明 '<' 的話，多數物件也是預設靠左，右補空白(補空白也是大部分物件預設)。值得注意的是，有設 align 的話，不論是靠左右，都需要設定寬度 width ，format()要知道總長度要是多少。

```python
'{0:3}'.format('x', 'y')

# 'x  ' 右補 2 個空白
```

看上面的例子，0 是 positional argument 為 'x'，如果是 1 的話就是 'y'。冒號 : 說明接來是 format_spec 開始，因為沒有接 '<' 而字串是預設靠左。3 指的是 width，所以會右補 2 個空白，總長度為 3 的字串 'x  '。如果設定的 width 小於要靠左的字串，則無任何改變。

```python
'{0:3}'.format('apple)

# 'apple' 沒變
```

如果加上'<'結果是不變的
  
```python
'{0:3}'.format('x')
# 'x  ' 右補 2 個空白
```

'>' 靠右左補空白(此為數字的預設)

```python
'{0:3}'.format(2)  # '  2' 數字的預設就是右靠左補空白
'{0:<3}'.format(2) # '  2' 同上
```

{:.note}
除非 minimum field width 有定義，不然欄位的長度都會等於傳入的長度，有 [align] 沒有 [width] 是沒用的

'=' 強制在 sign 之後與數字之前補空白，此選項僅適用於數字型別

```python
'{0:=3}'.format(2)  # '  2'
'{0:=3}'.format(-2) # '- 2'
```

{:.note}
{0:n}'.format(2) 僅於左側補 n-1 個空白

'^' 置中

```python
'{0:^3}'.format(-2) # ' 2 '
```

<br/>

### sign option

只用於數字符號

'+' 正負數都要有+-符號
```python
{'{0:*>+#5}.format(3)' # '000+3'
{'{0:*>+5}.format(3)'  # '000+3' 同樣結果 '#' 用於隔開 [width]
{'{0:*>+5}.format(-3)' # '000-3'
```

如果要強迫 padding 在 sign 之後，可以用 =
```python
{'{0:*=+#5}.format(3)' # '+0003'
{'{0:*=+5}.format(3)'  # '+0003' 
{'{0:*=+5}.format(-3)' # '-0003'
```

'-' 負數有-符號，此為預設行為
```python
{'{0:*>#5}.format(3)'  # '00003' 預設行為
{'{0:*>#5}.format(-3)' # '000-3' 預設行為

{'{0:*>-#5}.format(3)' # '00003'
{'{0:*>-5}.format(3)'  # '00003'
{'{0:*>-5}.format(-3)' # '000-3'
```

' ' 空白，表示前置空白用於正數，負號用於負數
```python
{'{0:*> #5}.format(3)' # '000 3'
{'{0:*> 5}.format(3)'  # '000 3' 同樣結果 '#' 用於隔開 [width]
{'{0:*> 5}.format(-3)' # '000-3'
```
<br/>
### '#' option

'#' 是所謂的 alternate form，此選項僅對 integer、float、complex types 有效。對於 integer，當使用 binray、octal或 hexadecimal 輸出時，此選項將相應的前綴'0b、'0o'、'0x'或'0X'加到輸出

```python
'{0:>#5b}'.format(2)
# 說明 : positional argument 0 | format 符號 | # | 寬度 5 | 2進位輸出
#  0b10

'{0:>#5o}'.format(8)
# 說明 : positional argument 0 | format 符號 | # | 寬度 5 | 二進位輸出
#  0o10

'{0:>#5x}'.format(16)
# 說明 : positional argument 0 | format 符號 | # | 寬度 5 | 16進位輸出
#  0x10
```

{:.note}
補充說明，若是要建立2, 8, 16進位的變數的話，則在數字前面加上 0b, 0o, 0x 即可，var = 0b11 表示數字 3，var = 0o11 表示數字 9，var = 0x11 表示數字 17。

對於 float、complex type，使用 # 會強制顯示小數點，像是 2.0 會顯示 2.。通常，僅有小數點後面有數字時，才會顯示 .。

```python
'{0:>.0f}'.format(2)
# 說明 : positional argument 0 | format 符號 : | >靠右 | .0 小數點後0位 | float 輸出
# 2

'{0:>.1f}'.format(2)
# 2.0
```

上面可以看到如果是小數點後為 .0 的，不會顯示點(decimal-point character)，以上是正常情況

```python
'{0:>#.0f}'.format(2)
# 說明 : positional argument 0 | format 符號 | >靠右 | # | .0 小數點後0位 | float 輸出
# 2.

'{0:>.1f}'.format(2)
# 2.0
```
如果使用 #，可以看到就算是小數點後為 .0 的，也是會顯示點(decimal-point character)

此外，對於 g/G 的轉換，使用 # 則不會從結果中刪除尾隨零。

```python
'{0:>.4g}'.format(2)
# 說明 : positional argument 0 | format 符號 | >靠右 |.4 小數點後4位 | g 輸出
# 2.

'{0:>#.4g}'.format(2)
# 說明 : positional argument 0 | format 符號 | >靠右 | # | .4 小數點後4位 | g 輸出
# 2.000
```
{:.note}
在設定 g 輸出時，雖然設定了小數點後4位，實際只有顯示小數點後3位。

### grouping_option

',' 用來表示數字的千分位的逗號
```python
{'{0:*>5,}.format(3000000)' # '3,000,000'
```

"_" 針對浮點數和整數("d" presentation type)使用_做千分位區隔。對於整數 presentation type "b"、"o"、"x"和"X"，將每隔 4 位插入_。對於其他 presentation type，指定此選項是錯誤的。
```python
{'{0:*>5_}.format(3000000.1314)' # '3_000_000.1314'
{'{0:*>5_}.format(3000000)' # '3_000_000'
```

<br/>

### [width] option
width 定義'最小'欄位寬度，包含任何 prefixes、分隔符號與其他 formatting 字元。如果沒有特別設定，則以內容的長度為準

如果沒有特別設定 alignment，在 width 前面加上 '0' 開啟 sign-aware zero-padding for numeric types. 等同於使用=並補上'0'

```python

'{0:8}'.format('leojang')  # 'leojang '
'{0:>8}'.format('leojang') # ' leojang'

{'{0:0=10_}.format(3000)'  # '00_003_000'
{'{0:010_}.format(3000)'   # '00_003_000' 兩者相同，沒設 align，設定 [0][width] 等同 [fill=0][align='=']
```

<br/>

### [precision] option
顯示在小數點後要顯示幾位，需搭配 'f' 與 'F' 的 presentation type，或是 presentation type 'g'/'G' 的浮點述前後幾位。如果是 string presentation types，則表示'最大'欄位長度。數字 presentation type 不可使用 precision

```python
{'0:0=4.3f'}.format(3000.14565) # 3000.146
{'0:0=4.3F'}.format(3000.14565) # 3000.146

{'0:0=6.3f'}.format(3000.14565) # 3000.146
{'0:0=6.3F'}.format(3000.14565) # 3000.146

{'0:0=10.3f'}.format(3000.14565) # 003000.146
{'0:0=10.3F'}.format(3000.14565) # 003000.146

{'0:0=10.3g'}.format(3000.14565) # 000003e+03
{'0:0=10.3G'}.format(3000.14565) # 000003E+03

'{0:>8.3}'.format('leojang') # '*****leo' 最大長度3，補到寬度8
```

<br/>

### [type] option
type 決定資料如何呈現

string presentation type 可使用's'，字串預設可以忽略不用寫's' 
```python
'{0:>8s}'.format('leojang') # 'leojang '
'{0:>8}'.format('leojang')  # 'leojang '
```

'b' 數字轉成二進位
```python
'{0:0=4b}'.format(8) # '1000'
'{0:0=5b}'.format(8) # '01000'
```

'c' 數字轉成 unicode
'd' 數字轉成10進位，數字預設可以忽略不用寫'd' 
'o' 數字轉成8進位
'x' 數字轉成16進位，超過9用小寫英文字
'X' 數字轉成16進位，超過9用大寫英文字，加上#顯示是會加上0x
'n' 等同'd'，差別在於使用 local setting 插入數字分隔字元

```python
'{0:x}'.format(31)	# 1f
'{0:X}'.format(31)	# 1F
'{0:#x}'.format(31)	# 0x1f
'{0:#X}'.format(31)	# 0X1F # 都變成大寫
```

'e' 科學符號
'e' 科學符號大寫
```python
'{0:e}'.format(314151617)	# 3.141516e+08 沒有設定 precision，float則顯示6位
'{0:E}'.format(3000)		# 3.000000e+03 大寫E
'{0:e}'.format(0.003)		# 3.000000e-03
'{0:E}'.format(0.003)		# 3.000000e-03

'{0:.3e}'.format(314151617)		# 3.142e+08 有設定 precision，顯示設定的3位
```

'f' 浮點數，預設小數點後6位
'F' 同'f'，但將 nan 顯示 NAN，inf 顯示 INF
'g'
'G'
'n'
'%' 數字乘100加上 %
```python
'{0:%}'.format(0.3) # 30.000000%
'{0:.0%}'.format(0.3) # 30%
```
