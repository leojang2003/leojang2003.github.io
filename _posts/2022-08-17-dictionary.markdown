
使用 {} 來建立一個新的 dict
{% highlight Python %}
dict= {}

dict["a"] = 1
dict["b"] = 2

dict # {'a': 1, 'b': 2}
{% endhighlight %}

使用 dict() 來建立一個新的 dict，傳入一個 tuple 的 list
{% highlight Python %}
dict([('a', 1), ('b', 2), ('c', 3)])
{'a': 1, 'b': 2, 'c': 3}
{% endhighlight %}

使用 dict() 可以搭配 keyword argument，比較簡單
{% highlight Python %}
dict(sape=4139, guido=4127, jack=4098)
{'sape': 4139, 'guido': 4127, 'jack': 4098}
{% endhighlight %}

Dictionary 是由 key:value 組成，key 必須是 immutable 的物件，如果是只包含 strings 或是 numbers 的 tuple 也可以當 key，如果 tuple 包含 mutable 的物件，則不能當作 key，另外 list 也不能當作 key
{% highlight Python %}
dict = {(1,2): 1, (1,4): 2, (1,2): 6} # 也可以用 tuple 當作 dict 的 key

dict {(1,2): 6, (1,4): 2} # 這邊可以看到舊的key被新的key取代了

{% endhighlight %}

刪除 dict 的物件
{% highlight Python %}
dict = {(1,2): 1, (1,4): 2, (1,2): 6}
del dict[1,2] # 或是 del dict[(1,2)] 也行

dict # {(1,4): 2}
{% endhighlight %}

取得所有 dict 的 key，可以使用 list(dict)，依照插入的順序產生list，可以使用sorted()來做排序
{% highlight Python %}
dict = {"c": 1, "b": 2, "a": 3}

list(dict) # ['c', 'b', 'a']

sorted(list(dict)) # ['c', 'b', 'a'] 排序

{% endhighlight %}

dict 是否包含 key 也可以使用 in / not in
{% highlight Python %}
dict = {"c": 1, "b": 2, "a": 3}

'c' in dict # True
'd' in dict # False
'd' not in dict # True

{% endhighlight %}

建立 Dictionary Comprehension，如同 set comprehension，都是使用{}，但是使用的是冒號
{% highlight Python %}
power = {x: x**2 for x in (2, 4, 6)}
# {2: 4, 4: 16, 6: 36}
{% endhighlight %}





