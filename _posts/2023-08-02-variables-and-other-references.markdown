---
layout: post
title: Variables and Other References
subtitle: 
tags: []
comments: true
---

節錄自 Python in a Nutshell 

{:.note}
名詞解釋 : 參考(reference)、變數(variable)、屬性(attribute)、項目item()、型態(type)、名稱(name) 

### References

Python 程式透過參考來存取資料。一個名稱參照到一個值(物件)的記憶體位置就是一個**參考**。參考可以是變數、屬性或項目等型式。參考背後綁定的物件沒有固定的型態。在某個時間點，參考綁定的物件是一個型態的，但隨著程式的執行，同一個參考可能會綁定到不同的型態。

> A Python program accesses data values through references. A reference is a name that refers to the specific location in memory of a value (object). References take the form of variables, attributes, and items. In Python, a variable or other reference has no intrinsic type. The object to which a reference is bound at a given time does have a type, however. Any given reference may be bound to objects of different types during the execution of a program.

```python

var = 1
l = [1,2,3]

class A
    var2 = 20

# 以下都是 reference 
var    # variable 
l[0]   # item
A.var2 # attribute  
```
<br/>

### Variables

在 Python 中是沒有變數宣告的。在綁定變數的 statement 執行後，變數才會存在，或者可以說，建立一個 name 來持有某個 object 的 reference。我們還可以通過重置 name 來取消綁定變數，使變數不再持有 reference。Assignment statements 是綁定變數和其他 reference 的最常見方法。del 語法可以取消綁定 reference。

> In Python, there are no declarations. The existence of a variable depends on a statement that binds the variable, or, in other words, that sets a name to hold a reference to some object. You can also unbind a variable by resetting the name so it no longer holds a reference. Assignment statements are the most common way to bind variables and other references. The del statement unbinds references. 

```python
# 執行 bind
var = 1
l = [1,2,3,4,5]
d = {'John':28, 'Bill':32}

# 執行 unbind
del l
```

綁定一個已綁定的 reference 也稱為再次綁定。重新綁定或再次綁定一個 reference 對被 reference 的物件沒有影響，除非當沒有任何 reference 到該 object 時該 object 才會消失。自動清理沒有 reference 的 object 稱為 garbage collection。  

> Binding a reference that was already bound is also known as rebinding it. Whenever binding is mentioned in this book, rebinding is implicitly included except where it is explicitly excluded. Rebinding or unbinding a reference has no effect on the object to which the reference was bound, except that an object disappears when nothing refers to it. The automatic cleanup of objects to which there are no references is known as garbage collection.  

我們可以使用 python 保留的 29 個 keyword 以外的任何 identifier 來命名 variable。variable 可以是 global variable 或 local variable。global varible 是 module object 的一個 attribute。local variable 存在於 function 的 local namespace 中。

> You can name a variable with any identifier except the 29 that are reserved as Python’s keywords (see Section 4.1.2.2 earlier in this chapter). A variable can be global or local. A global variable is an attribute of a module object. A local variable lives in a function’s local namespace  

### Object attributes and items

object 的 attribute 和 item 之間的差別在於用於存取它們的語法。object 的 attibute 表示如下， {指向 object 的 reference}.{attribute name}(即，x.y 指 object x 的名為 y 的 attribute)。  

> The distinction between attributes and items of an object is in the syntax you use to access them. An attribute of an object is denoted by a reference to the object, followed by a period (.), followed by an identifier called the attribute name (i.e., x.y refers to the attribute of object x that is named y).  

object 的 item 表示如下，{指向 object 的 reference}[{expression}]。括號中的 expression 稱為 item 的 index 或 key，object 是 item 的 container (即，x[y] 指 container object x 中位於 key 或 index y 的 item)。

> An item of an object is denoted by a reference to the object, followed by an expression within brackets ([ ]). The expression in brackets is called the index or key to the item, and the object is called the container of the item (i.e., x [ y ] refers to the item at key or index y in container object x).  

可呼叫的 attribute 也稱為 method。與其他語言不同，Python 在 callable 和 non-callable attribute 之間沒有嚴格分別。適用於 attribute 的一般規則同樣也適用於 callable attribute(a.k.a method)。

> Attributes that are callable are also known as methods. Python draws no strong distinction between callable and non-callable attributes, as other languages do. General rules about attributes also apply to callable attributes (methods).  

### 存取不存在的 reference

一個常見的程式錯誤是試著存取不存在的 reference。例如，variable 可能 unbound，或者 object 的 attribute name 或 item index 可能無效。Python 編譯器在分析和編譯原始碼時僅偵測 syntax error，編譯不會偵測 semantic error，semantic error 像是試著存取 unbound attribute、item 或 variable。  

Python 僅在程式執行時才偵測 semantic error。當某行程式是 semantic error 時，嘗試執行該程式會拋出例外(raise exception)。存取不存在的 attribute、item 或 variable，就像任何其他 semantic error 一樣，會拋出例外。

> A common programming error is trying to access a reference that does not exist. For example, a variable may be unbound, or an attribute name or item index may not be valid for the object to which you apply it. The Python compiler, when it analyzes and compiles source code, diagnoses only syntax errors. Compilation does not diagnose semantic errors such as trying to access an unbound attribute, item, or variable. Python diagnoses semantic errors only when the errant code executes, i.e., at runtime. When an operation is a Python semantic error, attempting it raises an exception (see Chapter 6). Accessing a nonexistent variable, attribute, or item, just like any other semantic error, raises an exception.  

### Assignment Statements

Assignment statements 可以是一般的，也可以是增量的。對 variable 做一般的 assignment (例如，name = value) 是建立新 variable，或將現有 variable rebind 到新值。對 object attribute 做一般的 assignment（例如，obj.attr = value）是對 object obj 建立 attribute attr 或 rebind attribute attr。對 container 中 item 的一般 assignment (例如，obj[key] = value) 是對 container obj 建立或 rebind 具有 index key 的 item。

> Assignment statements can be plain or augmented. Plain assignment to a variable (e.g., name = value) is how you create a new variable or rebind an existing variable to a new value. Plain assignment to an object attribute (e.g., obj.attr = value) is a request to object obj to create or rebind attribute attr. Plain assignment to an item in a container (e.g., obj [ key ]= value) is a request to container obj to create or rebind the item with index key.

增量賦值 (例如，name += value) 本身不能建立新的 reference。增量賦值可以 rebind variable，要求 object rebind 其**現有**的 attribute 或 item，或者要求 object 修改自身 (當然，object 可以在回應要求時建立任意新的 reference)。當我們向 object 發出要求時，由該 object 決定是接受要求還是拋出例外。

> Augmented assignment (e.g., name += value) cannot, per se, create new references. Augmented assignment can rebind a variable, ask an object to rebind one of its existing attributes or items, or request the target object to modify itself (an object may, of course, create arbitrary new references while responding to requests). When you make a request to an object, it is up to the object to decide whether to honor the request or raise an exception.

###  assignment

最簡單形式的普通 assignment語句的語法如下：    

target = expression  

當 assignment statement 執行時，Python 計算右側 expression，然後將該 expression 的值 bind 到左側 target。bind 與值的 type 無關。Python 不像其他一些語言那樣區分 callable object 和 non-callable object，因此我們可以將 functions、methods、types 與其他 callables 綁定到變數。

> A plain assignment statement in the simplest form has the syntax: target = expression
The target is also known as the left-hand side, and the expression as the right-hand side. When the assignment statement executes, Python evaluates the right-hand side expression, then binds the expression’s value to the left-hand side target. The binding does not depend on the type of the value. In particular, Python draws no strong distinction between callable and non-callable objects, as some other languages do, so you can bind functions, methods, types, and other callables to variables.

然而，綁定的細節確實取決於 target 的類型。賦值中的 target 可以是 identifier、attribute reference、indexing 或 slicing：

> Details of the binding do depend on the kind of target, however. The target in an assignment may be an identifier, an attribute reference, an indexing, or a slicing:

identifier 是變數的 name：對 identifier 的賦值會將變數與該 name 綁定。

> An identifier is a variable’s name: assignment to an identifier binds the variable with this name.

attribute reference 的語法為 obj.name。obj 是表示 object 的 expression，name 是 identifier，稱為 object 的 attribute name。對 attribute reference 的 assignment 是要求 object obj 綁定其名為 name 的 attribute。

> An attribute reference has the syntax obj.name. obj is an expression denoting an object, and name is an identifier, called an attribute name of the object. Assignment to an attribute reference asks object obj to bind its attribute named name.

indexing 的語法為 obj [ expr ]。 obj 和 expr 是表示任何 object 的 expression。對 indexing 的 assignment 是要求 container obj 綁定由 expr 的值（也稱為項目的索引或鍵）選擇的項目。

> An indexing has the syntax obj [ expr ]. obj and expr are expressions denoting any objects. Assignment to an indexing asks container obj to bind its item selected by the value of expr, also known as the index or key of the item.

slicing 的語法為 obj [ start:stop ] 或 obj [ start:stop:stride ]。obj、start、stop 和 stride 是表示任何 object 的 expression。start、stop 和 stride 都是 optional 的（即 obj [:stop :] 也是語法上正確的 slicing，相當於 obj [None:stop :None]）。對 slicing 的 assignment 要求 container obj 綁定或取消綁定它的一些 item。

> A slicing has the syntax obj [ start:stop ] or obj [ start:stop:stride ]. obj, start, stop, and stride are expressions denoting any objects. start, stop, and stride are all optional (i.e., obj [:stop :] is also a syntactically correct slicing, equivalent to obj [None:stop :None]). Assignment to a slicing asks container obj to bind or unbind some of its items.

當 assignment 的 target 是 identifier 時，assignment statement 指定變數的綁定。這是永遠不會被禁止的：當你提出要求時，它就會發生。在所有其他情況下，assignment statement 指定對 object 的請求以綁定其一個或多個 attribute 或 item。object 可能會拒絕建立或重新綁定某些（或全部）attribute 或 item，如果您嘗試不允許的建立或重新綁定，則會 raise exception。

> When the target of the assignment is an identifier, the assignment statement specifies the binding of a variable. This is never disallowed: when you request it, it takes place. In all other cases, the assignment statement specifies a request to an object to bind one or more of its attributes or items. An object may refuse to create or rebind some (or all) attributes or items, raising an exception if you attempt a disallowed creation or rebinding.

普通 assignment 中可以有多個 target 和等號(=)。例如：

> There can be multiple targets and equals signs (=) in a plain assignment. For example:

a = b = c = 0
將變數 a、b 和 c 綁定到值 0。每次執行該 statement 時，都會對右側 expression求值一次。每個 target 都綁定到 expression 回傳的 single object，就像幾個簡單的賦值相繼執行一樣。

> a = b = c = 0
> binds variables a, b, and c to the value 0. Each time the statement executes, the right-hand side expression is evaluated once. Each target gets bound to the single object returned by the expression, just as if several simple assignments executed one after the other.

普通 assignment 中的 target 可以列出兩個或多個用逗號分隔的 reference，可以選擇將其括在圓括號或方括號中。例如：

> The target in a plain assignment can list two or more references separated by commas, optionally enclosed in parentheses or brackets. For example:

a, b, c = x
這要求 x 是包含三個 item 的 sequence，並將 a 綁定到第一個 item，b 綁定到第二個 item，c 綁定到第三個 item。這種賦值稱為 unpacking assignment，一般來說，右側 expression 必須是一個 sequence，其 items 數與 target 中的 reference 的項數完全相同；否則，會raise exception。target 中的每個 reference 都綁定到 sequence 中的相應 item。unpacking assignment 還可以交換 reference：

> a, b, c = x
> This requires x to be a sequence with three items, and binds a to the first item, b to the second, and c to the third. This kind of assignment is called an unpacking assignment, and, in general, the right-hand side expression must be a sequence with exactly as many items as there are references in the target; otherwise, an exception is raised. Each reference in the target is bound to the corresponding item in the sequence. An unpacking assignment can also swap references:

a, b = b, a
這會重新綁定 a 以 reference b 所綁定的內容，反之亦然。

> a, b = b, a
> This rebinds a to refer to what b was bound to, and vice versa.

### Augmented assignment

增量 assignment 與普通 assignment 的不同之處在於，它使用增量 operator，所謂的增量 operator 就是二元 operator接一個等號，而不是在 target 和 expression 之間使用等號。增量 operator 有 +=、-=、*=、/=、//=、%=、**=、|=、>>=、<<=、&= 和 ^=。一項增量 assignment 在左側只能有一個 target；也就是說，增量 assignment 不支持多個 target。

> An augmented assignment differs from a plain assignment in that, instead of an equals sign (=) between the target and the expression, it uses an augmented operator: a binary operator followed by =. The augmented operators are +=, -=, *=, /=, //=, %=, **=, |=, >>=, <<=, &=, and ^=. An augmented assignment can have only one target on the left-hand side; that is, augmented assignment doesn’t support multiple targets.

在增量 assignment中，就像在普通 assignment中一樣，Python 首先計算右側 expression。然後，如果左側的 object 具有適用於 operator 的 in-place version 的 special method，則 Python 會使用右側值作為參數來呼叫該 special method。由該方法適當地修改左側 object 並回傳修改後的 object。如果左側 object 沒有適合的 in-place version 的 special method，Python 會將相對應的 binary operator 應用於左側和右側 object，然後將 target  reference 重新綁定到 operator 的結果。例如，當 x 有特殊方法 __iadd__ 時，x += y 類似於 x = x ._ _ iadd _ _(y)。否則 x += y 就像 x = x + y。

> In an augmented assignment, just as in a plain one, Python first evaluates the right-hand side expression. Then, if the left-hand side refers to an object that has a special method for the appropriate in-place version of the operator, Python calls the method with the right-hand side value as its argument. It is up to the method to modify the left-hand side object appropriately and return the modified object (Chapter 5 covers special methods). If the left-hand side object has no appropriate in-place special method, Python applies the corresponding binary operator to the left-hand side and right-hand side objects, then rebinds the target reference to the operator’s result. For example, x += y is like x = x ._ _iadd__( y ) when x has special method __iadd__. Otherwise x += y is like x = x + y.


增量 assignment 永遠不會建立其 target reference：執行增量 assignment 時，target 必須已經綁定。增量 assignment 可以將 target reference 重新綁定到新 object ，或者修改 target  reference 已綁定到 object 的值。相反的，普通 assignment 可以建立或重新綁定左側 target  reference，但它永遠不會修改 target reference 先前綁定到的 object （如果有）。object 和 object reference 之間的區別在這裡至關重要。例如，x = x + y 不會修改 name x 最初綁定的 object 。相反，它重新綁定 name x 以 reference 新 object 。相反，當 name x 綁定到的 object 具有特殊方法 __iadd__ 時，x += y 會修改該 object；否則，x += y 重新綁定 name x，就像 x = x + y 一樣。

> Augmented assignment never creates its target reference: the target must already be bound when augmented assignment executes. Augmented assignment can re-bind the target reference to a new object or modify the same object to which the target reference was already bound. Plain assignment, in contrast, can create or rebind the left-hand side target reference, but it never modifies the object, if any, to which the target reference was previously bound. The distinction between objects and references to objects is crucial here. For example, x = x + y does not modify the object to which name x was originally bound. Rather, it rebinds the name x to refer to a new object. x += y, in contrast, modifies the object to which the name x is bound when that object has special method __iadd__; otherwise, x += y rebinds the name x, just like x = x + y.

### del Statements

del 語句並不像字面的意思，並不會刪除 object ：相反，它會解除 reference 的綁定。當不再存在對 object 的 reference 時，object 就會被 garbage collection 刪除。

> Despite its name, a del statement does not delete objects: rather, it unbinds references. Object deletion may follow as a consequence, by garbage collection, when no more references to an object exist.

del 語句由關鍵字 del 和後跟一個或多個以逗號 (,) 分隔的 target reference 組成。每個 target 可以是變數、attribute reference、index 或 slicing，就像一樣 assignment statement，並且必須在 del 執行時有綁定。當 del target 是 identifier 時，del statement 指定變數的解除綁定。只要 identifier 已綁定，就永遠不會禁止取消綁定：當請求時，就會發生。

> A del statement consists of the keyword del, followed by one or more target references separated by commas (,). Each target can be a variable, attribute reference, indexing, or slicing, just like for assignment statements, and must be bound at the time del executes. When a del target is an identifier, the del statement specifies the unbinding of the variable. As long as the identifier is bound, unbinding it is never disallowed: when requested, it takes place.

在所有其他情況下，del 語句指定對 object 解除綁定其一個或多個 attribute 或 item 的請求。 object 可能會拒絕解除綁定某些（或全部）attribute 或 item，如果嘗試進行不允許的解除綁定，則會 raise exception。unbing slicing 通常與為該 slicing assign empty sequence 具有相同的效果，但 Python 交由 container object 來實現。

> In all other cases, the del statement specifies a request to an object to unbind one or more of its attributes or items. An object may refuse to unbind some (or all) attributes or items, raising an exception if a disallowed unbinding is attempted (see also __delattr__ in Chapter 5). Unbinding a slicing normally has the same effect as assigning an empty sequence to that slice, but it is up to the container object to implement this equivalence.

<br/>
<br/>
<br/>
