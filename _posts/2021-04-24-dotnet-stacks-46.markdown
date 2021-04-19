---
date: "2021-04-24"
title: "The .NET Stacks #46: 📒 What's new with your favorite IDE"
tags: [dotnet-stacks]
image: /assets/img/stacks-46-card.png 
description: Say something about this issue.
---

![Newsletter image](https://daveabrock.com/THE .NET STACKS.png)

Happy Monday! Here's what we're talking about this week:

- **One big thing**: Catching up on your IDEs
- **The little thing**: More on the Upgrade Assistant, k8s in Microsoft Learn, bUnit news
- Last week in the .NET world

***

# One big thing: Catching up on your IDEs

There's been a lot of news lately on the IDE front. Whether you're using Visual Studio or JetBrains Rider, there have been some big releases that are worth reviewing. (I'm focusing on IDEs here—I realize you can do .NET in VS Code, but that's more of an editor, even if it can sometimes feel like an IDE with the right extensions.)

As for VS, last Wednesday, Microsoft [released Visual Studio 2019 v16.10 Preview 2](https://devblogs.microsoft.com/visualstudio/visual-studio-2019-v16-10-preview-2/). From the .NET perspective (C++ is not part of the .NET family, so I don't mention those), it includes many IntelliSense updates. You'll now see [completions for casts, indexers, and operators](https://devblogs.microsoft.com/visualstudio/visual-studio-2019-v16-10-preview-2/#completions-for-casts-indexers-and-operators), the ability to [automatically insert method call arguments when writing method calls](https://devblogs.microsoft.com/visualstudio/visual-studio-2019-v16-10-preview-2/#automatically-insert-method-call-arguments), a [UI for working with EditorConfig files](https://devblogs.microsoft.com/visualstudio/visual-studio-2019-v16-10-preview-2/#user-interface-for-editorconfig-files), and a [new way to visualize inheritance](https://devblogs.microsoft.com/visualstudio/visual-studio-2019-v16-10-preview-2/#user-interface-for-editorconfig-files).

The release also contains [new features for working with containers](https://devblogs.microsoft.com/visualstudio/visual-studio-2019-v16-10-preview-2/#new-features-for-containers): you can [run services defined from your Docker Compose files](https://devblogs.microsoft.com/visualstudio/visual-studio-2019-v16-10-preview-2/#run-launch-services-defined-in-your-compose-files), and there's also a [new containers tooling window](https://devblogs.microsoft.com/visualstudio/visual-studio-2019-v16-10-preview-2/#advanced-interactions-with-containers-and-images). You'll also see some nice improvements with the Test Explorer. You can finally [view console logs there](https://devblogs.microsoft.com/visualstudio/visual-studio-2019-v16-10-preview-2/#view-console-logs-in-the-test-explorer), [navigate to links from log files](https://devblogs.microsoft.com/visualstudio/visual-studio-2019-v16-10-preview-2/#navigate-links-from-log-files), and [automatically create log files](https://devblogs.microsoft.com/visualstudio/visual-studio-2019-v16-10-preview-2/#automatically-create-log-files).

Finally, Visual Studio has pushed out updates to the Git experience. With v16.10, Visual Studio's status bar now features an enhanced branch picker, a repository picker, and a sync button. You can also select a commit to open an embedded view of its details and the file changes in the Git Repository window without having to navigate to other windows (an excellent way to compare commits). You can learn about the Git changes [in a devoted blog post](https://devblogs.microsoft.com/visualstudio/enhanced-productivity-with-git-in-visual-studio/). With the admission I haven't checked it out in a while, I've found the Git experience not to be very feature-rich. I might have to check it out again.

What about Rider? Earlier this month, [JetBrains wrote about the Rider 2021.1 release](https://blog.jetbrains.com/dotnet/2021/04/08/rider-2021-1-release/). If you work in ASP.NET MVC, Web APIs, or Razor Pages, Rider now features scaffolding for Areas, Controllers, Razor Pages, Views, and Identity. It also generates boilerplate code for EF CRUD operations. Rider now supports ASP.NET Core route templates with inspections to check HTTP syntax errors and quick-fixes to fix route parameter issues.

They've also made some improvements to support the latest C# features: there's increased support for patterns and records, and Rider has a new set of inspections and quick-fixes with records themselves. If you're into tuples, Rider now allows you to use them with its refactoring tooling. They've even started looking at C# 10 and have taught Rider to work with the new "Constant interpolation strings" feature.  

You can check out the [*What's New in Rider* page](https://www.jetbrains.com/rider/whatsnew/) for all the details on their latest release.

***

# The little things: More on the Upgrade Assistant, k8s in Microsoft Learn, bUnit news

Last week, I talked about hot reload in .NET 6. A few of you were kind enough to let me know that you were having issues seeing the GIFs. If you were having problems, I apologize (you can look at the [web version of the newsletter](https://daveabrock.com/2021/04/17/dotnet-stacks-45) and see them in action). I was looking into hot reload for a piece for the Telerik developer blog [that was published last week](https://www.telerik.com/blogs/instant-feedback-is-here-introducing-hot-reload-in-dotnet-6). Check it out to see how you can enable it, how it works, how it handles errors and its limitations.

***

While I'm already doing some shameless plugs, I also [wrote a piece about the .NET Upgrade Assistant](https://www.telerik.com/blogs/meet-dotnet-upgrade-assistant-your-dotnet-5-moving-company). We've mentioned it in passing, but it's a global command-line tool that guides you through migrating your .NET Framework apps to .NET 5. It doesn't do everything for you, as many APIs like `System.Web` isn't available in .NET Core—but it goes a long way in doing the painful bits. It comes with an (optional) accessibility model, which allows you to customize upgrade steps without modifying the tool itself. For example, you can explicitly map NuGet packages to their replacements, add custom template files and add custom upgrade steps. To do this, you include an `ExtensionManifest.json` file.

When I migrated an MVC app, I thought about using it to delete the `App_Start` folder and its contents automatically. It isn't supported (but [is now tracked!](https://github.com/dotnet/upgrade-assistant/issues/374)) as right now, you can only find files. In cases like these, though, I would hope this would be done automatically. But if you're migrating a bunch of projects and know how you want to port over some code, you can customize it yourself. It should save you quite a bit of time.

***

Kubernetes has [now made its way to Microsoft Learn](https://docs.microsoft.com/learn/modules/aks-app-package-management-using-helm/?WT.mc_id=twitter-0000-jeffsand). Microsoft shipped a new module that walks you through application and package management using Helm. You can provision Kubernetes resources from the in-browser Azure Cloud Shell.

Did you think I'd write about Kubernetes and not leave you with a funny joke?

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">A devops engineer walks into a bar, puts the bartender in a docker container, put kubernetes behind the bar, spins up 1000 bartenders, orders 1 beer.</p>&mdash; Ben Burton (@bjburton) <a href="https://twitter.com/bjburton/status/1106908952535728128?ref_src=twsrc%5Etfw">March 16, 2019</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

***

Congrats to Egil Hansen. bUnit, his popular Blazor testing library, is [out of preview and has officially hit v1.0](https://twitter.com/egilhansen/status/1381222883976998914). You can check out the release notes [for the latest release](https://github.com/egil/bUnit/discussions/364).

***

The API proposal for C# 10 interpolated strings is now officially approved. [Check it out on GitHub](https://github.com/dotnet/runtime/issues/50601).

***

# 🌎 Last week in the .NET world

## 🔥 The Top 4

- Andrew Lock [views overwritten configuration values in ASP.NET Core](https://andrewlock.net/viewing-overriden-configuration-values-in-aspnetcore/).
- Jeremy Likness [writes about an easier way to debounce in Blazor](https://blog.jeremylikness.com/blog/an-easier-blazor-debounce/).
- Steve Smith [tests exceptions with XUnit and Actions](https://ardalis.com/testing-exceptions-with-xunit-and-actions/).
- Anthony Giretti [uses fluent integration tests with EF Core and xUnit](https://anthonygiretti.com/2021/04/17/asp-net-core-5-entityframework-core-clean-clear-and-fluent-integration-tests-with-calzolari-testserver-entityframework-fluentassertion-web-and-xunit/).

## 📢 Announcements

- Microsoft releases [Windows Terminal Preview 1.8](https://devblogs.microsoft.com/commandline/windows-terminal-preview-1-8-release) and [Visual Studio 2019 v16.10 Preview 2](https://devblogs.microsoft.com/visualstudio/visual-studio-2019-v16-10-preview-2/?WT.mc_id=DOP-MVP-4025064).
- The Azure SDK team [provides its April update](https://devblogs.microsoft.com/azure-sdk/april-release-2021).

## 📅 Community and events

- Kevin Dockx [updates his Marvin.JsonPatch library](https://www.kevindockx.com/new-release-marvin-jsonpatch-2-2/).
- The Azure Cosmos DB Conf [starts on Tuesday](https://gotcosmos.com/conf).
- JetBrains is [hosting .NET Days on May 11-12](https://blog.jetbrains.com/dotnet/2021/04/14/dotnet-days-online-2021/).
- In community standups: ASP.NET standup [talks to Shaun Walker about Oqtane](https://www.youtube.com/watch?v=livNmRqDnMI), .NET Tooling [talks about .NET Interactive](https://www.youtube.com/watch?v=3hQB_ElxJXU), and Machine Learning [holds office hours](https://www.youtube.com/watch?v=o-WQyQc-rJ0).
- The .NET Docs Show [talks to Rehan Saeed about NuGet packages](https://www.youtube.com/watch?v=A93Fn_qMLX4).

## 🌎 Web development

- Ben Foster [customizes authorization responses in .NET 5.0](https://benfoster.io/blog/customize-authorization-response-aspnet-core/).
- Dave Brock [writes about hot reload in .NET 6](https://www.telerik.com/blogs/instant-feedback-is-here-introducing-hot-reload-in-dotnet-6).
- Matthew Jones [continues writing a Tetris game in Blazor](https://exceptionnotfound.net/tetris-in-blazor-part-4-displaying-the-grid-and-a-falling-tetromino/).
- Khalid Abuhakmeh [generates links to ASP.NET Core map endpoints](https://khalidabuhakmeh.com/generate-links-to-aspnet-core-map-endpoints), and also [adds models to ASP.NET Core](https://khalidabuhakmeh.com/how-to-add-models-to-aspnet-core).
- David Ramel [writes about the new Blazor WebView controls](https://visualstudiomagazine.com/articles/2021/04/09/blazorwebview.aspx).
- Marinko Spasojevic [works with tables in MudBlazor](https://code-maze.com/blazor-material-table-paging-searching-sorting/).
- Damien Bowden [secures Blazor Web Assembly using cookies and Auth0](https://damienbod.com/2021/04/12/securing-blazor-web-assembly-using-cookies-and-auth0/).

## 🥅 The .NET platform

- Nick Randolph [adds behaviors to a WinUI Uno Application](https://nicksnettravels.builttoroam.com/winui-uno-behaviors/).
- Jonathan Allen [writes about .NET 6 async improvements](https://www.infoq.com/news/2021/04/Net6-Async/).
- David Ramel [writes about what's new in .NET 6 Preview 3](https://visualstudiomagazine.com/articles/2021/04/12/maui-desktop.aspx).
- Nick Randolph [restyles controls in an Uno (Windows UI) application](https://nicksnettravels.builttoroam.com/restyling-winui-controls/).
- Laurent Ellerbach [writes about the story of .NET nanoFramework](https://devblogs.microsoft.com/dotnet/show-dotnet-build-your-own-unit-test-platform-the-true-story-of-net-nanoframework).

## 📔 Languages

- Jeff Fritz [writes about C# nullability](https://dev.to/dotnet/my-favorite-c-features-part-3-nullability-2mcg).
- Damir Arh [makes code simpler with C# 9](https://www.dotnetcurry.com/csharp/simpler-code-with-csharp-9).
- Tom Deseyn [writes about C# 9 new features for methods and functions](https://developers.redhat.com/blog/2021/04/13/c-9-new-features-for-methods-and-functions/).

## 🔧 Tools

- Dave Brock [writes about the .NET Upgrade Assistant](https://www.telerik.com/blogs/meet-dotnet-upgrade-assistant-your-dotnet-5-moving-company).
- Mark Downie [debugs managed Linux core dumps with Visual Studio](https://www.poppastring.com/blog/debug-managed-linux-core-dumps-with-visual-studio).
- Munib Butt [uses NDepend in Visual Studio](https://www.c-sharpcorner.com/article/an-introduction-to-using-ndepend-in-visual-studio/).
- Craig Morten [starts a series on his AKS performance journey](https://medium.com/asos-techblog/an-aks-performance-journey-part-1-sizing-everything-up-ee6d2346ea99).
- Chandra Kudumula [uses ML.NET to predict insurance rates](https://www.red-gate.com/simple-talk/cloud/data-science/insurance-price-prediction-using-machine-learning-ml-net/).
- Pratik Nadagouda [writes about using Visual Studio with Git](https://devblogs.microsoft.com/visualstudio/enhanced-productivity-with-git-in-visual-studio).
- Gerard Gallant [deploys C# web applications in Docker](https://platform.uno/blog/deploying-c-web-applications-with-docker/).
- Aaron Stannard [builds headless Akka.NET services with IHostedService](https://petabridge.com/blog/akkadotnet-ihostedservice/).
- Khalid Abuhakmeh [writes about ReSharper & Rider improvements For Avalonia](https://blog.jetbrains.com/dotnet/2021/04/12/improvements-for-resharper-rider-avalonia/).
- Elie Bou Issa [automates Azure DevOps with Logic Apps](https://www.red-gate.com/simple-talk/sysadmin/devops/automating-azure-devops-logic-apps/).

## 📱 Xamarin

- Sam Basu [provides another MAUI update](https://www.telerik.com/blogs/sands-of-maui-issue-4).
- Selva Ganapathy Kathiresan [replicates a Facebook-like UI](https://www.syncfusion.com/blogs/post/replicating-a-facebook-like-ui-in-xamarin.aspx).

## 🏗 Design, testing, and best practices

- Grant Fritchey [explains how microservices architecture changes database deployment](https://www.red-gate.com/blog/how-does-microservices-architecture-change-database-deployment).
- Derek Comartin [organizes microservices](https://codeopinion.com/organizing-commands-events-handlers-in-microservices/).
- Over at Software Alchemy, [scaffolding your clean DDD web app](https://blog.jacobsdata.com/2021/04/11/scaffold-your-clean-ddd-web-application-part-6-domain-driven-design-workflow-patterns).
- Scott Hannen [continues writing about his experiment with making integration tests easier to write](https://scotthannen.org/blog/2021/04/12/integration-test-experiment-2.html).

## 🎤 Podcasts

- The Adventures in .NET podcast [discusses the merits of CI/CD](https://devchat.tv/adventures-in-dotnet/net-064-to-ci-cd-or-not-to-ci-cd/).
- The Coding After Work podcast [catches up with Carl Franklin](http://codingafterwork.com/2021/04/14/episode-58-blazor-podcasting-and-music-with-carl-franklin/).
- The Working Code podcast [discusses feature flags](https://www.bennadel.com/blog/4027-working-code-podcast-episode-018-feature-flags.htm).
- The .NET Rocks podcast [talks to Ian Cooper about TDD in 2021](https://www.dotnetrocks.com/default.aspx?ShowNum=1735).
- The Changelog [talks to Daniel Stenberg about managing curl.](https://www.changelog.com/podcast/436).
- The Azure DevOps Podcast [talks about Azure development](http://azuredevopspodcast.clear-measure.com/paul-yuknewicz-on-azure-development-episode-136).

## 🎥 Videos

- The On .NET Show [secures apps with Microsoft Identity](https://www.youtube.com/watch?v=P25SIYLsH-g), and [target-typed new expressions in C# 9](https://channel9.msdn.com/Shows/On-NET/C-Language-Highlights-Target-Typed-new-expressions).
- The ASP.NET Monsters [deal with truncation with string and binary values](https://www.youtube.com/watch?v=emp1PVEt1X8).
