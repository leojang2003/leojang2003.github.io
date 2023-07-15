## Class Attribute

```python
class Car:
    
    model = 'Sedan'
    mileage = 10
        
    def roadTrip(self):
        Car.mileage *= 10
        
ford = Car()
nissan = Car()
ford.roadTrip()

print(ford.mileage)     # 100
print(nissan.mileage)   # 100

nissan.roadTrip()

print(ford.mileage)     # 1000
print(nissan.mileage)   # 1000
```

可以看到不同的 object 共用同一個 Class Attribute
