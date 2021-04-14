---
date: "2021-04-17"
title: "The .NET Stacks #45: 🔥 At last, hot reload is (initially) here"
tags: [dotnet-stacks]
image: /assets/img/stacks-45-card.png 
description: This week, we discuss .NET 6 Preview 3 and some C# updates.
---

![Newsletter image](https://daveabrock.com/THE .NET STACKS.png)

Happy Monday! Here's what we're talking about this week:

- **One big thing**: .NET 6 Preview 3 is here, and so is hot reload
- **The little thing**: C# updates
- Last week in the .NET world

***

# .NET 6 Preview 3 is here, and so is hot reload

On Thursday, Microsoft rolled out .NET 6 Preview 3. Richard Lander [has the announcement covered](https://devblogs.microsoft.com/dotnet/announcing-net-6-preview-3/). As he notes in the post, the release is "dedicated almost entirely to low-level performance features." For example, we've got [faster handling of structs as Dictionary values](https://devblogs.microsoft.com/dotnet/announcing-net-6-preview-3/#faster-handling-of-structs-as-dictionary-values), [faster interface checking and casting thanks to the use of pattern matching](https://devblogs.microsoft.com/dotnet/announcing-net-6-preview-3/#faster-interface-checking-and-casting), and [code generation improvements](https://devblogs.microsoft.com/dotnet/announcing-net-6-preview-3/#runtime-codegen). The team [resolved an issue](https://devblogs.microsoft.com/nuget/net-5-nuget-restore-failures-on-linux-distributions-using-nss-or-ca-certificates/) where NuGet restore failed on Linux thanks to previous certificate issues. There were also updates to [EF Core](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/6.0.0-preview.3.21201.2) and [ASP.NET Core](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/6.0.0-preview.3.21201.2).

Oh, and [initial hot reload support](https://devblogs.microsoft.com/aspnet/asp-net-core-updates-in-net-6-preview-3/#initial-net-hot-reload-support) is finally here.

Hot reload isn't just for Blazor developers to enjoy—it's built into the .NET 6 runtime. With Preview 3, you can use it by running `dotnet watch` in your terminal with ASP.NET Core web apps—Razor Pages, MVC, and Blazor (Server and WebAssembly). In future updates, you'll enjoy Visual Studio support and use it with other project types like mobile, console apps, and client and mobile apps.

It's been highly requested, and for a good reason—front-end frameworks and libraries based on interpreted languages like JS have enjoyed this for the last five years, and it's a fantastic productivity booster. It's easy to rag on Microsoft for taking this long—but they've had more significant problems during this time. Five years ago, Microsoft was preparing the roll out of the first version of .NET Core, and a component library like Blazor was just a thought, if at all. Now, the runtime is ready to address these issues (as it's [a main goal](https://github.com/dotnet/core/issues/5510) of .NET 6).

I tried out hot reload with Blazor Server this weekend (a blog post is coming). I'll include some GIFs that show what it's like.

Here's basic editing of static text:

![]({{ site.url }}{{site.baseurl}}/assets/img/hr-hello-friends.gif)

Here's what happens when I update C# code:

![]({{ site.url }}{{site.baseurl}}/assets/img/hr-increment-by-5.gif)

In the following example, you'll see here that it preserves state. When I change the `currentCount` value, the state of the component is maintained. I'll need to refresh the page to see the new `currentCount`.

![]({{ site.url }}{{site.baseurl}}/assets/img/hr-preserve-state.gif)

Here's where I put it all together by dropping in components (with independent state) and editing some CSS for good measure.

![]({{ site.url }}{{site.baseurl}}/assets/img/hr-put-it-together.gif)

However, [not all code actions are supported](https://docs.microsoft.com/visualstudio/debugger/supported-code-changes-csharp). When this happens—like renaming a method—it will revert to current `dotnet watch` behavior by recompiling and refreshing your page with the latest bits.

![]({{ site.url }}{{site.baseurl}}/assets/img/hr-method-rename.gif)

If you have runtime errors, a banner displays at the top with the appropriate information. When you resolve your issues, the app will recompile and refresh.

![]({{ site.url }}{{site.baseurl}}/assets/img/hr-build-error.gif)

On the subject of Blazor, Preview 3 [ships with a `BlazorWebView` control](https://devblogs.microsoft.com/aspnet/asp-net-core-updates-in-net-6-preview-3/#blazorwebview-controls-for-wpf-windows-forms). This allows WPF and Windows Forms developers to embed Blazor functionality into existing .NET 6 desktop apps.

***

# The little thing: C# updates

Last week, Bill Wagner [announced open-source C# standardization](https://devblogs.microsoft.com/dotnet/announcing-open-source-c-standardization-standards/). In addition to the compiler work repo (in dotnet/roslyn) and the repo for C# language evolution (dotnet/csharplang), there is now a new repo (dotnet/csharpstandard) dedicated to documenting the standard for the latest C# language versions.

The new repo sits under the .NET Foundation, and as Wagner states:

>Moving the standards work into the open, under the .NET Foundation, makes it easier for standardization work. Everything from language innovation and feature design through implementation and on to standardization now takes place in the open. It will be easier to ask questions among the language design team, the compiler implementers, and the standards committee. Even better, those conversations will be public ... The end result will be a more accurate standard for the latest versions of C#.

If you're having trouble distinguishing between dotnet/csharplang and dotnet/csharpstandard, [you aren't alone](https://devblogs.microsoft.com/dotnet/announcing-open-source-c-standardization-standards/#comment-8901). Bill Wagner notes that [there's some overlap between the repos](https://devblogs.microsoft.com/dotnet/announcing-open-source-c-standardization-standards/#comment-8902), and it's a work in progress.

Speaking of C#, it's nice to check out any [language proposals](https://github.com/dotnet/csharplang/tree/main/proposals) from time-to-time, and [file-scoped namespaces](https://github.com/dotnet/csharplang/blob/main/proposals/csharp-10.0/file-scoped-namespaces.md) is making progress as a C# 10 proposal.

***

# 🌎 Last week in the .NET world

## 🔥 The Top 3

- Richard Lander [announces .NET 6 Preview 3](https://devblogs.microsoft.com/dotnet/announcing-net-6-preview-3), and Dan Roth [shows off the ASP.NET Core updates in .NET 6 Preview 3](https://devblogs.microsoft.com/aspnet/asp-net-core-updates-in-net-6-preview-3).
- The .NET Docs Show [has a C# roundtable](https://www.youtube.com/watch?v=U6cwOzUqjxY) with C# gods Mads Torgersen, Bill Wagner, and Jon Skeet.
- The NuGet team addresses [restore failures on Linux distributions using NSS or ca-certificates](https://devblogs.microsoft.com/nuget/net-5-nuget-restore-failures-on-linux-distributions-using-nss-or-ca-certificates), and Nikolche Kolev [writes about NuGet performance improvements](https://devblogs.microsoft.com/visualstudio/performance-improvements-in-nuget).

## 📢 Announcements

- Microsoft announces [a preview of their own OpenJDK build](https://devblogs.microsoft.com/java/announcing-preview-of-microsoft-build-of-openjdk).
- Jorge Garcia Hirota [announces GA client libraries for Azure Communication Services](https://devblogs.microsoft.com/azure-sdk/communication-services-ga).
- Dapr v1.1.0 [is now available](https://blog.dapr.io/posts/2021/04/02/dapr-v1.1.0-is-now-available/).
- The AWS SDK for .NET version 1 [has reached the end of support](https://aws.amazon.com/blogs/developer/aws-sdk-for-net-version-1-has-reached-the-end-of-support/).
- Andrew Lock [announces the second edition of his book, ASP.NET Core in Action](https://andrewlock.net/my-new-book-aspnetcore-in-action-2e-is-available-now/).
  
## 📅 Community and events

- Bill Wagner [announces open-source C# standardization](https://devblogs.microsoft.com/dotnet/announcing-open-source-c-standardization-standards).
- For community standups: Languages & Runtime [discusses C# standardization](https://www.youtube.com/watch?v=Rksr7XzyOPA), Entity Framework [talks about Azure SQL](https://www.youtube.com/watch?v=GhIhwCafilk), and ASP.NET [discusses Microsoft Identity](https://www.youtube.com/watch?v=kF-su6ejbBI).

## 🌎 Web development

- Kevin W. Griffin asks: [does SignalR guarantee message deliverability?](https://consultwithgriff.com/signalr-message-guarantee-deliverability/)
- Claudio Bernasconi [works on static images in Blazor](https://www.claudiobernasconi.ch/2021/04/07/blazor-static-images/).
- Dave Brock [works with DynamicComponent in Blazor](https://daveabrock.com/2021/04/08/blazor-dynamic-component).
- Niels Swimberghe [creates ZIP files on HTTP request without intermediate files using the ASP.NET MVC framework](https://swimburger.net/blog/dotnet/create-zip-files-on-http-request-without-intermediate-files-using-aspdotnet-mvc-framework).
- Matthew Jones [continues building his Tetris app in Blazor](https://exceptionnotfound.net/tetris-in-blazor-part-3-tetrominos/).
- Damien Bowden [creates verifiable credentials in ASP.NET Core for decentralized identities using Trinsic](https://damienbod.com/2021/04/05/creating-verifiable-credentials-in-asp-net-core-for-decentralized-identities-using-trinsic/).
- Marinko Spasojevic [creates a Blazor nav menu using Material UI](https://code-maze.com/creating-blazor-material-navigation-menu/).
- Sam Xu [writes about attribute routing in ASP.NET Core OData 8.0 RC](https://devblogs.microsoft.com/odata/attribute-routing-in-asp-net-core-odata-8-0-rc).
- Kristoffer Strube [troubleshoots a 404 issue when deploying Blazor Web Assembly to GitHub Pages](https://blog.elmah.io/blazor-wasm-404-error-and-fix-for-github-pages/).

## 🥅 The .NET platform

- David Hayden [installs .NET on Ubuntu](https://www.davidhayden.me/blog/install-net5-on-ubuntu-20-04).
- Nick Randolph [gets started with the Uno platform](https://nicksnettravels.builttoroam.com/getting-started-uno-platform/).
- Peter Vogel [writes about moving to desktop applications in .NET 5](https://www.telerik.com/blogs/moving-to-desktop-applications-dotnet-core-dotnet-5).
- Andrea Chiarelli [writes about secrets management in .NET apps](https://auth0.com/blog/secret-management-in-dotnet-applications/).
- Sergey Tihon [configures dotnet watch with Microsoft.Identity.Web or custom IDistributedCache](https://sergeytihon.com/2021/04/05/dotnet-watch-with-microsoft-identity-web-or-custom-idistributedcache/).
- Tom Deseyn [writes about C# 9 pattern matching](https://developers.redhat.com/blog/2021/04/06/c-9-pattern-matching/).

## ⛅ The cloud

- GitHub explains [their new authentication token formats](https://github.blog/2021-04-05-behind-githubs-new-authentication-token-formats/), and also walks us through [how they scaled the GitHub API with a rate limiter in Redis](https://github.blog/2021-04-05-how-we-scaled-github-api-sharded-replicated-rate-limiter-redis/).
- John Bohannon [shows you how to write your first GitHub Action](https://devblogs.microsoft.com/devops/building-your-first-github-action).
- Adam Storr [works through why Azure Functions doesn't load his dependencies](https://adamstorr.azurewebsites.net/blog/azure-functions-not-loading-my-dependencies-what-have-i-missed).
- Jimmy Bogard [writes about local development with the Azure Service Bus](https://jimmybogard.com/local-development-with-azure-service-bus/).
- Paul Michaels [writes about batching and pre-fetching with Azure Service Bus](https://www.pmichaels.net/2021/04/03/service-bus-batching-and-pre-fetch/).
- Laurent Kempé [accesses the Dapr secrets building block using the Dapr .NET SDK](https://laurentkempe.com/2021/04/06/accessing-dapr-secrets-building-block-using-dapr-dotnet-sdk/).

## 🔧 Tools

- Michael Washington [creates Power BI paginated reports with Blazor](https://blazorhelpwebsite.com/ViewBlogPost/49).
- Khalid Abuhakmeh [writes about recursive Data With Entity Framework Core and SQL Server](https://khalidabuhakmeh.com/recursive-data-with-entity-framework-core-and-sql-server) and also [models SQL relationships in EF Core](https://khalidabuhakmeh.com/modeling-most-sql-relationships-in-entity-framework-core).
- Mark Heath [deploys an Azure Function app with Bicep](https://markheath.net/post/azure-functions-bicep).
- Scott Hanselman [adds more icons to Windows Terminal](https://www.hanselman.com/blog/take-your-windows-terminal-and-powershell-to-the-next-level-with-terminal-icons).

## 📱 Xamarin

- Leomaris Reyes [gets started with CollectionView](https://blog.logrocket.com/getting-started-with-collectionview-in-xamarin-forms/).
- Sam Basu [provides another MAUI update](https://www.telerik.com/blogs/sands-of-maui-issue-3).

## 🏗 Design, testing, and best practices

- Jay Krishna Reddy [unit tests using XUnit And Moq in ASP.NET Core](https://www.c-sharpcorner.com/article/unit-testing-using-xunit-and-moq-in-asp-net-core/).
- Derek Comartin [develops smarter SPAs with REST APIs](https://codeopinion.com/smarter-single-page-application-with-a-rest-api/).
- Scott Hannen [experiments experiments with making integration tests easier to write](https://scotthannen.org/blog/2021/04/07/integration-test-experiment-1.html).
- Patrick Smacchia [implements a domain with POCOs](https://blog.ndepend.com/implementing-a-domain-with-poco-plain-old-clr-objects/).
- Nish Anil [answers .NET microservices questions](https://devblogs.microsoft.com/aspnet/your-top-dotnet-microservices-questions-answered).
- Tobias Günther [explains how branches work in Git](https://stackoverflow.blog/2021/04/05/a-look-under-the-hood-how-branches-work-in-git/).

## 🎤 Podcasts

- The 6-Figure Developer podcast [talks to Sean Whitesell about microservices](https://6figuredev.com/podcast/episode-190-microservices-with-sean-whitesell/).
- Scott Hanselman [talks to Jean Yang about API observability](https://hanselminutes.simplecast.com/episodes/jean-yang-KS0GgL4x).
- The Azure Podcast [talks about cloud-native machine learning](http://azpodcast.azurewebsites.net/post/Episode-371-Cloud-Native-Machine-Learning), and also [talks about the API Management service](http://azpodcast.azurewebsites.net/post/Episode-372-API-Management).
- The Adventures in .NET podcast [discusses use cases for GraphQL in .NET
](https://devchat.tv/adventures-in-dotnet/net-063-use-cases-for-graphql-in-net/).
- The Complete Developer podcast [talks about state machines](https://completedeveloperpodcast.com/state-machines/).
- The Merge Conflict podcast [talks about satisfying business requirements](https://www.mergeconflict.fm/248).

## 🎥 Videos

- The On .NET Show [talks about .NET MAUI](https://channel9.msdn.com/Shows/On-NET/A-Journey-to-NET-MAUI), and also [creates .NET project templates](https://www.youtube.com/watch?v=H_pqfeRgTYw).
- The AzureFunBytes stream [talks to Mark Brown about Cosmos DB](https://devblogs.microsoft.com/devops/azurefunbytes-intro-to-cosmos-db-with-mark-brown).
- Steve Collins [talks to JetBrains about dependency injection](https://blog.jetbrains.com/dotnet/2021/04/09/net-5-dependency-injection-webinar-recording/).
- The Xamarin Show [discusses XAML hot reload updates](https://channel9.msdn.com/Shows/XamarinShow/XAML-Hot-Reload-Updates--Xamarin-Show).
- At Technology and Friends, [David Giard talks to Ted Neward about technology culture](https://www.davidgiard.com/2021/04/05/TedNewardOnTechnologyCulture.aspx).
- Data Exposed [talks about migrating databases to Azure](https://channel9.msdn.com/Shows/Data-Exposed/Get-Started-with-the-New-Database-Migration-Guides-to-Migrate-Your-Databases-to-Azure).
