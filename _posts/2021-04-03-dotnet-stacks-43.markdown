---
date: "2021-04-03"
title: "The .NET Stacks #43: 📅 DateTime might be seeing other people"
tags: [dotnet-stacks]
image: /assets/img/stacks-43-card.png 
description: This week, we discuss new Date and Time APIs, and have a first look at hot reload.
---

![Newsletter image](https://daveabrock.com/THE .NET STACKS.png)

Happy Monday! I hope you have a productive week and get more traction than an excavator on the Suez Canal. (Sorry.)

- **One big thing**: New APIs for working with dates and times
- **The little things**: Hot reload, debugging config settings, Clean Architecture resources
- Last week in the .NET world

***

# One big thing: New APIs for working with dates and times

If you're walking on the street and a person asks you the time, you can look at your watch and tell them. It's not so simple in .NET. If you only care about the time, you need to call and parse a `DateTime` or `DateTimeOffset` and scrap any date information. This approach can be incredibly error-prone, as these structs carry time-zone-related logic that isn't written for getting the absolute date and time. (That doesn't even include [its general weirdness](https://twitter.com/khellang/status/1374852735384834053), like `DateTime.TimeOfDay` returning a `TimeSpan` and `DateTime.Date` returning a new `DateTime`. Even on its best days, the `DateTime` struct is both overloaded and confusing.)

Over [on GitHub](https://github.com/dotnet/runtime/issues/49036), there's a spicy issue (with some bikeshedding) discussing the possibility of a `Date` struct and a `Time` struct. Initially, Microsoft chose to expose the features as an external NuGet package. However, .NET developers are looking for the changes to be in the core of the .NET BCL APIs to avoid third-party library dependencies. The issue [has a proposed API](https://github.com/dotnet/runtime/issues/49036#issue-820493809). 

You may see these named differently as [Visual Basic has used `Date`](https://github.com/dotnet/runtime/issues/49036#issuecomment-808582454) for three decades, and this would include a breaking change (and it might confuse folks using C# and VB). Right now, `DateOnly` seems to be the new name. I hope this is not a permanent name. 

Here's an interesting comment from the issue about a user's scenario:

> I'm instead interested in the Date class most of all ... We have a lot of products that is valid from date X to date Y. When we used DateTimeOffset we always had to remember to start at date X 00:00:00 and end at date Y 23:59:59.999. Then we switched to NodaTime and used LocalDate from there and all a sudden we could just compare dates without bother about time. We store Date in the database and map the column to a LocalDate now instead of a DateTimeOffset (which makes no sense since Date in the DB don't have time or offset) which makes our lives even easier.

This is all developers want from their SDKs: simplicity and predictability. The excellent [NodaTime library](https://nodatime.org/) provides these capabilities, but it isn't coming from Microsoft—which stifles adoption. It'll be great to have simpler APIs with working with dates *or* times, but it might be a while before we see it in the wild. (And for the record, `DateTime` isn't going anywhere.)

***

# The little things: Hot reload, debugging config settings, Clean Architecture resources

Say what you will about JavaScript (and believe me, [I have](https://daveabrock.com/2020/09/02/how-to-not-hate-javascript)), but it's no mystery why it's so widely used and adopted: the barrier of entry is low. I can open a text file, write some HTML markup (and maybe some JS), and open my web page to see the results. The instant feedback boosts productivity: in most common front-end frameworks, you can use "hot reload" functionality to see your changes immediately without having to restart the app.

That functionality is coming to ASP.NET Core (and Blazor) in .NET 6—we got a quick preview this week at Steve Sanderson's [talk at NDC Manchester](https://youtu.be/5NqXBFn9v20?t=16050). He showed off a promising prototype. Steve added and updated components (while preserving state), updated C# code, and worked well. The error handling looks nice, too—the app shows an error at the top of your page and will go away once you correct your issues and save. In some scenarios where you can't do hot reload, your app will refresh your browser as it does now. This functionality is almost ready to show off to a larger audience—look for it in .NET 6 Preview 3, which will be released in the next few weeks.

He also showed off an `ErrorBoundary` component, allowing developers to have greater control over error handling in Blazor apps. As it is today, when unhandled exceptions occur, the user connection is dropped. With this new functionality, you can wrap an `ErrorBoundary` around specific markup, where unhandled exceptions in that section of code still allow users to access other parts of the app.

***

A month ago, our friend [Cecil Phillip tweeted](https://twitter.com/cecilphillip/status/1362064796057755651):

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">IConfigurationRoot.GetDebugView is really useful for inspecting your resolved configuration settings! 👨🏽‍💻 It&#39;s been around since .NET Core 3.0 and I never knew! 👀<a href="https://t.co/iEKpDg4Rd3">https://t.co/iEKpDg4Rd3</a><a href="https://twitter.com/hashtag/dotnet?src=hash&amp;ref_src=twsrc%5Etfw">#dotnet</a> <a href="https://twitter.com/hashtag/aspnetcore?src=hash&amp;ref_src=twsrc%5Etfw">#aspnetcore</a> <a href="https://t.co/MxtFO9Btr5">pic.twitter.com/MxtFO9Btr5</a></p>&mdash; Cecil L. Phillip 🇦🇬 (@cecilphillip) <a href="https://twitter.com/cecilphillip/status/1362064796057755651?ref_src=twsrc%5Etfw">February 17, 2021</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

I never knew about this, either—it helps you discover your environmental variables and settings and where they're getting read. (And, of course, it should be used for development purposes only so you don't accidentally leak secrets.)

***

When it comes to recommending architecture solutions, I use the two words that consultants live by: It Depends®. Even so, there's a lot to like about the [Clean Architecture movement](https://pusher.com/tutorials/clean-architecture-introduction).

This week, Patrick Smacchia [wrote about](https://blog.ndepend.com/clean-architecture-for-asp-net-core-solution/) Jason Taylor's [popular solution template](https://github.com/jasontaylordev/CleanArchitecture). Also, Mukesh Murugan has [released *Blazor Hero*](https://codewithmukesh.com/blog/blazor-hero-quick-start-guide/), a clean architecture template that includes common app scenarios under one umbrella. These are both good resources on understanding Clean Architecture.

***

Lastly, [this is exciting](https://twitter.com/dodyg/status/1374992298661003267):

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">MapAction is a new way to create Web APIs without relying on Controller. (.NET 6 Preview 2)<a href="https://t.co/phsVF3UAgB">https://t.co/phsVF3UAgB</a><a href="https://twitter.com/hashtag/aspnetcore?src=hash&amp;ref_src=twsrc%5Etfw">#aspnetcore</a> <a href="https://t.co/Eveurkskk3">pic.twitter.com/Eveurkskk3</a></p>&mdash; dodyg (@dodyg) <a href="https://twitter.com/dodyg/status/1374992298661003267?ref_src=twsrc%5Etfw">March 25, 2021</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

# 🌎 Last week in the .NET world

These links are new. I checked.

## 🔥 The Top 5

- Andrew Lock [debugs configuration values in ASP.NET Core](https://andrewlock.net/debugging-configuration-values-in-aspnetcore/).
- Steve Gordon [provides an introduction to the Roslyn APIs](https://www.stevejgordon.co.uk/getting-started-with-the-roslyn-apis-writing-code-with-code).
- Rachel Appel [works with ASP.NET Core route templates in ReSharper and Rider](https://blog.jetbrains.com/dotnet/2021/03/24/work-with-asp-net-core-route-templates-in-resharper-and-rider/).
- Ben Nadel [writes about how he handles PRs on his team](https://www.bennadel.com/blog/4013-an-opinionated-guide-to-handling-pull-requests-prs-on-my-team.htm).
- Patrick Smacchia [writes about a case study on Clean Architecture](https://blog.ndepend.com/clean-architecture-for-asp-net-core-solution/).

## 📢 Announcements

- The Azure SDK team [provides their March update](https://devblogs.microsoft.com/azure-sdk/march-release-2021), and also [announce the new Azure Event Grid client libraries](https://devblogs.microsoft.com/azure-sdk/event-grid-ga).
- ReSharper 2021.1 [is in beta](https://blog.jetbrains.com/dotnet/2021/03/25/resharper-2021-1-beta/), and so is [Rider 2021.1](https://blog.jetbrains.com/dotnet/2021/03/25/rider-2021-1-beta/).
- Rick Strahl [goes v1.0 with his LiveReloadServer](https://weblog.west-wind.com/posts/2021/Mar/23/LiveReloadServer-A-NET-Core-Based-Generic-Static-Web-Server-with-Live-Reload).

## 📅 Community and events

- Alberto Gimeno [describes how GitHub Actions renders large-scale logs](https://github.blog/2021-03-25-how-github-actions-renders-large-scale-logs/).
- Check out [Tess Ferrandez's repo with some nice debugging labs](https://github.com/TessFerrandez/BuggyBits).
- For community standups: ASP.NET [talks about Balea authorization](https://www.youtube.com/watch?v=UnbJrC0WN1U) and Entity Framework [talks about what's new in EF Core Power Tools](https://www.youtube.com/watch?v=3-Izu_qLDqY). 
- Zack Koppert [writes about solving innersource discovery problems at GitHub](https://github.blog/2021-03-23-solving-the-innersource-discovery-problem/).
- The .NET Docs Show [talks to Steve Smith about API endpoints](https://www.youtube.com/watch?v=9oroj2TmxBs).

## 🌎 Web development

- Cody Merritt Anhorn [excludes files from the PWA asset cache in Blazor](https://codyanhorn.tech/blog/blazor/2021/03/24/Blazor-Excluding-Files-from-PWA-Asset-Cache.html).
- Kevin Dockx [works with OData in ASP.NET Core](https://www.pluralsight.com/blog/software-development/odata-asp-net-core).
- Matthew Jones [writes a Tetris game in Blazor WebAssembly](https://exceptionnotfound.net/tetris-in-blazor-webassembly/).
- Damien Bowden [sets dynamic metadata for Blazor Web Assembly](https://damienbod.com/2021/03/23/setting-dynamic-metadata-for-blazor-web-assembly/).
- Jeetendra Gund [gets HttpContext in ASP.NET Core](https://www.telerik.com/blogs/how-to-get-httpcontext-asp-net-core).
- The Code Maze blog [creates resilient microservices in .NET with Polly
](https://code-maze.com/creating-resilient-microservices-in-net-with-polly/).
- Daniel Gomez Jaramillo [builds an event information portal with ASP.NET Core 5](https://www.c-sharpcorner.com/article/building-an-event-information-portal-with-asp-net-5/).
- Claudio Bernasconi [handles APIs in Blazor](https://www.claudiobernasconi.ch/2021/03/20/blazor-api-handling/).
- Khalid Abuhakmeh [builds a basic HTTP API in ASP.NET Core](https://khalidabuhakmeh.com/how-to-build-a-basic-http-api-with-aspnet-core).
- Ricardo Peres [warns about null models in POST requests in ASP.NET Core](https://weblogs.asp.net/ricardoperes/asp-net-core-pitfalls-null-models-in-post-requests).

## 🥅 The .NET platform

- Kunal Pathak [writes about loop alignment in .NET 6](https://devblogs.microsoft.com/dotnet/loop-alignment-in-net-6).
- Niels Swimberghe says: [don't use `HttpContext.Current`, especially when using async](https://swimburger.net/blog/dotnet/don-t-use-httpcontext-current-especially-when-using-async).
- Peter Vogel [writes about WinUI](https://www.telerik.com/blogs/winui-yes-another-desktop-framework-but-cooler-than-you-might-think).
- David Ramel [writes about Microsoft's desktop dev options](https://visualstudiomagazine.com/articles/2021/03/25/desktop-options.aspx).
- Alex Klaus [works with caching and environment variables](https://alex-klaus.com/hosting-spa-in-dotnet/).

## ⛅ The cloud

- Paul Michaels [works on Service Bus management and auto-forwarding](https://www.pmichaels.net/2021/03/20/service-bus-management-and-auto-forwarding/).
- David Ramel [writes about what's next for Azure Functions](https://visualstudiomagazine.com/articles/2021/03/24/azure-functions-net5.aspx).
- Johnny Reilly [works with Bicep and Azure Pipelines](https://blog.johnnyreilly.com/2021/03/20/bicep-meet-azure-pipelines/).
- Joseph Guadagno creates a search suggestion widget [using Azure Maps Search Service and KendoUI](https://www.josephguadagno.net/2021/03/27/creating-a-search-suggestion-widget-using-azur-maps-search-service-and-kendoui-autocomplete.).

## 📔 Languages

- Jiří Činčura [fuses await using and await foreach and await](https://www.tabsoverspaces.com/233855-fusing-await-using-and-await-foreach-and-await).
- Jason Roberts [writes about async streams](http://dontcodetired.com/blog/post/ICYMI-C-8-New-Features-Asynchronous-Streams).
- Thomas Levesque [wraps up his series on C# 9 records as strongly-typed IDs](https://thomaslevesque.com/2021/03/19/csharp-9-records-as-strongly-typed-ids-part-5-final-bits-and-conclusion/).
- Thomas Claudius Huber [uses tuples in C# to initialize properties in the constructor and to deconstruct objects](https://www.thomasclaudiushuber.com/2021/03/25/csharp-using-tuples-to-initialize-properties/).
- Daniel B. Markham [blogs about the outlines of a supercompiler in F#](https://danielbmarkham.com/outlines-of-a-supercompiler-in-f/).

## 🔧 Tools

- Mark Downie [collects managed crash dumps on App Services for Linux](https://www.poppastring.com/blog/collecting-managed-crash-dumps-on-app-services-for-linux).
- Laurent Kempé [calls a Dapr service with gRPC](https://laurentkempe.com/2021/03/25/calling-dapr-service-with-grpc/).
- Kristoffer Strube [works with fake/dummy data in C# with Faker](https://blog.elmah.io/easy-generation-of-fake-dummy-data-in-c-with-faker-net/).
- Khalid Abuhakmeh [adds a view to an EFCore DbContext](https://khalidabuhakmeh.com/how-to-add-a-view-to-an-entity-framework-core-dbcontext).
- Cody Merritt Anhorn [displays a Docker build version](https://codyanhorn.tech/blog/2021/03/19/Display-a-Docker-Build-Version.html).
- Harshal Limaye [writes about 20 Git commands you should know](https://www.c-sharpcorner.com/article/20-git-commands-you-should-know/).

## 📱 Xamarin

- András Tóth [discuss best practices for reopening apps in Xamarin](https://www.banditoth.hu/2021/03/22/xamarin-forms-reopening-application-best-pratices/).
- Charlin Agramonte [animates page transitions in Xamarin Forms](https://xamgirl.com/animating-page-transitions-in-xamarin-forms/).
- Leomaris Reyes [replicates a banking exploration UI in Xamarin](https://www.syncfusion.com/blogs/post/replicating-banking-exploration-ui-in-xamarin.aspx).

## 🏗 Design, testing, and best practices

- The Software Alchemy blog [works on domain-driven design entity mapping strategies](https://blog.jacobsdata.com/2021/03/22/scaffold-your-clean-ddd-web-application-part-5-domain-driven-design-entity-mapping-strategies).
- Derek Comartin [asks: is a REST API with CQRS possible](https://codeopinion.com/is-a-rest-api-with-cqrs-possible/)?
- Ashley Davis [scales microservices on Kubernetes](https://thenewstack.io/scaling-microservices-on-kubernetes/).
- Priyanka [mocks with LINQ to unit test in ASP.NET Core](https://www.syncfusion.com/blogs/post/linq-mocking-in-asp-net-core.aspx).

## 🎤 Podcasts

- The .NET Rocks podcast [talks to Mads Kristensen about Visual Studio feedback](https://www.dotnetrocks.com/default.aspx?ShowNum=1732).
- The .NET Core Show discusses [emulating a video game system in .NET with Ryujinx](https://dotnetcore.show/episode-72-emulating-a-video-game-system-in-net-with-ryujinx/).
- The Azure DevOps Podcast talks to [Richard Campbell on the history of .NET](http://azuredevopspodcast.clear-measure.com/richard-campbell-on-the-history-of-net-episode-133).
- The Coding After Work podcast [talks to Carl Rippon about React, books, and Blazor](http://codingafterwork.com/2021/03/22/episode-57-react-writing-a-book-and-blazor-with-carl-rippon/).
- The 6-Figure Developer podcast [talks to James Avery about designing for scale](https://6figuredev.com/podcast/episode-188-designing-for-scale-with-james-avery/).
- The Adventures in .NET podcast [talks to Jason Bock about C# 9](https://devchat.tv/adventures-in-dotnet/net-061-C-9-deep-dive-with-jason-bock/).
- The Azure Podcast [talks about learning Azure SQL](http://azpodcast.azurewebsites.net/post/Episode-369-Learn-Azure-SQL).

## 🎥 Videos

- The Visual Studio Toolbox [talks to Sam Basu about desktop development choices](https://channel9.msdn.com/Shows/Visual-Studio-Toolbox/Choices-in-Desktop-Development?WT.mc_id=DOP-MVP-4025064).
- At Technology and Friends, [David Giard talks to Jason Farrell about Kubernetes](https://www.davidgiard.com/2021/03/22/JasonFarrellOnKubernetes.aspx).
- Azure Friday discusses [best practices for Azure Container Instances (ACI) with GitHub Actions](https://channel9.msdn.com/Shows/Azure-Friday/Best-practices-for-Azure-Container-Instances-ACI-with-GitHub-Actions).
- The On .NET Show [talks about C# source generators](https://dev.to/dotnet/on-net-episode-c-source-generators-3fd8) and [cloud native patterns](https://www.youtube.com/watch?v=PDdHa0ushJ0).

