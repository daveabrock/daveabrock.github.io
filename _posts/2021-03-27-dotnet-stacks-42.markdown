---
date: "2021-03-27"
title: "The .NET Stacks #42: 🔌 When Active Directory isn't so active"
tags: [dotnet-stacks]
image: /assets/img/stacks-42-card.png 
description: This week, we discuss the Azure AD outage and also talk about a variety of other topics.
---

![Newsletter image](https://daveabrock.com/THE .NET STACKS.png)

Happy Monday to you all. Here's what we have on tap this week.

- **One big thing**: When Active Directory isn't so active
- **The little things**: A bunch of odds and ends
- Last week in the .NET world

# One big thing: When Active Directory isn't so active

Mondays are typically long days. Tell that to Microsoft, who last Monday suffered [another Azure Active Directory outage](https://status.azure.com/status/history/) that took down most apps consuming AD, including the Azure Portal, Teams, Exchange, Azure Key Vault, Azure Storage, and more. The outage lasted a few hours (2 pm until 7 pm, in these parts), but lingering effects lasted much longer. The timing was unfortunate—isn't it always?—as they're rolling out [99.99% availability in April](https://redmondmag.com/articles/2021/01/05/microsoft-upping-azure-ad-sla.aspx) to customers with Premium licenses.

What happened? Azure AD runs an automated system that removes keys no longer in use. To support a "complex cross-cloud migration," a specific key was marked to retain for longer than usual. Due to a bug, the system ignored the flag, the key was removed, and Azure AD stopped trusting the tokens from the removed key. When you pair this with the outage [from September 2020](https://build5nines.com/azure-ad-is-down-blocking-access-to-azure-teams-and-more-september-28-2020-microsoft-azure-outage)—the culprit there was a code defect—you have a right to be concerned about Azure AD if you aren't already.

Meanwhile, updates were quicker on Twitter than on their status pages. Microsoft has owned up to this, saying: *"We identified some differences in detail and timing across Azure, Microsoft 365 and Dynamics 365 which caused confusion for customers ... We have a repair item to provide greater consistency and transparency across our services."*

For Microsoft's part, the notice says they are engaged in a two-stage process to improve Azure AD, including an effort to avoid what happened last Monday. This effort includes instituting a backend Safe Deployment Process (SDP) system to prevent these types of problems. The first stage is complete, and the second stage is planned for completion later this year.

Let's hope so. It's hard to swallow that such a critical service has a single point of failure. While there are many reasons for *and* against this design, we can all agree that Microsoft needs to improve resiliency for Azure AD. Instead of the time-honored tradition of Azure executives at Build or Ignite showing off a global map of all their new regions, I think we'd much rather have a slide showing off improvements to their flagship identity service.

# The little things: A bunch of odds and ends

In the [ASP.NET standup this week](https://www.youtube.com/watch?v=DkElWa3), James Newton-King joined Jon Galloway to talk about [gRPC improvements for .NET 5](https://devblogs.microsoft.com/aspnet/grpc-performance-improvements-in-net-5/). It gets low-level at times, but I enjoyed it and learned a lot. 

For the improvements, benchmarks show the .NET gRPC implementation just behind Rust (which isn't a framework, so that's saying something). Server performance is 60% faster than .NET Core 3.1, and client performance is 230% faster.

To answer your next question: since IIS and HTTP.sys now support gRPC, does Azure App Service support it too? Not yet, but [keep an eye on this issue](https://github.com/dotnet/aspnetcore/issues/9020) for the latest updates.

***

Adam Sitnik, an engineer on the .NET team and the person behind BenchmarkDotNet, has [a new repository](https://github.com/adamsitnik/awesome-dot-net-performance) full of valuable resources for learning about .NET performance.

***

Steve Sanderson, the creator of Blazor (and a [recent interview subject](https://daveabrock.com/2021/01/17/dev-discussions-steve-sanderson)), has [created an excruciatingly detailed Blazor issue in GitHub](https://github.com/dotnet/aspnetcore/issues/30940) to catch and handle exceptions thrown within a particular UI subtree. This capability accomplishes an idea of "[global exception handling](https://github.com/dotnet/aspnetcore/issues/13452)" in Blazor.

***

This week, Nick Craver noted why Stack Overflow likely isn't migrating to .NET 5. (You'll want to read [the entire thread](https://twitter.com/Nick_Craver/status/1371800961002405888) for context.)

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">I got several pings about .NET 5 for Stack Overflow over break: unfortunately, no good news there.<br><br>We&#39;ve hit many flaky build failures with the .NET 5 SDK even when building 3.1, so many that we&#39;ve reverted back to the 3.1 SDK and have no timeline on a 5.x rollout.</p>&mdash; Nick Craver (@Nick_Craver) <a href="https://twitter.com/Nick_Craver/status/1371800961002405888?ref_src=twsrc%5Etfw">March 16, 2021</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

***

Shay Rojansky [notes that EF Core](https://twitter.com/shayrojansky/status/1372669765995655170) is now fully annotated for C# reference nullability. As a whole, fully annotating nullability across .NET should be complete in .NET 6.

***

I've been intrigued this week by Daniel Terhorst-North writing about why he feels "[every single element of SOLID is wrong](https://dannorth.net/2021/03/16/cupid-the-back-story/)." It's quite the statement, but the more you read, the less revolutionary it sounds. Things change and evolve. Whether it's SOLID or any other prescribed "best practice," I've learned to take things with a grain of salt and consider the tradeoffs.

***

I've been working with many scheduled GitHub Actions to automate much of how I put together this newsletter every week (like adding links to a persistent store and generating my Markdown file). With scheduling tasks, as in timed Azure Functions triggers, CRON is still king. It's nice that GitHub Actions translates CRON syntax for you on hover, but I'm still going to mess it up.

What saves me every time is the *[crontab.guru](https://crontab.guru/)* site. (I'm not being asked to say this. I'm just a fan.) You can edit a CRON expression and easily see how it looks for a CRON amateur. You can also hit quick links with examples ready to go, like *[crontab.guru/every-day-8am](https://crontab.guru/every-day-8am)*.

![crontab.guru]({{ site.url }}{{ site.baseurl }}/assets/img/crontab.jpg)

***

# 🌎 Last week in the .NET world

## 🔥 The Top 3

- Thomas Ardal [avoids password reuse with Pwned Passwords and ASP.NET Core](https://blog.elmah.io/avoid-password-reuse-with-pwned-passwords-and-asp-net-core/).
- Jon Gallant [writes how the Azure SDK team decided to capitalize the T in Azure.IoT](https://blog.jongallant.com/2021/03/the-case-of-the-last-t/).
- Andrew Lock [runs Kubernetes and the dashboard with Docker Desktop](https://andrewlock.net/running-kubernetes-and-the-dashboard-with-docker-desktop/).

## 📢 Announcements

- Sourabh Shirhatti writes [how OpenTelemetry has reached v1.0](https://devblogs.microsoft.com/dotnet/opentelemetry-net-reaches-v1-0).
- Michael A. Hawker [announces Windows Community Toolkit v7.0](https://blogs.windows.com/windowsdeveloper/2021/03/16/announcing-windows-community-toolkit-v7-0).
- Bri Achtman [provides ML.NET and Model Builder March updates](https://devblogs.microsoft.com/dotnet/ml-net-and-model-builder-march-updates).
- Norm Johanson and Philip Pittle [writes about the new deployment experience for AWS on .NET](https://aws.amazon.com/blogs/developer/reimagining-the-aws-net-deployment-experience/).
- Jon Douglas [writes about the NuGet 5.9 release](https://devblogs.microsoft.com/nuget/performance-and-polish-with-nuget-5-9).
- Antonin Prochazka [announces PostSharp 6.9 RC](https://blog.postsharp.net/post/postsharp-6-9-rc-visual-studio-tooling-performance-improvements.html).
- JetBrains releases [Rider 2020.3.4 and ReSharper 2020.3.4](https://blog.jetbrains.com/dotnet/2021/03/17/rider-resharper-2020-3-4/), and also brings [scaffolding for ASP.NET Core projects Rider 2021.1](https://blog.jetbrains.com/dotnet/2021/03/18/scaffolding-for-asp-net-core-projects-comes-to-rider-2021-1/).
- Visual Studio 2019 v16.9.2 [is now available](https://docs.microsoft.com/visualstudio/releases/2019/release-notes#16.9.2).
- Taylor Blau [recaps Git 2.31](https://github.blog/2021-03-15-highlights-from-git-2-31/).

## 📅 Community and events

- For community standups: Machine Learning [talks about extending ML.NET](https://www.youtube.com/watch?v=Nj3z6d-SJWs) and ASP.NET [talks to James Newton-King about recent gRPC performance improvements](https://www.youtube.com/watch?v=DkElWa3--8s).
- Nick Randolph [writes about a vision for the Windows developer platform](https://nicksnettravels.builttoroam.com/windows-dev-platform/) and also [wants to stop talking about UWP](https://nicksnettravels.builttoroam.com/stop-talking-about-uwp/).
- David Ramel writes how the [Windows Community Toolkit is getting the .NET Standard MVVM Library](https://visualstudiomagazine.com/articles/2021/03/16/wct-7.aspx), and also [writes about the slow EF Core adoption](https://visualstudiomagazine.com/articles/2021/03/16/ef-core.aspx).
- Dirkjan Bussink [writes about how GitHub found and fixed a rare race condition in its session handling](https://github.blog/2021-03-18-how-we-found-and-fixed-a-rare-race-condition-in-our-session-handling/).
- The .NET Docs Show [talks to Vahid Farahmandian about the Middle East's largest maritime ERP](https://www.youtube.com/watch?v=UkFBwMOT5Ag).

## 🌎 Web development

- Dave Brock [adds a shared dialog component in Blazor](https://daveabrock.com/2021/03/17/blast-off-blazor-add-dialog).
- Josef Ottosson [selects an action method based on header values in ASP.NET Core](https://josef.codes/select-action-method-based-on-header-value-asp-net-core/).
- David Grace [writes about how to read the appsettings.json Configuration File in ASP.NET Core](https://www.roundthecode.com/dotnet/how-to-read-the-appsettings-json-configuration-file-in-asp-net-core).
- Scott Brady [integrates ASP.NET identity password policies with password managers](https://www.scottbrady91.com/ASPNET-Identity/ASPNET-Identity-Password-Policies-with-Password-Managers).
- Ian Russell [works with F# and Giraffe](https://www.softwarepark.cc/blog/2021/3/12/introduction-to-web-programming-in-f-with-giraffe-part-2).
- Muhammed Saleem [uses Azure SQL with an ASP.NET Core Web API](https://code-maze.com/azure-sql-with-asp-net-core-web-api/).
- Khalid Abuhakmeh [resolves services in ASP.NET Core](https://khalidabuhakmeh.com/resolve-services-in-aspnet-core-startup).
- Isaac Levin writes about migrating the [Rock, Paper, Scissors, Lizard, Spock (RPSLS) site to .NET 5 and Blazor Web Assembly](https://devblogs.microsoft.com/dotnet/the-path-to-net-5-and-blazor-webassembly-with-some-fun-sprinkled-in).

## ⛅ The cloud

- Aaron Powell [continues his series on GraphQL on Azure with some SignalR](https://www.aaron-powell.com/posts/2021-03-15-graphql-on-azure-part-6-subscriptions-with-signalr/).
- Mark Heath [generates an Azure Blob Storage User Delegation SAS](https://markheath.net/post/user-delegation-sas).

## 📔 Languages

- Matthew MacDonald [writes about three possible C# 10 features](https://medium.com/young-coder/c-10-3-candidate-features-that-could-make-the-final-cut-3b46f4a62284).
- Dave Brock [uses C# to upload files to a GitHub repository](https://daveabrock.com/2021/03/14/upload-files-to-github-repository).
- Khalid Abuhakmeh [creates a Zip file with .NET 5](https://khalidabuhakmeh.com/create-a-zip-file-with-dotnet-5).

## 🔧 Tools

- Laurent Kempé [works on service-to-service invocation with the Dapr SDK](https://laurentkempe.com/2021/03/16/service-to-service-invocation-with-dapr-dotnet-sdk/).
- Donovan Brown [develops Dapr components in a container](https://www.donovanbrown.com/post/Develop-Dapr-Components-in-a-container).
- Scott Hanselman [writes about Ryujinx, an experimental Nintendo Switch emulator written in C# for .NET Core](https://www.hanselman.com/blog/ryujinx-is-an-experimental-nintendo-switch-emulator-written-in-c-for-net-core).
- Khalid Abuhakmeh [generates Dockerfiles for .NET applications with Rider](https://blog.jetbrains.com/dotnet/2021/03/15/generate-dockerfile-for-net-applications-with-rider/).
- David Ramel [writes about WinUI teaming up with Uno for cross-platform apps](https://visualstudiomagazine.com/articles/2021/03/12/uno-winui.aspx).

## 📱 Xamarin

- Brad Dean [updates a Xamarin.Forms project to MAUI](https://truegeek.com/2021/03/16/i-updated-forms-to-maui/).
- Jesse Liberty [writes about Xamarin best practices](http://jesseliberty.com/2021/03/17/xamarin-best-practices/).
- Leomaris Reyes [works with local notifications in Xamarin.Forms](https://www.telerik.com/blogs/getting-started-with-local-notifications-xamarin-forms).
- James Montemagno [builds settings screens for Xamarin.Forms](https://devblogs.microsoft.com/xamarin/great-looking-settings-screens-for-xamarin-forms).

## 🏗 Design, testing, and best practices

- Steve Smith [writes about how hardware hides many sins](https://ardalis.com/hardware-hides-many-sins/).
- Derek Comartin [decomposes CRUD to a task-based UI](https://codeopinion.com/decomposing-crud-to-a-task-based-ui/).
- Richard Lander [investigates a Linux CVE with .NET images](https://devblogs.microsoft.com/dotnet/investigating-a-linux-cve-with-net-images).
- Daniel Terhorst-North [writes about his distaste for SOLID](https://dannorth.net/2021/03/16/cupid-the-back-story/).
- Ian Miell asks: [when should I interrupt someone](https://zwischenzugs.com/2021/03/15/when-should-i-interrupt-someone/)?
- Jason Farrell [addresses the misconception that Entity Framework needs a data layer](https://jfarrell.net/2021/03/14/common-misconception-1-entity-framework-needs-a-data-layer/).
- Jon Hilton [builds design system–friendly components](https://www.telerik.com/blogs/building-design-system-friendly-components).
- Subodh Sohoni [writes about microservices](https://www.dotnetcurry.com/microsoft-azure/microservices-architecture).
- Mark Seemann [writes about using pure functions by default](https://blog.ploeh.dk/2021/03/15/pendulum-swing-pure-by-default/).
- Amy Rigby [writes how consistency can make you successful](https://blog.trello.com/every-successful-person-has-in-common).
- Damien Bowden [writes about the authentication pyramid](https://damienbod.com/2021/03/17/the-authentication-pyramid/).

## 🎤 Podcasts

- The Complete Developer podcast [talks about coding coding analyzers](https://completedeveloperpodcast.com/metacoding-coding-code-analyzers/).
- The 6-Figure Developer podcast [has agile conversations with Fredrick & Squirrel](https://6figuredev.com/podcast/episode-187-agile-conversations-with-fredrick-squirrel/).
- The Azure DevOps Podcast [talks to Richard Campbell on the Humanitarian Toolbox](http://azuredevopspodcast.clear-measure.com/richard-campbell-on-the-humanitarian-toolbox-episode-132).
- The .NET Rocks podcast [talks about MongoDB in the cloud](https://www.dotnetrocks.com/default.aspx?ShowNum=1731).
- RunAsRadio [talks to Anna Hoffman about migrating to Azure SQL](http://www.runasradio.com/default.aspx?ShowNum=767).

## 🎥 Videos

- James Montemagno [works on HTTP web requests with data caching](https://www.youtube.com/watch?v=a37qBMt0V9w).
- The ON .NET Show [discussing messaging patterns for .NET developers](https://www.youtube.com/watch?v=ef1DK76rseM).
- The ASP.NET Monsters [discuss output formatters in ASP.NET Core](https://www.youtube.com/watch?v=nZBTW9SnnTw).
- Data Exposed [recaps what's new in Azure SQL auditing](https://channel9.msdn.com/Shows/Data-Exposed/Whats-New-in-Azure-SQL-Auditing).