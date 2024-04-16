---
layout: post
title: Descriptor
subtitle: 
tags: [property, descriptor, first-class object, data descriptor, non-data descriptor]
comments: true
---

可參考 <a href="../2022-11-23-class-instance-attribute/">類別與物件的屬性</a>
可參考 <a href="../2024-01-18-dot-operator/">點運算子</a>

### object.\_\_get__ ( self , owner , ownerType = None )

首先 <i>\_\_get__</i> 方法官網定義如下

> Called to get the attribute of the owner class (class attribute access) or of an instance of that class (instance attribute access). The optional owner argument is the owner class, while instance is the instance that the attribute was accessed through, or None when the attribute is accessed through the owner.

透過類別或物件去存取 <i>class attribute</i> 時，都會觸發 <i>class attribute</i> 的類別的 <i>\_\_get__</i> 方法，須注意不是所以有類別都有實作 <i>\_\_get__</i>。

<br/>

#### 透過 class 存取 class attribute

舉例來說下面的類別 <i>A</i>，

