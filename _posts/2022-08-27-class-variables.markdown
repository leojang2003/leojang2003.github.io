```python
class Dog:
    num_legs = 4

    def __init__(self, name):
        self.name = name

jack = Dog('Jack')
jill = Dog('Jill')

# 物件存取類別變數(class variable)
print('jack has ', jack.num_legs, 'legs') # jack has  4 legs
print('jill has ', jill.num_legs, 'legs') # jill has  4 legs

print(Dog.num_legs) # 4

# 修改類別變數，造成所有物件(即使是已經建立的) num_legs 變成 6
Dog.num_legs = 6

print('jack has ', jack.num_legs, 'legs') # jack has  6 legs
print('jill has ', jill.num_legs, 'legs') # jill has  6 legs

Dog.num_legs = 4 # 重置類別變數.
print('reset ....')
print('jack has ', jack.num_legs, 'legs') # jack has  4 legs
print('jill has ', jill.num_legs, 'legs') # jill has  4 legs

# 當物件取與類別變數同名的屬性時，物件會新增一個與類別變數同名的的物件變數，此物件變數會蓋掉同名的類別變數
jack.num_legs = 6
print('jack has ', jack.num_legs, 'legs') # jack has  6 legs

# 其他物件的類別變數的值，不受影響
print('jill has ', jill.num_legs, 'legs') # jill has  4 legs
```