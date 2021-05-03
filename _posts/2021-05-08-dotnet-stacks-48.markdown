---
date: "2021-05-08"
title: "The .NET Stacks #48: ⚡ Sockets. Sockets everywhere."
tags: [dotnet-stacks]
image: /assets/img/stacks-48-card.png 
description: What is Azure Web PubSub, and how is it different than SignalR?
---

![Newsletter image](https://daveabrock.com/THE .NET STACKS.png)

*NOTE: This is the web version of my weekly newsletter, which was released on May 03, 2021. To get the issues right away, subscribe at [dotnetstacks.com](https://dotnetstacks.com) or at the bottom of this post.*

Happy Monday! Here's what we're talking about this week:

- **One big thing**: Microsoft announces Azure Web PubSub
- **The little thing**: Logging middleware coming, new try-convert release, and EF perf improvements
- Last week in the .NET world

***

# Microsoft announces Azure Web PubSub

Last week, Microsoft rolled out a [public preview of Azure Web PubSub](https://azure.microsoft.com/blog/easily-build-realtime-apps-with-websockets-and-azure-web-pubsub-now-in-preview), a managed service for building real-time web applications using web sockets. According to Microsoft, Azure Web PubSub enables you to use WebSockets and the publish-subscribe pattern to build real-time web applications easily. This service helps you manage a lot of concurrent WebSocket connections for data-intensive apps. In these scenarios, implementing at scale is a challenge. It lowers the barrier to entry: all you need is a WebSocket client and an HTTP client. Think about simple messaging patterns with a lot of connections. What’s nice here is that you can use any addressable endpoint with Azure Web PubSub. Azure Web PubSub integrates with Azure Functions natively, with the capability to build serverless apps with WebSockets and C#, Python, Java, or JavaScript. You use anything that can connect from a web socket, even desktop apps.

Here's what's on our minds: how is this different than the SignalR service? While both services are internally built from similar tech, the most significant difference is that there's no client or protocol requirement with Azure Web PubSub. You can bring your WebSocket library if you wish. And also, unlike SignalR, you're just working with WebSockets here—you won't see automatic reconnect scenarios, long polling, and whatnot.

What does this mean for SignalR? Nothing. As a matter of fact, according to David Fowler, here's when you'd want to stick with SignalR:

- You're a .NET-specialized dev shop and have SignalR client libraries and expertise
- You need fallbacks other than WebSockets, like long polling or server-sent events
- The existing SignalR client platforms work for you
- You don't want to manage a custom protocol and need more complex patterns (and want SignalR to manage it for you)
- You don't want to manage the reconnect logic yourself

Anthony Chu sums it up pretty well [when he says](https://twitter.com/nthonyChu/status/1388010951144722432):

>Personally, I would use SignalR unless you need to support clients that don’t have a supported SignalR library. You could write the code connect/reconnect/messaging code yourself but SignalR does it all for you with a nice API.

Speaking of WebSockets, WebSocket compression [is coming to .NET 6](https://github.com/dotnet/runtime/pull/49304) thanks to a community contribution from Ivan Zlatanov. It's an opt-in feature, and it looks like [Blazor won't be using it for now](https://twitter.com/ivan_zlatanov/status/1387528689479569409). Security problems can arise when server messages contain payloads from users and, as a result, shouldn't be compressed.

***
# The little things: Logging middleware coming, new try-convert release, and EF perf improvements

As we discussed last week, .NET 6 Preview 4 will be a big release with lightweight APIs and AOT on the list (if not more). For Preview 5, ASP.NET Core will be [introducing logging middleware](https://github.com/dotnet/aspnetcore/issues/31843). Logging request and response information isn't the most fun, so middleware will do a lot of the heavy lifting for you (and you can extend it if needed). By default, the logging middleware won't log response bodies, but [that should be a configuration detail](https://twitter.com/JustinKotalik/status/1388249145916358656). This is a problem every ASP.NET developer has to deal with, so it's nice to see it being generalized.

***

A new try-convert [release was shipped last week](https://github.com/dotnet/try-convert/releases/tag/v0.7.222801). This release includes enhancements for VB.NET Windows Forms conversions. The new, snazzy .NET Upgrade Assistant relies on this try-convert tool, which means VB.NET WinForms support has arrived with the Upgrade Assistant. That's all I'm going to say about that because the more you talk about VB.NET, the more someone calls you an expert.

***

I missed this last week, but the Entity Framework team announced the results from new TechEmpower benchmarks. It looks like EF Core 6 is 33% faster than EF Core 5, and EF Core is also almost 94% of the Dapper performance.   

<blockquote class="twitter-tweet"><p lang="en" dir="ltr"><a href="https://twitter.com/hashtag/EntityFramework?src=hash&amp;ref_src=twsrc%5Etfw">#EntityFramework</a> biweekly update <a href="https://twitter.com/hashtag/efcore?src=hash&amp;ref_src=twsrc%5Etfw">#efcore</a><br><br>TechEmpower Fortunes perf!<br><br>⏫ EF Core 6.0 is 33.1% faster than EF Core 5.0<br>⏫ Dapper is also 6.0% faster<br>⏫ EF Core is now at 93.5% of Dapper perf<br><br>Plus: learn to contribute! We live-streamed a PR end-to-end.<a href="https://t.co/8kfj4du1wC">https://t.co/8kfj4du1wC</a> <a href="https://t.co/qGRT2GT2e3">pic.twitter.com/qGRT2GT2e3</a></p>&mdash; Arthur Vickers (@ajcvickers) <a href="https://twitter.com/ajcvickers/status/1385335542334705665?ref_src=twsrc%5Etfw">April 22, 2021</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

***

# 🌎 Last week in the .NET world

## 🔥 The Top 3

- Brian Lagunas [walks through yield return in C#](https://brianlagunas.com/c-yield-return-what-is-it-and-how-does-it-work/).
- Giorgi Dalakishvili [introduces GraphQLinq, a LINQ-to-GraphQL library](https://www.giorgi.dev/dotnet/introducing-graphqlinq-strongly-typed-graphql-queries-with-linq-to-graphql/).
- Microsoft announces [Azure Web PubSub](https://azure.microsoft.com/blog/easily-build-realtime-apps-with-websockets-and-azure-web-pubsub-now-in-preview).

## 📢 Announcements

- Microsoft announces that [.NET Framework 4.5.2, 4.6, 4.6.1 will reach End of Support on April 26, 2022](https://devblogs.microsoft.com/dotnet/net-framework-4-5-2-4-6-4-6-1-will-reach-end-of-support-on-april-26-2022).
- Asia Rudenko [introduces ReSharper 2021.1.2 and Rider 2021.1.2](https://blog.jetbrains.com/dotnet/2021/04/23/resharper_rider_2021_1_2/).
- Uno writes about [Uno Platform 3.7](https://platform.uno/blog/uno-platform-3-7-more-winui-project-reunion-and-linux-support-wasm-performance-boost/).
- Alexandra Kolesova writes how [ReSharper and Rider 2021.2 will require .NET Framework 4.7.2 or newer installed on Windows](https://blog.jetbrains.com/dotnet/2021/04/26/resharper-and-rider-2021-2-will-require-net-framework-4-7-2-on-windows/).

## 📅 Community and events

- The Azure SDK [wants your feedback on the next phase of the Azure SDK](https://devblogs.microsoft.com/azure-sdk/planning-2021).
- Alberto Gimeno [writes how GitHub uses feature flags](https://github.blog/2021-04-27-ship-code-faster-safer-feature-flags/).
- Microsoft Build registration [is now open](https://register.build.microsoft.com/).
- Maarten Balliauw [comments on ReSharper and Visual Studio 2022 64-bit](https://blog.jetbrains.com/dotnet/2021/04/28/resharper-and-visual-studio-2022-64-bit/).
- For community standups: ASP.NET [talks about SPA updates](https://www.youtube.com/watch?v=Nb5lV2i8bCI) and Machine Learning talks about [.NET Notebooks and .NET Interactive](https://www.youtube.com/watch?v=KlZje8GNDEQ).
- The .NET Docs Show [talks to Irina Scurtu about versioning APIs](https://www.youtube.com/watch?v=4fw2c_ukABM).

## 🌎 Web development

- Jeremy Likness [works on multi-tenancy with EF Core in Blazor Server apps](https://blog.jeremylikness.com/blog/multitenancy-with-ef-core-in-blazor-server-apps/).
- Damien Bowden [secures an ASP.NET Core app and Web API using Windows auth](https://damienbod.com/2021/04/26/securing-an-asp-net-core-app-and-web-api-using-windows-authentication/).
- Claudio Bernasconi [works on Blazor form handling and input validation
](https://www.claudiobernasconi.ch/2021/04/29/introduction-to-blazor-form-handling-and-input-validation/).
- Marinko Spasojevic [works on Blazor material form creation with file uploads and dialogs](https://code-maze.com/blazor-material-form-creation-with-file-upload-and-dialogs/).
- Daniel Jiminez Garcia [uses Blazor WebAssembly, SignalR and C# 9 to create Full-stack Real-time Applications](https://www.dotnetcurry.com/aspnet-core/realtime-app-using-blazor-webassembly-signalr-csharp9).
- Cody Merritt Anhorn [works with the IntersectionObserver API in Blazor](https://codyanhorn.tech/blog/blazor/2021/04/24/Blazor-IntersectionObserver-WebApi.html).
- Khalid Abuhakmeh [accesses background services from ASP.NET Core](https://khalidabuhakmeh.com/access-background-services-from-aspnet-core).
- Waqas Anwar [supports multiple versions of ASP.NET Core Web API](https://www.ezzylearning.net/tutorial/how-to-support-multiple-versions-of-asp-net-core-web-api).

## 🥅 The .NET platform

- Nick Randolph [explains why he doesn't want or need WinUI for UWP](https://nicksnettravels.builttoroam.com/we-dont-need-winui-for-uwp/).
- Matthew MacDonald [writes about a lack of Linux support for .NET 6 desktop apps](https://medium.com/young-coder/net-6-has-a-linux-shaped-hole-23010395d11e).
- Sam Walpole [warns against deferred execution in LINQ](https://dev.to/dr_sam_walpole/linq-beware-of-deferred-execution-59db).
- Maarten Balliauw [investigates a crashing devenv.exe](https://blog.jetbrains.com/dotnet/2021/04/27/sherlock-holmes-and-the-case-of-a-crashing-devenv-exe/).

## ⛅ The cloud

- Matias Quaranta [writes about improving your Azure Cosmos DB .NET SDK initialization](https://devblogs.microsoft.com/cosmosdb/improve-net-sdk-initialization).
- Carlos Souza [compares deploying containers in Azure and AWS](https://www.pluralsight.com/blog/it-ops/aws-vs-azure-containers).
- Jessica Deen [refactors for microservices using Linux and Windows containers](https://devblogs.microsoft.com/devops/rearchitecting-for-microservices-featuring-windows-linux-containers).
- Jamie Maguire [works on aspect-based sentiment analysis with Azure Cognitive Services Text Analytics](https://jamiemaguire.net/index.php/2021/04/24/aspect-based-sentiment-analysis-with-azure-cognitive-services-text-analytics-preview/).

## 📔 Languages

- Damir Arh [discusses nullable reference type best practices](https://www.dotnetcurry.com/csharp/nullable-reference-types-csharp).
- Jonathan Allen [walks through .NET 6 LINQ improvements](https://www.infoq.com/news/2021/04/Net6-Linq/).
- Khalid Abuhakmeh [reads and writes Excel spreadsheets in C#](https://khalidabuhakmeh.com/read-write-excel-spreadsheets-with-csharp).
- Tom Deseyn [completes his series on C# 9 features](https://developers.redhat.com/blog/2021/04/27/some-more-c-9/).

## 🔧 Tools

- Billy Griffin [writes about GitHub Desktop updates](https://github.blog/2021-04-28-github-desktop-hiding-whitespace-expanding-diffs-repo-aliases/).
- Davide Bellone [works with FluentValidation](https://www.code4it.dev/blog/fluentvalidation).
- Dave Brock [makes microservices fun again with Dapr](https://daveabrock.com/2021/04/29/meet-dapr).
- Andrew Lock [tries out the open-source eCommerce platform nopCommerce using Docker](https://andrewlock.net/trying-out-the-open-source-ecommerce-platform-nopcommerce-using-docker/).
- Rory Primrose [revisits NSubstitute and FluentAssertions](https://www.neovolve.com/2021/04/25/nsubstitute-and-fluentassertions-redux/).
- Peter Vogel [writes about strategy and tools for APIs](https://www.telerik.com/blogs/api-testing).

## 📱 Xamarin

- Leomaris Reyes [replicates a Book Worm UI in Xamarin Forms](https://askxammy.com/replicating-book-worm-ui-in-xamarin-forms/).
- Sam Basu [provides his weekly MAUI update](https://www.telerik.com/blogs/sands-of-maui-issue-6).
- Matt Lacey [writes about Uno Platform and Xamarin.Forms](https://www.infoq.com/articles/uno-xamarin/).

## 🏗 Design, testing, and best practices

- Derek Comartin [talks about aggregate design](https://codeopinion.com/aggregate-design-using-invariants-as-a-guide/).
- The NDepend blog [writes about a Clean Architecture refactoring case study](https://blog.ndepend.com/clean-architecture-refactoring-a-case-study/).

## 🎤 Podcasts

- The .NET Core Podcast [talks about dependency injection with Steve Collins](https://dotnetcore.show/episode-75-dependency-injection-with-steve-collins/).
- The Productive C# Podcast [discusses the new DateOnly and TimeOnly structs in C#](https://anchor.fm/productivecsharp/episodes/18--DateOnly-and-TimeOnly-structs-in--NET-6-evl2dp).
- The Azure DevOps Podcast [discusses Blazor architecture](http://azuredevopspodcast.clear-measure.com/a-special-group-presentation-on-blazor-architecture-episode-138).
- The Azure Podcast [talks about keeping up with Azure](http://azpodcast.azurewebsites.net/post/Episode-374-Keeping-up-with-Azure).
- The Adventures in .NET podcast [talks about interactive C# with VS Code Notebooks](https://devchat.tv/adventures-in-dotnet/net-066-interactive-c-with-vs-code-notebooks-with-eric-potter/).
- The .NET Rocks show [talks with Simon Cropp about Verify](https://www.dotnetrocks.com/default.aspx?ShowNum=1737).
- The Coding Blocks podcast [talks about writing great APIs](https://www.codingblocks.net/podcast/write-great-apis/).

## 🎥 Videos

- The On .NET Show [talks about authentication for serverless apps](https://channel9.msdn.com/Shows/On-NET/Authentication-for-Serverless-apps-with-Easy-Auth), [init-only setters](https://channel9.msdn.com/Shows/On-NET/C-Language-Highlights-Init-only-setters), [default interface methods in C#](https://channel9.msdn.com/Shows/On-NET/C-Language-Highlights-Default-Interface-Methods), and [GitHub Codespaces](https://www.youtube.com/watch?v=eIlbBAQ6fUk).
- Azure Enablement [talks about cloud financial considerations](https://channel9.msdn.com/Shows/Azure-Enablement/Evaluating-financial-considerations-during-your-cloud-adoptionjourney).
