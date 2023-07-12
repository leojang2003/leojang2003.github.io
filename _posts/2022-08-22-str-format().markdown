{:.note}

## str.format()

{% highlight Python %}
replacement_field ::=  "{" [field_name] ["!" conversion] [":" format_spec] "}"
field_name        ::=  arg_name ("." attribute_name | "[" element_index "]")*
arg_name          ::=  [identifier | digit+]
attribute_name    ::=  identifier
element_index     ::=  digit+ | index_string
index_string      ::=  <any source character except "]"> +
conversion        ::=  "r" | "s" | "a"
format_spec       ::=  <described in the next section>
{% endhighlight %}

## [field_name]

field_name 是 optional 的，放的是要被格式化的值，可以是數字或是 keyword，如果是數字，表示為 positional argument，如果是 keyword，則是 keyword argument，解釋如下
{% highlight Python %}

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
{% endhighlight %}

統整如下
{% highlight Python %}
"First, thou shalt count to {0}"  # 參考第一個 positional argument
"Bring me a {}"                   # 參考第一個 positional argument
"From {} to {}"                   # 等同 "From {} to {}"
"My quest is {name}"              # 參考 keyword argument 'name'
"Weight in tons {0.weight}"       # 參考第一個 positional argument 的 'weight' 屬性
"Units destroyed: {players[0]}"   # keyword argument 'players' 的第一個物件
{% endhighlight %}

上述最後兩個範例用新的例子解說如下
{% highlight Python %}
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
{% endhighlight %}

## ["!" conversion]

["!" conversion] 欄位會在 format 之前先做 type 目前共有3種 conversion flags '!s' 呼叫 str()， '!r' 會呼叫 repr()，'!a' 會呼叫 ascii()

{% highlight Python %}
"Harold's a clever {0!s}"        # Calls str() on the argument first
"Bring out the holy {name!r}"    # Calls repr() on the argument first
"More {!a}"                      # Calls ascii() on the argument first
{% endhighlight %}

{:.note}
repr() 會回傳物件的 printable representation
ascii() 會回傳物件的 printable representation，並使用 \x, \u or \U 跳脫非 ASCII 字元

## format_spec

{% highlight Python %}
format_spec     ::=  [[fill]align][sign][#][0][width][grouping_option][.precision][type]
fill            ::=  <any character>
align           ::=  "<" | ">" | "=" | "^"
sign            ::=  "+" | "-" | " "
width           ::=  digit+
grouping_option ::=  "_" | ","
precision       ::=  digit+
type            ::=  "b" | "c" | "d" | "e" | "E" | "f" | "F" | "g" | "G" | "n" | "o" | "s" | "x" | "X" | "%"
{% endhighlight %}

## alignment options

'<' 靠左右補空白(此為多數物件的預設)
{% highlight Python %}
'{0:3}'.format(x)  # 'a   ' str的預設就是左靠右補空白
'{0:<3}'.format(x) # 'a   ' 同上
{% endhighlight %}

'>' 靠右左補空白(此為數字的預設)
{% highlight Python %}
'{0:3}'.format(2)  # '  2' 數字的預設就是右靠左補空白
'{0:<3}'.format(2) # '  2' 同上
{% endhighlight %}

{:.note}
除非 minimum field width 有定義，不然欄位的長度都會等於傳入的長度，有 [align] 沒有 [width] 是沒用的

'=' 在sign之後/數字之前補空白，此選項僅適用於數字型別
{% highlight Python %}
'{0:=3}'.format(2)  # '  2'
'{0:=3}'.format(-2) # '- 2'
{% endhighlight %}

{:.note}
{0:n}'.format(2) 僅於左側補 n-1 個空白

'^' 置中
{% highlight Python %}
'{0:^3}'.format(-2) # ' 2 '
{% endhighlight %}

## sign option

只用於數字符號

'+' 正負數都要有+-符號
{% highlight Python %}
{'{0:*>+#5}.format(3)' # '000+3'
{'{0:*>+5}.format(3)'  # '000+3' 同樣結果 '#' 用於隔開 [width]
{'{0:*>+5}.format(-3)' # '000-3'
{% endhighlight %}

如果要強迫 padding 在 sign 之後，可以用 =
{% highlight Python %}
{'{0:*=+#5}.format(3)' # '+0003'
{'{0:*=+5}.format(3)'  # '+0003' 
{'{0:*=+5}.format(-3)' # '-0003'
{% endhighlight %}

'-' 負數有-符號，此為預設行為
{% highlight Python %}
{'{0:*>#5}.format(3)'  # '00003' 預設行為
{'{0:*>#5}.format(-3)' # '000-3' 預設行為

{'{0:*>-#5}.format(3)' # '00003'
{'{0:*>-5}.format(3)'  # '00003'
{'{0:*>-5}.format(-3)' # '000-3'
{% endhighlight %}

' ' 空白，表示前置空白用於正數，負號用於負數
{% highlight Python %}
{'{0:*> #5}.format(3)' # '000 3'
{'{0:*> 5}.format(3)'  # '000 3' 同樣結果 '#' 用於隔開 [width]
{'{0:*> 5}.format(-3)' # '000-3'
{% endhighlight %}

## '#' option
待補

## grouping_option
',' 用來表示數字的千分位的逗號
{% highlight Python %}
{'{0:*>5,}.format(3000000)' # '3,000,000'
{% endhighlight %}

"_" 針對浮點數和整數("d" presentation type)使用_做千分位區隔。對於整數 presentation type "b"、"o"、"x"和"X"，將每隔 4 位插入_。對於其他 presentation type，指定此選項是錯誤的。
{% highlight Python %}
{'{0:*>5_}.format(3000000.1314)' # '3_000_000.1314'
{'{0:*>5_}.format(3000000)' # '3_000_000'
{% endhighlight %}

## [width] option
width 定義'最小'欄位寬度，包含任何 prefixes、分隔符號與其他 formatting 字元。如果沒有特別設定，則以內容的長度為準

如果沒有特別設定 alignment，在 width 前面加上 '0' 開啟 sign-aware zero-padding for numeric types. 等同於使用=並補上'0'

{% highlight Python %}

'{0:8}'.format('leojang')  # 'leojang '
'{0:>8}'.format('leojang') # ' leojang'

{'{0:0=10_}.format(3000)'  # '00_003_000'
{'{0:010_}.format(3000)'   # '00_003_000' 兩者相同，沒設 align，設定 [0][width] 等同 [fill=0][align='=']
{% endhighlight %}

## [precision] option
顯示在小數點後要顯示幾位，需搭配 'f' 與 'F' 的 presentation type，或是 presentation type 'g'/'G' 的浮點述前後幾位。如果是 string presentation types，則表示'最大'欄位長度。數字 presentation type 不可使用 precision

{% highlight Python %}
{'0:0=4.3f'}.format(3000.14565) # 3000.146
{'0:0=4.3F'}.format(3000.14565) # 3000.146

{'0:0=6.3f'}.format(3000.14565) # 3000.146
{'0:0=6.3F'}.format(3000.14565) # 3000.146

{'0:0=10.3f'}.format(3000.14565) # 003000.146
{'0:0=10.3F'}.format(3000.14565) # 003000.146

{'0:0=10.3g'}.format(3000.14565) # 000003e+03
{'0:0=10.3G'}.format(3000.14565) # 000003E+03

'{0:>8.3}'.format('leojang') # '*****leo' 最大長度3，補到寬度8
{% endhighlight %}

## [type] option
type 決定資料如何呈現

string presentation type 可使用's'，字串預設可以忽略不用寫's' 
{% highlight Python %}
'{0:>8s}'.format('leojang') # 'leojang '
'{0:>8}'.format('leojang')  # 'leojang '
{% endhighlight %}

'b' 數字轉成二進位
{% highlight Python %}
'{0:0=4b}'.format(8) # '1000'
'{0:0=5b}'.format(8) # '01000'
{% endhighlight %}

'c' 數字轉成 unicode
'd' 數字轉成10進位，數字預設可以忽略不用寫'd' 
'o' 數字轉成8進位
'x' 數字轉成16進位，超過9用小寫英文字
'X' 數字轉成16進位，超過9用大寫英文字，加上#顯示是會加上0x
'n' 等同'd'，差別在於使用 local setting 插入數字分隔字元

{% highlight Python %}
'{0:x}'.format(31)	# 1f
'{0:X}'.format(31)	# 1F
'{0:#x}'.format(31)	# 0x1f
'{0:#X}'.format(31)	# 0X1F # 都變成大寫
{% endhighlight %}

'e' 科學符號
'e' 科學符號大寫
{% highlight Python %}
'{0:e}'.format(314151617)	# 3.141516e+08 沒有設定 precision，float則顯示6位
'{0:E}'.format(3000)		# 3.000000e+03 大寫E
'{0:e}'.format(0.003)		# 3.000000e-03
'{0:E}'.format(0.003)		# 3.000000e-03

'{0:.3e}'.format(314151617)		# 3.142e+08 有設定 precision，顯示設定的3位
{% endhighlight %}

'f' 浮點數，預設小數點後6位
'F' 同'f'，但將 nan 顯示 NAN，inf 顯示 INF
'g'
'G'
'n'
'%' 數字乘100加上 %
{% highlight Python %}
'{0:%}'.format(0.3) # 30.000000%
'{0:.0%}'.format(0.3) # 30%
{% endhighlight %}
