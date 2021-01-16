---
date: "2021-01-16"
title: "The .NET Stacks #32: 😎 SSR is cool again"
tags: [dotnet-stacks]
comments: false
share-img: /assets/img/ssr-cool.png 
subtitle: This week, we talk about SSR and Xamarin.Forms 5.0.
---

![Newsletter image](https://daveabrock.com/THE .NET STACKS.png)

Good morning and happy Monday! We've got a few things to discuss this week:

- The new/old hotness: HTML over the wire
- Xamarin.Forms 5.0 released this week
- Quick break: how to explaining C# string interpolation to the United States Senate
- Last week in the .NET world

## The new/old hotness: server-side rendering

Over the holidays, I was intrigued by the release of [the Hotwire project](https://hotwire.dev/), from the folks at Basecamp:

>Hotwire is an alternative approach to building modern web applications without using much JavaScript by sending HTML instead of JSON over the wire. This makes for fast first-load pages, keeps template rendering on the server, and allows for a simpler, more productive development experience in any programming language, without sacrificing any of the speed or responsiveness associated with a traditional single-page application.

Between this and other tech such as Blazor Server, the "DOM over the wire" movement is in full force. It's a testament to how bloated and complicated the front end has become.

Obviously, rendering partial HTML over the wire isn't anything new at all—especially to us .NET developers—and it's sure to bring responses like: *"Oh, you mean what I've been doing the last 15 years?"* As much as I enjoy the snark, it's important to not write it off as the front-end community embracing what we've become comfortable with, as the technical details differ a bit—and we can learn from it. For example, it looks like instead of Hotwire working with DOM diffs over the wire, it streams partial updates over WebSocket while dividing complex pages into separate components, with an eye on performance. I wonder how Blazor Server would have been architected if this was released 2 years ago.

## Xamarin.Forms 5.0 released this week

This week, the Xamarin team [released the latest stable release of Xamarin.Forms, version 5.0](https://devblogs.microsoft.com/xamarin/xamarin-forms-5-0-is-here/), which will be supported through November 2022. There's updates for App Themes, Brushes, and SwipeView, among other things. The team [had a launch party](https://www.youtube.com/watch?v=DVE-orw3p0k). Also, David Ramel writes that this [latest version drops support for Visual Studio 2017](https://visualstudiomagazine.com/articles/2021/01/07/xamarin-forms-5.aspx). Updates to Android and iOS are only delivered to 2019, and pivotal for getting the latest updates from Apple and Google.

2021 promises to be a big year for Xamarin, as they continue preparing to join .NET 6—as this November, Xamarin.Forms evolves into MAUI (the .NET Multi-Platform App UI). This means more than developing against iPhones and Android devices, of course. With .NET 6 this also includes native UIs for iOS, Android, and desktops. As David Ramel also writes, Linux [will not be supported out of the gate and VS Code support will be quite limited](https://visualstudiomagazine.com/articles/2021/01/05/maui.aspx).

As he also writes, in a community standup David Ortinau clarifies that MAUI is not a rewrite.

>So my hope and expectation, depending on the complexity of your projects, is you can be up and going within days ... It's not rewrites -- it's not a rewrite -- that's probably the biggest message that I should probably say over and over and over again. You're not rewriting your application.

## Quick break: how to explain C# string interpolation to the United States Senate

Did I ever think C# string interpolation would make it to the United States Senate? No, I most certainly did not. But last month, that's what happened as former Cybersecurity and Infrastructure Security Agency (CISA) head Chris Krebs explained a bug:

>It's on page 20 ... it says 'There is no permission to {0}'. ... Something jumped out at me, having worked at Microsoft. ... The election-management system is coded with the programming language called C#. There is no permission to {0}' is placeholder for a parameter, so it may be that it's just not good coding, but that certainly doesn't mean that somebody tried to get in there a 0. They misinterpreted the language in what they saw in their forensic audit.

It appears that the election auditors were scared by something like this:

```csharp
Console.WriteLine("There is no permission to {0}");
```

To us, we know it's just a log statement that verifies permission checks are working. It should have been coded using one of the following lines of code:

```csharp
Console.WriteLine("There is no permission to {0}", permission);
Console.WriteLine($"There is no permission to {permission}");
```

I'm available to explain string interpolation to my government for a low, low rate of $1000 an hour. All they had to do was ask.

<iframe width="560" height="315" src="https://www.youtube.com/embed/v0lCCRwRnOU" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

___

## 🌎 Last week in the .NET world

### 🔥 The Top 4

- Josef Ottosson [works with polymorphic deserialization with `System.Text.Json`](https://josef.codes/polymorphic-deserialization-with-system-text-json/).
- Shahed Chowdhuri [works with init-only setters in C# 9](https://dev.to/dotnet/c-a-to-z-assignment-with-init-only-setters-12mo).
- Khalid Abuhakmeh [writes about EF Core 5 interceptors](https://khalidabuhakmeh.com/entity-framework-core-5-interceptors).
- Over at the AWS site, the folks at DraftKings [have a nice read about modernizing with .NET Core and AWS](https://aws.amazon.com/blogs/modernizing-with-aws/modernizing-legacy-net-applications-draftkings-principles-for-success/).

### 📢 Announcements

- WinUI 3 Preview 3 [has been released](https://devblogs.microsoft.com/pax-windows/winui-3-preview-3).
- David Ortinau [announces the arrival of Xamarin.Forms 5.0](https://devblogs.microsoft.com/xamarin/xamarin-forms-5-0-is-here).
- Microsoft Learn [has a new module on learning Python](https://docs.microsoft.com/learn/modules/intro-to-python).
- James Newton-King [releases a new Microsoft doc, *Code-first gRPC services and clients with .NET*](https://docs.microsoft.com/aspnet/core/grpc/code-first?view=aspnetcore-5.0).
- Phillip Carter brings attention [to a living F# coding conventions document](https://docs.microsoft.com/dotnet/fsharp/style-guide/conventions).
- Patrick Svensson [releases version 0.37 of `Spectre.Console`](https://twitter.com/firstdrafthell/status/1348303900651249664).
- The Azure SDK team released new .NET packages to simplify migrations using `Newtonsoft.Json` and/or `Microsoft.Spatial`.
- The EF Core team releases `EFCore.NamingConventions` 5.0.1, which fixes issues with owned entities and table splitting in 5.0.0.

### 📅 Community and events

- Chris Noring introduces [GitHub's web dev for beginners tutorials](https://techcommunity.microsoft.com/t5/apps-on-azure/learn-web-dev-with-these-24-lessons-on-github/ba-p/2033829).
- Niels Swimberghe rolls out two utilities written in Blazor: a[ GZIP compressor/decompressor](https://swimburger.net/blog/dotnet/introducing-online-gzip-decompressor), and [a .NET GUID generator](https://newguids.swimburger.net/).
- ErikEJ [writes about some free resources for EF 5](https://erikej.github.io/efcore/2021/01/05/efcore-5-resources.html).
- The Xamarin community standup [is a launch party for Xamarin.Forms 5](https://www.youtube.com/watch?v=DVE-orw3p0k).
- The .NET Docs Show [talks to co-host David Pine about his localization project](https://www.youtube.com/watch?v=oX2COO_1iOM).
- Shahed Chowdhuri [previews a new C# A-Z project and a Marvel cinematic visualization app](https://wakeupandcode.com/dotnet5-blazor-2021/).
- Chris Woodruff kicks of an [ASP.NET 5 Web API blog series](https://t.co/BnfpqTR0mp?amp=1).
- VS Code Day [is slated for January 27](https://t.co/2ZVOzfkQSR?amp=1).

### 🌎 Web development

- Peter Vogel [writes about displaying lists efficiently in Blazor](https://visualstudiomagazine.com/articles/2021/01/06/blazor-lists.aspx).
- Over at Code Maze, [using the API gateway pattern in .NET to encapsulate microservices](https://code-maze.com/api-gateway-pattern-dotnet-encapsulate-microservices/).
- David Fowler notes that [web socket compression is coming to .NET 6](https://github.com/dotnet/runtime/issues/31088#issuecomment-754854011).
- Chris Noring [manages configuration in ASP.NET Core](https://techcommunity.microsoft.com/t5/apps-on-azure/learn-how-you-can-manage-configuration-in-asp-net/ba-p/2033895).
- Marinko Spasojevic [signs in with Google using Angular and ASP.NET Core Web API](https://code-maze.com/how-to-sign-in-with-google-angular-aspnet-webapi/).
- Damien Bowden [works with Azure AD access token lifetime policy management in ASP.NET Core](https://damienbod.com/2021/01/05/azure-ad-access-token-lifetime-policy-management-in-asp-net-core/).
- Paul Michaels [views server variables in ASP.NET Core](https://www.pmichaels.net/2021/01/02/viewing-server-variables-in-asp-net-core).
- Sahan Serasinghe [writes about using Web Sockets with ASP.NET Core](https://dev.to/sahan/understanding-websockets-with-asp-net-core-g94).

### 🥅 The .NET platform

- Richard Reedy [talks about the Worker Service in .NET Core](https://www.telerik.com/blogs/dotnet-worker-service-working-hard-so-you-dont-have-to).
- Marco Minerva [develops desktop apps with .NET 5](https://marcominerva.wordpress.com/2021/01/04/developing-desktop-applications-with-net-5-0/).
- Jimmy Bogard [works with ActivitySource and ActivityListener in .NET 5](https://jimmybogard.com/activitysource-and-listener-in-net-5).
- Nikola Zivkovic [introduces machine learning with ML.NET](https://rubikscode.net/2021/01/04/machine-learning-with-ml-net-introduction/).
- Nick Randolph [works with missing files in a multi-targeted project](https://nicksnettravels.builttoroam.com/missing-files-in-multi-targeted-project).
- Stefan Koell writes about [migrating Royal TS from WinForms to .NET 5](https://code4ward.net/2021/01/09/net-5-adventure-crossgen2/).

### ⛅ The cloud

- Andrew Lock [auto-assigns issues using a GitHub Action](https://andrewlock.net/auto-assigning-issues-using-a-github-action/).
- Richard Reedy [builds a chatbot to order a pizza](https://www.telerik.com/blogs/building-chatbot-to-order-pizza).
- Dave Brock [uses the Microsoft Bot Framework to analyze emotion with the Azure Face API](https://daveabrock.com/2021/01/05/azure-bot-service-image-emotion-api).
- Jonathan Channon [uses GCP Cloud Functions with F#](https://www.softwarepark.cc/blog/2021/1/8/using-gcp-cloud-functions-with-f).
- Justin Yoo [writes about using Azure EventGrid](https://dev.to/azure/eventgrid-subscription-to-custom-topic-using-azure-cli-130b).
- Mark Heath [writes about bulk uploading files to Azure Blob Storage with the Azure CLI](https://markheath.net/post/bulk-upload-azure-cli).
- Daniel Krzyczkowski [continues his series on writing an ASP.NET Core API secured by Azure AD B2C](https://daniel-krzyczkowski.github.io/Cars-Island-ASP-NET-Core-API-Secured-By-Azure-AD-B2C/).
- Paul Michaels [schedules message delivery with Azure Service Bus](https://www.pmichaels.net/2021/01/01/azure-service-bus-scheduled-message-delivery).

### 📔 Languages

- Rick Strahl [works with blank zero values in .NET number format strings](https://weblog.west-wind.com/posts/2021/Jan/05/Blank-Zero-Values-in-CSharp-Number-Format-Strings).
- David McCarter [analyzes code for issues in .NET 5](https://www.c-sharpcorner.com/article/analyzing-code-for-issues-in-net-5/).
- Khalid Abuhakmeh [plays audio files with .NET](https://khalidabuhakmeh.com/play-audio-files-with-net).
- Daniel Bachler talks about [what he wishes he knew when learning F#](https://danielbachler.de/2020/12/23/what-i-wish-i-knew-when-learning-fsharp.html).
- Michał Niegrzybowski writes about [signaling in WebRTC with Ably and Fable](https://www.mnie.me/webrtcandably).
- Mark-James McDougall talks about [why he's learning F# in 2021](https://markjames.dev/2021-01-04-why-learning-fsharp-2021/).

### 🔧 Tools

- Jason Robert [creates a serverless Docker image](https://espressocoder.com/2021/01/05/creating-a-serverless-docker-image/).
- Stephen Cleary [kicks off a series around asynchronous messaging](https://blog.stephencleary.com/2021/01/asynchronous-messaging-1-basic-distributed-architecture.html).
- Michał Białecki [recaps useful SQL statements when writing EF Core migrations](https://www.michalbialecki.com/2021/01/07/useful-sql-statements-when-writing-ef-core-5-migrations).
- Derek Comartin [splits up a monolith into microservices](https://codeopinion.com/splitting-up-a-monolith-into-microservices/).
- Frank Boucher [creates a CI/CD deployment solution for a Docker project](http://www.frankysnotes.com/2021/01/how-to-create-continuous-integration.html).
- Alex Orlov writes about [using TLS 1.3 for IMAP and SMTP connections through Mailbee.NET](https://mailbeenet.wordpress.com/2020/11/10/use-tls-1-3-with-mailbee-net-objects/).
- Brad Beggs [writes about using vertical rulers in VS Code](https://dev.to/brad_beggs/vs-code-vertical-rulers-for-prettier-code-3gp3).
- Tim Cochran [writes about maximizing developer effectiveness](https://martinfowler.com/articles/developer-effectiveness.html).

### 📱 Xamarin

- David Ramel [writes how Xamarin.Forms won't be on Linux or VS Code for MAUI in .NET 6](https://visualstudiomagazine.com/articles/2021/01/05/maui.aspx), and also [mentions that Xamarin.Forms 5 is dropping Visual Studio 2017 support](https://visualstudiomagazine.com/articles/2021/01/07/xamarin-forms-5.aspx).
- Leomaris Reyes [writes about Xamarin Essentials](https://www.telerik.com/blogs/xamarin-essentials-features-advantages-benefits).
- Anbu Mani [works with infinite scrolling in Xamarin.Forms](https://xmonkeys360.com/2021/01/04/xamarin-forms-infinite-scroll-listview-lazy-loading/).
- Matthew Robbins [embeds a JS interpreter into Xamarin apps with Jint](https://www.mfractor.com/blogs/news/embedding-a-javascript-interpreter-into-xamarin-apps-with-jint).

### 🎤 Podcasts

- Scott Hanselman [talks to Amanda Silver about living through 2020 as a remote developer](https://hanselminutes.simplecast.com/episodes/living-through-2020-as-a-remote-developer-with-amanda-silver-28t79VXg).
- The 6-Figure Developer Podcast [talks with Phillip Carter about F# and functional programming](https://6figuredev.com/podcast/episode-177-f-sharp-and-functional-programming-with-phillip-carter/).
- The Azure DevOps Podcast [talks with Sam Nasr about SQL Server for developers](http://azuredevopspodcast.clear-measure.com/sam-nasr-on-sql-server-for-developers-episode-122).

### 🎥 Videos

- Visual Studio Toolbox [talks about the Azure App Insights Profiler](https://channel9.msdn.com/Shows/Visual-Studio-Toolbox/Azure-Application-Insights-Profiler).
- The ASP.NET Monsters [talk with Andrew Stanton-Nurse](https://www.youtube.com/watch?v=gEXVfqyRADU).
- Gerald Versluis [secures a Xamarin app with fingerprint or face recognition](https://www.youtube.com/watch?v=k-eg3gcSMSU).
- James Montemagno [makes another Xamarin.Forms 101 video](https://www.youtube.com/watch?v=GLfR2uosoSw).
- At Technology and Friends, [David Giard talks to Javier Lozano about virtual conferences](http://davidgiard.com/2021/01/04/JavierLozanoOnVirtualConferences.aspx).
- Jeff Fritz works on [ASP.NET Core MVC](https://www.youtube.com/watch?v=0AhIXUqfXTo) and also [APIs with ASP.NET Core](https://www.youtube.com/watch?v=Y_hTutN7yOw).
- ON.NET discusses [cross-platform .NET development with OmniSharp](https://www.youtube.com/watch?v=C_d6y5OMtMs).
