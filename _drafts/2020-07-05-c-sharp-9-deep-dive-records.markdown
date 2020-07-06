---
date: "2020-07-05"
title: "C# 9 Deep Dive: Records"
excerpt: In a C# 9 deep dive, we go in-depth on records.
tags: [csharp]
header:
    overlay_image: /assets/images/records.png
    overlay_filter: 0.8
---

In the [previous post of this series](https://daveabrock.com/2020/06/29/c-sharp-9-deep-dive-inits), we discussed the init-only features of C# 9, which allowed you to make individual properties immutable. That works great on a case-by-case basis, but the real power in this functionality is when you can do this for objects. This is where records shine, and will be the focus of this post.

This is the second post in a five-post series on C# 9 features:

- Post 1 - [Init-only features](https://daveabrock.com/2020/06/29/c-sharp-9-deep-dive-inits)
- Post 2 (*this post*) - Records
- Post 3 (*future post*) - Pattern matching
- Post 4 (*future post*) - Target typing and covariant returns
- Post 5 (*future post*) - Putting it all together: an all-in-one application

**Heads up!** C# 9 is still in preview mode, so much of this content might change—this post was last updated on July 5, 2020. I will do my best to update it as I come across it, but that is not guaranteed. Have fun, but your experience may vary.
{: .notice--danger}

## The power of immutability

Before we get started, let's briefly talk about the concept of immutability. For sure, immutability to some is a stuffy word but its premise is simple: once instantiated or initialized, immutable types never change. This is at the core of functional, or "pure", programming—the concept of eliminating side effects by mutating objects directly.

What do you get with immutable types? First, you get simplicity. An immutable object only has one state, the state you specified when you created the object. You'll also see that they are secure and thread-safe with no required synchronization. Because you don't have threads fighting to change an object, they can be shared freely in your applications.

Put another way: **immutable types reduce risk, are safer, and help to prevent a lot of nasty bugs that occur when you update your objects.**

Even if you aren't familiar with this concept yet, or haven't been forced to think this way, you're already using it in the .NET world. For example, `System.DateTime` is immutable, as are strings. And now with records in C# 9, you can create your own immutable objects.

## Create your first record

To declare a record, you use the new `record` keyword.

If you've read Microsoft's [*Welcome to C# 9.0 post,*](https://devblogs.microsoft.com/dotnet/welcome-to-c-9-0/) they declare records using the `data class` syntax instead of `record`. Since then, it has changed to `record` and their article has not been updated. **EXPLAIN WHY**
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

As of this writing, the `record` declaration is not enough to get complete object immutability. You will also need to use the `{ get; init; }` syntax for your properties, as well. Likewise, if you've read about auto-property improvements (for example, changing `public string FirstName { get; init; }` to `public string FirstName;`), this does not make the fields immutable-by-default either. This allows you the flexibility to have some properties mutable on a case-by-case basis.
{: .notice--info}

## Use with expressions with records

Before C# 9, you would do the following to represent new state using immutability.

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
```

This pattern—creating new values from existing ones to represent new state—is referred to as non-destructive mutation (now *that* will make you seem smart at your next dinner party!). C# 9 has a new type of expression, a `with` expression, to assist you.

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

C# 9 introduces another great feature that enable a whole object to be immutable and make it acting like a value

**add information about structs here**

BY DEFAULT, RECORDS ARE NOT IMMUTABLE BY THEMSELVES

## Understand what records are to evaluate equality

We have been talking a lot of records being value-like, without much explanation. Now that we have some context, we can dig deeper on this concept.

Let's be abundantly clear: records are reference types.

**INCLUDE STRUCT INFO**

No! Records are reference type, they behave like value type (like Structs) for comparison. Like Structs they override Equals virtual method to have “value-based equality”, comparing each field of the struct by calling Equals on them recursively. – https://devblogs.microsoft.com/dotnet/welcome-to-c-9-0/#value-based-equality – Microsoft

It means ReferenceEquals (and ==) are comparing Records object reference, unlike Struct which will always return false. Examples:

```csharp
namespace CSharp9Demo
{
    public record RecordProduct
    {
      public string Name { get; set; }
      public int CategoryId { get; set; }
    }

    public struct StructProduct
    {
      public string Name { get; set; }
      public int CategoryId { get; set; }
    }

    class Program
    {
        static void Main(string[] args)
        {
            // Records
            var recordProduct1 = new RecordProduct
            {
                Name = "VideoGame",
                CategoryId = 1
            };

            var recordProduct2 = recordProduct1;

            Console.WriteLine(recordProduct2.Equals(recordProduct1)); // returns true;
            Console.WriteLine(recordProduct2 == recordProduct1); // returns true;
            Console.WriteLine(ReferenceEquals(recordProduct2, recordProduct1)); // returns true;

            // Structs
            var structProduct1 = new StructProduct
            {
                Name = "VideoGame",
                CategoryId = 1
            };

            var structProduct2 = structProduct1;

            Console.WriteLine(structProduct2.Equals(structProduct1)); // returns true;
            Console.WriteLine(structProduct2 == structProduct1); // returns false;
            Console.WriteLine(ReferenceEquals(structProduct2, structProduct1)); // returns false;
        }
    }
}
```

## Can class have record as a property?

Yes.

```csharp
namespace CSharp9Demo
{
    public record Specification 
    {
        public string Description { get; set; }
    }

    public class Product
    {
        public string Name { get; init; }
        public int CategoryId { get; init; }
        public Specification Specification { get; set; }
    }
    
    class Program
    {
        static void Main(string[] args)
        {
            var spec = new Specification
            {
                Description = "Spec description"
            };

            var product = new Product
            {
                Name = "VideoGame",
                CategoryId = 1,
                Specification = spec
            };
        }
    }
}
```

## Can a record have a struct and object?

Yes.

```csharp
namespace CSharp9Demo
{
    public class Specification
    {
        public string Description { get; set; }
    }

    public struct Options
    {
        public bool IsActive { get; set; }
    }

    public record Product
    {
        public string Name { get; init; }
        public int CategoryId { get; init; }
        public Specification Specification { get; set; }
        public Options Options { get; set; }
    }
    
    class Program
    {
        static void Main(string[] args)
        {
            var spec = new Specification
            {
                Description = "Spec description"
            };

            var options = new Options 
            {
                IsActive = true
            };

            var product = new Product
            {
                Name = "VideoGame",
                CategoryId = 1,
                Specification = spec,
                Options = options
            };
        }
    }
}
```

## Records are not immutable by default

## Are with expressions available for structs and classes?

No.

## Wrapping up

In this post, we **DISCUSS WHAT WE DID HERE**.
