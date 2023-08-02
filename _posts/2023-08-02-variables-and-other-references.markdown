---
layout: post
title: Variables and Other References
subtitle: 
tags: []
comments: true
---

節錄自 Python in a Nutshell 

### References

Python 程式透過__參考__來存取資料。參考是一個名稱參照到記憶體特定位置的值(物件)。參考可以是 variables、attributes、items。Python 中的變數或其他參考是沒有固定的 type 的。參考在某個時間點綁定的物件是有 type 的，但隨著程式的執行，任何既有的參考可能會綁定到不同的 type。

> A Python program accesses data values through references. A reference is a name that refers to the specific location in memory of a value (object). References take the form of variables, attributes, and items. In Python, a variable or other reference has no intrinsic type. The object to which a reference is bound at a given time does have a type, however. Any given reference may be bound to objects of different types during the execution of a program.    

### Variables

In Python, there are no declarations. The existence of a variable depends on a statement that binds the variable, or, in other words, that sets a name to hold a reference to some object. You can also unbind a variable by resetting the name so it no longer holds a reference. Assignment statements are the most common way to bind variables and other references. The del statement unbinds references.  

在 Python 中是沒有變數宣告的。變數只有在綁定該變數的 statement 後才會存在，或者可以說，建立一個 name  來持有某個 object 的 reference。我們還可以通過重置 name 來取消綁定變數，使該變數不再持有 reference。Assignment statements 是綁定變數和其他 reference 的最常見方法。 del 語法可以取消綁定 reference。  

Binding a reference that was already bound is also known as rebinding it. Whenever binding is mentioned in this book, rebinding is implicitly included except where it is explicitly excluded. Rebinding or unbinding a reference has no effect on the object to which the reference was bound, except that an object disappears when nothing refers to it. The automatic cleanup of objects to which there are no references is known as garbage collection.  

綁定一個已綁定的 reference 也稱為 rebinding。rebinding 或 unbinding 一個 reference 對被 reference 的物件沒有影響，除非當沒有任何 reference 到該 object 時該 object 會消失。自動清理沒有 reference 的 object 稱為 garbage collection。  

You can name a variable with any identifier except the 29 that are reserved as Python’s keywords (see Section 4.1.2.2 earlier in this chapter). A variable can be global or local. A global variable is an attribute of a module object (Chapter 7 covers modules). A local variable lives in a function’s local namespace (see Section 4.10 later in this chapter).  

我們可以使用 python 保留的 29 個 keyword 以外的任何 identifier 來命名變數。變數可以是 global variable 或 local variable。global varible 是 module object 的一個 attribute。local variable 存在於 function 的 local namespace 中。

### Object attributes and items

The distinction between attributes and items of an object is in the syntax you use to access them. An attribute of an object is denoted by a reference to the object, followed by a period (.), followed by an identifier called the attribute name (i.e., x.y refers to the attribute of object x that is named y).  

object 的 attribute 和 item 之間的差別在於用於存取它們的語法。object 的 attibute 表示如下， {object 的 reference}.{attribute name}（即，x.y 指 object x 的名為 y 的 attribute）。  

An item of an object is denoted by a reference to the object, followed by an expression within brackets ([ ]). The expression in brackets is called the index or key to the item, and the object is called the container of the item (i.e., x [ y ] refers to the item at key or index y in container object x).  

object 的 item 表示如下，{object 的 reference}[{expression}]。括號中的表達式稱為該 item 的 index 或 key，該 object 稱為該 item 的 container（即，x [ y ] 指 container object x 中位於 key 或 index y 的 item）。    

Attributes that are callable are also known as methods. Python draws no strong distinction between callable and non-callable attributes, as other languages do. General rules about attributes also apply to callable attributes (methods).  

可呼叫的 attribute 也稱為 method。與其他語言不同，Python 在 callable 和 non-callable attribute 之間沒有嚴格區分。有關 attribute 的一般規則也適用於 callable attribute (method)。

### Accessing nonexistent references

A common programming error is trying to access a reference that does not exist. For example, a variable may be unbound, or an attribute name or item index may not be valid for the object to which you apply it. The Python compiler, when it analyzes and compiles source code, diagnoses only syntax errors. Compilation does not diagnose semantic errors such as trying to access an unbound attribute, item, or variable. Python diagnoses semantic errors only when the errant code executes, i.e., at runtime. When an operation is a Python semantic error, attempting it raises an exception (see Chapter 6). Accessing a nonexistent variable, attribute, or item, just like any other semantic error, raises an exception.  

一個常見的編程錯誤是嘗試訪問不存在的引用。例如，變量可能未綁定，或者屬性名稱或項目索引對於應用它的對象可能無效。 Python 編譯器在分析和編譯源代碼時僅診斷語法錯誤。編譯不會診斷語義錯誤，例如嘗試訪問未綁定的屬性、項目或變量。 Python 僅在錯誤代碼執行時（即運行時）診斷語義錯誤。當某個操作是 Python 語義錯誤時，嘗試執行該操作會引發異常（請參閱第 6 章）。訪問不存在的變量、屬性或項目，就像任何其他語義錯誤一樣，會引發異常。

Assignment Statements
Assignment statements can be plain or augmented. Plain assignment to a variable (e.g., name = value) is how you create a new variable or rebind an existing variable to a new value. Plain assignment to an object attribute (e.g., obj.attr = value) is a request to object obj to create or rebind attribute attr. Plain assignment to an item in a container (e.g., obj [ key ]= value) is a request to container obj to create or rebind the item with index key.

Augmented assignment (e.g., name += value) cannot, per se, create new references. Augmented assignment can rebind a variable, ask an object to rebind one of its existing attributes or items, or request the target object to modify itself (an object may, of course, create arbitrary new references while responding to requests). When you make a request to an object, it is up to the object to decide whether to honor the request or raise an exception.

Plain assignment
A plain assignment statement in the simplest form has the syntax:

                     target = expression
The target is also known as the left-hand side, and the expression as the right-hand side. When the assignment statement executes, Python evaluates the right-hand side expression, then binds the expression’s value to the left-hand side target. The binding does not depend on the type of the value. In particular, Python draws no strong distinction between callable and non-callable objects, as some other languages do, so you can bind functions, methods, types, and other callables to variables.

Details of the binding do depend on the kind of target, however. The target in an assignment may be an identifier, an attribute reference, an indexing, or a slicing:

An identifier is a variable’s name: assignment to an identifier binds the variable with this name.

An attribute reference has the syntax obj.name. obj is an expression denoting an object, and name is an identifier, called an attribute name of the object. Assignment to an attribute reference asks object obj to bind its attribute named name.

An indexing has the syntax obj [ expr ]. obj and expr are expressions denoting any objects. Assignment to an indexing asks container obj to bind its item selected by the value of expr, also known as the index or key of the item.

A slicing has the syntax obj [ start:stop ] or obj [ start:stop:stride ]. obj, start, stop, and stride are expressions denoting any objects. start, stop, and stride are all optional (i.e., obj [:stop :] is also a syntactically correct slicing, equivalent to obj [None:stop :None]). Assignment to a slicing asks container obj to bind or unbind some of its items.

We’ll come back to indexing and slicing targets later in this chapter when we discuss operations on lists and dictionaries.

When the target of the assignment is an identifier, the assignment statement specifies the binding of a variable. This is never disallowed: when you request it, it takes place. In all other cases, the assignment statement specifies a request to an object to bind one or more of its attributes or items. An object may refuse to create or rebind some (or all) attributes or items, raising an exception if you attempt a disallowed creation or rebinding.

There can be multiple targets and equals signs (=) in a plain assignment. For example:

a = b = c = 0
binds variables a, b, and c to the value 0. Each time the statement executes, the right-hand side expression is evaluated once. Each target gets bound to the single object returned by the expression, just as if several simple assignments executed one after the other.

The target in a plain assignment can list two or more references separated by commas, optionally enclosed in parentheses or brackets. For example:

a, b, c = x
This requires x to be a sequence with three items, and binds a to the first item, b to the second, and c to the third. This kind of assignment is called an unpacking assignment, and, in general, the right-hand side expression must be a sequence with exactly as many items as there are references in the target; otherwise, an exception is raised. Each reference in the target is bound to the corresponding item in the sequence. An unpacking assignment can also swap references:

a, b = b, a
This rebinds a to refer to what b was bound to, and vice versa.

Augmented assignment
An augmented assignment differs from a plain assignment in that, instead of an equals sign (=) between the target and the expression, it uses an augmented operator: a binary operator followed by =. The augmented operators are +=, -=, *=, /=, //=, %=, **=, |=, >>=, <<=, &=, and ^=. An augmented assignment can have only one target on the left-hand side; that is, augmented assignment doesn’t support multiple targets.

In an augmented assignment, just as in a plain one, Python first evaluates the right-hand side expression. Then, if the left-hand side refers to an object that has a special method for the appropriate in-place version of the operator, Python calls the method with the right-hand side value as its argument. It is up to the method to modify the left-hand side object appropriately and return the modified object (Chapter 5 covers special methods). If the left-hand side object has no appropriate in-place special method, Python applies the corresponding binary operator to the left-hand side and right-hand side objects, then rebinds the target reference to the operator’s result. For example, x += y is like x = x ._ _iadd__( y ) when x has special method __iadd__. Otherwise x += y is like x = x + y.

Augmented assignment never creates its target reference: the target must already be bound when augmented assignment executes. Augmented assignment can re-bind the target reference to a new object or modify the same object to which the target reference was already bound. Plain assignment, in contrast, can create or rebind the left-hand side target reference, but it never modifies the object, if any, to which the target reference was previously bound. The distinction between objects and references to objects is crucial here. For example, x = x + y does not modify the object to which name x was originally bound. Rather, it rebinds the name x to refer to a new object. x += y, in contrast, modifies the object to which the name x is bound when that object has special method __iadd__; otherwise, x += y rebinds the name x, just like x = x + y.

del Statements
Despite its name, a del statement does not delete objects: rather, it unbinds references. Object deletion may follow as a consequence, by garbage collection, when no more references to an object exist.

A del statement consists of the keyword del, followed by one or more target references separated by commas (,). Each target can be a variable, attribute reference, indexing, or slicing, just like for assignment statements, and must be bound at the time del executes. When a del target is an identifier, the del statement specifies the unbinding of the variable. As long as the identifier is bound, unbinding it is never disallowed: when requested, it takes place.

In all other cases, the del statement specifies a request to an object to unbind one or more of its attributes or items. An object may refuse to unbind some (or all) attributes or items, raising an exception if a disallowed unbinding is attempted (see also __delattr__ in Chapter 5). Unbinding a slicing normally has the same effect as assigning an empty sequence to that slice, but it is up to the container object to implement this equivalence.
