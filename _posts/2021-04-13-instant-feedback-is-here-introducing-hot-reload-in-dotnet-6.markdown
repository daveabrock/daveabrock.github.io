---
date: "2021-04-13"
title: "Instant Feedback Is Here: Introducing Hot Reload in .NET 6"
description: "With .NET 6 Preview 3, you can use hot reloading with your ASP.NET Core apps."
tags: [blazor,csharp]
canonical_url: "https://www.telerik.com/blogs/instant-feedback-is-here-introducing-hot-reload-in-dotnet-6"
---

***NOTE**: This post originally appeared on the [Progress Telerik Blog](https://www.telerik.com/blogs/instant-feedback-is-here-introducing-hot-reload-in-dotnet-6).*

With .NET 6—and its release officially going live in November 2021—a big focus is [improving developer inner-loop performance](https://github.com/dotnet/core/issues/5510). The idea of inner-loop performance comes down to this: when I make a code change, how quickly can I see it reflected in my application?

With .NET Core, you can do better than the traditional save-build-run workflow [with `dotnet watch`](https://docs.microsoft.com/aspnet/core/tutorials/dotnet-watch?view=aspnetcore-5.0). The tool can watch for your source files to change, then trigger compilation. When running `dotnet watch` against your ASP.NET Core web apps—for MVC, Blazor, or Razor Pages—you still need to wait for recompilation and your app to reload. If you do this repeatedly throughout the day, sitting and waiting can become frustrating. Waiting 10 seconds one time doesn't seem like a huge deal. If you do this 100 times a day, you're killing almost 17 minutes waiting for your app to reload! 

Developers in other ecosystems—especially in the front-end space—are familiar with the concept of hot reloading: you save a file, and the change appears almost instantaneously. Once you work with hot reloading, it's tough to go back. As the .NET team tries to attract outsiders and new developers ([another goal for the .NET 6 release](https://github.com/dotnet/core/issues/5465)), not having this feature can be a non-starter to outsiders, even considering all the other wonderful capabilities a mature framework like .NET has to offer. (Not to mention that the insiders have been impatiently waiting for this for quite some time, too.)

The wait is finally over! Starting with .NET 6 Preview 3, [you can use hot reload](https://devblogs.microsoft.com/aspnet/asp-net-core-updates-in-net-6-preview-3/#initial-net-hot-reload-support) with your ASP.NET Core applications—including Blazor (both Blazor Server and Blazor WebAssembly), Razor Pages, and MVC. You can see hot reloading for static assets like CSS and compiled C# code as well. 

In this post, I'll show you how it works with a Blazor Server application. Hot reload currently only works with running `dotnet watch` from your terminal. In future previews, you'll see Visual Studio integration and support for client and mobile applications.

## How to Use Hot Reload Today

To try out hot reload today, you'll first need to install [the latest .NET 6 preview SDK](https://dotnet.microsoft.com/download/dotnet/6.0) (at the time of this post, it's *SDK 6.0.100-preview.3*). You'll need to do this whether or not you have the latest Visual Studio 2019 bits installed. It'll be a few months before the .NET 6 preview updates are in sync with Visual Studio updates.

Once you've installed the latest .NET 6 preview SDK, update your profile in your `Properties/launchSettings.json` file to one of the following.

- Blazor Server, MVC, and Razor Pages: `"hotReloadProfile": "aspnetcore"`
- Blazor WebAssembly: `"hotReloadProfile": "blazorwasm"`

Since I'm using Blazor Server, my profile looks like the following.

```json
"profiles": {
    "HotReloadDotNet6": {
      "commandName": "Project",
      "dotnetRunMessages": "true",
      "launchBrowser": true,
      "applicationUrl": "https://localhost:5001;http://localhost:5000",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      },
      "hotReloadProfile": "aspnetcore"
    }
  }
```

## Basic Usage

To start, let's change the heading of our `Index` component and see how quickly the reload occurs—it seems instantaneous.

![Change the heading text to see hot reload in action]({{ site.url }}{{site.baseurl}}/assets/img/hr-hello-friends.gif)

It's great that it's fast, but that's not very exciting. I just changed static text. What about changing the code that previously required recompilation of our application? In the `Counter` component, I'm going to increase the count by five every time a user clicks the button. You'll see the change occurs just as fast.

![Update C# code to see the instant feedback]({{ site.url }}{{site.baseurl}}/assets/img/hr-increment-by-5.gif)

What happens when we are working with user state? For example, if I am in the middle of a user interaction and change the app, will that interaction be reset? In my previous example, I clicked the `Counter` component's button until the count reached a value of 20. If I change the `currentCount` from `0` to `10`, will things reset?

The answer is no—the state is independently preserved! Watch as I continue increasing the counter. When I refresh the page, my counter starts at 10, as I'd expect.

![Hot reload preserves a component's state]({{ site.url }}{{site.baseurl}}/assets/img/hr-preserve-state.gif)

As we put it all together, watch how I can add multiple components with their own state and modify CSS—all with the instant feedback that hot reloading provides.

![Hot reload allows me to iterate quickly]({{ site.url }}{{site.baseurl}}/assets/img/hr-put-it-together.gif)

## Working with Errors

You might be wondering what happens when `dotnet watch` encounters runtime exceptions or build errors. Will it exit and decorate my screen with a stack trace? To try this out, let me fat-finger a variable name by changing the iterator to `currentCounts`.

![Hot reload handles errors gracefully]({{ site.url }}{{site.baseurl}}/assets/img/hr-build-error.gif)

When this happens, you'll see a red banner at the top of the page. It includes the specific error message and where it occurs. When I fix the error, a rebuild occurs. It isn't instantaneous, but it works well—most times, you don't have to shut down and restart the application.

![When I resolve my error, a rebuild occurs]({{ site.url }}{{site.baseurl}}/assets/img/hr-build-error-fix.gif)

## What Are the Limitations?

When you initially use `dotnet watch` with hot reload, your terminal will display the following message:

```bash
Hot reload enabled. For a list of supported edits, see https://aka.ms/dotnet/hot-reload. Press "Ctrl + R" to restart.
```

That link is [worth checking out](https://aka.ms/dotnet/hot-reload) to see which changes are supported. With a few exceptions, most of these changes are supported.

- Types
- Iterators
- Async/await expressions
- LINQ expressions
- Lambdas
- Dynamic objects

As for the unsupported changes, here are a few of the major ones:

- Renaming elements
- Deleting namespaces, types, and members
- Adding or modifying generics
- Modifying interfaces
- Modifying method signatures

When you make a change that hot reload doesn't support, it'll fall back to typical `dotnet watch` behavior. Your app will recompile and reload in your browser. For example, this occurs when I rename my method from `IncrementCount` to `IncrementCounts`.

![When hot reload isn't supported, it falls back]({{ site.url }}{{site.baseurl}}/assets/img/hr-method-rename.gif)

## Conclusion

In this post, I introduced hot reload functionality that is new in .NET 6 Preview 3. I discussed how to use it today, and how it handles various development scenarios. I also discussed which changes are unsupported. 

While Blazor receives a lot of fanfare in ASP.NET Core, it's important to note that this benefits the entire .NET ecosystem. For example, you can use this now with Razor Pages and MVC—and you'll soon see support with client and mobile apps as well. Give it a shot today, and let me know what you think!
