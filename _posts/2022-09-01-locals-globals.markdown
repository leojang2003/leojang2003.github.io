# locals()

{:note}
名詞定義: function : 函式 / method : 方法

Update and return a dictionary representing the current local symbol table. Free variables are returned by locals() when it is called in function blocks, but not in class blocks. Note that at the module level, locals() and globals() are the same dictionary.
更新並回傳表示 current local symbol table 的 dictionary。在函式區塊而不是 class 區塊中呼叫 locals() 時會回傳自由變數 (Free variables)。請注意，在 module 階層中，locals() 和 globals() 是相同的 dictionary。

# globals()
Return the dictionary implementing the current module namespace. For code within functions, this is set when the function is defined and remains the same regardless of where the function is called.
回傳一個 dictionary，該 dictionary 實作目前的 module namespace。 對於在 function 來說，globals() 在 function 定義後執行的話，globals() 才會包含該 function


# free variables

變數 a 定義在 outer()，如果我們看 inner()，變數 a 沒有定義，但 inner() 有使用變數 a 因此變數 a 在 inner() function 叫做 Free Variable.

{% highlight Python %}
def outer():
    a = 20
  
    def inner():
        print('Value of a: %d' %(a))
    
    inner()

outer()
Output
{% endhighlight %}


測試如下
{% highlight Python %}
import builtins

author = 'Mike'

print(globals().keys())
# dict_keys(['__name__', '__doc__', ... 'builtins', 'author'])
print(locals().keys())
# dict 內容同上


def test_func():
	'''
		this is description of test_func
	'''		
	print(globals().keys())		
	print(locals().keys())		
	
test_func()
# 呼叫結果如下
# 同外層的 globals().keys() 多了 'test_func'
# dict_keys([]) author 此時非自由變數

def test_func2():    
    
    print(author)				# author 在此變成自由變數了，所以會出現在 locals()
    year = '2022'
    print(locals().keys())		


test_func2()
# 呼叫結果如下
# dict_keys(['author', 'year'])

class Person:
	
	author = 'Lisa'	

	# 以下兩行在呼叫 Person() 產生物件前就會先執行，
	print(globals().keys())		# 同外層的 globals().keys() 多了 'test_func', 'test_func2'
	print(locals().keys())		# dict_keys(['__module__', '__qualname__', 'author'])	
	
	def __init__(self):
		pass
		
	def a_run(self, distance):
		self.shoes = 'Nike' 	# locals() 不會增加 shoes
        shirt = 'Adidas'		# locals() 不會增加 shoes
        print(author)   		# Mike
        print(A.author) 		# Lisa
		print(globals().keys())	
		print(locals().keys())	


tom = Person()
tom.a_run(20)
# 同外層的 globals().keys() 多了 'test_func', 'test_func2', 'A', 'tom'
# dict_keys(['self', 'distance', 'shirt'])

print(tom.a_run.__func__)

{% endhighlight %}


		


