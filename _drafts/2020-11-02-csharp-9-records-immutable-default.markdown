---
date: "2020-08-09"
title: "Why aren't C# 9 records immutable by default?"
excerpt: "We discuss the value of named arguments in the C# language."
tags: [csharp,dotnet]
header:
    overlay_image: /assets/images/stacks-10-card.png
    overlay_filter: 0.8

---

One of the most popular features in C# 9 is [records](https://daveabrock.com/2020/07/06/c-sharp-9-deep-dive-records). I wrote about records in exruciating detail this summer: but the gist is that records are types that allow you to perform value-like behaviors on properties with the promise of *immutability*. They [support `with` expressions](https://daveabrock.com/2020/07/06/c-sharp-9-deep-dive-records#use-with-expressions-with-records), [inheritance](https://daveabrock.com/2020/07/06/c-sharp-9-deep-dive-records#use-inheritance-with-the-with-expression), and [positional records](https://daveabrock.com/2020/07/06/c-sharp-9-deep-dive-records#implementing-positional-records), too. 

The docs *currently* say [they are immutable by default](https://docs.microsoft.com/dotnet/csharp/whats-new/csharp-9). I don't agree (and [Jimmy Bogard has opened a GitHub issue with a similar concern](https://github.com/dotnet/docs/issues/21224)). Let's unpack why C# records are not immutable by default.

## You can't make a record immutable just by saying "record"

Let's say I create a `Person` construct—I am avoiding the word `object` to avoid confusion—by declaring a `record`.

```csharp
public record Person 
{
    public string FirstName { get; set; }
    public string LastName { get; set; }
}
```

In my main method, I can instantiate it and modify properties after the fact. I can't just declare the data type and expect immutability.

```csharp
public static void Main() 
{
    var person = new Person();
    person.FirstName = "Dave";
    person.LastName = "Brock";
    Console.WriteLine($"Hi, I'm {person.FirstName} {person.LastName}");

    person.FirstName = "Bob";
    Console.WriteLine($"Hi, I'm {person.FirstName} {person.LastName}");
}
```

## You need to use the `init` setter for immutability

If we change our record class to this:

```csharp
public record Person 
{
    public string FirstName { get; init; }
    public string LastName { get; init; }
}
```



