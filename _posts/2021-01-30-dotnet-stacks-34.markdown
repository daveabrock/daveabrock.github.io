---
date: "2021-01-30"
title: "The .NET Stacks #34: 🎙 Visual Studio gets an update, and you get a rant"
tags: [dotnet-stacks]
comments: false
image: /assets/img/rant-card.png 
description: This week, we talk about Visual Studio, EF Core 6, and GitHub Pages.
---

![Newsletter image](https://daveabrock.com/bernie-stacks.png)

Happy Monday to you all. I hope you had a good week. Put on your comfy mittens and let's get started.

- Visual Studio gets an update, and you get a rant
- EF Core 6 designs take shape
- GitHub Pages gets more enterprise-y
- A quick correction

***

# Visual Studio gets an update, and you get a rant

The Visual Studio team [has released v16.9 Preview 3](https://devblogs.microsoft.com/visualstudio/visual-studio-2019-v16-9-preview-3/). This one's a hodgepodge, but a big call out is the ability to [view source generators from the Solution Explorer](https://devblogs.microsoft.com/visualstudio/visual-studio-2019-v16-9-preview-3/). This allows you to inspect the generated code easily. People are [already getting excited](https://twitter.com/konradkokosa/status/1352313233705676800).

Since we're on the subject, Jerry Nixon polled some folks this week about [their preferred IDE](https://twitter.com/jerrynixon/status/1351602460121567239). (I say VS Code is only an IDE when you install extensions, but I digress.) As of Sunday night, JetBrains Rider comes in at around 26%. It might seem low to you, but not to me. After years of Windows-only .NET making Visual Studio the only realistic IDE, it's amazing that a third-party tool is gaining so much traction. This recognition is well-deserved, by the way—it's fast, feature-rich, and does so much out-of-the-box that Visual Studio doesn't. I installed it this week after thinking about it for years, and am impressed so far.

Of course, your mileage may vary and the "new and different" is always alluring at first. But, to me, what's more troubling is my *reason* for giving it a try. I wouldn't be offended if you called me a Visual Studio fanboy. I've used it for over a decade and a half. Even so: the Razor editing experience is a pain (with the admission they are rolling out a new editor), and component discovery is a hassle when working with Blazor, and closing and restarting Visual Studio is still a thing, in 2021. This is part of a general theme ([and I'm not the only one](https://twitter.com/ChaseAucoin/status/1352668578684796930)): why is Blazor tooling in Visual Studio still so subpar? I've found Rider to provide a better Blazor development experience. Is it antecdotal? Yes. Do I think it's just me? No.

I know it's being worked on, I get it, but: when you deliver a wonderful, modern library with hype that hasn't been seen in years ... how is your flagship IDE not providing a development experience to match? Whether it's internal priorities or a million other reasons that impact a large corporation, the expectation that Visual Studio users will have to wait and deal with it is suitable when customers don't have a choice. They do now, and a non-Microsoft tool is consistently beating them on the Blazor development experience. I'm disappointed.

***

# EF Core 6 designs take shape

This week, Jeremy Likness [shared](https://devblogs.microsoft.com/dotnet/the-plan-for-entity-framework-core-6-0/) the high-level, subject-to-change [plan for EF Core 6](https://docs.microsoft.com/ef/core/what-is-new/ef-core-6.0/plan)—to be released with .NET 6 in November. It'll target .NET 6, of course, won't run on .NET Framework and likely will not support any flavor of .NET Standard.

Some big features make the list, like SQL Server temporal tables (allowing you to create them from migrations and the ability to access historical data), JSON columns, compiled models, and a plan to match Dapper performance. The latter definitely caught my eye. As David Ramel [writes this week](https://visualstudiomagazine.com/articles/2021/01/19/ef-core-plans.aspx), he quotes Jeremy as saying: "This is a significant challenge which will likely not be fully achieved. Nevertheless, we will get as close as we can."

In other EF news, the team released a [new doc on EF Core change tracking](https://docs.microsoft.com/ef/core/change-tracking/).

***

# GitHub Pages gets more enterprise-y

As Microsoft is trying to make sense of competing workloads with GitHub and consolidate products–we've talked a few times about Azure DevOps and GitHub Actions—look for things in GitHub to get a little more enterprise-*y*.

This week, GitHub released [something called](https://github.blog/changelog/2021-01-21-access-control-for-github-pages/) "Access Control for GitHub Pages", rolled out to the GitHub Enterprise Cloud. This gives you the option to limit access of GitHub Pages to users with repository access. This is ideal for documentation and knowledge bases, or any other static site needs.

***

# A quick correction

There is no magic "undo" command with newsletters, sadly. I'd like to make a correction from last week. 

I hope you enjoyed last week's [interview with Steve Sanderson, the creator of Blazor](https://daveabrock.com/2021/01/17/dev-discussions-steve-sanderson). I made a mistake in the introduction. Here's what I originally wrote:

>It seems like forever ago when, [at NDC Oslo in 2017](https://youtu.be/MiLAE6HMr10?t=1612), Steve Sanderson talked about a fun project he was working on, called .NET Anywhere. In the demo, he was able to load and run C# code—*ConsoleApp1.dll*, specifically—in the browser, using Web Assembly. C# in the browser! In the talk, he called it "*an experiment, something for you to be amused by*."

 I made the implication that Steve created the Dot Net Anywhere (DNA) runtime. With apologies to Chris Bacon, not true!

Here's what I *should* have said:

>It seems like forever ago when, [at NDC Oslo in 2017](https://youtu.be/MiLAE6HMr10?t=1612), Steve Sanderson showed off a new web UI framework with the caveat: "_an experiment, something for you to be amused by_." By extending [Dot Net Anywhere](https://github.com/chrisdunelm/DotNetAnywhere) (DNA), Chris Bacon's portable .NET runtime, on WebAssembly, he was able to load and run C# in the browser. In the browser!

Thanks to Steve for [the clarification](https://twitter.com/stevensanderson/status/1351078228971159553).

***

# 🌎 Last week in the .NET world

## 🔥 The Top 4

- Andrew Lock [enables prerendering for Blazor WebAssembly apps](https://andrewlock.net/enabling-prerendering-for-blazor-webassembly-apps/).
- Daniel Krzyczkowski [integrates an Azure Storage Account with his API project](https://daniel-krzyczkowski.github.io/Cars-Island-ASP-NET-Core-API-Integration-With-Azure-Storage-Accont/).
- Iris Classon [deals with Azure credentials issues](https://www.irisclasson.com/2021/01/20/get-azstorageaccount-your-azure-credentials-have-not-been-set-up-or-have-expired/).
- Neils Swimberghe [uses YARP to host a client and API server on a single origin to avoid CORS](https://swimburger.net/blog/dotnet/use-yarp-to-host-client-and-api-server-on-a-single-origin).

## 📢 Announcements

- The Azure SDK team [has shipped their January release](https://devblogs.microsoft.com/azure-sdk/january-2021-release).
- Jacqueline Widdis [announces Visual Studio 2019 v16.9 Preview 3](https://devblogs.microsoft.com/visualstudio/visual-studio-2019-v16-9-preview-3).
- Jeremy Likness [writes about the plan for EF Core 6](https://devblogs.microsoft.com/dotnet/the-plan-for-entity-framework-core-6-0).

## 📅 Community and events

- In .NET community standups: Machine Learning [talks about FSharp.Stats](https://www.youtube.com/watch?v=i3xjS5Kj9rU), and ASP.NET [continues talking architecture with David Fowler](https://www.youtube.com/watch?v=x_AXKLfG8o0).
- The .NET Docs Show [talks to Jason Bock about source generators](https://www.youtube.com/watch?v=mIo_HTz2viI).
- Neil MacMullen [introduces Textrude for generating code from data](https://neil-macmullen.medium.com/introducing-textrude-using-scriban-to-generate-code-from-data-dad38b280076).
- Sébastien Ros is at it again, releasing a [fast parser combinator library called Parlot](https://twitter.com/sebastienros/status/1351326122915467264).

- Paul Krill [writes about a new nanoFramework project](https://www.infoworld.com/article/3604055/net-nanoframework-taps-c-for-embedded-systems.html).
- There's a .NET Conf on February 25, [focused on Windows development](https://focus.dotnetconf.net/).
- Konrad Kokosa [is making a mini-series on .NET GC internals](https://tooslowexception.com/net-gc-internals-mini-series/).
- Gregor Suttie and Richard Hooper [are starting an AKS Zero to Hero series](https://gregorsuttie.com/2021/01/20/aks-zero-to-hero-series-for-everyone/).
- David Ramel [writes how Radzen has open-sourced a ton of their Blazor components](https://visualstudiomagazine.com/articles/2021/01/21/radzen-open-source.aspx).

## 🌎 Web development

- Imar Spaanjaars [builds an auto-deploys an ASP.NET Core app](https://imar.spaanjaars.com/617/building-and-auto-deploying-an-aspnet-core-application-part-1-introduction).
- Muhammed Saleem [deploys Blazor WebAssembly to Azure Static Web Apps](https://code-maze.com/deploying-blazor-webassembly-into-azure-static-web-apps/).
- Silambarasan Ilango [authenticates a Blazor WASM app with Azure AD](https://www.syncfusion.com/blogs/post/how-to-authenticate-a-blazor-webassembly-hosted-app-with-azure-active-directory.aspx).
- Damien Bowden [uses ASP.NET Core Controllers and Razor Pages from a separate shared project or assembly](https://damienbod.com/2021/01/18/using-asp-net-core-controllers-and-razor-pages-from-a-separate-shared-project-or-assembly/).
- David Grace [explores how Blazor performs against other frameworks](https://www.telerik.com/blogs/how-blazor-performs-against-other-frameworks).
- Shawn Wildermuth [forces ASP.NET WebForms Designer files to regenerate](http://wildermuth.com/2021/01/17/Forcing-ASP-NET-WebForms-Designer-Files-to-Regenerate).
- Kristoffer Strube [fixes Blazor WASM base path problems](https://blog.elmah.io/how-to-fix-blazor-wasm-base-path-problems/).
- Over at Code Maze, [they write about downloading files with ASP.NET Core Web API and Angular](https://code-maze.com/download-files-dot-net-core-angular/).
- Joseph Guadagno [uses Yarn with ASP.NET Core projects](https://www.josephguadagno.net/2021/01/18/using-yarn-with-asp-net-core-projects).

## ⛅ The cloud

- Bryan Soltis [connects an Azure Logic Web App to a local Web API](https://soltisweb.com/blog/detail/2020-05-20-connectinganazurelogicapptoalocalwebapi).
- Jonathan George [triggers an Azure Synapse pipeline run from C#](https://endjin.com/blog/2021/01/how-to-trigger-an-azure-synapse-pipeline-run-from-csharp.html).
- Mark Heath [automates Azure access restrictions with the Azure CLI](https://markheath.net/post/azure-cli-access-restrictions).
- Justin Yoo [uses Azure Functions to work with CloudEvents for Azure EventGrid](https://techcommunity.microsoft.com/t5/apps-on-azure/cloudevents-for-azure-eventgrid-via-azure-functions).
- Bryan Soltis [imports an OpenAPI API into Azure API Management](https://soltisweb.com/blog/detail/2020-08-19-importingopenapiapiintoazureapim).
- Johan Danforth [sets up CI/CD to push an ASP.NET MVC app to Azure Web App](https://weblogs.asp.net/jdanforth/how-to-setup-continuous-integration-and-deploy-pipeline-for-asp-net-mvc-framework-application-to-azure-web-app).
- Brendan Burns [chimes in on a concern about AKS utilization](https://twitter.com/dustinmoris/status/1351842103475765248).

## 📔 Languages

- Khalid Abuhakmeh [flattens strings with RegEx.Replace](https://khalidabuhakmeh.com/flatten-strings-with-regex-replace).
- Dave Brock [uses configuration with C# 9 top-level programs](https://daveabrock.com/2021/01/19/config-top-level-programs).
- Mark Downie [writes about viewing the origin of a repeating call stack](https://www.poppastring.com/blog/view-the-origin-of-a-repeating-call-stack).

## 🔧 Tools

- Michał Białecki [performs a bulk copy with EF Core 5](https://www.michalbialecki.com/2021/01/21/bulk-copy-with-entity-framework-core-5/).
- Aaron Powell [extends the GitHub CLI](https://www.aaron-powell.com/posts/2021-01-22-extending-the-github-cli/).
- David Ramel [writes about what developers want with EF 6](https://visualstudiomagazine.com/articles/2021/01/19/ef-core-plans.aspx), and also [writes about AWS open sourcing the .NET porting assistant GUI](https://visualstudiomagazine.com/articles/2021/01/19/net-porting-tool.aspx).
- Jon P. Smith [writes about new features for unit testing EF Core 5](https://www.thereformedprogrammer.net/new-features-for-unit-testing-your-entity-framework-core-5-code/).
- Rick Strahl [takes the new Chromium WebView2 control for a spin in .NET](https://weblog.west-wind.com/posts/2021/Jan/14/Taking-the-new-Chromium-WebView2-Control-for-a-Spin-in-NET-Part-1).
- Julie Lerman [keeps us updated on what she's working on](http://thedatafarm.com/data-access/entity-framework-core-5-resources/).
- Patrick Smacchia [writes about Visual Studio IntelliCode](https://blog.ndepend.com/visual-studio-intellicode-ai-assisted-coding/).
- Miguel Bernard [figures out stages in YAML pipelines](https://blog.miguelbernard.com/figuring-out-stages-in-yaml-pipelines).

## 📱 Xamarin

- Rendy Del Rosario [uses dynamic data in Xamarin Forms](https://www.xamboy.com/2021/01/20/using-dynamic-data-in-xamarin-forms-part-1/).
- CodeChem [sets up a Xamarin app with SQLite local storage](https://hackernoon.com/how-to-set-up-a-xamarin-forms-app-with-sqlite-local-storage-dw3z31q0?source=rss).

## 🏗 Design, testing, and best practices

- Jeremy Miller [re-evaluates test-driven development](https://jeremydmiller.com/2021/01/21/re-evaluating-the-double-ds-of-software-development-test-driven-development/).
- Stephen Cleary [continues his series on asynchronous messaging](https://blog.stephencleary.com/2021/01/asynchronous-messaging-3-backend-processor.html).
- Steve Smith [writes about embracing API endpoints over controllers](https://ardalis.com/mvc-controllers-are-dinosaurs-embrace-api-endpoints/).

## 🎤 Podcasts

- The Azure Podcast [talks to the Project Natick team](http://azpodcast.azurewebsites.net/post/Episode-361-Project-Natick).
- The .NET Rocks podcast [talks to Tom Kerkhove about containers on Azure](https://www.dotnetrocks.com/default.aspx?ShowNum=1723).
- The Coding After Work podcast [talks with Jérôme Laban about Uno and AOT](http://codingafterwork.com/2021/01/15/episode-54-uno-platform-why-aot-is-awesome-and-toast-with-jerome-laban/).
- The Coding Blocks podcast asks: [who owns open-source software](https://www.codingblocks.net/podcast/who-owns-open-source-software/)?
- The Adventures in .NET podcast [talks to Jon Skeet](https://devchat.tv/adventures-in-dotnet/net-052-abusing-c-calendars-epochs-and-the-net-functions-framework-with-jon-skeet/).
- The 6-Figure Developer podcast [talks about Uno with Jérôme Laban](https://6figuredev.com/podcast/episode-179-uno-platform-with-jerome-laban/).

## 🎥 Videos

- James Montemagno [works with MVVM Helpers](https://www.youtube.com/watch?v=y8ZqEOLDeo8).
- Data Exposed [talks about Azure SQL connectivity performance tips and tricks](https://channel9.msdn.com/Shows/Data-Exposed/Azure-SQL-Connectivity-Performance-Tips--Tricks).
- At Technology and Friends, [David Giard talks to Kevin Griffin about SignalR](http://davidgiard.com/2021/01/18/KevinGriffinOnSignalRRealWorldProjects.aspx).
- The ASP.NET Monsters [talk to Adam Dymitruk about event modeling](https://www.youtube.com/watch?v=NGgHJ8HJ1-s), and also [talk with Scott Hunter](https://www.youtube.com/watch?v=2TeKeGOQGeg).
