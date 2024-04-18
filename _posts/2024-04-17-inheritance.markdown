---
layout: post
title: Inheritance & super()
subtitle: Python
tags: [inheritance, super]
comments: true
---

### 不接受參數的 super ( )

super ( ) 回傳一個 proxy 物件，一個暫時的父類別物件，讓我們可以存取父類別的方法

```python

class Animal:
            
    def __init__(self, legs):
        self.legs = legs
        print('Animal Legs :', legs)

    def walk(self):
        print('Animal walks with ', self.legs, 'legs')
              
class Human(Animal):
  
    def __init__(self):
        super().__init__(2)
        
    def walk(self):
        super().walk()
        print('Human walks with ', self.legs, 'legs')
 
lisa = Human()

```

我們可以看到 super ( ) . \_\_init__ ( 2 )，會將 Human 物件傳入 Animal 的 \_\_init__ ，所以 self . legs = legs 其實是將 legs 加到 Human 物件的 instance dict 中。

### 接受參數的 super ( )

super ( ) 也可以接受參數如下，type 是 subclass，obj 是 subclass 的實例，第二個參數傳入讓 super 回傳一個 bound method。

```python

class super(object)
|  super(type, obj) -> bound super object; requires isinstance(obj, type)

```

因此 Human 的 \_\_init__ 可以改寫成如下，據定義 super ( Human , self ) . \_\_init__( 2 ) 等同於 super ( ) . \_\_init__( 2 )

```python

class Human(Animal):
  
    def __init__(self):
        super(Human, self).__init__(2)
        
    def walk(self):
        super().walk()
        print('Human walks with ', self.legs, 'legs') 

```

Super 傳入的第一個類別，可以是別的類別(決定MRO用)，下面可以看到一個新的類別 Asian，walk 方法的第一個參數傳入如果是 Asian，三個 walk 方法都會呼叫。

```python

class Asian(Human):
  
    def __init__(self):
        super(Human, self).__init__(2)  
    
    def walk(self):
        super(Asian, self).walk()
        print('Asian walks with ', self.legs, 'legs')

lisa = Asian()
lisa.walk()

# Animal walks with  2 legs
# Human walks with  2 legs
# Asian walks with  2 legs

```

我們知道 Asian 的 mro ( ) 是 Asian > Human > Animal > object，若 Asian 的 walk 方法的第一個參數傳入的是 Human，則會從 Human 上一層 Animal 開始找尋 walk 方法。因此不會呼叫 Human 的 walk 方法。

```python

class Asian(Human):
  
    def __init__(self):
        super(Human, self).__init__(2)  
    
    def walk(self):
        super(Human, self).walk()
        print('Asian walks with ', self.legs, 'legs')

lisa = Asian()
lisa.walk()

# Animal walks with  2 legs
# Asian walks with  2 legs

```

{:.note}
但一般來說，沒有參數的 super ( ) 用法已足夠適用多數的情況

### 多重繼承中的 super ( ) 與 MRO

現在有一個新的類別繼承 Asian 與類別 Gender，MRO 告訴 Python 如何去搜尋繼承的方法，因為類別 Taiwanese 的 mro ( ) 為 Taiwanese > Gender > Asian > Human > Animal > object，MRO 會告訴 Python 如何依序呼叫 super ( )

```python

class Gender():
  
    def __init__(self, gender):
        self.gender = gender
    
    def walk(self):
        super().walk()
        print(self.gender, ' walks')

class Taiwanese(Gender, Asian):
    
    def __init__(self, gender, legs):
        self.gender = gender
        self.legs = legs
    
    def walk(self):
        super().walk()
        print('Taiwanese walks with ', self.legs, 'legs')

Taiwanese.mro()

shawn = Taiwanese('Man', 2)
shawn.walk()

# Animal walks with  2 legs
# Asian walks with  2 legs
# Man  walks
# Taiwanese walks with  2 legs

```

<br>
<br>
<br>
