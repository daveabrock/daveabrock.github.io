---
date: "2020-09-12"
title: "The .NET Stacks #17: Something, something, something"
tags: [dotnet-stacks]
comments: false
---

## EF Core 5 is "done"

## Unit testing is dead, long live unit testing

## September is F#-tember at *The .NET Stacks*

Do you work in F#? Or are you a C# developer and intrigued by its possibilities but haven't found time to dive in? As C# has "borrowed" a lot of functional programming paradigms, you might be wondering about tradeoffs between C# "with functional bits" and straight F#. I've got you covered.

Later this month, I'll be interviewing Isaac Abraham, author of *[Get Programming with F#](https://www.manning.com/books/get-programming-with-f-sharp)*, and Phillip Carter, the PM for F# at Microsoft. Stay tuned for some great conversations.

## 🌎 Last week in the .NET world

### 🔥 The Top 3

* Michael Shpilt [discusses assembly versioning and "DLL hell" in .NET](https://michaelscodingspot.com/dotnet-dll-hell/).
* Dave Brock [walks through CSS isolation in Blazor](https://daveabrock.com/2020/09/10/blazor-css-isolation). Quite the shameless plug, I agree, but I'm proud of it. 😁
* The .NET Docs Show [talks with Jon Skeet](https://www.youtube.com/watch?v=3i8zDtpw-kQ).

### 📢 Announcements

* Nish Anil [announces a free e-book on Blazor for ASP.NET Web Forms developers](https://devblogs.microsoft.com/aspnet/blazor-aspnet-webforms-ebook).
* Timothé Larivière [introduces Fabulous, an OSS framework for mobile and desktop apps using functional programming](https://devblogs.microsoft.com/xamarin/fabulous-functional-app-development).
* Pierre Boulay [discusses how in the latest Windows Insider build, you can attach and mount a physical disk inside WSL 2](https://devblogs.microsoft.com/commandline/access-linux-filesystems-in-windows-and-wsl-2).
* GitHub announced [an integration with Microsoft Teams](https://github.blog/2020-09-10-announcing-the-github-integration-with-microsoft-teams/).
* Rahul Bhandari [announces the release of the .NET Core September 2020 security update](https://devblogs.microsoft.com/dotnet/net-core-september-2020), and Tara Overfield [announces the September 2020 security and quality updates for .NET Framework](https://devblogs.microsoft.com/dotnet/net-framework-september-2020-security-and-quality-rollup-updates).
* The .NET Docs team [shows off what's new from the month of August](https://docs.microsoft.com/en-us/dotnet/whats-new/2020-08).
* Visual Studio Codespaces [is consolidating into GitHub Codespaces](https://devblogs.microsoft.com/visualstudio/visual-studio-codespaces-is-consolidating-into-github-codespaces/).

### 📅 Community and events

We had three community standups: Languages & Runtime [explores miscellaneous topics](https://www.youtube.com/watch?v=XU3-xVtqJy4), Machine Learning [talks SciSharp](https://www.youtube.com/watch?v=ngvR-BNQsJE), and ASP.NET [talks about Microsoft.Identity.Web](https://www.youtube.com/watch?v=hxDli4imREE).

### 😎 Blazor

* Peter Vogel [compares performance options among Telerik DataGrid, JavaScript, and Blazor](https://www.telerik.com/blogs/comparing-performance-telerik-datagrid-javascript-blazor-code), and also [works with local storage in a Blazor PWA](https://visualstudiomagazine.com/articles/2020/09/08/blazor-pwa-local-storage.aspx).
* Karl Shifflett [works with a Blazor WASM GraphQL client](https://oceanware.wordpress.com/2020/09/08/blazor-wasm-graphql-client/).
* Julio Sampaio [gets started with Blazor](https://www.red-gate.com/simple-talk/dotnet/c-programming/first-steps-with-blazor/).

### 🚀 .NET Core

* Nickolas Fisher [uses Okta to migrate an ASP.NET Framework to ASP.NET Core](https://developer.okta.com/blog/2020/09/09/aspnet-migration-dotnet-core).
* Vladimir Pecanac [creates a custom config provider in ASP.NET Core](https://code-maze.com/aspnet-configuration-creating-custom-provider/).
* David Grace asks: [is Entity Framework Core quicker than Dapper?](https://www.roundthecode.com/dotnet/entity-framework/is-entity-framework-core-quicker-than-dapper).
* Michał Białecki [works with views in EF Core 5](http://www.michalbialecki.com/2020/09/09/working-with-views-in-entity-framework-core-5/).
* Tomasz Pęczek [works with HTTP trailers in ASP.NET Core](https://www.tpeczek.com/2020/09/little-known-aspnet-core-features-http.html).
* Mark Heath [migrates from ASP.NET to ASP.NET Core](https://markheath.net/post/migrate-aspnet-core).

### ⛅ The cloud

* Damien Bowden [secures Azure Functions using an Azure virtual network](https://damienbod.com/2020/09/10/securing-azure-functions-using-an-azure-virtual-network/).
* Seth Juarez [fixes mixed-content and CORS issues at ML Model inference time with Azure Functions](https://www.sethjuarez.com/2020/09/10/cors-and-mixed-content-with-azure-functions/).
* Jason Farrell [continues his work on Azure Durable Functions](https://jfarrell.net/2020/09/05/durable-functions-part-4-analyze-and-download/).
* Siva Ramani and Naveen Balaraman [deploy a .NET Core Web API container to Amazon EKS](https://aws.amazon.com/blogs/developer/build-and-deploy-net-core-webapi-container-to-amazon-eks-using-cdk-cdk8s).
* Jamie Maguire [adds speech to a chatbot using the Bot Framework and Azure Speech Services](http://www.jamiemaguire.net/index.php/2020/09/05/adding-speech-capability-to-your-chatbot-using-bot-framework-and-azure-speech-services/).

### 📔 Languages

* Patrick Lioi [talks about evolving an open source test framework with .NET](https://opensource.com/article/20/9/testing-net-fixie).
* Suresh M [talks about understanding C# anonymous types](https://www.syncfusion.com/blogs/post/understanding-csharp-anonymous-types.aspx).
* Lawrence Hecht [talks about how Visual Basic is lingering on](https://thenewstack.io/visual-basic-lingers-on/).
* Thomas Claudius Huber [discusses target-typed new expressions in C# 9](https://www.thomasclaudiushuber.com/2020/09/08/c-9-0-target-typed-new-expressions/).
* Andrew Nosenko [works on async subroutines with C# 8 and IAsyncEnumerable](https://dev.to/noseratio/asynchronous-coroutines-with-c-8-0-and-iasyncenumerable-2e04).

### 🔧 Tools

* Ian Miell [talks about the many ways to undo a commit in Git](https://zwischenzugs.com/2020/09/10/five-ways-to-undo-a-commit-in-git/).
* Andrew Lock [continues his k8s series: this time, configuring resources with YAML manifests](https://andrewlock.net/deploying-asp-net-core-applications-to-kubernetes-part-2-configuring-resources-with-yaml-manifests/).
* Matthew Jones [uses a Dapper base repository in C#](https://exceptionnotfound.net/using-a-dapper-base-repository-in-c-to-improve-readability).
* ErikEJ uses [EF Core Power Tools to rename entities and properties when reverse engineering a database](https://erikej.github.io/efcore/2020/09/07/ef-core-power-tools-renaming-advanced.html).
* Jason Gaylord [adds Azure architecture icons to diagrams](https://www.jasongaylord.com/blog/2020/09/06/azure-architecture-icons).
* Adam Storr [uses Project Tye to run dependent services with ASP.NET Core](https://adamstorr.azurewebsites.net/blog/using-project-tye-to-run-dependent-services-for-use-with-aspnetcore).

### 📱 Xamarin

* Jean-Marie Alfonsi [walks through the TaskLoaderCommand](https://www.sharpnado.com/taskloadercommand-asynccommand/).
* Vicente Gerardo Guzmán Lucio [creates custom renderers for a control](https://www.syncfusion.com/blogs/post/how-to-create-custom-renderers-for-a-control-in-xamarin-forms.aspx).
* James Montemagno [quickly discusses modal navigation](https://devblogs.microsoft.com/xamarin/xamarin-forms-shell-quick-tip-modal-navigation/).

### 🎤 Podcasts

* The .NET Rocks podcast [gets started with Xamarin](https://www.dotnetrocks.com/default.aspx?ShowNum=1704).
* The Xamarin Podcast [talks about the Surface Duo, Android startup times, and Xamarin.Essentials](https://www.xamarinpodcast.com/77).
* The RunAsRadio podcast [gets snarky about the cloud with Corey Quinn](http://www.runasradio.com/default.aspx?ShowNum=724).
* The Stack Overflow Podcast discusses [how developers can become successful writers](https://the-stack-overflow-podcast.simplecast.com/episodes/how-developers-can-become-successful-writers-QkFip8NF).
* The Merge Conflict podcast [discusses Blazor](https://www.mergeconflict.fm/218).
* The .NET Core Podcast [talks about IoT and .NET Core with Pete Gallagher](https://dotnetcore.show/episode-59-iot-and-net-core-with-pete-gallagher/).
* The Loosely Coupled Show [talks about loosely-coupled monoliths](https://www.youtube.com/watch?v=xODJrIYDxA4).

### 🎥 Videos

* The Visual Studio Toolbox [talks about the new Git experience](https://channel9.msdn.com/Shows/Visual-Studio-Toolbox/The-New-Git-Experience).
* Data Exposed [talks about Azure SQL Edge](https://channel9.msdn.com/Shows/Data-Exposed/Azure-SQL-Edge-Demo-Renewable-Energy).
* At the Maintainers, [Shawn Wildermuth talks with Dennis Doomen about FluentAssertions](http://wildermuth.com/2020/09/10/The-Maintainers-Dennis-Doomen-and-FluentAssertions).
* The AI Show [discusses OSS framework support in the Azure Machine Learning service](https://channel9.msdn.com/Shows/AI-Show/OSS-Framework-Support-in-Azure-Machine-Learning-Service).
* Data Exposed [works with Power BI and Azure Synapse](https://channel9.msdn.com/Shows/Data-Exposed/Analyzing-your-Data-Lake-with-Power-BI-and-Azure-Synapse).
* The ON.NET Show [uses .NET microservices with Steeltoe](https://channel9.msdn.com/Shows/On-NET/NET-Microservices-with-Steeltoe).
