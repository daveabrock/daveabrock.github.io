---
date: "2020-07-06"
title: "C# 9 Deep Dive: Records"
excerpt: In a C# 9 deep dive, we go in-depth on records.
tags: [csharp]
header:
    overlay_image: /assets/images/records.png
    overlay_filter: 0.8
---

In the [previous post of this series](https://daveabrock.com/2020/06/29/c-sharp-9-deep-dive-inits), we discussed the init-only features of C# 9, which allowed you to make individual properties immutable. That works great on a case-by-case basis, but the real power in leveraging C# immutability is when you can do this for custom types. This is where records shine, and will be the focus of this post.

This is the second post in a six-post series on C# 9 features in-depth:

- Post 1 - [Init-only features](https://daveabrock.com/2020/06/29/c-sharp-9-deep-dive-inits)
- Post 2 (*this post*) - Records
- Post 3 - [Pattern matching](https://daveabrock.com/2020/07/06/c-sharp-9-pattern-matching)
- Post 4 - [Top-level programs](https://daveabrock.com/2020/07/09/c-sharp-9-top-level-programs)
- Post 5 - [Target typing and covariant returns](https://daveabrock.com/2020/07/14/c-sharp-9-target-typing-covariants)
- Post 6 - [Putting it all together with a scavenger hunt](https://daveabrock.com/2020/07/21/c-sharp-9-scavenger-hunt)

**Heads up!** C# 9 is still in preview mode, so much of this content might change—this post was last updated on July 6, 2020. I will do my best to update it as I come across it, but that is not guaranteed. Have fun, but your experience may vary.
{: .notice--danger}

## A quick primer on immutable types

Before we get started, let's briefly talk about the concept of immutable types. For sure, immutability is a stuffy word to some but the premise here is simple: once instantiated or initialized, immutable types never change.

What do you get with immutable types? First, you get simplicity. An immutable object only has one state, the state you specified when you created the object. You'll also see that they are secure and thread-safe with no required synchronization. Because you don't have threads fighting to change an object, they can be shared freely in your applications.

Put another way: **immutable types reduce risk, are safer, and help to prevent a lot of nasty bugs that occur when you update your objects.**

Even if you aren't familiar with this concept yet, or haven't been forced to think this way, you're already using it in the .NET world. For example, `System.DateTime` is immutable, as are strings. And now with records in C# 9, you can create your own immutable types.

## OK, so what is a record?

What is a record, exactly? A record is a construct that allows you to encapsulate property state. (I am avoiding the use of the word object, for clarity.) Or, put in less geeky terms, records allow you to perform value-like behaviors on properties. This is why I'm avoiding saying "objects" when I speak of records. We need to start thinking in terms of data, and not objects. Records aren't meant for mutable state—if you want to represent change, create a new record. That way, you define them by working with the data, and not passing around a single object that gets changed by multiple functions.

Records appear to be super flexible. Anthony Giretti [found that classes can have records as properties, and also that records can contain structs and objects](https://anthonygiretti.com/2020/06/19/introducing-c-9-questions-answers-about-records/).

## What is the difference between structs and records?

Allow me to read your mind—you might be asking: how is this different than structs? If you haven't used it before, we can define a value type, called a `struct`, in C# today. So, why don't we just build on the `struct` functionality instead of introducing a new member (ha!) to C#? After all, you can declare an immutable value type by saying `readonly struct`.

Records are offering the following advantages, from what I can see:

- An easy, simplified construct whose intent is to use as an immutable data structure with easy syntax, like `with` expressions to copy objects (keep reading for details!)
- Robust equality support with `Equals(object)`, `IEquatable<T>`, and `GetHashCode()`
- Constructor/deconstructor support with simplified positional records

Of course you can do this with structs, and even classes, but this requires tedious boilerplate. The idea here is to have a construct that is simple and straightforward to implement.

**UPDATE**: One of the biggest draws of records over structs is the reduced memory allocation that is required. Since C# records are compiled to reference types behind the scenes, they are accessed by a reference and not as a copy. As a result, no additional memory allocation is required other than the original record allocation. **Thanks to commenter Tecfield for mentioning this!**

Hopefully by now, if I did my job, you know what records are and the rationale for them. Let's see some code.

If you want to play along, the easiest way as of now is to [download LinqPad 6 Beta](https://www.linqpad.net/linqpad6.aspx#beta), then select **Edit** > **Preferences** > **Query** > **Use Roslyn Daily build for experimental C# 9 support).**

## Create your first record

To declare a record, you use the new `record` keyword. Brilliant, right?

If you've read Microsoft's [*Welcome to C# 9.0 post,*](https://devblogs.microsoft.com/dotnet/welcome-to-c-9-0/) they declare records using the `data class` syntax instead of `record`. Since then, it has changed to `record` and their article has not been updated. The `data class` was considered because the compiler team was also considering `struct class` support as well but, [from these meeting notes](https://github.com/dotnet/csharplang/blob/master/meetings/2020/LDM-2020-05-27.md), it has been scrapped for now.
{: .notice--danger}

Here is a `Person` record using init-only properties.

```csharp
public record Person
{
    public string FirstName { get; init; }
    public string LastName { get; init; }
    public string Address { get; init; }
    public string City { get; init; }
    public string FavoriteColor { get; init; }
    // and so on...
}
```

As of this writing, the `record` declaration is not enough to get complete object immutability. You will also need to use the `{ get; init; }` syntax for your properties, as well. Likewise, if you've read about auto-property improvements (for example, changing `public string FirstName { get; init; }` to `public string FirstName;`), this does not make the fields immutable-by-default either. This allows you the flexibility to have some properties mutable on a case-by-case basis. The compiler team is considering implementing something like `public data string FirstName;` to improve on this, but [from the latest discussion](https://github.com/dotnet/csharplang/blob/master/meetings/2020/LDM-2020-06-22.md), appears this will stay in the preview bits and not make it to the initial C# 9 release in November 2020.
{: .notice--info}

## Use `with` expressions with records

Before C# 9, you would likely represent new state by creating new values from existing ones.

```csharp
var person = new Person
{
    FirstName = "Tony",
    LastName = "Stark",
    Address = "10880 Malibu Point",
    City = "Malibu",
    FavoriteColor = "red"
};

var newPerson = person;
newPerson.FirstName = "Howard";
newPerson.City = "Pasadena";
```

This pattern is referred to as non-destructive mutation (now *that* will make you seem smart at your next dinner party!). C# 9 has a new type of expression, a `with` expression, to assist you.

This functionality is only available in records, and not structs or classes.
{: .notice--info}

```csharp
var person = new Person
{
    FirstName = "Tony",
    LastName = "Stark",
    Address = "10880 Malibu Point",
    City = "Malibu",
    FavoriteColor = "red"
};

var newPerson = person with { FirstName = "Howard", City = "Pasadena" };
```

You can easily use your familiar object initializer syntax to differentiate what has changed between objects, including multiple properties. Under the covers, the record has a `protected` copy constructor. If you wish, you can change the default behavior of the copy constructor but the default behavior should be suitable in most of your cases (to create a copy, change properties to what you passed in, and copy the properties that you don't).

### Use inheritance with the `with` expression

Remember when I said records support inheritance, while structs do not? I wasn't lying. We can use inheritance with our `with` expression. Let's say we have a `Superhero` class that inherits from `Person` and has a new `MaxSpeed` property.

```csharp
class Program
{
    static void Main(string[] args)
    {
        var person = new Superhero
        {
            FirstName = "Tony",
            LastName = "Stark",
            Address = "10880 Malibu Point",
            City = "Malibu",
            FavoriteColor = "red",
            MaxSpeed = 1000
        };

        var newPerson = person with { FirstName = "Howard", City = "Pasadena" };

        Console.WriteLine(newPerson.FirstName); // Howard
        Console.WriteLine(newPerson.MaxSpeed); // 1000
    }
}

public record Person
{
    public string FirstName { get; init; }
    public string LastName { get; init; }
    public string Address { get; init; }
    public string City { get; init; }
    public string FavoriteColor { get; init; }
}

public record Superhero : Person
{
    public int MaxSpeed { get; init; }
}
```

Hey, this works! How? Records actually have a hidden virtual method that clones the *entire* object. An inherited record overrides this method to call the copy constructor of that type, chaining to the copy constructor of the base record.

## Implementing positional records

Sometimes, you'll need to take a positional approach—meaning data is supplied from constructor arguments. You can definitely do this with records.

Here's what you can do to enable your own constructor and deconstructor.

```csharp
class Program
{
    static void Main(string[] args)
    {
        var person = new Person("Tony", "Stark", "10880 Malibu Point", "Malibu", "red");
        Console.WriteLine(person.FirstName); // Tony
        Console.WriteLine(person.LastName); // Stark
    }
}


public record Person
{
    public string FirstName;
    public string LastName;
    public string Address;
    public string City;
    public string FavoriteColor;

    public Person(string firstName, string lastName, string address, string city, string favoriteColor)
      => (FirstName, LastName, Address, City, FavoriteColor) = (firstName, lastName, address, city, favoriteColor);
    public void Deconstruct(out string firstName, out string lastName, out string address, 
                            out string city, out string favoriteColor) 
      => (firstName, lastName, address, city, favoriteColor) = (FirstName, LastName, Address, City, FavoriteColor);
}
```

You can clean this up with new simplified syntax.

```csharp
class Program
{
    static void Main(string[] args)
    {
        var person = new Person("Tony", "Stark", "10880 Malibu Point", "Malibu", "red");
        var (first, last, address, city, color) = person;

        Console.WriteLine(person.FirstName); // Tony
        Console.WriteLine(person.LastName); // Stark
        Console.WriteLine(first); // Tony
        Console.WriteLine(last); // Stark
    }
}

public record Person(string FirstName, string LastName, string Address, string City, string FavoriteColor);

```

## Evaluating record equality

If we're being honest, *technically* records are a kind of class, which also means they are *technically* reference types. But that's OK—like structs, records override the `Equals(object)` method that every class has, to achieve the value-ness we are after. This means we can work with value-based equality as well.

For example, if we create two new objects we know that they have different references in memory, so a `ReferenceEquals` call (or even a `==` call) will return `false` even if they have the same values. This is different than structs—because structs are value types, this will not occur.

With records, we'll compare values.

Watch what happens as we:

- Create a new `Person` called `person`, Tony Stark
- Create another `Person`, called `newPerson`, Howard Stark, with two different properties (`FirstName` and `City`)
- Create a third `Person` called `anotherPerson`, and set `anotherPerson` to the same values as the original `person`

```csharp
class Program
{
    static void Main(string[] args)
    {
        var person = new Person
        {
            FirstName = "Tony",
            LastName = "Stark",
            Address = "10880 Malibu Point",
            City = "Malibu",
            FavoriteColor = "red"
        };

        var newPerson = person with { FirstName = "Howard", City = "Pasadena" };

        Console.WriteLine(Object.ReferenceEquals(person, newPerson)); // false
        Console.WriteLine(Object.Equals(person, newPerson)); // false

        var anotherPerson = newPerson with { FirstName = "Tony", City = "Malibu" };
        Console.WriteLine(Object.Equals(person, anotherPerson)); // true
    }
}

public record Person
{
    public string FirstName { get; init; }
    public string LastName { get; init; }
    public string Address { get; init; }
    public string City { get; init; }
    public string FavoriteColor { get; init; }
}
```

Nice! Also, along with the `Equals` override records ship with a `GetHashCode()` override if you're so inclined.

## Wrapping up

In this post, we discussed a lot about records in C# 9. We discussed what a record is, how it compares to a struct, `with` expressions, inheritance, positional records, and how to evaluate equality.

As I discussed, this is continually changing—let me know how this works for you, and if things have changed since this was published. These are exciting new features and I hope you are able to see their benefit.
