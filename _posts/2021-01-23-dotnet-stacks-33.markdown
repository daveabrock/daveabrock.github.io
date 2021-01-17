---
date: "2021-01-23"
title: "The .NET Stacks #33: 🚀 A blazing conversation with Steve Sanderson"
tags: [dotnet-stacks]
comments: false
share-img: /assets/img/stacks-33-card.png 
subtitle: It's a jam-packed issue this week, as we talk about Blazor with Steve Sanderson.
---

![Newsletter image](https://daveabrock.com/THE .NET STACKS.png)

Happy Monday, all. What did you get NuGet [for its 10th birthday](https://devblogs.microsoft.com/nuget/happy-10th-birthday-nuget/)?

This week:

- Microsoft blogs about more .NET 5 improvements
- A study on migrating a hectic service to .NET Core
- Meet Jab, a new compile-time DI library
- Dev Discussions: Steve Sanderson
- Last week in the .NET world

___

## Microsoft blogs about more .NET 5 improvements

This week, Microsoft pushed a few more blog posts to promote .NET 5 improvements: Sourabh Shirhatti [wrote about diagnostic improvements](https://devblogs.microsoft.com/dotnet/diagnostics-improvements-in-net-5/), and Máňa Píchová [writes about .NET networking improvements](https://devblogs.microsoft.com/dotnet/net-5-new-networking-improvements/).

### Diagnostic improvements

With .NET 5, the diagnostic suite of tools does not require installing them as .NET global tools—they can now be installed without the .NET SDK. There's now a single-file distribution mechanism that only requires a runtime of .NET Core 3.1 or higher. You can [check out the GitHub repo](https://github.com/dotnet/diagnostics/blob/master/documentation/single-file-tools.md) to geek out on all the available diagnostics tools. In other news, you can now perform startup tracing from EventPipe as the tooling can now suspend the runtime during startup until a tool is connected. Check out the [blog post for the full treatment](https://devblogs.microsoft.com/dotnet/diagnostics-improvements-in-net-5/).

### Networking improvements

In terms of .NET 5 networking improvements, the team added the ability to use cancellation timeouts from `HttpClient` without the need for a custom `CancellationToken`. While the client still throws a `TaskCanceledException`, the inner exception is a `TimeoutException` when timeouts occur. .NET 5 also supports [multiple connections with HTTP/2](https://github.com/dotnet/diagnostics/blob/master/documentation/single-file-tools.md), a [configurable ping mechanism](https://devblogs.microsoft.com/dotnet/net-5-new-networking-improvements/#configurable-ping), [experimental support for HTTP/3](https://devblogs.microsoft.com/dotnet/net-5-new-networking-improvements/#http-3), and [various telemetry improvements](https://devblogs.microsoft.com/dotnet/net-5-new-networking-improvements/#telemetry). Check out the [networking blog post for details](https://devblogs.microsoft.com/dotnet/net-5-new-networking-improvements). It's a nice complement to [Stephen Toub's opus about .NET 5 performance improvements](https://devblogs.microsoft.com/dotnet/performance-improvements-in-net-5/).

___

## A study on migrating a hectic service to .NET Core

This week, Avanindra Paruchuri [wrote about migrating the Azure Active Directory gateway](https://devblogs.microsoft.com/dotnet/azure-active-directorys-gateway-service-is-on-net-core-3-1/)—and its 115 billion daily requests—over to .NET Core. While there's nothing preventing you hosting .NET Framework apps in the cloud, the bloat of the framework often leads to expensive cloud spend.

>The gateway’s scale of execution results in significant consumption of compute resources, which in turn costs money. Finding ways to reduce the cost of executing the service has been a key goal for the team behind it. The buzz around .NET Core’s focus on performance caught our attention, especially since TechEmpower listed ASP.NET Core as one of the fastest web frameworks on the planet.

>In Azure AD gateway’s case, we were able to cut our CPU costs by 50%. As a result of the gains in throughput, we were able to reduce our fleet size from ~40k cores to ~20k cores (50% reduction) ... Our CPU usage was reduced by half on .NET Core 3.1 compared to .NET Framework 4.6.2 (effectively doubling our throughput).

It's a nice piece on how they were able to gradually move over and gotchas they learned along the way.

___

## Meet Jab, a new compile-time DI library

This week, Pavel Krymets introduced Jab, a [library used for compile-time dependency injection](https://github.com/pakrym/jab). Pavel works with the Azure SDKs and used to work on the ASP.NET Core team. Remember a few weeks ago, when we said that innovation in C# source generators will be coming in 2021? Here we go.

From the GitHub readme, it promises fast startup (200x more than `Microsoft.Extensions.DependencyInjection`), fast resolution (a 7x improvement), no runtime dependencies, with all code generating during project compilation. Will it run on ASP.NET Core? Not likely, since ASP.NET Core is heavily dependent on the runtime thanks to type accessibility and dependency discovery, but Pavel [wonders if there's a middle ground](https://twitter.com/pakrym/status/1348862830368358401).

___

## Dev Discussions: Steve Sanderson

It seems like forever ago when, [at NDC Oslo in 2017](https://youtu.be/MiLAE6HMr10?t=1612), Steve Sanderson talked about a fun project he was working on, called .NET Anywhere. In the demo, he was able to load and run C# code—*ConsoleApp1.dll*, specifically—in the browser, using Web Assembly. C# in the browser! In the talk, he called it "*an experiment, something for you to be amused by*."

Of course, this amusing experiment has grown [into Blazor](https://dotnet.microsoft.com/apps/aspnet/web-apps/blazor), a robust system for writing web UIs in C#. I was happy to talk to Steve Sanderson about his passions for the front-end web, how far Blazor has come, and what's coming to Blazor in .NET 6.

![Steve Sanderson profile photo]({{ site.url }}{{ site.baseurl }}/assets/img/steve-sanderson.jpg)

> Years ago, you probably envisioned what Blazor could be. Has it met its potential, or are there other areas to focus on?

We’re not there yet. If you go on YouTube and find the [first demo I ever did of Blazor at NDC Oslo in 2017](https://youtu.be/MiLAE6HMr10?t=1612), you’ll see my original prototype had near-instant live reloading while coding, and the download size was really tiny. I still aspire to get the real version of Blazor to have those characteristics. Of course, the prototype had the advantage of only needing to do a tiny number of things—creating a production-capable version is 100x more work, which is why it hasn’t yet got there, but has of course exceeded the prototype vastly in more important ways.

Good news though is that in .NET 6 [we expect to ship](https://github.com/dotnet/aspnetcore/issues/5456) an even better version of live-updating-while-coding than I had in that first prototype, so it’s getting there!

> When looking at AOT, you'll see increased performance but a larger download size. Do you see any other tradeoffs developers will need to consider?

The mixed-mode flavour of AOT, in which some of your code is interpreted and some is AOT, allows for a customizable tradeoff between size and speed, but also includes some subtleties like extra overhead when calling from AOT to interpreted code and vice-versa.

Also, when you enable AOT, your app’s publish time may go up substantially (maybe by 5-10 minutes, depending on code size) because the whole [Emscripten toolchain](https://emscripten.org/) just takes that long. This wouldn’t affect your daily development flow on your own machine, but likely means your CI builds could take longer.

> It's still quite impressive to see the entire .NET runtime run in the browser for Blazor Web Assembly. That comes with an upfront cost, as we know. I know that the Blazor team has done a ton of work to help lighten the footprint and speed up performance. With the exception of AOT, do you envision more work on this? Do you see a point where it'll be as lightweight as other leading front-end frameworks, or will folks need to understand it's a cost that comes with a full framework in the browser?

The size of the .NET runtime isn’t ever going to reduce to near-zero, so JS-based microframeworks (whose size could be just a few KB) are always going to be smaller. We’re not trying to win outright based on size alone—that would be madness. Blazor WebAssembly is aimed to be maximally productive for developers while being small enough to download that, in very realistic business app scenarios, the download size shouldn’t be any reason for concern.

That said, it’s conceivable that new web platform features like [Signed HTTP Exchanges](https://developers.google.com/web/updates/2018/11/signed-exchanges) could let us smartly pre-load the .NET WebAssembly runtime in a browser in the background (directly from some Microsoft CDN) while you’re visiting a Blazor WebAssembly site, so that it’s instantly available at zero download size when you go to other Blazor WebAssembly sites. Signed HTTP Exchanges allow for a modern equivalent to the older idea of a cross-site CDN cache. We don’t have a definite plan about that yet as not all browsers have added support for it.

*Check out the [entire interview at my site](https://daveabrock.com/2021/01/17/dev-discussions-steve-sanderson).*

___

## 🌎 Last week in the .NET world

### 🔥 The Top 3

- Andrew Lock [introduces the ASP.NET Core Data Protection system](https://andrewlock.net/an-introduction-to-the-data-protection-system-in-asp-net-core/).
- Maarten Balliauw [writes about building a friendly .NET SDK](https://blog.maartenballiauw.be/post/2021/01/13/the-process-thought-and-technology-behind-building-a-friendly-net-sdk-for-jetbrains-space.html).
- Josef Ottosson [writes an Azure Function to zip multiple files from Azure Storage](https://josef.codes/azure-storage-zip-multiple-files-using-azure-functions/).

### 📢 Announcements

- Shelley Bransten [announces Microsoft Cloud for Retail](https://cloudblogs.microsoft.com/industry-blog/retail/2021/01/13/introducing-microsoft-cloud-for-retail/).
- Christopher Gill [celebrates NuGet's 10th birthday](https://devblogs.microsoft.com/nuget/happy-10th-birthday-nuget).
- Tara Overfield [releases the January 2021 Security and Quality Rollup Updates for .NET Framework](https://devblogs.microsoft.com/dotnet/net-framework-january-security-and-quality-rollup-update), and Rahul Bhandari [writes about the .NET January 2021 updates](https://devblogs.microsoft.com/dotnet/net-january-2021).
- .NET 6 nightly builds for Apple M1 [are now available](https://t.co/3SZmy73Z10?amp=1).
- The Visual Studio team [wants your feedback](https://t.co/NMJgBobHzR?amp=1) on Razor syntax coloring.

### 📅 Community and events

- The .NET Docs Show [talks to Luis Quintanilla about F#](https://www.youtube.com/watch?v=biL1XOcg8Us).
- Pavel Krymets [introduces Jab](https://t.co/NnOE2bnefq?amp=1), a compile-time DI container.
- The Entity Framework Standup [talks about EF Core 6 survey results](https://www.youtube.com/watch?v=IiAS61uVDqE), and the Languages & Runtime standup [discusses plans for .NET 6 and VB source generators](https://www.youtube.com/watch?v=jv84Ewh28OI).
- Sarah Novotny [writes about 4 open source lessons for 2021](https://cloudblogs.microsoft.com/opensource/2021/01/14/four-open-source-lessons).
- IdentityServer v5 [has shipped](https://blog.duendesoftware.com/posts/20210114_v5_release/).
- Khalid Abuhakmeh [rethinks OSS attribution in .NET](https://khalidabuhakmeh.com/rethinking-oss-attribution-in-net).
- TechBash 2021 [is slated for October 19-22, 2021](https://www.techbash.com/blog/2021/01/14/announcing-techbash-2021).

### 🌎 Web development

- Dave Brock [builds a "search-as-you-type" box in Blazor](https://daveabrock.com/2021/01/14/blast-off-blazor-search-box).
- Cody Merritt Anhorn [uses localization with Blazor](https://codyanhorn.tech/blog/2021/01/13/Blazor-Getting-Started-with-Localization.html).
- Changhui Xu [uploads files with Angular and .NET Web API](https://codeburst.io/upload-files-with-angular-and-net-web-api-77a7966ed226).
- Mark Pahulje [uses HtmlAgilityPack to get all emails from an HTML page](http://metadataconsulting.blogspot.com/2021/01/CSharp-dotNet-How-to-get-all-emails-from-a-HTML-page-with-a-href-inner-text.html).
- Jon Hilton [uses local storage with Blazor](https://jonhilton.net/blazor-tailwind-dark-mode-local-storage/).
- Anthony Giretti [tests gRPC endpoints with gRPCurl](https://anthonygiretti.com/2021/01/13/grpc-asp-net-core-5-test-grpc-endpoints-with-grpcurl/), and [also explores gRPCui](https://anthonygiretti.com/2021/01/17/grpc-asp-net-core-5-discover-grpcui-the-gui-alternative-to-grpcurl/).
- The folks at Uno [write about building a single-page app in XAML and C# with WebAssembly](https://platform.uno/blog/how-to-build-a-single-page-web-app-in-xaml-and-c-with-webassembly-using-uno-platform/).
- Marinko Spasojevic [handles query strings in Blazor WebAssembly](https://code-maze.com/query-strings-blazor-webassembly/).
- Daniel Krzyczkowski [continues building out his ASP.NET Core Web API by integrating with Azure Cosmos DB](https://daniel-krzyczkowski.github.io/Cars-Island-ASP-NET-Core-API-Integration-With-Azure-Cosmos-DB/).

### 🥅 The .NET platform

- Sean Killeen [describes the many flavors of .NET](https://flavorsof.net/).
- Mattias Karlsson [writes about his boilerplate starting point for .NET console apps](https://www.devlead.se/posts/2021/2021-01-15-my-preferred-console-stack).
- David Ramel [delivers a one-stop shop for .NET 5 improvements](https://visualstudiomagazine.com/articles/2021/01/15/net-5-improvements.aspx).
- Sam Walpole [discusses writing decoupled code with MediatR](https://dev.to/dr_sam_walpole/writing-decoupled-code-with-mediatr-the-mediator-pattern-34p5).
- Sourabh Shirhatti [writes about diagnostics improvements with .NET 5](https://devblogs.microsoft.com/dotnet/diagnostics-improvements-in-net-5).
- Máňa Píchová [writes about .NET 5 networking improvements](https://devblogs.microsoft.com/dotnet/net-5-new-networking-improvements).

### ⛅ The cloud

- Avanindra Paruchuri [writes about migrating the Azure AD gateway to .NET Core](https://devblogs.microsoft.com/dotnet/azure-active-directorys-gateway-service-is-on-net-core-3-1).
- Johnny Reilly [works with Azure Easy Auth](https://blog.johnnyreilly.com/2021/01/azure-easy-auth-and-roles-with-dotnet-and-core.html).
- Muhammed Saleem [works with Azure Functions](https://code-maze.com/creating-serverless-apps-with-dotnet-using-azure-functions/).
- Chris Noring [uses Azure Key Vault to manage secrets](https://dev.to/azure/using-azure-key-vault-to-manage-your-secrets-546g).
- Bryan Soltis [posts a file to an Azure Function in 3 minutes](https://soltisweb.com/blog/detail/2020-11-10-howtopostafiletoazurefunctionin3minutes).
- Damian Brady [generates a GitHub Actions workflow with Visual Studio or the dotnet CLI](https://devblogs.microsoft.com/devops/generate-a-github-actions-workflow-with-visual-studio-or-the-dotnet-cli).
- Thomas Ardal [builds and tests multiple .NET versions with GitHub Actions](https://blog.elmah.io/building-and-testing-on-multiple-net-versions-with-github-actions/).
- Dominique St-Amand [works with integration tests using Azure Storage emulator and .NET Core in Azure DevOps](https://www.domstamand.com/integration-tests-using-azure-storage-emulator-and-net-core-in-azure-devops/).
- Aaron Powell [uses environments for approval workflows with GitHub Actions](https://www.aaron-powell.com/posts/2021-01-11-using-environments-for-approval-workflows-with-github/).
- Damien Bowden [protects legacy APIs with an ASP.NET Core YARP reverse proxy and Azure AD Auth](https://damienbod.com/2021/01/11/protecting-legacy-apis-with-an-asp-net-core-yarp-reverse-proxy-and-azure-ad-oauth/).

### 📔 Languages

- Khalid Abuhakmeh [writes about Base64 encoding with C#](https://khalidabuhakmeh.com/base64-encoding-with-csharp).
- Franco Tiveron [writes about a developer's C# 9 cheat sheet](https://developer.okta.com/blog/2021/01/13/developers-cheatsheet-csharp-9).
- Bruno Sonnino [uses C# to convert XML data to JSON](https://blogs.msmvps.com/bsonnino/2021/01/08/converting-xml-data-to-json/).
- Jacob E. Shore [writes about his first impressions of F#](https://dev.to/dewofyouryouth_43/first-impressions-of-f-1g9m).
- Matthew Crews [writes about learning resources for F#](https://matthewcrews.com/blog/2021/01/2021-01-09/).
- Mark-James McDougall [writes an iRacing SDK implementation in F#](https://markjames.dev/2021-01-08-writing-an-iracing-sdk-implementation-fsharp/).

### 🔧 Tools

- Elton Stoneman [writes about understanding Microsoft's Docker images for .NET apps](https://blog.sixeyed.com/understanding-microsofts-docker-images-for-net-apps/).
- Jon P. Smith [writes about updating many-to-many relationships in EF Core 5 and above](https://www.thereformedprogrammer.net/updating-many-to-many-relationships-in-ef-core-5-and-above/).
- Ruben Rios [writes about a more integrated terminal experience with Visual Studio](https://devblogs.microsoft.com/visualstudio/a-more-integrated-terminal-experience).
- Benjamin Day [writes about tests in Visual Studio for Mac](https://devblogs.microsoft.com/visualstudio/visual-studio-for-mac-helps-you-write-tests).
- The folks at Packt [write about DAPR](https://hackernoon.com/microsofts-dapr-distributed-application-runtime-an-overview-nd2m34gj?source=rss).
- Peter De Tender [publishes Azure Container Instances from the Docker CLI](https://devblogs.microsoft.com/devops/publishing-azure-container-instances-from-docker-cli).
- Nikola Zivkovic [writes about linear regression with ML.NET](https://rubikscode.net/2021/01/11/machine-learning-with-ml-net-linear-regression/).
- Patrick Smacchia [writes how NDepend used Resharper to quickly refactored more than 23,000 calls to Debug.Assert()](https://blog.ndepend.com/how-we-quickly-refactored-with-resharper-more-than-23-000-calls-to-debug-assert-into-more-meaningful-assertions/).
- Mark Heath [discusses his plans for NAudio 2](https://markheath.net/post/naudio-2-plans).
- Michał Białecki [asks: is Entity Framework Core fast](https://www.michalbialecki.com/2021/01/10/entity-framework-core-is-it-fast)?
- Jon P. Smith [introduces a library to automate soft deletes in EF Core](https://www.thereformedprogrammer.net/introducing-the-efcore-softdeleteservices-library-to-automate-soft-deletes/).

### 📱 Xamarin

- Leomaris Reyes [introduces UX design with Xamarin Forms](https://www.telerik.com/blogs/design-for-developers-introduction-xamarin-forms).
- Charlin Agramonte [writes about XAML naming conventions in Xamarin.Forms](https://xamgirl.com/xaml-naming-conventions-in-xamarin-forms/).
- Leomaris Reyes [works with the Infogram in Xamarin.Forms 5.0](https://askxammy.com/infogram-about-xamarin-forms-5-0-%f0%9f%98%8d/).
- Rafael Veronezi [previews XAML UIs](https://www.mfractor.com/blogs/news/previewing-xaml-uis-in-xamarin-forms).
- James Montemagno [writes about how to integrate support emails in mobile apps with data and logs](https://montemagno.com/how-to-add-support-email-xamarin-apps/).
- Leomaris Reyes [writes about the Xamarin.Forms File Picker](https://askxammy.com/knowing-the-file-picker-in-xamarin-forms/).

### 👍 Design, testing, and best practices

- Steve Gordon [writes about how to become a better developer by asking questions](https://www.stevejgordon.co.uk/how-to-become-a-better-developer-by-asking-questions).
- Derek Comartin [says: start with a monolith, not microservices](https://codeopinion.com/start-with-a-monolith-not-microservices/).
- Stephen Cleary [writes about durable queues](https://blog.stephencleary.com/2021/01/asynchronous-messaging-2-durable-queues.html).

### 🎤 Podcasts

- Scott Hanselman [explores event modeling with Adam Dymitruk](https://hanselminutes.simplecast.com/episodes/exploring-event-modeling-with-adam-dymitruk-xvAdQlCd).
- At Working Code podcast, [a discussion on monoliths vs. microservices](https://www.bennadel.com/blog/3963-working-code-podcast-episode-005-monoliths-vs-microservices.htm).
- The .NET Rocks podcast [checks in on IdentityServer](https://www.dotnetrocks.com/default.aspx?ShowNum=1722).
- The .NET Core Show [talks Blazor with Chris Sainty](https://dotnetcore.show/episode-67-blazor-in-action-with-chris-sainty/).
- The 6-Figure Developer podcast [talks to Christos Matskas about Microsoft Identity](https://6figuredev.com/podcast/episode-178-identity-with-christos-matskas/).

### 🎥 Videos

- The ON.NET Show [inspects application metrics with dotnet-monitor](https://www.youtube.com/watch?v=hbgPvjTJSLY), [works on change notifications with Microsoft Graph](https://www.youtube.com/watch?v=ef_TBD3nkmQ), and [inspects application metrics with dotnet-monitor](https://channel9.msdn.com/Shows/On-NET/Inspecting-application-metrics-with-dotnet-monitor).
- Scott Hanselman [shows you what happens when after you enter a URL in your browser](https://www.youtube.com/watch?v=vvpCnjyjTuU).
- The ASP.NET Monsters [talk about migrating their site to Azure Blob Storage.](https://www.youtube.com/watch?v=FgoJt1c6R6U).
- At Technology and Friends, [David Giard talks to Mike Benkovich about GitHub Actions and Visual Studio](http://davidgiard.com/2021/01/11/MikeBenkovichOnGitHubActionsAndVisualStudio.aspx).
