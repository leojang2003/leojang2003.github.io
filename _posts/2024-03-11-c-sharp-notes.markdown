---
layout: post
title: C# notes 
subtitle: 
tags: ['C#']
comments: true
---
{: .box-note}
"How you do anything is how you do everything" - Dirk Nowitzki

<br/>

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

單一 expression 的 method 可以使用 =>

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

類別的方法預設不是 virtual，也就是類別的方法預設不可覆寫。若要可以覆寫方法，則方法需要定義 virtual。父類別的 method 若有定義 virtual，則子類別的同名 method 可以

1. 不加 override 或 new，編譯器視為默認有加 new，同 3.

2. 加 override 視為覆寫父類別的 method

3. 加 new 視為隱藏父類別的 method

<br/>

以下範例顯示，子類別的 Run() 方法視同有 new，子類別的方法視為一個新的方法

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

也可以明確增加 new 如下

```csharp
public class Ford : Car
{
    public new void Run()
    {
        Console.WriteLine("Ford.Run");            
    }
}
```

如果是要覆寫父類別的方法，則需使用 override

```csharp
public class Ford : Car
{
    public override void Run()
    {
        Console.WriteLine("Ford.Run");            
    }
}
```
{:.note}
virtual 不可與 abstract、static、private、override 一起使用。

<br/>

### abstract

abstract 可以修飾類別、方法、屬性。

#### abstract 類別

- 不可建立 abstract 類別的物件<br class="new">

- 只有 abstract 類別可以有 abstract 方法與 accessor<br class="new">

- abstract 不可同時為 sealed<br class="new">

繼承自 abstract 類別的非 abstract 類別需實作所有繼承的 abstract 方法與 accessor

#### abstract 方法

- abstract 方法就是隱含的 virtual 方法<br class="new">

- abstract 方法只能在 abstract 類別中宣告<br class="new">

- abstract 方法宣告沒有 {}，實作 abstract 方法要用 override<br class="new">

- abstract 不可與 static、virtual 共用<br class="new">

#### abstract 屬性

- 類似 abstract 方法，要使用 override 覆寫

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

實作 abstract 方法，如同實作 virtual 方法，都是使用 override

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

建立 abstract property

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
}
```
<br/>

### constructor

```csharp
MyList<int> a = new(); a = new();
```
<br/>

### delegate

```csharp
delegate string Function(string x);

class ToUpper
{
    public string Upper(string x) => x.ToUpper();
}

class ToLower
{
    public static string Lower(string x) => x.ToLower();
}

class DelegateExample
{
    // 將 function j
    static string[] Apply(string[] a, Function f)
    {
        var result = new string[a.Length];
        for (int i = 0; i < a.Length; i++) 
            result[i] = f(a[i]);
        return result;
    }

    public void Deleage()
    {
        string[] a = { "Tom", "Harry", "Ron" };
        ToUpper u = new();
        var upper = Apply(a, u.Upper); // delegate 可以是類別方法
        var lower = Apply(a, ToLower.Lower); // delegate 可以是靜態方法
        var anno = Apply(a, x => x + ","); // delegate 可以是匿名方法或 lamda

        upper.ToList().ForEach(x => Console.WriteLine(x));            
    }
}
```
<br/>

