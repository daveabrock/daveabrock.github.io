---
date: 2021-01-19 00:00:00 -0600
title: How to use configuration with C# 9 top-level programs
subtitle: In this post, let's use C# 9 top-level programs to read from configuration.
tags:
- csharp-9
- csharp
share-img: "/assets/img/top-level-config.png"

---
I've been working with [top-level programs in C# 9](https://daveabrock.com/2020/07/09/c-sharp-9-top-level-programs) quite a bit lately. When writing simple console apps in .NET 5, it allows you to remove the ceremony of a namespace and a `Main(string[] args)` method. It's very beginner-friendly and allows developers to get going without having to worry about learning about namespaces, arrays, arguments, and so on. While I'm not a beginner—although I feel like it some days—I enjoy it for prototyping things quickly.

You can work with normal functions, use `async` and `await`, access command-line arguments, and use local functions. For example, here's me getting a random quote from the [Ron Swanson Quotes API](https://github.com/jamesseanwright/ron-swanson-quotes#ron-swanson-quotes-api):

    using System;
    using System.Net.Http;
     
    var quote = await new HttpClient().GetStringAsync("https://ron-swanson-quotes.herokuapp.com/v2/quotes");
    Console.WriteLine($"A quote to live by: {quote}");
    

This outputs the following:

    A quote to live by: ["If there were more food and fewer people, this would be a perfect party."]