---
date: "2021-02-06"
title: "The .NET Stacks #35: 🔑 Nothing is certain but death and expiring certificates"
tags: [dotnet-stacks]
comments: false
share-img: /assets/img/stacks-35-card.png 
subtitle: This week, we talk about NuGet, Razor improvements, and more.
---

![Newsletter image](https://daveabrock.com/THE .NET STACKS.png)

Welcome to another week, everybody. I’ve been getting some complaints; I haven’t had a bad joke in a while. I know it’d be a nice shot in the arm, but now’s not the time to be humerus.

We're covering a lot of little things this week, so let's get started.

***

This week, the .NET team announced improvements to the new Razor editor in Visual Studio (obviously in response to [my complaints last week](https://daveabrock.com/2021/01/30/dotnet-stacks-34)).

Six months ago, the team [announced a preview of an experimental Razor editor](https://devblogs.microsoft.com/aspnet/improvements-to-the-new-razor-editor-in-visual-studio/) based on a common Razor language server. (We [talked about it in Issue #9](https://daveabrock.com/2020/07/25/dotnet-stacks-9), if you want all the specifics.) It's now available in the latest Visual Studio preview ([16.9 version 3](https://visualstudio.com/preview)).

The new Razor editor allows the team to enable C# code actions more easily—with this update, Razor can help you discover using statements and null checks. This also extends to Blazor. Check out the [blog post](https://devblogs.microsoft.com/aspnet/improvements-to-the-new-razor-editor-in-visual-studio/) for details (and you can fill out a survey [about syntax coloring](https://www.surveymonkey.com/r/LFYCKPN).)

***

The NuGet team needed some #HugOps this week as they [dealt with an expired certificate](https://visualstudiomagazine.com/articles/2021/01/27/nuget-issue.aspx) that temporarily broke .NET 5 projects running on Debian Linux.

Here's what the [NuGet team had to say](https://github.com/NuGet/Announcements/issues/49):

>This is because of an issue reported in the ca-certificates package in which the root CA being used are not trusted and must be installed manually due to Symantec Issues. Debian removed the impacted Symantec certificate from their ca-certificates package in buster (Debian 10) impacting all user ... The Debian patch was too broad in removing the certificate for all uses and created the error messages seen today.

For some good news, the NuGet website has a new *Open in FuGet Package Explorer* link that allows you to [explore packages with FuGet](https://twitter.com/JamesMontemagno/status/1355231390967721985).

***

This week, I [wrote about Signed HTTP Exchanges](https://daveabrock.com/2021/01/26/signed-http-exchanges-cdn-cache). In our interview [with Blazor creator Steve Sanderson](https://daveabrock.com/2021/01/17/dev-discussions-steve-sanderson), he mentioned it as a way to potentially help speed up runtime loading for Blazor WebAssembly applications by using it as a type of cross-site CDN cache.

Here's what he told us:

>It’s conceivable that new web platform features like Signed HTTP Exchanges could let us smartly pre-load the .NET WebAssembly runtime in a browser in the background (directly from some Microsoft CDN) while you’re visiting a Blazor WebAssembly site, so that it’s instantly available at zero download size when you go to other Blazor WebAssembly sites. Signed HTTP Exchanges allow for a modern equivalent to the older idea of a cross-site CDN cache. We don’t have a definite plan about that yet as not all browsers have added support for it.

It isn't a sure thing because the technology is still quite new, but it's a promising idea to overcome the largest drawback of using Blazor WebAssembly.

***

I've written about [prerendering Blazor WebAssembly apps](https://daveabrock.com/2020/12/27/blast-off-blazor-prerender-wasm). It's a little weird, as you give up the ability to host your app as static files. Why not host over Blazor Server, then?

This week, Andrew Lock [wrote about a creative approach](https://andrewlock.net/prerending-a-blazor-webassembly-app-without-an-asp-net-core-host-app/) where you can prerender an app to static files without a host app. It does have tradeoffs—you need to define routes beforehand and it doesn't work well if you're working with dynamic data—but it's worth checking out if it fits your use case.

***

At the Entity Framework [community standup this week](https://www.youtube.com/watch?v=lmHU1zD2mvA), they discussed the `MSBuild.Sdk.SqlProj` project with Jonathan Mezach. Already [sitting at 42k downloads from GitHub](https://github.com/rr-wfm/MSBuild.Sdk.SqlProj), it's an SDK that produces *.dacpac* files from SQL scripts that can be deployed using `SqlPackage.exe` or `dotnet publish`. It's like SQL Server Data Tools but built with the latest tooling.

You can check out Jonathan's [announcement on his blog](https://jmezach.github.io/post/introducing-msbuild-sdk-sqlproj/) to understand how it all works and his motivations for building it.

***

## 🌎 Last week in the .NET world

### 🔥 The Top 3

- Daniel Roth [writes about improvements to the new Razor editor in Visual Studio](https://devblogs.microsoft.com/aspnet/improvements-to-the-new-razor-editor-in-visual-studio).
- Andrew Lock [prerenders a Blazor WebAssembly app to static files without an ASP.NET host app](https://andrewlock.net/prerending-a-blazor-webassembly-app-without-an-asp-net-core-host-app/).
- Jimmy Bogard [writes how to choose a ServiceLifetime](https://jimmybogard.com/choosing-a-servicelifetime/).

### 📢 Announcements

- Kayla Cinnamon [announces Windows Terminal Preview 1.6](https://devblogs.microsoft.com/commandline/windows-terminal-preview-1-6-release).
- Mads Kristensen [writes about new ways to provide Visual Studio feedback](https://devblogs.microsoft.com/visualstudio/new-experience-for-sending-us-your-feedback).
- Gerald Versluis [introduces the Xamarin Community Toolkit](https://devblogs.microsoft.com/xamarin/xamarin-community-toolkit).
- PostSharp introduces [Project Caravela, a Roslyn-based aspect framework](https://blog.postsharp.net/post/announcing-caravela-preview.html).
- W3C welcomes [Open Web Docs](https://www.w3.org/blog/2021/01/welcome-to-open-web-docs/), while [Microsoft](https://blogs.windows.com/msedgedev/2021/01/25/welcome-open-web-docs) and [Mozilla](https://hacks.mozilla.org/2021/01/welcoming-open-web-docs-to-the-mdn-family/) also write about it.
- As David Ramel writes, [GitHub ships Enterprise Server 3.0 Release Candidate](https://visualstudiomagazine.com/articles/2021/01/22/github-enterprise-server.aspx).
- In JetBrains news, Matthias Koch [writes about the Rider 2021.1 roadmap](https://blog.jetbrains.com/dotnet/2021/01/25/rider-2021-1-roadmap/), Matt Ellis [provides a ReSharper 2021.1 roadmap](https://blog.jetbrains.com/dotnet/2021/01/27/resharper-2021-1-roadmap/), and Friedrich von Never has an [interesting Twitter thread](https://twitter.com/fvnever/status/1354092934073872385) about how they debug Blazor WebAssembly.
.
- David McCarter [updates his dotNetTips utility](https://www.c-sharpcorner.com/article/coding-faster-with-the-dotnettips-utility-february-2021-update/).

### 📅 Community and events

- For the community standups, ASP.NET [talks about better Razor editing in Visual Studio](https://www.youtube.com/watch?v=_znjce8Cs2k) and Entity Framework [introduces MsBuild.Sdk.SqlProj](https://www.youtube.com/watch?v=lmHU1zD2mvA).
- The .NET Docs Show [talks about creating an OSS mobile app with Xamarin and Azure](https://www.youtube.com/watch?v=TCnPdRxWDj4).

### 🌎 Web development

- Dave Brock [writes about using Signed HTTP Exchanges to help with runtime loading in Blazor WebAssembly](https://daveabrock.com/2021/01/26/signed-http-exchanges-cdn-cache).
- Marinko Spasojevic [uses roles in Blazor Web Assembly hosted applications](https://code-maze.com/using-roles-in-blazor-webassembly-hosted-applications/).
- Jeetendra Gund [uses the Select tag helper in ASP.NET Core MVC](https://www.telerik.com/blogs/select-tag-helper-asp-net-core-mvc).
- Marinko Spasojevic [writes about authentication in Blazor WebAssembly hosted application](https://code-maze.com/authentication-in-blazor-webassembly-hosted-applications/).
- Changhui Xu [writes about uploading multiple files with Angular and ASP.NET Web API](https://codeburst.io/uploading-multiple-files-with-angular-and-net-web-api-7560303d9345).
- Anthony Giretti [adds a gRPC service reference from a remote protobuf over Route-To-Code](https://anthonygiretti.com/2021/01/25/grpc-asp-net-core-5-add-a-grpc-service-reference-from-a-remote-protobuf-over-route-to-code/).

### 🥅 The .NET platform

- Konrad Kokosa [continues writing about .NET GC internals](https://tooslowexception.com/net-gc-internals-the-mark-phase/).
- Andrea Chiarelli [creates a .NET project template](https://auth0.com/blog/create-dotnet-project-template/).

### ⛅ The cloud

- Ryan Palmer [automates database deployments with Azure and Farmer](https://www.compositional-it.com/news-blog/automated-database-deployment-with-azure-and-farmer/).
- Peter De Tender [demystifies service principals and managed identities](https://devblogs.microsoft.com/devops/demystifying-service-principals-managed-identities).
- Shelton Graves [writes about choosing an event-driven architecture](https://techcommunity.microsoft.com/t5/apps-on-azure/event-driven-on-azure-part-1-why-you-should-consider-an-event/ba-p/2106983).
- Damien Bowden [implements an OAuth device code flow with Azure AD and ASP.NET Core](https://damienbod.com/2021/01/28/implement-oauth-device-code-flow-with-azure-ad-and-asp-net-core/).
- Daniel Krzyczkowski [builds out his ASP.NET Core API with an Azure Service Bus](https://daniel-krzyczkowski.github.io/Cars-Island-ASP-NET-Core-API-Integration-With-Azure-Service-Bus-Queue/).

### 📔 Languages

- Jon Skeet [writes an OSC mixer control in C#](https://codeblog.jonskeet.uk/2021/01/27/osc-mixer-control-in-c).
- Luca Bolognese [uses C# source generators to create an external DSL](https://devblogs.microsoft.com/dotnet/using-c-source-generators-to-create-an-external-dsl).
- Karthik Chintala [writes about the Observer design pattern](https://coderethinked.com/observer-design-pattern-with-an-example-in-csharp/).
- Khalid Abuhakmeh [gets you started with .NET 5 source generators](https://khalidabuhakmeh.com/dotnet-5-source-generators-jump-start).
- Jason Roberts [writes about switch expressions](http://dontcodetired.com/blog/post/ICYMI-C-8-New-Features-Switch-Expressions).
- Jon P. Smith [uses ValueTask to create methods that work as sync or async](https://www.thereformedprogrammer.net/using-valuetask-to-create-methods-that-can-work-as-sync-or-async/).
- Matthew Crews [writes about scheduling jobs in F# for maximum efficiency](https://matthewcrews.com/blog/2021/01/2021-01-25/).

### 🔧 Tools

- Rick Strahl [continues writing about the Chromium WebView2 control and .NET](https://weblog.west-wind.com/posts/2021/Jan/26/Chromium-WebView2-Control-and-NET-to-JavaScript-Interop-Part-2).
- Elton Stoneman [builds Docker images quickly with GitHub Actions and a self-hosted runner](https://blog.sixeyed.com/build-docker-images-quickly-with-github-actions-and-a-self-hosted-runner/).
- Jon P. Smith [updates a database schema without using EF Core's migrate feature](https://www.thereformedprogrammer.net/how-to-update-a-databases-schema-without-using-ef-cores-migrate-feature/).
- ErikEJ [writes about advanced automated deployments of an Azure SQL database with Azure DevOps](https://erikej.github.io/sqlserver/2021/01/25/azure-sql-advanced-deployment-part3.html).
- Dane Vinson [writes about the JsonEnvelopes library that helps serialize and deserialize message receivers](https://developingdane.azurewebsites.net/jsonenvelopes/).
- Microsoft announces [Windows ARM64 support for CMake projects in Visual Studio](https://devblogs.microsoft.com/cppblog/windows-arm64-support-for-cmake-projects-in-visual-studio).
- Mark-James McDougall [plots graphs from CSV files in F# using XPlot](https://markjames.dev/2021-01-23-plotting-csv-files-fsharp/).

### 📱 Xamarin

- Leomaris Reyes [replicates a talent casting UI](https://www.syncfusion.com/blogs/post/replicating-talent-casting-ui-in-xamarin-forms.aspx).
- The Xamarin Show [discusses the Xamarin Community Toolkit](https://channel9.msdn.com/Shows/XamarinShow/Introducing-the-Xamarin-Community-Toolkit).
- Luis Matos warns: [don't use CollectionView inside ScrollView in Xamarin.Forms](https://luismts.com/dont-use-collectionview-inside-scrollview/).

### 🏗 Design, testing, and best practices

- Stephen Cleary [writes about retrieving results from async messaging](https://blog.stephencleary.com/2021/01/asynchronous-messaging-4-retrieve-results.html).
- Derek Comartin [writes about avoiding a big ball of mud](https://codeopinion.com/avoiding-a-big-ball-of-mud/).
- Changhui Xu [writes integration tests for ASP.NET Core Web APIs using MSTest](https://codeburst.io/integration-tests-for-asp-net-core-web-apis-using-mstest-f4e222a3bc8a).
- Piotr Martyniuk [writes about connecting microservices](https://www.cncf.io/blog/2021/01/22/how-to-connect-microservices-part-1-types-of-communication/).

### 🎤 Podcasts

- The .NET Rocks podcast [talks to Julie Lerman about .NET 5](https://www.dotnetrocks.com/default.aspx?ShowNum=1724).
- On Yet Another Podcast, [Jesse Liberty talks C# with Mads Torgersen](http://jesseliberty.com/2021/01/26/mads-torgersen-c-9-beyond).
- The Adventures in .NET podcast [continues talking with Jon Skeet](https://devchat.tv/adventures-in-dotnet/net-053-abusing-c-calendars-epochs-and-the-net-functions-framework-with-jon-skeet-part-2/).
- The .NET Core Show [talks about Xamarin with Luce Carter](https://dotnetcore.show/episode-68-xamarin-catch-up-with-luce-carter/).

### 🎥 Videos

- The ON.NET Show [talks about using Azure AD B2C for authenticating users](https://www.youtube.com/watch?v=QuXI4rPaczc), and also [about distributed applications with ZeroMQ](https://www.youtube.com/watch?v=jIT8r2r5kV8).
- Jeremy Likness and Abel Wang [talk about hosting NuGet packages](https://channel9.msdn.com/Shows/On-NET/DevOps-for-ASPNET-Developers-Hosting-NuGet-Packages).
- Brigit Murtaugh [starts a beginner's series on dev containers](https://channel9.msdn.com/Series/Beginners-Series-to-Dev-Containers/Introduction-1-of-8--Beginners-Series-to-Dev-Containers).
- At Technology and Friends, [David Giard talks with Dustin Campbell about WinForms support for Visual Studio Designer](http://davidgiard.com/2021/01/25/DustinCampbellOnSupportForWinFormsToTheVisualStudioDesigner.aspx).