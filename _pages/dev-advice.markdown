---
title: "Dev Advice"
permalink: /dev-advice
---

As part of my weekly newsletter, The .NET Stacks, I interview leaders throughout the community. The last question is always the same: what is your one piece of programming advice?

I've compiled all the answers here. Enjoy!

## Isaac Levin

The one piece of advice I have is to not worry about your quality as a developer. For a long time in my career (and still to this day) I don’t think I am any good. But honestly, whatever you think isn’t probably true. Are there bad developers? Of course. But there are far more good developers to think they are bad.

The only way you fix this is to alter your mindset, and write code and learn.

Take an opportunity to learn new things, watch dev streamers, read docs, whatever you can to learn. Slowly you will realize the only thing that was causing your imposter syndrome was in your head.

## Shahed Chowdhuri

One piece of advice I would give anyone is to always keep learning. That doesn’t necessarily mean that you should learn every new programming language or framework that pops up on your radar. It could mean that you may want to dig deeper into a language you’ve already been working with. It could be some business skills that you want to pick up, to run your own software business or work as a consultant to help others run theirs.

## Michael Crump

I actually [published a list of my top 12 things every developer should know](https://dev.to/mbcrump/12-things-every-software-developer-should-be-doing-in-2020-5hbp).

My top one would probably be to learn a different programming language (other than your primary language). Simply put, it broadens your perspective and permits a deeper understanding of how a computer and programming languages work.

## Jeremy Likness

After decades of building software my biggest piece of advice is that your first step shouldn’t be to find the library or framework or tool, but to solve the problem.

Too often people add frameworks or tools or patterns because they’re recommended, rather than actually determining if they add value. Many times the overhead outweighs the benefit. I’m often told, “That solution isn’t right because you don’t have a business logic layer.” My response is, “So?” It’s not that I don’t see value in that layer for certain implementations, but that it’s not always necessary.

Have you worked on that project that forced an architecture so that one change involves updating five projects and the majority of them are just default “pass through” implementations? I am a fan of solving for the solution, and only then if you find some code is repeated, refactor. Don’t over-engineer or complicate. One of my favorite starting points for a solution is to consider, “What is the ideal way I’d like to code for this?”

For example, I am building a Blazor MVVM implementation. For testing the filtering and sorting that the view model applies, I would love to pass an `IQueryable<Entity>` then do something like this…

```csharp
Assert.That(query.CalledMethod(nameof(Enumerable.Take)).WithValue(5))
```

…to verify that the view model applied the right extensions for paging. I start with what is easy to read and understand, then work backwards to provide an API that satisfies it.

As another example, when you raise property change notifications, you often have dependencies. If I have Quantity and CostPerItem, then TotalCost changes whenever Quantity or CostPerItem does.

I’d love to say …

```csharp
TotalCost.DependsOn(CostPerItem).AndAlso(Quantity)
```

… so I start with that, then build the appropriate interfaces to make it work. That in my opinion leads to readable, maintainable code.

## Scott Addie

My advice is to ask probing questions and challenge the status quo. Regardless of your organizational rank, ask why your team operates the way they do. Ask why certain technology choices were made. The answers might surprise you. Equipped with those answers, you can begin to understand existing processes and formulate improvements. If you have an idea, don’t be afraid to speak up. Everyone brings a unique perspective to the table.

## Luis Quintanilla

Just do it! Everyone has different learning styles, but I strongly believe no amount of videos, books or blog posts compare to actually getting your hands dirty. It can definitely be daunting at first, but no matter how small or basic the application is, building things is always a good learning experience. Often there is a lot of work that goes into producing content, so end users typically get the polished product and the happy path. When you’re not sure of where to start or would like to go more in depth, these resources are excellent. However, once you stray from that guided environment, you start making mistakes. Embrace these mistakes because they’re a learning experience.

## Isaac Abraham

Great question 😊 I think one thing I try to keep in mind is to avoid premature optimisation and design. Design systems for what you know is going to be needed, with extension points for what will most likely be required. You can never design for every eventuality, and you’ll sometimes get it wrong, that’s life—optimise for what is the most likely outcome.

## Phillip Carter

The biggest piece of advice I would give people is to look up the different programming paradigms, functional being one of them, and try out a language some of them. Most programmers are used to an ALGOL-derived language, and although they are great languages, they all tend to encourage the same kind of thought process for how you write programs. Programming can be a tool for thought, and languages from different backgrounds encourage different ways of thinking about solving problems with programming languages. I believe that this can make people better programmers even if they never use F# or other languages outside of the mainstream ones. Additionally, all languages do borrow from other ones to a degree, and understanding different languages can help you master new things coming into mainstream languages.
