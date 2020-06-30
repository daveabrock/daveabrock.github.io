---
date: "2020-06-24"
title: "On simplifying null validation with C# 9"
excerpt: An update on simplified null checking in C# 9.
tags: [csharp]
---

In my last post, I took a [test drive through some C# 9 features](https://daveabrock.com/2020/06/18/reduce-mental-energy-with-c-sharp) that might make your developer life easier. In it, I mentioned using logical patterns, such as the `not` keyword to throw an `ArgumentException`, if wanted:

```csharp
not null => throw new ArgumentNullException($"Not sure what this is: {yourArgument}", nameof(yourArgument))
```

## Championed proposal: simplified null-parameter checking

As it turns out, there is a new championed proposal which appears to gain some traction called [simplified null-parameter checking](https://github.com/dotnet/csharplang/issues/2145). Some folks have written about this, making it seem like it's a sure thing in C# 9, so I'd like to clarify some things I learned after doing some research (and, of course, hit up the comments if I'm incorrect as things are changing frequently).

As for the proposal itself: let's say you do what you've done quite a few times in regular C# code:

```csharp
public void DoSomethingCool(string coolString)
{
    if (coolString is null)
    {
        throw new ArgumentNullException($"Ooh, can't do anything with {coolString}", nameof(coolString));
    }

    // proceed to do some cool things
}
```

In this C# 9 proposal, you can add `!` to your parameter name to simplify things. Try this one instead:

```csharp
public void DoSomethingCool(string coolString!)
{
    // proceed to do some cool things
}
```

I have mixed feelings about this proposal.

Not only are you *super excited* about your parameter, you're also asking the C# parameter to trigger standard null checks for it. It is important to mention this is for runtime checks only and does not impact the type system. Therefore, the check is on the value and not the type. I love the clarity.

But, there's a lot here:

- Adding `!` in a C-based language might confuse developers who have a mental model of the negation operator
- It's very single-use and not very extensible
- There seems to be other approaches that make more sense like a `[NullCheck]` attribute, as was suggested, or using asserts, or even a project file directive

Anyway, if you look at the [Language Feature Status](https://github.com/dotnet/roslyn/blob/master/docs/Language%20Feature%20Status.md) and even the [spirited issue itself](https://github.com/dotnet/csharplang/issues/2145), it is very much in progress. If you're looking to simplify null checking in C# 9, for now, I would depend on using logical patterns instead.
