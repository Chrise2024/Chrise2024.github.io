---
title: C#鸭子类型
description: 如果它走起来像鸭子，游泳起来像鸭子，叫起来也像鸭子，那么它就是一只鸭子。
tags: [Programing,CSharp]
categories: Programing
math: true
lang: zh-CN
date: 2025-01-24 13:03 +0800
--- 

鸭子类型只在乎对象的行为，并不关注对象的类名与继承关系，只要一个对象包含了某个方法或属性，就可以被视为拥有这个方法或属性的另一个类，从而进行对特定类的操作。鸭子类型在许多现代编程语言中都有体现（e.g. `TypeScript` ），增加了代码的灵活性和适应性。

在 `C#` 中就存在鸭子类型的机制，尽管文档中并没有说明。

## IEnumerable

 `IEnumerable` 接口包含 `System.Collections.IEnumerable` 和它的泛型版本 `System.Collections.Generic.IEnumerable<T>` ，用于提供枚举器，可以用 `foreach` 语句进行枚举。一般情况下，只有实现了这两个接口之一的类才能进行枚举，但是编译器对类是否能枚举的判断依据不是类是否实现了这些接口，而是是否实现这两个接口所包含的方法 `public IEnumerator GetEnumerator()` 或 `public IEnumerator<T> GetEnumerator()` ，即使这个类并没有直接“实现”接口。

```csharp
//C#

{
    public static void Main(string[] args)
    {
        foreach (var i in new MyEnumerator())
        {
            Console.WriteLine(i);
        }
    }
}

public class MyEnumerator
{
    public IEnumerator<int> GetEnumerator()
    {
        for (int i = 0;i < 10;i++)
        {
            yield return i;
        }
    }
}
```

可以看到， `MyEnumerator` 并没有实现 `IEnumerable<T>` ，但是依然可以被枚举。

查看编译器生成的低级 `C#` 代码可以发现，

```csharp
//C#

namespace Project
{
  public static class Program
  {
    [NullableContext(1)]
    public static void Main(string[] args)
    {
      IEnumerator<int> enumerator = new MyEnumerator().GetEnumerator();
      try
      {
        while (enumerator.MoveNext())
          Console.WriteLine(enumerator.Current);
      }
      finally
      {
        if (enumerator != null)
          enumerator.Dispose();
      }
    }
  }
}
```

生成的代码直接通过 `GetEnumerator()` 拿到了枚举器，这时候表现得和有 `IEnumerable<T>` 接口约束一样了。

而且，在 `C#` 中存在一种特殊的方法，称为拓展方法，可以附加方法在类上，而这样附加上去的方法一样可以触发鸭子类型的判定，从而达成一系列奇技淫巧。

```csharp
//C#

public static class Program
{
    public static void Main(string[] args)
    {
        foreach (int i in 114)
        {
            Console.WriteLine(i);
        }
        foreach (int i in 2..6)
        {
            Console.WriteLine(i);
        }
    }
}

public static class MyExtension
{
    public static IEnumerator<int> GetEnumerator(this int num)
    {
        for (int i = 0; i < num; i++)
        {
            yield return i;
        }
    }

    public static IEnumerator<int> GetEnumerator(this Range range)
    {
        for (int i = range.Start.Value; i < range.End.Value; i++)
        {
            yield return i;
        }
    }
}
```

给 `Range` 类型加上 `GetEnumerator()` ，这样可以直接 `foreach` 一个 `Range` 表达式，嗯，有 `Python` 那味了。

## GetAwaiter

在 `C#` 中，可以通过 `await` 关键字等待一个 `Task` ，但是，一个类只要包含 `public TaskAwaiter GetAwaiter()` 方法，那么就可以被当作一个 `Task` 来进行 `await` ，同样的，对拓展方法也生效。

```csharp
//C#

public static class Program
{
    public static async Task Main(string[] args)
    {
        Console.WriteLine(await new MyAwait<int>(114514));
        await TimeSpan.FromSeconds(6);
        await 114.514D;
    }
}

public class MyAwait<T>(T result)
{
    //TaskAwaiter 的构造函数为 internal，所以无法直接 new TaskAwaiter<T>(result)，但是羊毛出在羊身上，可以直接从一个Task上获取TaskAwaiter来用
    public TaskAwaiter<T> GetAwaiter() => Task.FromResult(result).GetAwaiter();
}

public static class MyExtension
{
    public static TaskAwaiter GetAwaiter(this TimeSpan time)
    {
        return Task.Delay(time).GetAwaiter();
    }
    public static TaskAwaiter GetAwaiter(this double num)
    {
        return Task.Delay(TimeSpan.FromSeconds(num)).GetAwaiter();
    }
}
```

## 元组拆分

 `C#` 引入元组后，可以像 `Python` 一样拆分元组，除了元组本身可以拆分，很多类型也可以拆分。

```csharp
//C#

public static class Program
{
    public static void Main(string[] args)
    {
        (int a, int b) = (114, 514);
        Dictionary<string,int> dict = new(){{"111",2},{"222",3}};
        foreach (KeyValuePair<string, int> pair in dict)
        {
            (string a, int b) = pair;
            Console.WriteLine("{0}:{1}",a,b);
        }
    }
}
```

元组的拆分依靠 `Deconstruct` 方法，这个方法依靠out来传参，只要一个类包含这个方法就可以进行元组拆分。

```csharp
//C#

public static class Program
{
    public static void Main(string[] args)
    {
        MyTuple mt = new("1", 2);
        (string a,int b) = mt;
    }
}

public class MyTuple(string a, int b)
{
    public string A = a;
    public int B = b;
    public void Deconstruct(out string a, out int b)
    {
        a = A;
        b = B;
    }
}
```

## 写在最后

 `C#` 的鸭子类型依靠编译器的预处理，将高级代码转为低级代码，在IDE自带的 `IL View` 就能查看，每种鸭子类型都有对应的底层写法。
