```python
```

## 繼承

```python
class Animal:
	
	def __init__(self, legs):
		self.legs = legs
		self.num_hands = 4 - legs
		pass
	
	def walk(self):
		print(f'walk with {legs}')
		
	def eat(self)
		print(f'animal eating')
		
class Human(Animal):

	def __init__(self, legs):
		super().__init__(legs)
		pass
	
	def walk(self):
		print(f'walk with {self.legs} legs')
		
	def hands(self):
		print(f'I have {self.num_hands} hands')
		
	def eat(self):
		Animal.eat(self)
		super().eat()
		
tom = Human(3)
tom.walk() 	# walk with 3 legs
tom.hands() # I have 1 hands
```

在 Human 的 __init__ 使用 super().__init__() 呼叫父類別的 __init__()，因此子類別可以使用 hands() 使用 num_hands

```python
# 也可以這樣呼叫
tom = Human(3)
lisa = Human(1)

Human.walk(tom))	# walk with 3 legs
Human.hands(lisa)	# I have 3 hands
```

在子類別的方法中呼叫父類別方法，可以使用以下 2 中方法

```python
# 要在 method 裡存取 class attributes，要使用 dot notation
Animal.eat(self)

# 呼叫 super()
super().eat()
```


