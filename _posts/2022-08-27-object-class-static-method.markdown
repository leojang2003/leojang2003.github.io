{% highlight Python %}
class Pizza:
    # 類別變數
    store = 'Papa John'

    # 透過 __init__ 建立的變數為物件變數
    def __init__(self, ingredients):
        self.ingredients = ingredients

    # def __repr__(self):
    #     return f'Pizza({self.ingredients!r})'

    @classmethod
    def margherita(cls):
        return cls(['mozzarella', 'tomatoes'])

    @classmethod
    def hawaii(cls):
        cls.zipcode = '75252'
        return __class__(['ham', 'pineapple'])

# 讀取類別變數
print(Pizza.store) # Papa John

# 透過類別存取物件變數會出錯
print(Pizza.ingredients) # type object 'Pizza' has no attribute 'ingredients'

# 變更類別變數
Pizza.store = 'Pizza Hut'

# 工廠方法建立物件
pizza1 = Pizza.margherita()
print(pizza1) # <__main__.Pizza object at ...>
print(pizza1.store) # 'Pizza Hut'
try:
    print(pizza1.zipcode) # 'Pizza Hut'
except AttributeError as e: # 'Pizza' object has no attribute 'zipcode'
    print(e)

# 工廠方法建立第二個物件
pizza2 = Pizza.hawaii()
print(pizza2) # <__main__.Pizza object at ...>
print(pizza2.store)
print(pizza2.zipcode) # 75252

# hawaii() 建立了類別別數 zipcode，而類別變數的變動會反映在所有物件上
# 所以現在 pizza1 也有 zipcode 值了
print(pizza1.zipcode) # 75252
{% endhighlight %}
