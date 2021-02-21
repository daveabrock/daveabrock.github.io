---
date: "2021-02-20"
title: "The .NET Stacks #37: 😲 When your private NuGet feed isn't so private"
tags: [dotnet-stacks]
comments: false
image: /assets/img/stacks-37-card.png 
description: This week, we talk about a supply chain attack, container updates, Blazor REPL, and more.
---

![Newsletter image](https://daveabrock.com/THE .NET STACKS.png)

Good morning to you. Is it summer yet? This week, I'm trying a new format: an in-depth topic, a lot of little things I've found, and, of course, the links. I appreciate any feedback you have.

- One big thing: A supply chain attack that should scare any company
- The little things: Container updates, typed exceptions, Blazor REPL
- Last week in the .NET world

***

## One big thing: A supply chain attack that should scare any company

When installing packages—whether over NPM, Python, and for us, NuGet—you’re happy not to have to deal with the complexities of dependency management personally. When you do this, you're trusting publishers to install packages and code on your machine. When you install a package, do you ever wonder if this is open to exploits? It certainly is.

This week, Alex Birsan [wrote about](https://medium.com/@alex.birsan/dependency-confusion-4a5d60fec610) what he's been working on for the last 6-7 months: hacking into dozens of companies—using a supply chain attack (or, as he calls it, *dependency confusion*). The ethical hack stems from a finding at Paypal last year, where Justin Gardner found that PayPal included private dependencies in a `package.json` hosted on GitHub. Those dependencies didn't exist in the public NPM registry then, begging the question: what happens if you upload malicious code to NPM under those package names? Will the code get installed on PayPal-owned servers?

It isn't just PayPal:

>Apparently, it is quite common for internal package.json files, which contain the names of a javascript project’s dependencies, to become embedded into public script files during their build process, exposing internal package names. Similarly, leaked internal paths or require() calls within these files may also contain dependency names. Apple, Yelp, and Tesla are just a few examples of companies who had internal names exposed in this way.

It worked like a charm. *The .NET Stacks* is a family newsletter, so [I'll channel *The Good Place*](https://www.youtube.com/watch?v=cJhPbmtDfoE): **holy forking shirtballs**.

>From one-off mistakes made by developers on their own machines, to misconfigured internal or cloud-based build servers, to systemically vulnerable development pipelines, one thing was clear: squatting valid internal package names was a nearly sure-fire method to get into the networks of some of the biggest tech companies out there, gaining remote code execution, and possibly allowing attackers to add backdoors during builds.

I know what you're thinking: *JavaScript is insecure. Is this where I pretend to be shocked*? The hack had impacts on .NET Core as well:

>Although this behavior was already commonly known, simply searching GitHub for --extra-index-url was enough to find a few vulnerable scripts belonging to large organizations — including a bug affecting a component of Microsoft’s .NET Core.

From the .NET side, [Barry Dorrans noted](https://twitter.com/blowdart/status/1359221803453800468?s=20):

![Barry explains]({{ site.url }}{{ site.baseurl }}/assets/img/dorrans-attacks.jpg)

So Microsoft fixed their problem. What about us? Microsoft has [developed a whitepaper](https://azure.microsoft.com/resources/3-ways-to-mitigate-risk-using-private-package-feeds/) for using private feeds. Their three suggestions for NuGet feeds:

- Reference a single private feed, not multiple, by using a single `<add/>` entry for a private feed in `nuget.config`, and a `<clear />` entry to remove inherited configuration
- Use controlled scopes using an ID prefix to restrict uploads to the public gallery
- Use a `packages.lock.json` file to validate packages have not changed using version pinning and integrity checking.

If you manage private NuGet feeds, make sure to heed the advice.

***

## The little things: Container updates, typed exceptions, Blazor REPL

If you're like me, pulling containers from the Microsoft container registry is a simple process. I pull the images and move on. This week, Rich Lander [outlined the complexities](https://devblogs.microsoft.com/dotnet/staying-safe-with-dotnet-containers/) the team encounters. How hard can managing Dockerfiles be? When it comes to supporting a million*ish* pulls a month, it can be. A lot of the issues come with [tracking potential vulnerabilities](https://devblogs.microsoft.com/dotnet/staying-safe-with-dotnet-containers/#vulnerabilities) and how to manage them. He also offers some tips we can use: like rebuilding images frequently, reading CVE reports, and so on. It's a long one but worth a read if you work with .NET container images.

On a related note, this week Mark Heath [wrote an excellent post](https://markheath.net/post/docker-tooling-vs2019) around Docker tooling for Visual Studio 2019.

--

In this week's [Entity Framework community standup](https://www.youtube.com/watch?v=aUl5QfswNU4), the team talked with Giorgi Dalakishvili about his `EntityFramework.Exceptions` project, which brings typed exceptions to Entity Framework Core. The project gets past digging into `DbUpdateException` to find your exact issue—whether it's constraints, value types, or missing required values. 

With `EntityFramework.Exceptions`, you can configure the `DbContext` to throw different exceptions, like `UniqueConstraintException` or `CannotInsertNullException`. It's another great project where we wonder why it isn't baked in, but happy it's here.

--

Have you heard of [the Blazor REPL project](https://github.com/BlazorRepl/BlazorRepl)? 

>Blazor REPL is a platform for writing, compiling, executing and sharing Blazor components entirely in the browser. It's perfect for code playground and testing. It's fast and secure. The platform is built and is running entirely on top of Blazor WASM - the WebAssembly hosting model of Blazor.

You can write and run components right in the client. It's perfect for you to test component libraries before you bring them into your project—the MudBlazor library has a site for this at *[try.mudblazor.com](https://try.mudblazor.com/snippet)*.

This week, [the Blazor REPL project teased](https://twitter.com/BlazorREPL/status/1359177364001992705) what's coming—the ability to download NuGet packages and save public snippets.

--

ASP.NET Core architect David Fowler showed off some underrated .NET APIs for working with strings. I learned quite a bit, as I typically blindly use the same string methods all the time:

- `StringBuilder.GetChunks`: you can [get access to the buffer](https://t.co/HmOxJmc8PF?amp=1) of the `StringBuilder`
- `StringSplitOptions.TrimEntries`: you can split a string [without trimming each entry](https://t.co/bVuM8b0YFT?amp=1)
- `Path.TrimEndingDirectorySeparator`: [eliminates you](https://docs.microsoft.com/dotnet/api/system.io.path.trimendingdirectoryseparator?view=net-5.0) manually checking for that trailing slash

--

I recently [learned about](https://twitter.com/JustinWGrote/status/1359214289962233857?s=20) *github1s.com*, which allows you to browse code on GitHub with VS Code embedded in a browser for you. While you're browsing a GitHub repo, change *github.com* to *github1s.com* to see it in action. Very nice.

***

## 🌎 Last week in the .NET world

### 🔥 The Top 3

- Andrew Lock [uses source generators to find all routable components in a Blazor WebAssembly app](https://andrewlock.net/using-source-generators-to-find-all-routable-components-in-a-webassembly-app/).
- David McCarter [goes deep on C# 9 records](https://www.c-sharpcorner.com/article/everything-you-want-to-know-about-the-record-type-in-net-5-but-were-afraid-to/).
- Viktoria Grozdancheva [uses JustMock to mock C# unit tests](https://www.telerik.com/blogs/how-to-unit-test-existing-csharp-app-easy-with-mocking-tool).

### 📢 Announcements

- Rahul Bhandari [releases the .NET February 2021 updates](https://devblogs.microsoft.com/dotnet/net-february-2021).
- Microsoft announced [a new datacenter in Georgia](https://azure.microsoft.com/blog/microsoft-will-establish-its-next-us-datacenter-region-in-georgia-s-fulton-and-douglas-counties/) (the state in the US, not the country).
- Simon Treanor [rolls out FunStripe, an unofficial F# library for the Stripe API](https://simontreanor.github.io/blog/20210205_FunStripe.html).
- Azure Cosmos DB is [getting their own conference](https://gotcosmos.com/conf) in April.

### 📅 Community and events

- Shameless plug: later today, I'll be [speaking to the Bournemouth ASP.NET Blazor User Group](https://www.meetup.com/Bournemouth-ASP-NET-Blazor-Meetup-Group/events/275962302/) about my Blast Off with Blazor app.
- Check out *k8s.af* for [real-world Kubernetes failure stories](https://k8s.af/).
- The .NET Docs Show [talks to Sophie Obomighie about APIs](https://www.youtube.com/watch?v=ekezoV4DcNA).
- In community standups, ASP.NET [talks to Chris Sainty about his Blazor projects](https://www.youtube.com/watch?v=v8UWYwAhKZA), and Entity Framework [talks about typed exceptions](https://www.youtube.com/watch?v=aUl5QfswNU4).
- GitHub writes about [how they designed and wrote the narrative for their homepage](https://github.blog/2021-02-11-how-we-designed-and-wrote-the-narrative-for-our-homepage/).
- The Uno Platform team [writes about sustaining their open-source project](https://platform.uno/blog/sustaining-the-open-source-uno-platform/).
- Microsoft joins [as a founding member of the Rust Foundation](https://cloudblogs.microsoft.com/opensource/2021/02/08/microsoft-joins-rust-foundation).
- The Azure Dev Advocates [released an Azure Space Mystery game](https://dev.to/azure/blast-off-with-azure-advocates-presenting-the-azure-space-mystery-mdd).

### 🌎 Web development

- Brady Gaster [writes about open-source HTTP API packages and tools](https://devblogs.microsoft.com/aspnet/open-source-http-api-packages-and-tools).
- Aram Tchekrekjian [integrates an ASP.NET Core Web API with Android](http://codingsonata.com/a-complete-tutorial-to-connect-android-with-asp-net-core-web-api/).
- Khalid Abuhakmeh [structures endpoints with ASP.NET Core and EF Core](https://khalidabuhakmeh.com/ef-core-and-aspnet-core-cycle-issue-and-solution).
- Thomas Ardal [rate limits API requests with ASP.NET Core and AspNetCoreRateLimit](https://blog.elmah.io/rate-limiting-api-requests-with-asp-net-core-and-aspnetcoreratelimit/).
- Marinko Spasojevic [uploads files to Azure with .NET Core Web API and Blazor WebAssembly](https://code-maze.com/upload-files-to-azure-aspnet-core-blazor-webassembly/), and also [downloads files with .NET Core Web API and Blazor WebAssembly](https://code-maze.com/download-files-from-azure-with-net-core-web-api-and-blazor-webassembly/).
- Xavier Geerinck [writes about how Roadwork uses Dapr in production](https://blog.dapr.io/posts/2021/02/09/running-dapr-in-production-at-roadwork/).

### 🥅 The .NET platform

- Richard Lander [writes about staying safe with .NET containers](https://devblogs.microsoft.com/dotnet/staying-safe-with-dotnet-containers/).
- Nick Randolph [writes about how to upgrade a UWP app to WinUI 3.0](https://nicksnettravels.builttoroam.com/upgrade-uwp-to-winui/).
- Alexandre Zollinger Chohfi [writes a Windows Service with C# and .NET 5](https://devblogs.microsoft.com/ifdef-windows/creating-a-windows-service-with-c-net5).
- Thomas Claudius Huber [writes about UWP, WinUI, MSIX, and Project Reunion](https://www.thomasclaudiushuber.com/2021/02/05/what-is-actually-the-universal-windows-platform-and-what-is-winui-msix-and-project-reunion/).

### ⛅ The cloud

- Henk Boelman [writes about using Cognitive Services and containers](https://techcommunity.microsoft.com/t5/azure-ai/how-to-use-cognitive-services-and-containers/ba-p/2113684).
- Johnny Reilly [works on zero downtime deployments with Azure App Service](https://blog.johnnyreilly.com/2021/02/azure-app-service-health-checks-and-zero-downtime-deployments.html).
- Matt Ellis [builds a custom Event Hubs event processor in .NET](https://devblogs.microsoft.com/azure-sdk/custom-event-processor).
- Daniel Krzyczkowski [integrates his Functions app with Azure SendGrid](https://daniel-krzyczkowski.github.io/Cars-Island-Azure-Functions-Integration-With-SendGrid/).
- Damien Bowden [secures Azure AD user file uploads with Azure AD storage and ASP.NET Core](https://damienbod.com/2021/02/08/secure-azure-ad-user-account-file-upload-with-azure-ad-storage-and-asp-net-core/).
- Mahesh Chand [introduces Azure Functions](https://www.c-sharpcorner.com/article/what-is-azure-functions/).
- Jon Gallant [uses the Azure APIs with Postman](https://blog.jongallant.com/2021/02/azure-rest-apis-postman-2021/).

### 📔 Languages

- Jason Roberts [writes about property pattern matching in C#](http://dontcodetired.com/blog/post/ICYMI-C-8-New-Features-Simplify-If-Statements-with-Property-Pattern-Matching).
- Oren Eini [wraps up his series on writing a social media platform](https://ayende.com/blog/193062-A/building-a-social-media-platform-without-going-bankrupt-part-x-optimizing-for-whales?Key=718095b6-a3fd-472c-aa28-6112d985f66f).
- Jeremy Clark [introduces channels in C#](https://jeremybytes.blogspot.com/2021/02/an-introduction-to-channels-in-c.html), and also [differentiates Channel and ConcurrentQueue](https://jeremybytes.blogspot.com/2021/02/whats-difference-between-channel-and.html).
- Mark-James McDougall [works on iRacing telemetry in F#](https://markjames.dev/2021-02-09-iracing-telemetry-fsharp/).
- Isaac Abraham [writes about custom equality and comparison in F#](https://www.compositional-it.com/news-blog/custom-equality-and-comparison-in-f/).
- Urs Enzler [writes about F# libraries he uses](https://www.planetgeek.ch/2021/02/11/our-journey-to-f-libraries-we-use/).

### 🔧 Tools

- Khalid Abuhakmeh [uses Bebop with a C# TCP server](https://khalidabuhakmeh.com/bebop-with-csharp).
- Tobias Günther [gets the most out of Git](https://www.smashingmagazine.com/2021/02/getting-the-most-out-of-git/).
- Scott Hanselman [writes about what's new with Windows Terminal](https://www.hanselman.com/blog/get-on-the-windows-terminal-preview-train-now-with-settings-ui).
- Rachel Appel [shows off ReSharper](https://blog.jetbrains.com/dotnet/2021/02/08/make-code-more-readable-by-refactoring-with-resharper/).
- Horatiu Vladasel [creates an MSIX package in Visual Studio](https://www.advancedinstaller.com/msix-package-in-visual-studio.html).
- Scott Hanselman [works on tiny top-level programs with C# 9 and SmallSharp and Visual Studio](https://www.hanselman.com/blog/tiny-toplevel-programs-with-c-9-and-smallsharp-and-visual-studio).
- Thomas Cladius Huber [works with .editorconfig settings in Visual Studio](https://www.thomasclaudiushuber.com/2021/02/06/configure-naming-styles-and-rules-in-visual-studio-and-also-at-the-solution-project-level-with-an-editorconfig-file/).
- Mark Heath [writes about Visual Studio 2019 Docker tooling](https://markheath.net/post/docker-tooling-vs2019).

### 📱 Xamarin

- Sam Basu [works with custom Xamarin controls with Blazor Mobile Bindings](https://www.telerik.com/blogs/custom-xamarin-controls-blazor-mobile-bindings).
- James Montemagno [works on cross-plat in-app purchases for Xamarin.Mac](https://montemagno.com/cross-platform-in-app-purchases-for-xamarin-mac-apps/).
- Luis Matos [uses sticky headers and navigation bars](https://luismts.com/stickyheader-xamarin-forms/).

### 🏗 Design, testing, and best practices

- Patrick Smacchia [writes about why you should write tests](https://blog.ndepend.com/10-reasons-why-you-should-write-tests/).
- Steve Smith [uses extension methods to keep tests short and DRY](https://ardalis.com/keep-tests-short-and-dry-with-extensions/).
- Derek Comartin [talks about persisting Aggregates](https://codeopinion.com/aggregate-root-design-behavior-data/).
- Jon P. Smith [writes about his experience using modular monoliths and DDD architectures](https://www.thereformedprogrammer.net/my-experience-of-using-modular-monolith-and-ddd-architectures/).

### 🎤 Podcasts

- The Xamarin Podcast [has a Xamarin Community Toolkit extravaganza](https://www.xamarinpodcast.com/87).
- The Adventures in .NET podcast [talks about microservices](https://devchat.tv/adventures-in-dotnet/net-055-microservices-or-should-they-be-called-single-responsibility-services-w-christian-horsdal/).
- The 6-Figure Developer podcast [talks about application security with Tanya Janca](https://6figuredev.com/podcast/episode-182-application-security-with-tanya-janca/).
- The .NET Rocks podcast [talks with Phil Haack](https://www.dotnetrocks.com/default.aspx?ShowNum=1726).
- The Azure DevOps Podcast [talks to Jeff Fritz about Blazor WebAssembly architecture](http://azuredevopspodcast.clear-measure.com/is-blazor-ready-for-prime-time-episode-127).
- The .NET Core Podcast [talks to Niels Tanis about the risks of third party code](https://dotnetcore.show/episode-69-the-risks-of-third-party-code-with-niels-tanis/).
- The Productive C# Podcast [talks about The Mikado Method](https://anchor.fm/productivecsharp/episodes/16--The-Mikado-Method-eq0iek).

### 🎥 Videos

- The ON.NET Show [generates docs for ASP.NET Core Web APIs with Swashbuckle](https://www.youtube.com/watch?v=SCR1IdW0PCk).
- The Visual Studio Toolbox [analyzes code with Infer#](https://channel9.msdn.com/Shows/Visual-Studio-Toolbox/Analyzing-Code-with-Infer).
- The Loosely Coupled Show [talks about various domains and projects they've worked on](https://www.youtube.com/watch?v=nmvadY1OPOQ).
- The ASP.NET Monsters [talks to Chris Patterson about MassTransit](https://www.youtube.com/watch?v=gpi-h-2GJ7w).
- Technology and Friends [talks to Kevin Pilch about gRPC](https://www.davidgiard.com/2021/02/08/KevinPilchOnGRPC.aspx).
- Azure Friday [discusses operational best practices for Azure App Service](https://channel9.msdn.com/Shows/Azure-Friday/Operational-best-practices-for-web-apps-on-Azure-App-Service), and also [scales apps with Azure Cache for Redis](https://channel9.msdn.com/Shows/Azure-Friday/Scale-Your-Cloud-App-with-Azure-Cache-for-Redis).
- Web Wednesday [introduces Tailwind CSS](https://channel9.msdn.com/Shows/Web-Wednesday/What-is-Tailwind-CSS).
