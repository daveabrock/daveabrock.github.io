---
date: "2021-03-20"
title: "The .NET Stacks #41: 🎁 Your monthly preview fix has arrived"
tags: [dotnet-stacks]
image: /assets/img/stacks-41-card.png 
description: This week, Ignite wraps and we get a new upgrade assistant.
---

![Newsletter image](https://daveabrock.com/THE .NET STACKS.png)

Did you know [e-mail turns 50 this year](https://www.thestar.com/opinion/star-columnists/2021/03/13/email-anniversary-offers-moment-to-reimagine-a-more-productive-less-stressful-workplace-culture.html?utm_source=Twitter&utm_medium=SocialMedia&utm_campaign=OpinionStaff&utm_content=emailanniversary)? Do you remember when in 2004, Bill Gates [pledged to rid the world of spam emails](https://www.theguardian.com/technology/2004/jan/25/billgates.spam) in two years?

- **One big thing**: .NET 6 Preview 2 is here
- **The little things**: Azure Functions .NET 5 support goes GA, new .NET APIs coming, event sourcing examples
- Last week in the .NET world

# One big thing: .NET 6 Preview 2 is here

Last week, [Microsoft released .NET Preview 2](https://devblogs.microsoft.com/dotnet/announcing-net-6-preview-2). From here on out, the .NET team will ship a new preview every month until .NET 6 goes live in November 2021. As a reminder, .NET 6 is an LTS release, meaning Microsoft will support it for three years.

A big focus for .NET 6 is improving the inner loop experience and performance: maximizing developer productivity by optimizing the tools we use. While it's early [Stephen Toub writes](https://devblogs.microsoft.com/dotnet/announcing-net-6-preview-2/#theme-improve-net-inner-loop-performance) that the team has trimmed overheads when running `dotnet new`, `dotnet build`, and `dotnet run`. This includes fixing issues where tools were unexpectedly JIT'ing and changing the ASP.NET Razor compiler to use a Roslyn source generator to avoid extra compilation steps. A chart of the drastic build time improvements shows a clean build of Blazor Server decreasing from over 3600 milliseconds to around 1500 milliseconds. If you bundle this with a best-in-class hot reload capability, developers have a lot to be excited about with .NET 6.

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">The crazy wins that <a href="https://t.co/kdc3WLDiTQ">https://t.co/kdc3WLDiTQ</a> Razor is getting by flipping their build to use source generators <a href="https://t.co/woe3LGe8kS">pic.twitter.com/woe3LGe8kS</a></p>&mdash; Jared Parsons (@jaredpar) <a href="https://twitter.com/jaredpar/status/1370133973112778753?ref_src=twsrc%5Etfw">March 11, 2021</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

In .NET 6, you'll be [hearing a lot about MAUI](https://devblogs.microsoft.com/dotnet/announcing-net-6-preview-2/#net-multi-platform-app-ui) (Multi-Platform App UI), which is the next iteration of Xamarin.Forms. With MAUI, Xamarin developers can use the latest .NET SDKs for the apps they build. If you aren't a Xamarin developer, you can still take advantage of MAUI: for example, using MAUI Blazor apps can run natively on Windows and macOS machines. With Preview 2, Microsoft enabled a single-project experience that reduces overhead when running Android, iOS, and macOS apps.

While .NET library improvements don't come with the same fanfare, a couple of improvements caught my eye. The `System.Text.Json` library now has a `ReferenceHandler.IgnoreCycles` option that allows you to ignore cycles when you serialize a complex object graph—a [big step for web developers](https://devblogs.microsoft.com/dotnet/announcing-net-6-preview-2/#system-text-json-referencehandler-ignorecycles). Little by little, Microsoft's [Newtonsoft comparison chart](https://docs.microsoft.com/dotnet/standard/serialization/system-text-json-migrate-from-newtonsoft-how-to?pivots=dotnet-5-0#table-of-differences-between-newtonsoftjson-and-systemtextjson) is getting better. Additionally, a new `PriorityQueue<TElement, TPriority>` collection enables you to [add new items with a value and a priority](https://devblogs.microsoft.com/dotnet/announcing-net-6-preview-2/#priorityqueue).

Aside from the Razor performance improvements, ASP.NET Core now has [support for custom event arguments in Blazor](https://devblogs.microsoft.com/aspnet/asp-net-core-updates-in-net-6-preview-2/#support-for-custom-event-arguments-in-blazor), [CSS isolation for MVC and Razor Pages](https://devblogs.microsoft.com/aspnet/asp-net-core-updates-in-net-6-preview-2/#css-isolation-for-mvc-views-and-razor-pages), and the ability to [preserve prerendered state in Blazor apps](https://devblogs.microsoft.com/aspnet/asp-net-core-updates-in-net-6-preview-2/#preserve-prerendered-state-in-blazor-apps). (And no, AOT is not ready yet—the comment *CTRL + F “AOT” 0 results, closes the tab* from the last preview [is the hardest I've laughed in a while](https://devblogs.microsoft.com/dotnet/announcing-net-6-preview-1/#comment-8437).) In Entity Framework Core land, the team is working on preserving the synchronization context in `SaveChangesAsync`, [flexible free-text search](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-6-0-preview-2/#more-flexible-free-text-search), and [smoother integration with `System.Linq.Async`](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-6-0-preview-2/#smoother-integration-with-system-linq-async).

If you want the full details on what's new in Preview 2, [Richard Lander has it all](https://devblogs.microsoft.com/dotnet/announcing-net-6-preview-2). You can also check out [what's new in ASP.NET Core](https://devblogs.microsoft.com/aspnet/asp-net-core-updates-in-net-6-preview-2) and [Entity Framework Core as well](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-6-0-preview-2).

# The little things: Azure Functions .NET 5 support goes GA, new .NET APIs coming, event sourcing examples

I've mentioned this a few times before, but the Azure Functions team has developed a new out-of-process worker that will help with quicker support of .NET versions. Here's the gist of why they are decoupling a worker from the host, [from Anthony Chu](https://techcommunity.microsoft.com/t5/apps-on-azure/net-on-azure-functions-roadmap/ba-p/2197916):

>The Azure Functions host is the runtime that powers Azure Functions and runs on .NET and .NET Core. Since the beginning, .NET and .NET Core function apps have run in the same process as the host. Sharing a process has enabled us to provide some unique benefits to .NET functions, most notably is a set of rich bindings and SDK injections. ... However, sharing the same process does come with some tradeoffs. In earlier versions of Azure Functions, dependencies could conflict. While flexibility of packages was largely addressed in Azure Functions V2 and beyond, there are still restrictions on how much of the process is customizable by the user. Running in the same process also means that the .NET version of user code must match the .NET version of the host. These tradeoffs of sharing a process motivated us to choose an out-of-process model for .NET 5.

A few weeks ago, [I wrote about](https://daveabrock.com/2021/02/24/functions-dotnet-5) how you can use this with .NET 5 today, but it was very much in preview and involved a lot of manual work. This week, it became production-ready. .NET 6 will support both the in-process and out-of-process options so that .NET Core 3.1 can remain supported. Long term (for .NET 7 and beyond), the team hopes to move new feature capabilities to the out-of-process worker completely—allowing support of .NET 7 on Day 1 from only the isolated worker.

***

The .NET team [approved a proposal for a new Timer API](https://github.com/dotnet/runtime/issues/31525). As ASP.NET Core architect David Fowler notes in the issue, the current implementation can incur overlapping callbacks (which aren't async). Additionally, he notes it always captures the execution context, which can cause problems for long-lived operations. How will this new API help? It pauses when user code is executing while resuming the next period when it ends, can be stopped using a `CancellationToken`, and doesn't capture an execution context. If you're only firing a timer once, `Task.Delay` will continue to be a better option.

Also, .NET is [getting a new `Task.WaitAsync` API](https://github.com/dotnet/runtime/issues/47525#issuecomment-794272772). This appears to replace `WhenAny`, allowing Microsoft to assure us that naming things is still the hardest part of computer science. I say call it `WhenAsync`. If you look at the name hard enough after a few drinks, `WhenAny` is still there.

***

If you're into event sourcing, check out [Oskar Dudycz's EventSourcing.NetCore repo](https://github.com/oskardudycz/EventSourcing.NetCore). It's full of nice tutorials and practical examples. That's all.

***

Lastly, a nice tip on how you can tell when a site's static file was last updated (even if several factors can make this unpredictable).

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">Did you know web servers tell you when a static file on a website was last updated? There&#39;s a LOT of caveats since many things can influence this timestamp, do NOT blindly trust it, but it can be helpful.<br><br>F12 &gt; Network &gt; Select file in list &gt; Headers<br><br>Look for &quot;last-modified&quot; <a href="https://t.co/8WGRyfQeGe">pic.twitter.com/8WGRyfQeGe</a></p>&mdash; SwiftOnSecurity (@SwiftOnSecurity) <a href="https://twitter.com/SwiftOnSecurity/status/1370586569397186563?ref_src=twsrc%5Etfw">March 13, 2021</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

***
# 🌎 Last week in the .NET world

## 🔥 The Top 3

- Richard Lander [announces .NET 6 Preview 2](https://devblogs.microsoft.com/dotnet/announcing-net-6-preview-2). Daniel Roth [writes about ASP.NET Core updates](https://devblogs.microsoft.com/aspnet/asp-net-core-updates-in-net-6-preview-2) and Jeremy Likness [updates us on Entity Framework Core](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-6-0-preview-2).
- Elton Stoneman [runs containers in Azure VM Scale Sets instead of using Kubernetes](https://blog.sixeyed.com/you-cant-always-have-kubernetes-running-containers-in-azure-vm-scale-sets/).
- Sudhir Jonathan [writes a big little guide to message queues](https://sudhir.io/the-big-little-guide-to-message-queues/).

## 📢 Announcements

- David Ramel [writes about MAUI support with .NET 6 Preview 2](https://visualstudiomagazine.com/articles/2021/03/11/net-6-preview-2.aspx).
- Angelos Petropoulos [writes about what's new with GitHub Actions tooling in Visual Studio](https://devblogs.microsoft.com/visualstudio/whats-new-with-github-actions-tooling-in-visual-studio).
- Anthony Chu [provides an update on .NET Azure Functions support](https://techcommunity.microsoft.com/t5/apps-on-azure/net-on-azure-functions-roadmap/ba-p/2197916).

## 📅 Community and events

- For community standups: the EF team [talks to Julie Lerman](https://www.youtube.com/watch?v=oZVsZrFKp48), ASP.NET [discusses contributing to Blazor](https://www.youtube.com/watch?v=gRg0xxK8L6w), and the Languages & Runtime team [discusses the C# design process](https://www.youtube.com/watch?v=lzyWFew_w8Y).
- The .NET Docs Show [talks with Yair Halberstadt about compile-time DI](https://www.youtube.com/watch?v=8cyumKVEth0).

## 🌎 Web development

- Claudio Bernasconi [works with Blazor form validation](https://www.claudiobernasconi.ch/2021/03/10/blazor-form-component-validation/).
- Madhu Sudhanan P [builds a Blazor app with Dapper](https://www.syncfusion.com/blogs/post/build-blazor-crud-application-with-dapper.aspx).
- Rick Strahl [writes about role-based JWT tokens in ASP.NET Core](https://weblog.west-wind.com/posts/2021/Mar/09/Role-based-JWT-Tokens-in-ASPNET-Core), and Jason Farrell [performs manual JWT validation in .NET Core](https://jfarrell.net/2021/03/09/manual-jwt-validation-in-net-core/).
- The Code Maze blog [introduces benchmarking C# and ASP.NET Core projects](https://code-maze.com/benchmarking-csharp-and-asp-net-core-projects/).
- Khalid Abuhakmeh [answers a question about working with data in an HTTP API](https://khalidabuhakmeh.com/community-question-working-with-data-in-an-http-api), and also [hosts two ASP.NET Core apps in one host](https://khalidabuhakmeh.com/hosting-two-aspnet-core-apps-in-one-host).
- Damien Bowden [secures Blazor WebAssembly using cookies](https://damienbod.com/2021/03/08/securing-blazor-web-assembly-using-cookies/).
- Jeremy Miller [uses Alba to integration test ASP.NET services](https://jeremydmiller.com/2021/03/09/using-alba-to-test-asp-net-services/).
- Sam Basu [works on a real-time WinUI dashboard with  a SignalR backend](https://www.telerik.com/blogs/real-time-winui-dashboard-with-signalr-backend).
- Matthew Jones [wraps up building a blackjack game in Blazor](https://exceptionnotfound.net/blackjack-in-blazor-part-4-putting-it-all-together/).
- Aleksandr Hovhannisyan [writes about why he doesn't like Tailwind](https://www.aleksandrhovhannisyan.com/blog/why-i-dont-like-tailwind-css/).

## 🥅 The .NET platform

- Mark Downie [writes about the rundown provider](https://www.poppastring.com/blog/why-we-need-the-rundown-provider).
- David Ramel [writes about the new .NET Upgrade Assistant](https://visualstudiomagazine.com/articles/2021/03/10/upgrade-assistant.aspx).
- Arthur Casals [writes about Project Reunion 0.5 Preview](https://www.infoq.com/news/2021/03/msft-project-reunion-05-preview/).
- Konrad Kokosa [shows .NET debugging in a single picture](https://tooslowexception.com/net-debugging-in-a-single-picture/).

## ⛅ The cloud

- Anthony Chu [writes about running Node.js 14 with Azure Functions](https://techcommunity.microsoft.com/t5/apps-on-azure/run-node-js-14-in-azure-functions/ba-p/2195063?WT.mc_id=DOP-MVP-4025064).
- James Randall [compares performance between AWS Lambda and Azure Functions](https://www.azurefromthetrenches.com/comparative-performance-of-azure-functions-and-aws-lambda/).
- Scott Hanselman [starts using Azure Static Web Apps](https://www.hanselman.com/blog/penny-pinching-in-the-cloud-azure-static-web-apps-are-saving-me-money).
- Johnny Reilly [uses Managed Identity, Azure SQL and Entity Framework](https://blog.johnnyreilly.com/2021/03/managed-identity-azure-sql-and-entity.html).

## 📔 Languages

- Jiří Činčura [shows off ConfigureAwaitChecker with support for “await using” and “await foreach” ](https://www.tabsoverspaces.com/233854-configureawaitchecker-with-support-for-await-using-and-await-foreach).
- Niels Swimberghe [downloads the right ChromeDriver version and uses C# to keep it up to date on Windows, Linux, and macOS](https://swimburger.net/blog/dotnet/download-the-right-chromedriver-version-and-keep-it-up-to-date-on-windows-linux-macos-using-csharp-dotnet).
- Thomas Claudius Huber [writes about covariant return types in C# 9](https://www.thomasclaudiushuber.com/2021/03/11/c-9-0-covariant-return-types/).
- Andrea Chiarelli [writes about five C# features you might not know about](https://auth0.com/blog/five-csharp-features-you-dont-know/).
- Jeff Fritz [writes about LINQ](https://dev.to/dotnet/my-favorite-c-features-part-2-linq-57kd).
- Mark Seemann [uses sealed by default, for C# classes](https://blog.ploeh.dk/2021/03/08/pendulum-swing-sealed-by-default/).
- Munib Butt [writes about pattern matching in C#](https://www.c-sharpcorner.com/article/pattern-matching-in-c-sharp/).
- Jason Roberts [writes about default interface implementations in C#](http://dontcodetired.com/blog/post/ICYMI-C-8-New-Features-Upgrade-Interfaces-Without-Breaking-Existing-Code).

## 🔧 Tools

- Scott Hanselman [shows off the GitHub command line](https://www.hanselman.com/blog/dont-forget-about-the-github-command-line).
- Richard Lander [uses blinking APIs with Raspberry Pi and .NET](https://devblogs.microsoft.com/dotnet/blinking-leds-with-raspberry-pi).
- Check out [Oh My Git, a fun way to learn the ins and outs of Git](https://ohmygit.org/).
- Mislav Marohnić [scripts with the GitHub CLI](https://github.blog/2021-03-11-scripting-with-github-cli/).
- Andrew Lock [installs Docker Desktop for Windows and WSL 2](https://andrewlock.net/installing-docker-desktop-for-windows/).
- Imar Spaanjaars [sets up a CI pipeline in Azure DevOps to build and test code](https://imar.spaanjaars.com/621/building-and-auto-deploying-an-aspnet-core-application-part-4-setting-up-a-ci-pipeline-in-azure-devops-to-build-and-test-your-code).
- Oskar Duducz [has a GitHub repo showing off examples and resources for using event sourcing in .NET Core](https://github.com/oskardudycz/EventSourcing.NetCore).
- Regis Wilson asks: [why is Kubernetes so hard?](https://dev.to/rwilsonrelease/why-is-kubernetes-so-hard-i42).
- Neel Bhatt [takes a look at InferSharp](https://neelbhatt.com/2021/03/07/first-look-at-infersharp-a-c-version-of-facebooks-infer/).
- Laurent Kempé [gets started with Dapr for .NET developers](https://laurentkempe.com/2021/03/09/getting-started-with-dapr-for-dotnet-developers/).
- Thomas Ardal [shows off 6 free tools for .NET developers](https://blog.elmah.io/6-free-tools-for-net-developers/).

## 📱 Xamarin

- Nick Randolph [converts Xamarin.Forms to WinUI and Uno](https://nicksnettravels.builttoroam.com/uno-safe-area/).
- Selva Ganapathy Kathiresan [writes about the advantages of using MAUI over Xamarin](https://www.syncfusion.com/blogs/post/advantages-net-maui-over-xamarin.aspx).

## 🏗 Design, testing, and best practices

- Uduak Obong-Eren [writes about red flags to look out for while interviewing](https://meekg33k.dev/6-red-flags-i-saw-while-doing-60-technical-interviews-in-30-days-ckm53wt5f00avscs13xf9fhcs).
- Mauro Servienti [writes about how not all changes are equal](https://milestone.topics.it/2021/03/10/not-all-changes-are-born-equal.html).
- The ThoughtWorks blog [continues writing about Kubernetes adoption best practices](https://www.thoughtworks.com/insights/blog/shift-mindset-needed-kubernetes-adoption-part-2).
- Oren Eini [writes about a coding interview that he failed](https://ayende.com/blog/193409-B/the-coding-interview-that-i-failed?Key=91893b7f-c17a-4135-ae0e-6315cfde09aa).

## 🎤 Podcasts

- The .NET Rocks podcast [talks to Daniel Roth about .NET 6](https://www.dotnetrocks.com/default.aspx?ShowNum=1730).
- The Xamarin Podcast [offers an update on MAUI](https://www.xamarinpodcast.com/88).
- The Adventures in .NET podcast [talks to Daniel Roth about Blazor](https://devchat.tv/adventures-in-dotnet/net-059-blazor-keeps-getting-better-with-daniel-roth/).
- The Merge Conflict podcast [introduces microservices](https://www.mergeconflict.fm/244).
- The 6-Figure Developer podcast [talks GitOps with Kelsey Hightower](https://6figuredev.com/podcast/episode-186-gitops-with-kelsey-hightower/).

## 🎥 Videos

- James Montemagno [discusses whether you should develop in Xamarin today or wait for MAUI](https://www.youtube.com/watch?v=zvPPz6DABi8).
- Scott Hanselman and Azure Barry [discuss what to use for monitoring your applications in Azure](https://channel9.msdn.com/Shows/Azure-Friday/What-to-use-for-monitoring-your-applications-in-Azure), and also [discuss what to use for deploying and testing your applications in Azure](https://channel9.msdn.com/Shows/Azure-Friday/What-to-use-for-deploying-and-testing-your-applications-in-Azure).
- The ON .NET Show [builds microservices with Tye](https://dev.to/dotnet/on-net-episode-building-microservices-with-tye-3ci), and also [discusses commands, queries, and other fun architectural patterns](https://www.youtube.com/watch?v=ysxgpVfyeNA).