---
date: "2021-01-31"
title: "How to achieve style inheritance with Blazor CSS isolation"
tags: [csharp,blazor]
share-img: /assets/img/signed-exchanges.png 
subtitle: We explore how to achieve traditional style inheritance with Blazor CSS isolation.
---

Are you familiar with Blazor CSS isolation? I [wrote about it in tremendous detail](https://daveabrock.com/2020/09/10/blazor-css-isolation) last year. You can check out the post for details but long story short, Blazor CSS isolation allows you to scope styles to a specific component. This allows you to write styles in a way that doesn't lead to the issues that occur when you throw all your styles in a global file.

As I've spoken about Blazor CSS isolation to a few user groups in my local community, the inevitable question always comes up: how can I inherit styles? The purpose of this post is to understand how to make this happen.

# Defining what "style inheritance" means

When I say "style inheritance" you might interpret it in a variety of ways. To be clear, I am *not* referring to how you can pass styles down to child components, such as when you want all of a component's children to have the same `h1` style declaration. As I [wrote about in my introductory post](https://daveabrock.com/2020/09/10/blazor-css-isolation#how-to-work-with-child-components), you can do this using a `::deep` combinator.

When I say style inheritance, I'm referring to having a base set of styles shared across your components, then overriding when appropriate. You may be doing this without CSS isolation. For example, if you define most of your styles by bringing in a CSS library, like Bootstrap, you'll get 90% there—then you'll have a custom stylesheet (like `custom.css`) that overrides those when appropriate.

When thinking of Blazor CSS isolation and how to inherit styles, it's common for developers to think of it as a class-based system. For example, If you think of it like we do in C# code, it's like having an `Animal` base class that contains common behaviors across animals. Then, specific animals will implement their own behavior (like when a `Dog` barks).

But here's the thing: as a compile-time, file-based solution, CSS isolation doesn't know anything about your class hierarchy. Instead, you'll need to group scope identifiers to accomplish this functionality.

Scope identifiers allow you to understand how to do a thing, and another thing, and another thing...






