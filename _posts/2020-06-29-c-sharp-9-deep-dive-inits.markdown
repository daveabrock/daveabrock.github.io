---
date: "2020-06-29"
title: "C# 9 Deep Dive: Init-only features"
excerpt: In a C# 9, deep dive, we will first walk through init-only features.
tags: [csharp]
header:
    og_image: /assets/images/stacks-graph.jpg
---

A few weeks ago, we took a quick tour of some upcoming C# 9 features that will [make your development life easier](https://daveabrock.com/2020/06/18/reduce-mental-energy-with-c-sharp). We dipped our toes in the water. But now it's time to dig a little deeper.

I'm starting a new series over the next several weeks, that will showcase [all of the announced features](https://devblogs.microsoft.com/dotnet/welcome-to-c-9-0/) incrementally. Then, we will tie it all together with an all-in-one app. As for the features we are showing off, we could always dig deeper by what we see in the [Language Feature Status in GitHub](https://github.com/dotnet/roslyn/blob/master/docs/Language%20Feature%20Status.md), but the publicly-announced-at-Build features will most likely make it when .NET 5.0 launches in November 2020.

Here's what we'll be walking through:

- Post 1 (*this post*) - Init-only features
- Post 2 (*future post*) - Records
- Post 3 (*future post*) - Pattern matching
- Post 4 (*future post*) - Target typing and covariant returns
- Post 5 (*future post*) - Putting it all together: an all-in-one application

**Heads up!** C# 9 is still in preview mode, so much of this content might change. I will do my best to update it as I come across it, but that is not guaranteed. Have fun, but your experience may vary.
{: .notice--danger}

## Init-only features

With C# 9, the team is shipping a bunch of great init-only features. This includes init-only properties, init accessors, and readonly fields.

### Init-only properties

For virtually all of your C# life, you've done something like the following:

```csharp
public class Person
{
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public string Address { get; set; }
    public string City { get; set; }
    public string FavoriteColor { get; set; }
    // and so on...
}
```

Of course, you can throw a `private set` in there for access control, but your options have typically been limited for your getters and setters. However, this forces mutability when you perform object initialization. For this to work, properties must be mutable. To set values in a new `Person`, you'll need to call the object's constructor (in this case, as in most cases, a parameterless constructor) and then perform assignment from the setters.

To initialize a new `Person`, you're used to doing this:

```csharp
var person = new Person
{
    FirstName = "Tony",
    LastName = "Stark",
    Address = "10880 Malibu Point",
    City = "Malibu",
    FavoriteColor = "Red"
};
```

And since this is mutable, you can easily do this:

```csharp
Console.WriteLine(person.FirstName); // Tony
person.FirstName = "Howard";
Console.WriteLine(person.FirstName); // Howard
```

With C# 9, we can change this with an `init` accessor. This means you can only create and set a property when you initialize the object. If we modify our `Person` model like this, we can prevent the `FirstName` from being changed:

```csharp
public class Person
{
    public string FirstName { get; init; }
    public string LastName { get; set; }
    public string Address { get; set; }
    public string City { get; set; }
    public string FavoriteColor { get; set; }
    // and so on...
}
```

With this, that means this code will work:

```csharp
var person = new Person
{
    FirstName = "Tony",
    LastName = "Stark",
    Address = "10880 Malibu Point",
    City = "Malibu",
    FavoriteColor = "Red"
};
```

However, when you try to execute this code:

```csharp
person.FirstName = "Howard";
```

The compiler will not be happy.

### Init accessors and read-only fields

As we just saw, `init` accessors can only be called when you initialize. If you wish to work with `readonly` fields, the mutability only applies during initialization, just as with non-read-only ones.

With this in mind, using private fields will set a value during initialization - otherwise, we will have an `ArgumentNullException` thrown:

```csharp
public class Person
{
    private readonly string _firstName;
    private readonly string _lastName;
    private readonly string _address;
    private readonly string _city;
    private readonly string _favoriteColor;

    public string FirstName
    {
        get => _firstName;
        init => _firstName = (value ?? throw new ArgumentNullException(nameof(FirstName)));
    }
    public string LastName
    {
        get => _lastName;
        init => _lastName = (value ?? throw new ArgumentNullException(nameof(LastName)));
    }
    public string Address
    {
        get => _address;
        init => _address = (value ?? throw new ArgumentNullException(nameof(Address)));
    }
    public string City
    {
        get => _city;
        init => _city = (value ?? throw new ArgumentNullException(nameof(City)));
    }
    public string FavoriteColor
    {
        get => _favoriteColor;
        init => _favoriteColor = (value ?? throw new ArgumentNullException(nameof(FavoriteColor)));
    }
}
```

## What's next

In this post, we learned how to make individual properties become immutable. If you want this behavior for your entire object, you'll want to work with records - one of the best new features in C# 9, in my opinion. Stay tuned for my next post to discuss this.
