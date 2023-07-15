## 多重繼承

我們可以看到繼承的 2 個父類別，同時有 walk() 方法，當我們呼叫 Human 物件的 walk() 方法時，實際執行的 walk() 方法與繼承的順序有關，因為先繼承 Mamal，所以會執行 Mamal 類別的 walk()

```python
class Animal:
	
	blood = 'Red'
	
	def __init__(self, legs):
		self.legs = legs
		self.hands = f'Animal have {self.hands} hands'
		pass
	
	def walk(self):
		print(f'Animal walk with {self.legs}')
		
class Mammal:

	def __init__(self, legs):
		self.legs = legs
		self.hands = f'Mammal have {self.hands} hands'
		pass
	
	def walk(self):
		print(f'Mammal walk with {self.legs}')	
		
class Human(Mammal, Animal):
	
	gender = 'male'
	
	def __init__(self, legs):
		super().__init__(legs)		
	
	def walk(self):
		super().walk()
		
	def hands(self):
		print(self.num_hands)

Lisa = Human(3)
Lisa.walk() # Mammal walk with 3
```

如果將 Human 繼承的順序顛倒如下，當我們呼叫 Human 物件的 walk() 方法時，實際執行的會是 Animal 類別 walk() 

```python
class Human(Animal, Mammal):
	
	gender = 'male'
	
	def __init__(self, legs):
		super().__init__(legs)		
	
	def walk(self):
		super().walk()
		
	def hands(self):
		print(self.num_hands)

Lisa = Human(3)
Lisa.walk() # Animal walk with 3
```
