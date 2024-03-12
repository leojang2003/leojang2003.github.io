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

<i>class</i> 的所有 <i>instance</i> 只有一個 <i>copy</i>

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

<br/><i></i>

### method

單一 <i>expression</i> 的 <i>method</i> 可以使用以下範例

```csharp
public override string ToString() => "This is an object";
```

<br/><i></i>

### ref 關鍵字

<i>ref</i> 參數必須要有值

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

<br/><i></i>

### out 關鍵字

<i>out</i> 參數不必要有值

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
<br/><i></i>

### params 關鍵字

可使用 <i>params</i> 使用 <i>parameter array</i>，只能放在最後一個參數，傳入參數長度不一定。

```csharp
public class Console
{
    public static void Write(string fmt, params object[] args) { }
    public static void WriteLine(string fmt, params object[] args) { }
    // ...
}
```

<br/><i></i>

### virtual 關鍵字



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


```csharp
public class Ford : Car
{
    public new void Run()
    {
        Console.WriteLine("Ford.Run");            
    }
}
```

如果要覆寫父類別的方法

```csharp
public class Ford : Car
{
    public override void Run()
    {
        Console.WriteLine("Ford.Run");            
    }
}
```
