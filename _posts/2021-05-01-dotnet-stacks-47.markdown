---
date: "2021-05-01"
title: "The .NET Stacks #47: 🧨 Now with 32 more bits"
tags: [dotnet-stacks]
image: /assets/img/stacks-47-card.png 
description: This week, we talk about Visual Studio 2022, API updates, and more.
---

![Newsletter image](https://daveabrock.com/THE .NET STACKS.png)

*NOTE: This is the web version of my weekly newsletter, which was released on April 26, 2021. To get the issues right away, subscribe at [dotnetstacks.com](https://dotnetstacks.com) or at the bottom of this post.*

Happy Monday! Here's what we're talking about this week:

- **One big thing**: Visual Studio 2022 to be 64-bit
- **The little thing**: API updates coming to Preview 4, memory dump tooling, dotnet monitor updates
- Last week in the .NET world

***

# One big thing: Visual Studio 2022 to be 64-bit

Last week, I [provided a tooling update](https://daveabrock.com/2021/04/24/dotnet-stacks-46). Figuring the next major updates would occur around Build, it was a good time to get up to speed. I pushed the newsletter, started my workday, then an hour later, Amanda Silver wrote a post about [Visual Studio 2022](https://devblogs.microsoft.com/visualstudio/visual-studio-2022/). Oops.

The biggest news: Visual Studio 2022 will be 64-bit and is no longer chained to limited memory (4 gigs or so) in the main *devenv.exe* process. If you look at [Silver's post](https://devblogs.microsoft.com/visualstudio/visual-studio-2022/#visual-studio-2022-is-64-bit), she has a video of a 64-bit Visual Studio app working easily with 1600 projects and 300k files. Finally.

With [interest dating back in 2011](https://web.archive.org/web/20170922120502/https://visualstudio.uservoice.com/forums/121579-visual-studio-2015/suggestions/2255687-make-vs-scalable-by-switching-to-64-bit?page=1&per_page=20), the Visual Studio team responded in 2016, saying that "at this time we don’t believe the returns merit the investment and resultant complexity." Times have changed—hardware capabilities have improved, and VS is no longer the only .NET IDE option—and it's now worth it.

For example, if you look at the issue [in the Developer Community](https://developercommunity.visualstudio.com/t/Visual-Studio-x64-implementation/1372671?space=8&ftype=idea&sort=votes&stateGroup=active), a few comments echo what Microsoft has heard for a few years now:

>This year my company(size 15k+) started to provide RAIDER to employees and plans to cancel our Enterprise VS subscriptions as VS is highly unproductive because of memory limitations of 32 bit application.

>Lately I’m mainly using Rider. It does about 80% of what VS does, but it does that 80% without worrying about running out of ram.

To be clear, as [David Ramel notes](https://visualstudiomagazine.com/articles/2021/04/19/vs-2022.aspx), Visual Studio has been available in a 64-bit edition to create 64-bit apps on 64-bit machines, but the application itself has previously been a 32-bit app. JetBrains [cheekily welcomed Visual Studio to the 64-bit club](https://twitter.com/JetBrainsRider/status/1384164326328606728), but this also means ReSharper will have more room to breathe. When the time comes, you'll need to grab 64-bit versions of your favorite extensions as well.

Of course, 64-bit support isn't the only thing coming to Visual Studio 2022. VS 2022 promises a [better UI interface](https://devblogs.microsoft.com/visualstudio/visual-studio-2022/#designing-for-everyone), [enhanced diagnostics and debugging](https://devblogs.microsoft.com/visualstudio/visual-studio-2022/#diagnostics-and-debugging), [productivity updates](https://devblogs.microsoft.com/visualstudio/visual-studio-2022/#insights-and-productivity), [improved code search](https://devblogs.microsoft.com/visualstudio/visual-studio-2022/#improved-code-search), and more. The first preview will ship this summer, with UI refinements and accessibility improvements. In the meantime, you can [check out the most requested features from the community](https://developercommunity.visualstudio.com/search?space=8&ftype=idea&sort=votes&stateGroup=active).

***

# The little things: API updates coming to Preview 4, async analyzers, dotnet monitor updates

A couple weeks ago, we talked [about the FeatherHttp project](https://daveabrock.com/2021/04/10/dotnet-stacks-44), a lightweight, scalable way of writing APIs without the typical overhead that comes with .NET APIs today.

Here's what I said about it:

>Fowler says the repo has three key goals: to be built on the same primitives on .NET Core, to be optimized to build HTTP APIs quickly, and having the ability to take advantage of existing .NET Core middleware and frameworks. According to a GitHub comment, the solution uses 90% of ASP.NET Core and changes the Startup pattern to be more lightweight.

That functionality will start rolling out in .NET 6 Preview 4 in a few weeks. (Along with AOT support, Preview 4 promises to be a satisfying update.)

David Fowler tweeted about it and the discussion was spicy.

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">Want to throw together a quick web application in C#? This is what that will look like in the future. <a href="https://twitter.com/hashtag/dotnet?src=hash&amp;ref_src=twsrc%5Etfw">#dotnet</a> <a href="https://twitter.com/hashtag/aspnetcore?src=hash&amp;ref_src=twsrc%5Etfw">#aspnetcore</a> <a href="https://t.co/3RR8uYdS1o">pic.twitter.com/3RR8uYdS1o</a></p>&mdash; David Fowler 🇧🇧 (@davidfowl) <a href="https://twitter.com/davidfowl/status/1385290460613144577?ref_src=twsrc%5Etfw">April 22, 2021</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

I'm personally excited to give this a spin. If it isn't your jam, it doesn't have to be—it isn't a breaking change, and you can always use your current solution. But as APIs evolve with microservices, much of the core logic is done outside of the endpoints anyway, so you may find you don't need the complexity and overhead with writing APIs in MVC.

***

Mark Downie and his team have been doing some great work with managed memory dump analyzers. If you think that's too low-level, reconsider—when logging and local debugging fail you (or issues aren't reproducible easily), memory dump analysis can be your friend. It isn't always easy to work with memory dumps, though. To help folks new to dump debugging, Microsoft has developed a new [.NET Diagnostics Analyzer tool](https://docs.microsoft.com/visualstudio/debugger/how-to-debug-managed-memory-dump?view=vs-2019) to quickly identify or rule out issues.

A great use case is [finding async anti-patterns](https://devblogs.microsoft.com/visualstudio/managed-memory-dump-analyzers/), a common cause of thread starvation, which only may become prevelant when you hit high production workloads. Check out the blog post to see how it can detect some "sync-over-async" issues.

***

In passing, we've talked about the dotnet monitor tool. It allows you to access diagnostics information with a dotnet process. It's no longer experimental: it's [now a supported tool in the .NET ecosystem](https://devblogs.microsoft.com/dotnet/whats-new-in-dotnet-monitor/). Sourabh Shirhatti wrote about [that and recent updates](https://devblogs.microsoft.com/dotnet/whats-new-in-dotnet-monitor/).

***

Lastly, a nice tip from Khalid Abuhakmeh:

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">In <a href="https://twitter.com/dotnet?ref_src=twsrc%5Etfw">@dotnet</a> 6 preview 3+ you can validate IOptions on application startup. No misconfiguration issues in production. <a href="https://twitter.com/hashtag/dotnet?src=hash&amp;ref_src=twsrc%5Etfw">#dotnet</a> <a href="https://twitter.com/hashtag/aspnet?src=hash&amp;ref_src=twsrc%5Etfw">#aspnet</a> <a href="https://twitter.com/hashtag/aspnetcore?src=hash&amp;ref_src=twsrc%5Etfw">#aspnetcore</a> <a href="https://t.co/Rg7w4OhVWL">pic.twitter.com/Rg7w4OhVWL</a></p>&mdash; Khalid ✨ (@buhakmeh) <a href="https://twitter.com/buhakmeh/status/1385329674276966407?ref_src=twsrc%5Etfw">April 22, 2021</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

***

# 🌎 Last week in the .NET world

## 🔥 The Top 3

- Kristoffer Strube [works with DynamicComponent in Blazor](https://blog.elmah.io/rendering-dynamic-content-in-blazor-wasm-using-dynamiccomponent/).
- Leomaris Reyes [writes about getting started with Xamarin Forms](https://www.telerik.com/blogs/are-you-starting-xamarin-forms-lets-create-simple-ui).
- Konrad Kokosa [has a quiz to check your level of knowledge about .NET memory management](https://tooslowexception.com/net-quiz-check-your-level-of-knowledge-about-net-memory-management/).

## 📢 Announcements

- Amanda Silver [rolls out Visual Studio 2022](https://devblogs.microsoft.com/visualstudio/visual-studio-2022).
- Sourabh Shirhatti [writes about what's new in dotnet monitor](https://devblogs.microsoft.com/dotnet/whats-new-in-dotnet-monitor).
- Mark Downie [announces memory dump analyzers that detect async antipatterns](https://devblogs.microsoft.com/visualstudio/managed-memory-dump-analyzers).

## 📅 Community and events

- GitHub rolls out [a new Mars 2020 Helicopter Mission badge](https://github.blog/2021-04-19-open-source-goes-to-mars/).
- Maarten Balliauw [announces new .NET tutorials from JetBrains](https://blog.jetbrains.com/dotnet/2021/04/19/new-net-guide-tutorials-from-visual-studio-to-rider-resharper-essentials-docker-and-more/).
- Microsoft is putting on [a free live stream event on Friday called "Let's Learn .NET"](https://dev.to/dotnet/let-s-learn-net-c-free-live-stream-event-1ak0).
- Shahed Chowdhuri [is launching a new "Azure, Simplified" video series](https://wakeupandcode.com/azure-simplified-new-video-series/).
- For weekly standups: ASP.NET [talks about .NET 6 ASP.NET Core updates](https://www.youtube.com/watch?v=Mot8qAWEnj8) and Entity Framework [shows off how to contribute](https://www.youtube.com/watch?v=9OMxy1wal1s).
- The .NET Docs Show [optimizes Linux containers with Robert Rozas](https://www.youtube.com/watch?v=9OMxy1wal1s).

## 🌎 Web development

- Matthew Jones [continues building a Tetris app in Blazor](https://exceptionnotfound.net/tetris-in-blazor-part-5-controls-upcoming-tetrominos-and-clearing-rows/).
- Jeff Fritz [introduces his KlipTok project](https://jeffreyfritz.com/2021/04/introducing-kliptok/).
- Jon Hilton asks: [will .NET 6 fix Blazor Prerendering](https://jonhilton.net/blazor-prerendering-net6/)?
- Claudio Bernasconi [builds a dashboard in Blazor](https://www.claudiobernasconi.ch/2021/04/23/building-a-dashboard-blazor/).
- David Grace [explains why the ASP.NET Core FromBody can return null](https://www.roundthecode.com/dotnet/asp-net-core-web-api/why-asp-net-core-frombody-not-working-returning-null).
- Sardar Mudassar Ali [tests an ASP.NET Core 5 Web API service with Swagger](https://www.c-sharpcorner.com/article/how-to-test-asp-net-core-web-api-service-with-swagger/).
- Thomas Claudius Huber [works with local storage and session storage in Blazor](https://www.thomasclaudiushuber.com/2021/04/19/store-data-of-your-blazor-app-in-the-local-storage-and-in-the-session-storage/).
- Muhammad Rehan Saeed [writes about CSS best practices](https://rehansaeed.com/css-general-rules-of-thumb/).
- Damien Bowden [secures multiple Auth0 APIs in ASP.NET Core using OAuth bearer tokens](https://damienbod.com/2021/04/19/securing-multiple-auth0-apis-in-asp-net-core-using-oauth-bearer-tokens/).
- Marinko Spasojevic [uses MudBlazor to work with a product detail page](https://code-maze.com/using-mudblazor-to-create-product-details-page/).
- Khalid Abuhakmeh [adds Svelte to ASP.NET Core projects](https://khalidabuhakmeh.com/add-svelte-to-aspnet-core-projects).

## 🥅 The .NET platform

- Andrew Lock [fixes a build warning when building projects with end-of-life .NET frameworks](https://andrewlock.net/fixing-build-warning-netsdk1138-when-building-end-of-life-projects-with-dotnet-5/).
- David Ramel [writes about the top feature requests for Visual Studio 2022](https://visualstudiomagazine.com/articles/2021/04/21/top-vs-requests.aspx).
- Scott Hanselman [converts his 13-year-old .NET BabySmash app to a self-contained .NET 5 app with the .NET Upgrade Assistant](https://www.hanselman.com/blog/converting-a-13-year-old-net-wpf-app-called-babysmash-to-a-selfcontained-net-5-app-with-the-net-upgrade-assistant).
- Richard Lander [talks about the crossgen2 tool](https://devblogs.microsoft.com/dotnet/conversation-about-crossgen2).

## ⛅ The cloud

- Daniel Krzyczkowski [starts a series on Azure Identity](https://daniel-krzyczkowski.github.io/Lost-In-Azure-Cloud-Identity-Series-Introduction/).
- Imar Spaanjaars [deploys to the Azure App Service](https://imar.spaanjaars.com/623/building-and-auto-deploying-an-aspnet-core-application-part-6-setting-up-a-cd-pipeline-deploying-to-an-azure-app-service).
- The AWS Blog [writes about the AWS SDK for .NET’s runtime pipeline and client configuration](https://aws.amazon.com/blogs/developer/dive-into-the-aws-sdk-for-dotnet-runtime-pipeline/), and also [introduces the AWS Toolkit for Visual Studio support for AWS SSO and Assume Role with MFA](https://aws.amazon.com/blogs/developer/introducing-aws-toolkit-for-visual-studio-support-for-aws-sso-and-assume-role-with-mfa/).
- GitHub Actions [rolls out protections to fight cryptomining](https://github.blog/2021-04-22-github-actions-update-helping-maintainers-combat-bad-actors/).

## 📔 Languages

- Brian Lagunas [compares Task and ValueTask](https://brianlagunas.com/task-vs-valuetask-when-should-i-use-valuetask/).
- Tom Deseyn [works with init accessors and records in C# 9](https://developers.redhat.com/blog/2021/04/20/c-9-init-accessors-and-records/).

## 🔧 Tools

- Steve Smith [adds ImgBot to his GitHub repository](https://ardalis.com/add-imgbot-to-github-repository/).
- Paul DeVito [works on .NET Core integration tests using a SQL Server database in Docker](https://wrapt.dev/blog/integration-tests-using-sql-server-db-in-docker).
- Jay Krishna Reddy [uses CQRS with MediatR in .NET 5](https://www.c-sharpcorner.com/article/cqrs-mediatr-in-net-5/).
- ErikEJ [uses Entity Framework Core with Azure Managed Identity, App Service/Functions, and Azure SQL DB](https://erikej.github.io/efcore/azure/2021/04/20/efcore-managed-identity.html).
- David Hayden [works with Orchard Core, .NET 5, and C# 9](https://www.davidhayden.me/blog/orchard-core-net5-and-csharp9).
- Auth0 [introduces a new CLI](https://auth0.com/blog/introducing-auth0-cli/).
- Khalid Abuhakmeh [fixes .NET ICU build issues in GitHub Actions](https://khalidabuhakmeh.com/fix-dotnet-icu-build-issues-in-github-actions).

## 📱 Xamarin

- Rakeshkumar Desai [gets started with .NET MAUI](https://www.c-sharpcorner.com/article/getting-started-with-net-maui-first-impressions/).
- Almir Vuk [talks to David Ortinau about .NET MAUI](https://www.infoq.com/articles/net-maui/).

## 🏗 Design, testing, and best practices

- Derek Comartin asks: [do microservices require containers, Docker, or Kubernetes](https://codeopinion.com/do-microservices-require-containers-docker-kubernetes/)?
- Scott Hannen [writes about the Partial/Optional Object Population (POOP) anti-pattern](https://scotthannen.org/blog/2021/04/19/partial-optional-object-population.html). (Finally, a blog post suitable for both me and my 3-year-old.)
- Adam Storr [creates and manipulates mock anonymous data for unit tests](https://adamstorr.azurewebsites.net/blog/easily-create-and-manipulate-mock-anonymous-data-for-unit-tests).

## 🎤 Podcasts

- The .NET Core Podcast [talks about libvlcsharp and .NET with Martin Finkel](https://dotnetcore.show/episode-74-libvlcsharp-and-net-with-martin-finkel/).
- The Complete Developer podcast [talks about quickly learning new technologies](https://completedeveloperpodcast.com/quickly-learning-new-technology/).
- The Productive C# Podcast [discusses how to become a fluent C# developer with Codingame](https://anchor.fm/productivecsharp/episodes/17--How-to-become-a-fluent-C-developer-with-Codingame-ev5pf6).
- The 6-Figure Developer podcast [talks about Blazor with Carl Franklin](https://6figuredev.com/podcast/episode-192-blazor-with-carl-franklin/).
- The .NET Rocks podcast [talks to Gerald Versluis about going from Xamarin to MAUI](https://www.dotnetrocks.com/default.aspx?ShowNum=1736).
- The Merge Conflict podcast [answers your questions](https://www.mergeconflict.fm/250).
- Serverless Chats [discusses how serverless fits in to the cyclical nature of the industry](https://www.serverlesschats.com/97/).

## 🎥 Videos

- Visual Studio Toolbox [gets started with Jupyter notebooks in VS Code](https://channel9.msdn.com/Shows/Visual-Studio-Toolbox/Getting-Started-with-Jupyter-Notebooks-in-VS-Code).
- The On .NET Show [talks about build automation with NUKE](https://channel9.msdn.com/Shows/On-NET/Build-Automation-with-NUKE), [works through top-level statements in C# 9](https://channel9.msdn.com/Shows/On-NET/C-Language-Highlights-Top-level-statements), and [uses the static directive in C#](https://channel9.msdn.com/Shows/On-NET/C-Language-Highlights-Using-Static-Directive).
