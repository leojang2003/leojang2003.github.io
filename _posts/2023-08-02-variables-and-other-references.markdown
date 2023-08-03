---
layout: post
title: Variables and Other References
subtitle: 
tags: []
comments: true
---

節錄自 Python in a Nutshell 

{:.note}
名詞解釋 : 參考(reference)、變數(variable)、屬性(attribute)、項目item()、類型(type)、名稱(name)、語句(statement)、賦值(assignment)、識別字(identifier)、模組(module)、函數(function)、方法(method)

### References

Python 程式透過參考來存取資料。一個名稱參照到一個值(物件)的記憶體位置就是一個**參考**。參考可以是變數、屬性或項目等型式。參考背後綁定的物件沒有固定的類型。在某個時間點，參考綁定的物件是一個類型的，但隨著程式的執行，同一個參考可能會綁定到不同的類型。

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

在 Python 中是沒有變數宣告的。在綁定變數的語句執行後，變數才會存在，或者可以說，建立一個名稱來持有某個物件的參考。我們還可以通過重置名稱來取消綁定變數，使變數不再持有參考。賦值語句是綁定變數和其他參考的最常見方法。del 語句可以取消綁定參考。

> In Python, there are no declarations. The existence of a variable depends on a statement that binds the variable, or, in other words, that sets a name to hold a reference to some object. You can also unbind a variable by resetting the name so it no longer holds a reference. Assignment statements are the most common way to bind variables and other references. The del statement unbinds references. 

```python
# 執行綁定
var = 1
l = [1,2,3,4,5]
d = {'John':28, 'Bill':32}

# 執行取消綁定
del l
```

綁定一個已綁定的參考也稱為重新綁定。重新綁定或取消綁定一個參考對被參考的物件沒有影響，除非當沒有任何參考到該物件時，該物件才會消失。自動清理沒有參考的物件稱為垃圾清理。  

> Binding a reference that was already bound is also known as rebinding it. Whenever binding is mentioned in this book, rebinding is implicitly included except where it is explicitly excluded. Rebinding or unbinding a reference has no effect on the object to which the reference was bound, except that an object disappears when nothing refers to it. The automatic cleanup of objects to which there are no references is known as garbage collection.  

我們可以使用 Python 保留的 29 個關鍵字以外的任何識別字來命名變數。變數可以是全域變數或區域變數。全域變數是模組物件的一個屬性。區域變數存在於函數的區域命名空間中。

> You can name a variable with any identifier except the 29 that are reserved as Python’s keywords (see Section 4.1.2.2 earlier in this chapter). A variable can be global or local. A global variable is an attribute of a module object. A local variable lives in a function’s local namespace  

### Object attributes and items

物件的屬性和項目之間的差別在於用於存取它們的語法。物件的屬性表示如下，{ 指向物件的屬性 }.{ 屬性名稱 } ( 即，x.y 指物件 x 的名為 y 的屬性 )。  

> The distinction between attributes and items of an object is in the syntax you use to access them. An attribute of an object is denoted by a reference to the object, followed by a period (.), followed by an identifier called the attribute name (i.e., x.y refers to the attribute of object x that is named y).  

物件的表示如下，{ 指向物件的參考 }[ {表達式} ]。括號中的表達式稱為項目的索引或鍵值，物件是項目的容器 ( 即，x[y] 指容器物件 x 中位於索引或鍵值 y 的項目 )。

> An item of an object is denoted by a reference to the object, followed by an expression within brackets ([ ]). The expression in brackets is called the index or key to the item, and the object is called the container of the item (i.e., x [ y ] refers to the item at key or index y in container object x).  

可呼叫的屬性也稱為方法。與其他語言不同，Python 在可呼叫和不可呼叫屬性之間沒有嚴格分別。適用於屬性的一般規則同樣也適用於可呼叫屬性(方法)。

> Attributes that are callable are also known as methods. Python draws no strong distinction between callable and non-callable attributes, as other languages do. General rules about attributes also apply to callable attributes (methods).  

### 存取不存在的 reference

一個常見的程式錯誤是試著存取不存在的參考。例如，變數可能是未綁定，或者物件的屬性名稱或項目索引可能無效。Python 編譯器在分析和編譯原始碼時僅偵測語法錯誤，編譯不會偵測語義錯誤，語意錯誤像是試著存取未綁定屬性、項目或變數。  

Python 僅在程式執行時才偵測語義錯誤。當某行程式是語義錯誤時，嘗試執行該程式會拋出例外。存取不存在的屬性、項目或變數，就像任何其他語義錯誤一樣，會拋出例外。

> A common programming error is trying to access a reference that does not exist. For example, a variable may be unbound, or an attribute name or item index may not be valid for the object to which you apply it. The Python compiler, when it analyzes and compiles source code, diagnoses only syntax errors. Compilation does not diagnose semantic errors such as trying to access an unbound attribute, item, or variable. Python diagnoses semantic errors only when the errant code executes, i.e., at runtime. When an operation is a Python semantic error, attempting it raises an exception (see Chapter 6). Accessing a nonexistent variable, attribute, or item, just like any other semantic error, raises an exception.  

### Assignment Statements

賦值語句可以是一般的，也可以是增量的。對變數做一般的賦值 (例如，name = value) 是建立新變數，或將現有變數重新綁定到新值。對物件屬性 做一般的賦值（例如，obj.attr = value）是對物件 obj 建立屬性 attr 或重新綁定屬性 attr。對容器中項目的一般賦值 (例如，obj[key] = value) 是對容器 obj 建立或重新綁定具有索引鍵值的項目。

> Assignment statements can be plain or augmented. Plain assignment to a variable (e.g., name = value) is how you create a new variable or rebind an existing variable to a new value. Plain assignment to an object attribute (e.g., obj.attr = value) is a request to object obj to create or rebind attribute attr. Plain assignment to an item in a container (e.g., obj [ key ]= value) is a request to container obj to create or rebind the item with index key.

增量賦值 (例如，name += value) 本身不能建立新的參考。增量賦值可以重新綁定變數，要求物件重新綁定其**現有**的屬性或項目，或者要求物件修改自身 (當然，物件可以在回應要求時建立任意新的參考)。當我們向物件發出要求時，由該物件決定是接受要求還是拋出例外。

> Augmented assignment (e.g., name += value) cannot, per se, create new references. Augmented assignment can rebind a variable, ask an object to rebind one of its existing attributes or items, or request the target object to modify itself (an object may, of course, create arbitrary new references while responding to requests). When you make a request to an object, it is up to the object to decide whether to honor the request or raise an exception.

###  assignment

最簡單形式的一般賦值語句的語法如下：    

target = expression  

當賦值語句執行時，Python 計算右側表達式，然後將該表達式的值綁定到左側目標。綁定與值的類型無關。Python 不像其他一些語言那樣區分可呼叫物件和不可呼叫物件，因此我們可以將函數、方法、類型與其他 callables 綁定到變數。

> A plain assignment statement in the simplest form has the syntax: target = expression
The target is also known as the left-hand side, and the expression as the right-hand side. When the assignment statement executes, Python evaluates the right-hand side expression, then binds the expression’s value to the left-hand side target. The binding does not depend on the type of the value. In particular, Python draws no strong distinction between callable and non-callable objects, as some other languages do, so you can bind functions, methods, types, and other callables to variables.

然而，綁定的細節確實取決於目標的類型。賦值中的目標可以是識別字、屬性參考、索引或切片：

> Details of the binding do depend on the kind of target, however. The target in an assignment may be an identifier, an attribute reference, an indexing, or a slicing:

識別字是變數的名稱：對識別字的賦值會將變數與該名稱綁定。

> An identifier is a variable’s name: assignment to an identifier binds the variable with this name.

屬性參考的語法為 obj.name。obj 是表示物件的表達式，name 是識別字，稱為物件的屬性名稱。對屬性參考的賦值是要求物件 obj 綁定其名為 name 的屬性。

> An attribute reference has the syntax obj.name. obj is an expression denoting an object, and name is an identifier, called an attribute name of the object. Assignment to an attribute reference asks object obj to bind its attribute named name.

索引的語法為 obj [ expr ]。obj 和 expr 是表示任何物件的表達式。對索引的賦值是要求容器 obj 綁定由 expr 的值（也稱為項目的索引或鍵）選擇的項目。

> An indexing has the syntax obj [ expr ]. obj and expr are expressions denoting any objects. Assignment to an indexing asks container obj to bind its item selected by the value of expr, also known as the index or key of the item.

切片的語法為 obj [ start:stop ] 或 obj [ start:stop:stride ]。obj、start、stop 和 stride 是表示任何物件的表達式。start、stop 和 stride 都是非強制的（即 obj [:stop :] 也是語法上正確的切片，相當於 obj [None:stop :None]）。對切片的賦值要求容器 obj 綁定或取消綁定它的一些項目。

> A slicing has the syntax obj [ start:stop ] or obj [ start:stop:stride ]. obj, start, stop, and stride are expressions denoting any objects. start, stop, and stride are all optional (i.e., obj [:stop :] is also a syntactically correct slicing, equivalent to obj [None:stop :None]). Assignment to a slicing asks container obj to bind or unbind some of its items.

當賦值的目標是識別字時，賦值語句指定變數的綁定。這是永遠不會被禁止的：當你提出要求時，它就會發生。在所有其他情況下，賦值語句指定對物件的請求以綁定其一個或多個屬性或項目。物件可能會拒絕建立或重新綁定某些（或全部）屬性或項目，如果您嘗試不允許的建立或重新綁定，則會拋出例外。

> When the target of the assignment is an identifier, the assignment statement specifies the binding of a variable. This is never disallowed: when you request it, it takes place. In all other cases, the assignment statement specifies a request to an object to bind one or more of its attributes or items. An object may refuse to create or rebind some (or all) attributes or items, raising an exception if you attempt a disallowed creation or rebinding.

普通賦值中可以有多個目標和等號(=)。例如：

> There can be multiple targets and equals signs (=) in a plain assignment. For example:

a = b = c = 0
將變數 a、b 和 c 綁定到值 0。每次執行該語句時，都會對右側表達式求值一次。每個目標都綁定到表達式回傳的單一物件，就像幾個簡單的賦值相繼執行一樣。

> a = b = c = 0
> binds variables a, b, and c to the value 0. Each time the statement executes, the right-hand side expression is evaluated once. Each target gets bound to the single object returned by the expression, just as if several simple assignments executed one after the other.

普通賦值中的目標可以列出兩個或多個用逗號分隔的參考，可以選擇將其括在圓括號或方括號中。例如：

> The target in a plain assignment can list two or more references separated by commas, optionally enclosed in parentheses or brackets. For example:

a, b, c = x
這要求 x 是包含三個項目的序列，並將 a 綁定到第一個項目，b 綁定到第二個項目，c 綁定到第三個項目。這種賦值稱為 unpacking assignment，一般來說，右側表達式必須是一個序列，其項目數與目標中的參考的項數完全相同；否則，會拋出例外。目標中的每個參考都綁定到序列中的相應項目。unpacking assignment 還可以交換參考：

> a, b, c = x
> This requires x to be a sequence with three items, and binds a to the first item, b to the second, and c to the third. This kind of assignment is called an unpacking assignment, and, in general, the right-hand side expression must be a sequence with exactly as many items as there are references in the target; otherwise, an exception is raised. Each reference in the target is bound to the corresponding item in the sequence. An unpacking assignment can also swap references:

a, b = b, a
這會重新綁定 a 以參考 b 所綁定的內容，反之亦然。

> a, b = b, a
> This rebinds a to refer to what b was bound to, and vice versa.

### Augmented assignment

增量賦值與普通賦值的不同之處在於，它使用增量運算子，所謂的增量運算子就是二元運算子接一個等號，而不是在目標和表達式之間使用等號。增量運算子有 +=、-=、*=、/=、//=、%=、**=、|=、>>=、<<=、&= 和 ^=。一項增量賦值在左側只能有一個目標；也就是說，增量賦值不支持多個目標。

> An augmented assignment differs from a plain assignment in that, instead of an equals sign (=) between the target and the expression, it uses an augmented operator: a binary operator followed by =. The augmented operators are +=, -=, *=, /=, //=, %=, **=, |=, >>=, <<=, &=, and ^=. An augmented assignment can have only one target on the left-hand side; that is, augmented assignment doesn’t support multiple targets.

在增量賦值中，就像在普通賦值中一樣，Python 首先計算右側表達式。然後，如果左側的物件具有適用於運算子的 in-place version 的 special method，則 Python 會使用右側值作為參數來呼叫該特殊方法。由該方法適當地修改左側 物件並回傳修改後的物件。如果左側 物件沒有適合的 in-place version 的特殊方法，Python 會將相對應的二元運算子應用於左側和右側物件，然後將目標參考重新綁定到運算子的結果。例如，當 x 有特殊方法 _ _ iadd _ _ 時，x += y 類似於 x = x ._ _ iadd _ _(y)。否則 x += y 就像 x = x + y。

> In an augmented assignment, just as in a plain one, Python first evaluates the right-hand side expression. Then, if the left-hand side refers to an object that has a special method for the appropriate in-place version of the operator, Python calls the method with the right-hand side value as its argument. It is up to the method to modify the left-hand side object appropriately and return the modified object (Chapter 5 covers special methods). If the left-hand side object has no appropriate in-place special method, Python applies the corresponding binary operator to the left-hand side and right-hand side objects, then rebinds the target reference to the operator’s result. For example, x += y is like x = x ._ _iadd__( y ) when x has special method __iadd__. Otherwise x += y is like x = x + y.


增量賦值永遠不會建立其目標的參考：執行增量賦值時，目標必須已經綁定。增量賦值可以將目標參考重新綁定到新物件，或者修改目標參考已綁定到物件的值。相反的，普通賦值可以建立或重新綁定左側目標參考，但它永遠不會修改目標參考先前綁定到的 物件（如果有）。物件和物件參考之間的區別在這裡至關重要。例如，x = x + y 不會修改名稱 x 最初綁定的物件。相反，它重新綁定名稱 x 以參考新物件。相反，當名稱 x 綁定到的物件具有特殊方法 _ _ iadd _ _ 時，x += y 會修改該物件；否則，x += y 重新綁定名稱 x，就像 x = x + y 一樣。

> Augmented assignment never creates its target reference: the target must already be bound when augmented assignment executes. Augmented assignment can re-bind the target reference to a new object or modify the same object to which the target reference was already bound. Plain assignment, in contrast, can create or rebind the left-hand side target reference, but it never modifies the object, if any, to which the target reference was previously bound. The distinction between objects and references to objects is crucial here. For example, x = x + y does not modify the object to which name x was originally bound. Rather, it rebinds the name x to refer to a new object. x += y, in contrast, modifies the object to which the name x is bound when that object has special method __iadd__; otherwise, x += y rebinds the name x, just like x = x + y.

### del Statements

del 語句並不像字面的意思，並不會刪除 物件：相反，它會解除參考的綁定。當不再存在對物件的參考時，物件就會被 garbage collection 刪除。

> Despite its name, a del statement does not delete objects: rather, it unbinds references. Object deletion may follow as a consequence, by garbage collection, when no more references to an object exist.

del 語句由關鍵字 del 和後跟一個或多個以逗號 (,) 分隔的目標參考組成。每個目標可以是變數、屬性參考、索引或切片，就像一樣賦值語句，並且必須在 del 執行時有綁定。當 del 目標是識別字時，del 語句指定變數的解除綁定。只要識別字已綁定，就永遠不會禁止取消綁定：當請求時，就會發生。

> A del statement consists of the keyword del, followed by one or more target references separated by commas (,). Each target can be a variable, attribute reference, indexing, or slicing, just like for assignment statements, and must be bound at the time del executes. When a del target is an identifier, the del statement specifies the unbinding of the variable. As long as the identifier is bound, unbinding it is never disallowed: when requested, it takes place.

在所有其他情況下，del 語句指定對物件解除綁定其一個或多個屬性或項目 的請求。物件可能會拒絕解除綁定某些（或全部）屬性或項目，如果嘗試進行不允許的解除綁定，則會拋出例外。取消綁定切片通常與為該指派空的序列具有相同的效果，但 Python 交由容器物件來實現。

> In all other cases, the del statement specifies a request to an object to unbind one or more of its attributes or items. An object may refuse to unbind some (or all) attributes or items, raising an exception if a disallowed unbinding is attempted (see also __delattr__ in Chapter 5). Unbinding a slicing normally has the same effect as assigning an empty sequence to that slice, but it is up to the container object to implement this equivalence.

<br/>
<br/>
<br/>
