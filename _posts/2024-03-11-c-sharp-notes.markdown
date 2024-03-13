---
layout: post
title: C# notes 
subtitle: 
tags: ['C#']
comments: true
---
{: .box-note}
"How you do anything is how you do everything" - Dirk Nowitzki

### Type parameters

```csharp
public class Pair<TFirst, TSecond>
{
    public TFirst First { get; }
    public TSecond Second { get; }
    
    public Pair(TFirst first, TSecond second) => 
        (First, Second) = (first, second);
}
```

<br/>

### static field

class 的所有 instance 只有一個 copy

```csharp
public class Color
{
    public static readonly Color Black = new(0, 0, 0);    
    
    public byte R;
    public byte G;
    public byte B;

    public Color(byte r, byte g, byte b)
    {
        R = r;
        G = g;
        B = b;
    }
}
```

<br/>

### method

單一 expression 的 method 可以使用以下範例

```csharp
public override string ToString() => "This is an object";
```

<br/>

### ref 關鍵字

ref 參數必須要有值

```csharp
static void Swap(ref int x, ref int y)
{
    int temp = x;
    x = y;
    y = temp;
}

public static void SwapExample()
{
    int i = 1, j = 2;
    Swap(ref i, ref j);
    Console.WriteLine($"{i} {j}");    // "2 1"
}
```

<br/>

### out 關鍵字

out 參數不必要有值

```csharp
static void Divide(int x, int y, out int quotient, out int remainder)
{
    quotient = x / y;
    remainder = x % y;
}

public static void OutUsage()
{
    Divide(10, 3, out int quo, out int rem);
    Console.WriteLine($"{quo} {rem}");	// "3 1"
}
```
<br/>

### params 關鍵字

可使用 params 使用 parameter array，只能放在最後一個參數，傳入參數長度不一定。

```csharp
public class Console
{
    public static void Write(string fmt, params object[] args) { }
    public static void WriteLine(string fmt, params object[] args) { }
    // ...
}
```

<br/>

### virtual 關鍵字

類別的方法預設不是 virtual，若要可以覆寫方法，則方法需要定義 virtual。base class 的 method 若有定義 virtual，則 derived class 的同名 method 可以

1. 不加 override 或 new，編譯器視為默認有加 new

2. 加 override 視為覆寫 base class 的 method

3. 加 new 視為隱藏 base class 的 method

<br/>

```csharp
public class Car
{
    public virtual void Run()
    {
        Console.WriteLine("Car.Run");
    }
}

public class Ford : Car
{
    public void Run()
    {
        Console.WriteLine("Ford.Run");            
    }
}
```

這樣是可以的，子類別的 Run() 方法視同有 new，子類別的方法視為一個新的方法

```csharp
public class Ford : Car
{
    public new void Run()
    {
        Console.WriteLine("Ford.Run");            
    }
}
```

如果是要覆寫覆類別的方法，則需使用 override

```csharp
public class Ford : Car
{
    public override void Run()
    {
        Console.WriteLine("Ford.Run");            
    }
}
```

virtual 不可與 abstract、static、private、override 一起使用。

<br/>

### abstract

abstract 有以下特性

#### abstract 類別

不可建立 abstract 類別的物件<br class="new">

只有 abstract 類別可以有 abstract 方法與 accessor<br class="new">

abstract 不可同時為 sealed<br class="new">

繼承自 abstract 類別的非 abstract 類別虛實作所有繼承的 abstract 方法與 accessor

#### abstract 方法

abstract 方法就是隱含的 virtual 方法<br class="new">

abstract 方法只能在 abstract 類別中宣告<br class="new">

abstract 方法宣告沒有 {}，實作 abstract 方法要用 override<br class="new">

abstract 不可與 static、virtual 共用<br class="new">

#### abstract 屬性

類似 abstract 方法，要使用 override 覆寫

```csharp
abstract class Car
{
    // abstract 類別的方法不一定是 abstract
    public virtual void Run()
    {
        Console.WriteLine("Car.Run");
    }
        
    public void Break()
    {
        Console.WriteLine("Car.Break");
    }
	
	// abstract 方法是沒有大括號的
    public abstract void Horn(); 
}
```

```csharp
abstract class Ford : Car
{    
    public override void Run()
    {
        Console.WriteLine("Ford.Run");
    }

    // 實作 abstract 方法需用 override    
    public override void Horn()
    {
        Console.WriteLine("Car.Horn");
    }
}
```

```csharp
abstract class Nissan : Car
{
    // 一個 abstract property
    public abstract int Year { get; set; }
}
```

覆寫 abstract property 用法如下，順便介紹 property 的各種形式

```csharp
class GTR : Nissan
{
    private int _year = 2020;
    private int _mileage = 2020;
    private readonly int _tires = 4;

    // 同樣使用 override 覆寫抽象屬性
    public override int Year
    {
        get { return _year; }
        set { _year = value; }
    }

    // 自動實作的屬性
    public bool BackupCamera { get; set; }

    // 使用 expression-bodied member 的 get/set 屬性
    public int Mileage
    {
        get => _mileage;
        set => _mileage = value;
    }

    // expression-bodied member 只有 get，可以不用寫 get 或 return
    public int Tires => _tires;

	// C# 11 之後可以設定 required
    public required bool LeatherSeats { get; set; }

    public override void Horn()
    {
        throw new NotImplementedException();
    }
}
```

