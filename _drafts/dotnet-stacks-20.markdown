---
date: "2020-10-02"
title: "The .NET Stacks #20: Hey, 20"
tags: [dotnet-stacks]
comments: false
---

![Newsletter image]({{ site.url }}{{ site.baseurl }}/THE .NET STACKS.png)

## Understand Blazor Web Assembly performance best practices

This week, Steve Sanderson—the architect at Microsoft behind Blazor—announced he's working on [documenting Blazor Web Assembly performance best practices](https://github.com/dotnet/AspNetCore.Docs/blob/1e199f340780f407a685695e6c4d953f173fa891/aspnetcore/blazor/webassembly-performance-best-practices.md#aspnet-core-blazor-webassembly-performance-best-practices). While performance is always important, it appears doubly so when you're loading a complete .NET runtime into the browser.

It's definitely worth your time. 

**Add more stuff about that here.**

## Use "route to code" with ASP.NET Core

**Add more stuff about that here.**

## Surveys are in for .NET repos in GitHub

**Add more stuff about that here.**

## 🌎 Last week in the .NET world

### 🔥 The Top 3

* Heather Downing [talks about building securely with Blazor Web Assembly](https://developer.okta.com/blog/2020/09/30/blazor-webassembly-wasm-dotnetcore).
* Patrick Smacchia [discusses app trimming in .NET 5](https://blog.ndepend.com/net-5-0-app-trimming-and-potential-for-future-progress/).
* Phil Haack [finds a subtle gotcha with Azure deployment slots and ASP.NET Core](https://haacked.com/archive/2020/09/28/azure-swap-with-warmup-aspnetcore).

### 📢 Announcements

* Microsoft released [its survey results on repo experience](https://devblogs.microsoft.com/dotnet/repo-experience-survey-results), and folks in the WPF community [have voiced their displeasure](https://www.zdnet.com/article/microsoft-told-were-not-happy-by-github-contributors-to-open-source-net-core-wpf/#ftag=RSSbaffb68).
* Maria Naggaga [announces .NET Interactive Preview 3](https://devblogs.microsoft.com/dotnet/net-interactive-preview-3-vs-code-insiders-and-polyglot-notebooks).
* Bri Achtman [gives us September updates on ML.NET](https://devblogs.microsoft.com/dotnet/ml-net-september-updates).
* David Ortinau [previews Xamarin.Forms 5's advanced UI controls](https://devblogs.microsoft.com/xamarin/xamarin-forms-5-preview).

### 📅 Community and events

* The .NET Docs Show [double-clicks on accessibility and autonomous systems with John Alexander](https://www.youtube.com/watch?v=TqiMDHhADi8).
* Three .NET community standups this week: Xamarin [talks with Theodora Tataru](https://www.youtube.com/watch?v=zyFxAJjlPys), Entity Framework [runs through geographic data with NetTopologySuite](https://www.youtube.com/watch?v=IHslY5rrxD0), and ASP.NET [runs through .NET 5 with Scott Hunter](https://www.youtube.com/watch?v=l5nnYDd7gpk).

### 😎 ASP.NET Core / Blazor

* Shaun Curtis [adds new record types to his Blazor database app](https://www.codeproject.com/Articles/5281000/Building-a-Database-Application-in-Blazor-Part-6-A).
* Jon Hilton [renders diagrams on the fly in Blazor](https://jonhilton.net/blazor-diagrams/).
* Michael Shpilt [uses attributes and middleware in ASP.NET Core](https://michaelscodingspot.com/attributes-and-middleware-in-asp-net-core/).
* Michał Białecki [reads request headers as an object in ASP.NET Core](https://www.michalbialecki.com/2020/09/28/read-request-headers-as-an-object-in-asp-net-core-api).
* Anthony Giretti [uses Microsoft.AspNetCore.Http JSON extensions to assist with "route to code."](https://anthonygiretti.com/2020/09/29/asp-net-core-5-route-to-code-taking-advantage-of-microsoft-aspnetcore-http-json-extensions/)
* Andrew Lock [sets environment variables for ASP.NET Core apps in a Helm chart](https://andrewlock.net/deploying-asp-net-core-applications-to-kubernetes-part-5-setting-environment-variables-in-a-helm-chart/).

### ⛅ The cloud

* Pavel Krymets [discusses connection pool limits for .NET Framework when using the new .NET Azure SDK](https://devblogs.microsoft.com/azure-sdk/net-framework-connection-pool-limits).
* Muhammed Saleem [publishes an ASP.NET Core app to Azure App Service from Visual Studio](https://code-maze.com/publishing-an-asp-net-core-app-to-azure-app-service-using-visual-studio/).

### 📔 Languages

* Khalid Abuhakmeh [reads and converts QueryCollection values in ASP.NET](https://khalidabuhakmeh.com/read-and-convert-querycollection-values-in-aspnet).
* Franco Tiveron [shows how to use SameSite with .NET](https://developer.okta.com/blog/2020/09/28/adapt-dotnet-app-for-samesite-fix).
* Matthew Jones [discusses casting, conversion, and parsing in C#](https://exceptionnotfound.net/csharp-in-simple-terms-3-casting-conversion-parsing-is-as-and-typeof).
* Khalid Abuhakmeh [serializes interface instances with System.Text.Json](https://khalidabuhakmeh.com/serialize-interface-instances-system-text-json).
* David Grace [shows off four new C# 9 features](https://www.roundthecode.com/dotnet/four-features-c-sharp-9-that-arent-in-c-sharp-8).
* Matthew Jones [runs through primitive types, literals, and nullables in C#](https://exceptionnotfound.net/csharp-in-simple-terms-2-primitive-types-literals-and-nullables).
* Konrad Kokosa [explores getting rid of array bound checks, ref-returns, and .NET 5](https://tooslowexception.com/getting-rid-of-array-bound-checks-ref-returns-and-net-5/).
* Mark Seemann [uses EnsureSuccessStatusCode as an assertion](https://blog.ploeh.dk/2020/09/28/ensuresuccessstatuscode-as-an-assertion/).

### 🔧 Tools

* Dominick Baier [writes about the future of IdentityServer](https://leastprivilege.com/2020/10/01/the-future-of-identityserver/).
* Michał Białecki [talks about how not to pass parameters in Entity Framework 5](https://www.michalbialecki.com/2020/09/26/how-not-to-pass-parameters-in-entity-framework-core-5).
* Kevin Logan [writes about transitioning from TFVC to Git](https://www.aligneddev.net/blog/2020/transitioning-from-tfvc-to-git/).
* Nicholas Blumhardt [works with user-defined functions in Serilog expressions](https://nblumhardt.com/2020/10/serilog-expressions-user-defined-functions/), and also [discusses programmable text and JSON formatting for Serilog](https://nblumhardt.com/2020/10/programmable-serilog-formatting/).
* Scott Hanselman [works with dotnet-monitor](https://www.hanselman.com/blog/ExploringYourNETApplicationsWithDotnetmonitor.aspx).
* The Edge team [talks about bringing the browser dev tools experience to Visual Studio Code](https://blogs.windows.com/msedgedev/2020/10/01/microsoft-edge-tools-vscode).
* Sophia Prafina [writes about avoiding Kubernetes anti-patterns](https://www.pulumi.com/blog/kubernetes-anti-patterns/).
* Dmitrij Kovaliov [writes about resiliency in your .NET apps with Polly](https://hackernoon.com/dont-let-your-net-applications-fail-resiliency-with-polly-uz1z3t8t?source=rss).
* Ian Griffiths [minimizes allocations in AIS.NET](https://endjin.com/blog/2020/09/arraypool-vs-memorypool-minimizing-allocations-ais-dotnet.html).
* Dan Clarke [shows how to deal with unrelated changes on feature branches with Git](https://www.danclarke.com/git-unrelated-changes-in-feature-branch).
* James Dawson [streamlines .NET dependency management with NuGet meta packages](https://endjin.com/blog/2020/09/streamline-dependency-management-with-nuget-meta-packages.html).
* David Walsh [shows how to detect the default branch in a Git repository](https://davidwalsh.name/get-default-branch-name).

### 📱 Xamarin

* Jean-Marie Alfonsi [hunts down Xamarin shadows leaks on Android](https://www.sharpnado.com/shadows-leaks/).
* Denys Fiediaiev [creates a custom Xamarin pop-up using MvvmCross](https://medium.com/@prin53/pop-up-xamarin-e2d815441a54).
* Leomaris Reyes [replicates a Sneaker UI app](https://askxammy.com/replicating-sneakers-ui-in-xamarin-forms/).
* Alex Shirshov [shows you to use C# 9 in Xamarin today](https://omnitalented.com/how-to-use-csharp9-with-xamarin-forms-today/).

### 🎤 Podcasts

* Scott Hanselman [talks to Dr. Michaela Greiler about enjoyable code reviews](https://hanselminutes.simplecast.com/episodes/enjoyable-code-reviews-with-dr-michaela-greiler-8NZo1UeJ).
* The Microsoft Cloud Show [recaps Ignite 2020](https://www.microsoftcloudshow.com/podcast/Episodes/378-microsoft-ignite-2020-review).
* The Coding Blocks podcast [continues looking at the DevOps Handbook](https://www.codingblocks.net/podcast/the-devops-handbook-the-value-of-a-b-testing/).
* The 6-Figure Developer podcast [talks about MLOps and ML.NET with Alexander Slotte](https://6figuredev.com/podcast/episode-163-mlops-and-ml-net-with-alexander-slotte/).

### 🎥 Videos

* The IoT Show [talks about connecting sensors to Azure](https://channel9.msdn.com/Shows/Internet-of-Things-Show/Connect-any-IoT-sensor-to-Azure).
* The Visual Studio Toolbox talks about [the Bridge to Kubernetes feature](https://channel9.msdn.com/Shows/Visual-Studio-Toolbox/Bridge-to-Kubernetes).
* The AI Show [automates machine learning on Azure](https://channel9.msdn.com/Shows/AI-Show/Automated-Machine-Learning-on-Azure).
* Azure Friday [goes under the hood with Azure Container Instances (ACI)](https://channel9.msdn.com/Shows/Azure-Friday/Azure-Container-Instances-ACI-under-the-hood).
* The ASP.NET Monsters [discusses static site generators with Khalid Abuhakmeh](https://www.youtube.com/watch?v=MrpNY1c7qiI).
* Data Exposed [begins a series on Infrastructure as Code and Azure](https://channel9.msdn.com/Shows/Data-Exposed/Infrastructure-as-Code-and-Azure--A-Match-Made-in-the-Cloud-Part-1).
