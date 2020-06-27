---
date: "2020-06-24"
title: "Let's go on a C# 9 scavenger hunt!"
excerpt: Let's go through the major C# 9 features in a single application.
tags: [csharp]
---

Last week, we took a quick tour of some upcoming C# 9 features that will [make your development life easier](https://daveabrock.com/2020/06/18/reduce-mental-energy-with-c-sharp). We dipped our toes in the water with some simple, random examples - which is great! But now it's time to dig a little deeper, and have some fun while we're at it.

I had a thought: why not show off [all of the announced features](https://devblogs.microsoft.com/dotnet/welcome-to-c-9-0/) in a simple, centralized application? We could always dig deeper by what we see in the [Language Feature Status in GitHub](https://github.com/dotnet/roslyn/blob/master/docs/Language%20Feature%20Status.md), but the publicly-announced-at-Build features will most likely make it when .NET 5.0 launches in November 2020.

Here's our list. We will check off our list as we go.

- [ ] Init-only properties, accessors, and readonly fields
- [ ] Records - with-expressions
- [ ] Records - with-expressions and inheritance
- [ ] Records - value-based equality and inheritance
- [ ] Records - value-based equality
- [ ] Records - data members
- [ ] Records - positional records
- [ ] Top-level programs
- [ ] Pattern matching - simple type patterns
- [ ] Pattern matching - relational patterns
- [ ] Pattern matching - logical patterns
- [ ] Target typing - `new` expressions
- [ ] Target typing - `??` and `?:`
- [ ] Covariant returns

This is a lot. More than I bargained for, you might say!

If you'd like to play along, I'll be using [SharpLab](https://sharplab.io/) - a wonderful .NET compiler playground. In my last post, I mentioned you can use LinqPad to play with preview C# bits - and you still can! - but I prefer SharpLab for an exercise such as this, for fast feedback on all our tinkering.

In SharpLab, make sure you have the C# language selected in the **Code** drop-down menu, and in the top-right drop-down, select **Roslyn branches > master**. Feels good living on the edge, right? Let's party, like we all want to party: with a disclaimer.

**Heads up!** C# 9 is still in preview mode, so much of this content might change. I will do my best to update it as I come across it, but that is not guaranteed. Have fun, but your experience may vary.
{: .notice--danger}

## Top-level programs

Let's start with an easy one. Throw this into SharpLab, and from the **Results** drop down, select **Experimental > Run**.

```csharp
using System;

Console.WriteLine("This is our new app.");
```

Check it out, a running app with no `Main()` method! Any statement is allowed, so long as it is after your `using` statements, and before namespace declarations. We can even work with `args`, built-in, and local functions!

One off the list, good job by you.

- [ ] Init-only properties, accessors, and readonly fields
- [ ] Records - with-expressions
- [ ] Records - with-expressions and inheritance
- [ ] Records - value-based equality and inheritance
- [ ] Records - value-based equality
- [ ] Records - data members
- [ ] Records - positional records
- [x] Top-level programs
- [ ] Pattern matching - simple type patterns
- [ ] Pattern matching - relational patterns
- [ ] Pattern matching - logical patterns
- [ ] Target typing - `new` expressions
- [ ] Target typing - `??` and `?:`
- [ ] Covariant returns

## Target-typed new expressions