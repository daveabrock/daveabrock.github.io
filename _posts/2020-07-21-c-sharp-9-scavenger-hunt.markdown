---
date: "2020-07-21"
title: "C# 9: Putting it all together with a scavenger hunt"
excerpt: Let's go through the major C# 9 features in a single post.
header:
    overlay_image: /assets/images/scavenger-card.png
    overlay_filter: 0.8
tags: [csharp]
---

Here we are, friends: our last post in our C# 9 deep dive series. We've discussed a lot of topics, so I thought it'd be fun to bundle all we've learned into a single post. (You can think of this as a [CliffsNotes](https://en.wikipedia.org/wiki/CliffsNotes) of the 10,000 or so words I've written about C# 9 so far.)

So, let's go on a scavenger hunt! (I know I have readers from different countries and customs—where I'm from, it's a fun game where you have to run around outside and find a number of miscellaneous things from a list). So this is how us nerds do it: inside, with air conditioning, geeking out about features that aren't fully released yet.

I'll be focusing on the how, not the why—you can see all my other posts for the full details on the concepts.

Of course, the complete and up-to-date feature set on C# 9 can be found by reviewing the [Language Feature Status in GitHub](https://github.com/dotnet/roslyn/blob/master/docs/Language%20Feature%20Status.md). As that moves along, I'll probably have some more posts on any new features that weren't covered during Build.
{: .notice--info}

This is the sixth—and last!—post on my C# 9 deep dive series.

- Post 1 - [Init-only features](https://daveabrock.com/2020/06/29/c-sharp-9-deep-dive-inits)
- Post 2 - [Records](https://daveabrock.com/2020/07/06/c-sharp-9-deep-dive-records)
- Post 3 - [Pattern matching](https://daveabrock.com/2020/07/06/c-sharp-9-pattern-matching)
- Post 4 - [Top-level programs](https://daveabrock.com/2020/07/09/c-sharp-9-top-level-programs)
- Post 5 - [Target typing and covariant returns](https://daveabrock.com/2020/07/14/c-sharp-9-target-typing-covariants)
- Post 6 (*this post*) - Putting it all together with a scavenger hunt

**Heads up!** C# 9 is still in preview mode, so much of this content might change. I will do my best to update it as I come across it, but that is not guaranteed. Have fun, but your experience may vary.
{: .notice--danger}

## Our scavenger hunt: the list

Here's our list for our scavenger hunt. We'll check it off as we go!

(Target typing `??` and `?:` and data member simplification have been removed from the list, as they do not compile in today's previews.)

- [ ] Init-only properties
  - [ ] Init accessors and readonly fields
- [ ] Records
  - [ ] `with` expressions
  - [ ] Value-based equality
  - [ ] Positional records
  - [ ] `with` expressions and inheritance
  - [ ] Value-based equality and inheritance
- [ ] Top-level programs  
- [ ] Improved pattern matching
  - [ ] Simple type patterns
  - [ ] Relational patterns
  - [ ] Logical patterns
- [ ] Improved target typing
  - [ ] Target-typed `new` expressions
- [ ] Covariant returns  

## The low-hanging fruit: top-level programs

For a quick win, let's make a top-level program. If you remember, this allows us to run a program without that pesky `Main` method.

```csharp
using System;

Console.WriteLine("I am Groot.");
```

Well, that was easy! Knock one off the list.

- [ ] Init-only properties
  - [ ] Init accessors and readonly fields
- [ ] Records
  - [ ] `with` expressions
  - [ ] Value-based equality
  - [ ] Positional records
  - [ ] `with` expressions and inheritance
  - [ ] Value-based equality and inheritance
- [x] ~~Top-level programs~~
- [ ] Improved pattern matching
  - [ ] Simple type patterns
  - [ ] Relational patterns
  - [ ] Logical patterns
- [ ] Improved target typing
  - [ ] Target-typed `new` expressions
- [ ] Covariant returns

## Init-only properties

With init-only properties, we can use object initializers with immutable fields.

Let's create an `Avenger` class:

```csharp
public class Avenger
{
    public string FirstName { get; init; }
    public string LastName { get; init; }
}
```

Now, we can use them with object initializers in C# 9.

```csharp
using System;

var ironMan = new Avenger { FirstName = "Tony", LastName = "Stark" };
var hulk = new Avenger { FirstName = "Bruce", LastName = "Banner" };
var blackWidow = new Avenger { FirstName = "Natasha", LastName = "Romanova"};

Console.WriteLine($"Iron Man is {ironMan.FirstName} {ironMan.LastName}.");
Console.WriteLine($"The Hulk is {hulk.FirstName} {hulk.LastName}.");
Console.WriteLine($"The Black Widow is {blackWidow.FirstName} {blackWidow.LastName}.");
```

Great! Let's scratch another quick one off the list with some target typing. We can leave `ironMan` alone, but for the others we'll replace `new Avenger` with just `new` and it'll work the same.

```csharp
using System;

var ironMan = new Avenger { FirstName = "Tony", LastName = "Stark" };
Avenger hulk = new { FirstName = "Bruce", LastName = "Banner" };
Avenger blackWidow = new { FirstName = "Natasha", LastName = "Romanova"};

Console.WriteLine($"Iron Man is {ironMan.FirstName} {ironMan.LastName}.");
Console.WriteLine($"The Hulk is {hulk.FirstName} {hulk.LastName}.");
Console.WriteLine($"The Black Widow is {blackWidow.FirstName} {blackWidow.LastName}.");
```

**UPDATE:** As of now, it even works with the `var` usage, like below. However, for clarity sake I prefer declaring the type before using target typing.

```csharp
var hulk = new { FirstName = "Bruce", LastName = "Banner" };
var blackWidow = new { FirstName = "Natasha", LastName = "Romanova"};
```

Now we have two more off the list, hooray!

- [x] ~~Init-only properties~~
  - [ ] Init accessors and readonly fields
- [ ] Records
  - [ ] `with` expressions
  - [ ] Value-based equality
  - [ ] Positional records
  - [ ] `with` expressions and inheritance
  - [ ] Value-based equality and inheritance
- [x] ~~Top-level programs~~
- [ ] Improved pattern matching
  - [ ] Simple type patterns
  - [ ] Relational patterns
  - [ ] Logical patterns
- [x] ~~Improved target typing~~
  - [x] ~~Target-typed `new` expressions~~
- [ ] Covariant returns

## Init accessors and readonly fields

As we've learned, `init` accessors can only be called when you initialize. If you try it after, you'll be greeted with this:

```dos
FirstName cannot be assigned to -- it is read-only
```

With these `init` accessors, you can also mutate read-only fields—which you previously could do with constructors. With only changing the `Avengers` class, do it this way:

```csharp
class Avenger
{
    private readonly string firstName;
    private readonly string lastName;

    public string FirstName
    {
        get => firstName;
        init => firstName = (value ?? throw new ArgumentNullException(nameof(FirstName)));
    }
    public string LastName
    {
        get => lastName;
        init => lastName = (value ?? throw new ArgumentNullException(nameof(LastName)));
    }
}
```

OK, one more gone. On to records!

- [x] ~~Init-only properties~~
  - [x] ~~Init accessors and readonly fields~~
- [ ] Records
  - [ ] `with` expressions
  - [ ] Value-based equality
  - [ ] Positional records
  - [ ] `with` expressions and inheritance
  - [ ] Value-based equality and inheritance
- [x] ~~Top-level programs~~
- [ ] Improved pattern matching
  - [ ] Simple type patterns
  - [ ] Relational patterns
  - [ ] Logical patterns
- [x] ~~Improved target typing~~
  - [x] ~~Target-typed `new` expressions~~
- [ ] Covariant returns

## Records

Let's convert our object to a record. This allows an entire object-like construct to be immutable and act like a value.

Change the `Avenger` object to a record and our program should still run as expected:

```csharp
record Avenger
{
    public string FirstName { get; init; }
    public string LastName { get; init; }
}
```

### Use `with` expressions

With records, we can leverage *non-destructive mutation*, or a way to create new values from existing ones to resemble new state. We can make a copy of a record, and then change what we need. What if the Hulk had a little boy, who joins him and also turns green when he's angry?

Change the main part of the program to this:

```csharp
using System;

var hulk = new Avenger { FirstName = "Bruce", LastName = "Banner"};
var babyHulk = hulk with { FirstName = "Baby" };

Console.WriteLine($"The Hulk is {hulk.FirstName} {hulk.LastName}.");
Console.WriteLine($"The Baby Hulk is {babyHulk.FirstName} {babyHulk.LastName}.");
```

#### Use `with` expressions with inheritance

Records support inheritance. Structs do not. We can even use it with the `with` expression.

Change your program to this, and it works through the power of a hidden virtual method, which I showed off when [discussing records in-depth](https://daveabrock.com/2020/07/06/c-sharp-9-deep-dive-records).

```csharp
using System;

var hulk = new Hulk { FirstName = "Bruce", LastName = "Banner", AngerLevel = 90};
var babyHulk = hulk with { FirstName = "Baby" };

Console.WriteLine($"The Hulk is {hulk.FirstName} {hulk.LastName}, " +
                    $"and anger level is {hulk.AngerLevel}.");
Console.WriteLine($"The Baby Hulk is {babyHulk.FirstName} {babyHulk.LastName}, " +
                    $"and anger level is {babyHulk.AngerLevel}.");

record Hulk : Avenger
{
    public int AngerLevel { get; init; }
}

record Avenger
{
    public string FirstName { get; init; }
    public string LastName { get; init; }
}
```

#### Use value-based equality and inheritance

When it comes to records, C# 9 takes care of value-based equality for you through an `EqualityContract`. Every derived record overrides it and for equal comparison to occur the two objects must both have an `EqualityContract.`

Because we are being immutable, we can create a new record, set the value back, and see the object comparison returns equal.

```csharp
using System;

// init-only properties
var hulk = new Hulk { FirstName = "Bruce", LastName = "Banner", AngerLevel = 90};
var babyHulk = hulk with { FirstName = "Baby" };

Console.WriteLine(Object.ReferenceEquals(hulk, babyHulk)); // false
Console.WriteLine(Object.Equals(hulk, babyHulk)); // false

var backToRegularHulk = babyHulk with { FirstName = "Bruce" };

Console.WriteLine(Object.Equals(hulk, backToRegularHulk)); // true

record Hulk : Avenger
{
    public int AngerLevel { get; init; }
}

record Avenger
{
    public string FirstName { get; init; }
    public string LastName { get; init; }
}
```

### Positional records

We can take a positional approach, where we hand out data from constructor arguments and we can use deconstruction to extract data.

```csharp
using System;

var hulk = new Avenger("Bruce", "Banner");
var (first, last) = hulk;

Console.WriteLine(first); // false
Console.WriteLine(last); // false

public record Avenger
{
    string FirstName;
    string LastName;
    public Avenger(string firstName, string lastName) 
      => (FirstName, LastName) = (firstName, lastName);
    public void Deconstruct(out string firstName, out string lastName) 
      => (firstName, lastName) = (FirstName, LastName);
}
```

That should do it for records! How are we doing so far?

- [x] ~~Init-only properties~~
  - [x] ~~Init accessors and readonly fields~~
- [x] ~~Records~~
  - [x] ~~`with` expressions~~
  - [x] ~~Value-based equality~~
  - [x] ~~Positional records~~
  - [x] ~~`with` expressions and inheritance~~
  - [x] ~~Value-based equality and inheritance~~
- [x] ~~Top-level programs~~
- [ ] Improved pattern matching
  - [ ] Simple type patterns
  - [ ] Relational patterns
  - [ ] Logical patterns
- [x] ~~Improved target typing~~
  - [x] ~~Target-typed `new` expressions~~
- [ ] Covariant returns

## Improved pattern matching

C# 9 offers improved pattern matching for simple type patterns, relational patterns, and logical patterns.

In this example, we will calculate a monthly insurance cost depending on the Hulk's `AngerLevel`. Our records look like this:

```csharp
record Hulk : Avenger
{
    public int AngerLevel { get; init; }
}

record Avenger
{
    public string FirstName { get; init; }
    public string LastName { get; init; }
}
```

### Simple type patterns

Using simple type patterns, we don't need that discard (`_`) operator to just declare the type. Now, we can just say `Hulk => 1000` in `CalculateInsuranceCost`:

```csharp
static int CalculateInsuranceCost(object hulk) =>
    hulk switch
    {
        Hulk t when t.AngerLevel > 90 => 10000,
        Hulk t when t.AngerLevel < 20 => 100,
        Hulk => 1000,

        _ => throw new ArgumentException("Not a known hulk", nameof(hulk))
    ;
```

### Relational patterns

With relational patterns, we can use `<`, `>`, and so on with pattern matching. Take a look at this `CalculateInsuranceCost` method:

```csharp
static int CalculateInsuranceCost(Hulk hulk) =>
  hulk.AngerLevel switch
  {
    > 90 => 10000,
    < 20 => 100,
    _ => 1000,
  };
```

### Logical patterns

With logical patterns, you can use `and`, `or`, and `not`:

```csharp
static int CalculateInsuranceCost(Hulk hulk) =>
  hulk.AngerLevel switch
  {
    > 90 => 10000,
    < 20 and < 80 => 100,
    _ => 1000,
  };
```

How's our scavenger hunt going? Are we done yet?

- [x] ~~Init-only properties~~
  - [x] ~~Init accessors and readonly fields~~
- [x] ~~Records~~
  - [x] ~~`with` expressions~~
  - [x] ~~Value-based equality~~
  - [x] ~~Positional records~~
  - [x] ~~`with` expressions and inheritance~~
  - [x] ~~Value-based equality and inheritance~~
- [x] ~~Top-level programs~~
- [x] ~~Improved pattern matching~~
  - [x] ~~Simple type patterns~~
  - [x] ~~Relational patterns~~
  - [x] ~~Logical patterns~~
- [x] ~~Improved target typing~~
  - [x] ~~Target-typed `new` expressions~~
- [ ] Covariant returns

## Covariant returns

One item left: covariant returns. If you read the last post, this should be familiar to you. With return type covariance, you can override a base class method (that has a less-specific type) with one that returns a more specific type. To return some new objects, you could try this.

```csharp
public virtual Avenger GetHero()
{
    // this is the parent (or base) class
    return new Avenger();
}

public override Hulk GetHero()
{
    // better!
    return new Hulk();
}
```

We did it!

- [x] ~~Init-only properties~~
  - [x] ~~Init accessors and readonly fields~~
- [x] ~~Records~~
  - [x] ~~`with` expressions~~
  - [x] ~~Value-based equality~~
  - [x] ~~Positional records~~
  - [x] ~~`with` expressions and inheritance~~
  - [x] ~~Value-based equality and inheritance~~
- [x] ~~Top-level programs~~
- [x] ~~Improved pattern matching~~
  - [x] ~~Simple type patterns~~
  - [x] ~~Relational patterns~~
  - [x] ~~Logical patterns~~
- [x] ~~Improved target typing~~
  - [x] ~~Target-typed `new` expressions~~
- [x] ~~Covariant returns~~

## And that's it, friends  

Thank you so much for reading and providing feedback on all these articles. This won't be the last I write about C#, obviously, but it does conclude my first pass on C# 9 features. As always, I'm excited to hear how things are going for you—don't be shy!
