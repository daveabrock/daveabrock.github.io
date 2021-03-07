---
date: "2021-03-13"
title: "The .NET Stacks #40: 📚 Ignite is in the books"
tags: [dotnet-stacks]
comments: false
image: /assets/img/stacks-40-card.png 
description: This week, Ignite wraps and we get a new upgrade assistant.
---

![Newsletter image](https://daveabrock.com/THE .NET STACKS.png)

We've made it to 40. Does this mean we're over the hill?

- **One big thing**: Ignite recap
- **The little things**: a new upgrade assistant, Okta buys Auth0, hash codes
- Last week in the .NET world

# One big thing: Ignite recap

Microsoft [held Ignite this week](https://myignite.microsoft.com/home). If you're looking for splashy developer content, don't hold your breath—that's more suited for Build. Still, it's an excellent opportunity to see what Microsoft is prioritizing. While most of this section won't be pure .NET content, it's still valuable (especially if you deploy to Azure). You can read a rundown of key announcements in the [Book of News](https://news.microsoft.com/ignite-march-2021-book-of-news/), and you can also [hit up the YouTube playlist](https://www.youtube.com/watch?v=VjR09KSMDKg&list=PLQXpv_NQsPIALDgjX4bEmxxjMtuDir6ra).

A big part of the VR-friendly keynote involved Satya Nadella sharing the virtual stage with Microsoft Mesh, the new mixed-reality platform. From the *Book of News*, it "powers collaborative experiences with a feeling of presence–meaning users feel like they are physically present with one another even when they are not." Folks can interact with 3D objects and other people through Mesh-enabled apps across a wide variety of devices. If this is where you say, *Dave, this a .NET newsletter* - noted. However, as a technology that received attention at the Ignite keynote, it bears mention. (Especially with its applications in today's pandemic world.)

If you're into Azure Cognitive Services, Microsoft rolled out [semantic capabilities](https://www.microsoft.com/research/blog/the-science-behind-semantic-search-how-ai-from-bing-is-powering-azure-cognitive-search/). Also, their Form Recognizer service now has support for ID documents and invoice extraction. There are new Enterprise tiers for Azure Cache for Redis, and Azure Cosmos DB now has continuous backup and point-in-time support.

Azure Communication Services, announced last fall, is hitting general availability in a few weeks. If you aren't familiar with it, Azure Communication Services allows developers to integrate voice, video, and text communication with their apps. (It certainly makes the relationship with Twilio a little more interesting.) The power of having these capabilities in Azure gives it the ability to integrate with other Azure products and services. For example, Azure Bot Service has a new telephony channel built on Azure Communication Services. If you're writing a bot, you can leverage the AI from Cognitive Services and integrate it with your phone and messaging capabilities. These capabilities provide a distinct advantage to other services that focus on one need.

# The little things: a new upgrade assistant, Okta buys Auth0, hash codes

This week, Microsoft announced [the .NET Upgrade Assistant](https://devblogs.microsoft.com/dotnet/introducing-the-net-upgrade-assistant-preview/), a [global tool](https://dotnet.microsoft.com/platform/upgrade-assistant) that promises to help you upgrade your .NET Framework-based applications to .NET 5. It's a global CLI tool that offers a guided experience for "incrementally updating your applications." The assistant determines which projects you need to upgrade and recommends the order to be upgraded. Unlike tools like try-convert, you can see recommendations and choose how to upgrade your code.

The .NET Upgrade Assistant is not a tool meant to upgrade with a click of a button—you'll likely need to do some manual work. It does promise to make your upgrade experience a lot easier. You can [check out the GitHub repo](https://github.com/dotnet/upgrade-assistant) for details as well. Side note: I'll be releasing a detailed post on the .NET Upgrade Assistant by month's end.

***

Last week, Okta [bought Auth0 for $6.5 billion](https://techcrunch.com/2021/03/03/okta-acquires-cloud-identity-startup-auth0-for-6-5b/). (I think I need to start an API company.)

It makes sense: at the risk of oversimplifying, Okta provides IAM to companies that use it for SSO. Auth0 works on the app level, allowing developers API access to SSO. Okta has a reputation for being very enterprise-*y*, and Auth0 is known as a company with a startup feel that provides a developer-first experience. With this news, IdentityServer v4 now a paid product, and the Azure AD offerings, you many of choices when it comes to integrating auth in your .NET apps.

***

Did you know about `HashCode.Combine`? If you're using custom `GetHashCode` implementations and C# 9 records don't suit your needs it makes overriding hash codes easier.

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">I had no idea this feature existed in .NET - HashCode.Combine, for custom GetHashCode implementations. No more fancy bitwise operations needed to do this anymore. <a href="https://t.co/x0DHiqeCpx">https://t.co/x0DHiqeCpx</a> <a href="https://t.co/net5avsZTd">pic.twitter.com/net5avsZTd</a></p>&mdash; Aaron Stannard (@Aaronontheweb) <a href="https://twitter.com/Aaronontheweb/status/1368258898512216065?ref_src=twsrc%5Etfw">March 6, 2021</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

***

# 🌎 Last week in the .NET world

## 🔥 The Top 3

- Luis Quintanilla [serves ML.NET models as HTTP APIs with minimal configuration](https://devblogs.microsoft.com/dotnet/serve-ml-net-models-as-http-apis-with-minimal-configuration).
- Isaac Abraham [creates Azure solutions with the new Azure SDKs, F#, and Farmer](https://devblogs.microsoft.com/azure-sdk/azure-solutions-azure-sdk-fsharp-farmer).
- Microsoft [announces the .NET Upgrade Assistant, now in preview](https://devblogs.microsoft.com/dotnet/introducing-the-net-upgrade-assistant-preview).

## 📢 Announcements

- Phillip Carter [writes about F# and F# tools updates for Visual Studio 16.9](https://devblogs.microsoft.com/dotnet/f-and-f-tools-update-for-visual-studio-16-9).
- Maria Naggaga [announces .NET Interactive with SQL](https://devblogs.microsoft.com/dotnet/net-interactive-with-sql-net-notebooks-in-visual-studio-code).
- .NET Core 2.1 [will reach End of Support on August 21](https://devblogs.microsoft.com/dotnet/net-core-2-1-will-reach-end-of-support-on-august-21-2021).
- Visual Studio 2019 [v16.9 and v16.10 Preview are now available](https://devblogs.microsoft.com/visualstudio/vs2019-v16-9-and-v16-10-preview-1), and Peter Groenewegen [writes about IntelliCode suggestions in completion lists in the new version](https://devblogs.microsoft.com/visualstudio/repeated-edits-intellicode-suggestions-completion-list).
- Visual Studio 2019 [for Mac version 8.9 is now available](https://devblogs.microsoft.com/visualstudio/visual-studio-2019-for-mac-version-8-9-is-now-available).
- Jiachen Jiang [writes about the state of the NuGet ecosystem](https://devblogs.microsoft.com/nuget/state-of-the-nuget-ecosystem), then Drew Gillies [explains how to scan NuGet packages for security vulnerabilities](https://devblogs.microsoft.com/nuget/how-to-scan-nuget-packages-for-security-vulnerabilities).
- Kayla Cinnamon [releases Windows Terminal Preview 1.7](https://devblogs.microsoft.com/commandline/windows-terminal-preview-1-7-release).

## 📅 Community and events

- The Ignite 2021 [playlist is here](https://www.youtube.com/watch?v=VjR09KSMDKg&list=PLQXpv_NQsPIALDgjX4bEmxxjMtuDir6ra).
- Scott Addie [writes about what's new in the ASP.NET Core docs](https://docs.microsoft.com/aspnet/core/whats-new/2021-02?view=aspnetcore-5.0).
- The .NET Docs Show [talks about home automation](https://www.youtube.com/watch?v=j18TR2YKKOg).
- Microsoft publishes the [Book of News for the Ignite conference](https://news.microsoft.com/ignite-march-2021-book-of-news/).
- Jon Galloway [writes about getting started with writing HTTP APIs in .NET](https://devblogs.microsoft.com/dotnet/learn-to-build-http-apis-with-net).
- Blazorise [0.9.3 is out](https://blazorise.com/news/release-notes/093/).
- Claudio Bernasconi [rolls out a new video series on getting started with Blazor](https://www.claudiobernasconi.ch/2021/03/04/free-blazor-crash-course/).
- Cecil Phillip [rolls out a beginner's guide to Web APIs](https://channel9.msdn.com/Series/Beginners-Series-to-Web-APIs/What-are-Web-APIs-1-of-18--Beginners-Series-to-Web-APIs?WT.mc_id=DOP-MVP-4025064).
- For community standups: Xamarin [talks to Brandon Minnick about GitTrends](https://www.youtube.com/watch?v=NEbRo0ltniM), Machine Learning [talks tooling](https://www.youtube.com/watch?v=jWr_D_R2nZw), and ASP.NET [talks about Web Live Preview](https://www.youtube.com/watch?v=Qfy467I6Yts).

## 🌎 Web development

- Jon Hilton [gives Razor Pages some love](https://jonhilton.net/razor-pages-components/).
- Andrew Lock [configures HTTPS using a custom TLS cert with Netlify and Cloudflare](https://andrewlock.net/configuring-https-with-netlify-and-cloudflare/).
- Marinko Spasojevic [uses HttpClientFactory in ASP.NET Core apps](https://code-maze.com/using-httpclientfactory-in-asp-net-core-applications/), and also [cancels HTTP requests in ASP.NET Core with CancellationToken](https://code-maze.com/canceling-http-requests-in-asp-net-core-with-cancellationtoken/).
- Grant Hair [talks about how he tests his .NET APIs](https://dev.to/granthair5/a-couple-ways-i-test-my-net-apis-omk).

## 🥅 The .NET platform

- Michał Białecki [refactors with reflection](https://www.michalbialecki.com/2021/02/28/refactoring-with-reflection/).
- Maoni Stephens [writes about the internals of the pinned object heap (POH)](https://devblogs.microsoft.com/dotnet/internals-of-the-poh).
- Nick Randolph [writes about what's in store for WinUI, Project Reunion, and MAUI](https://nicksnettravels.builttoroam.com/breaking-changes/).
- Sagar Shetty [writes about new dynamic instrumentation profiling for .NET](https://devblogs.microsoft.com/visualstudio/new-dynamic-instrumentation-profiling).

## ⛅ The cloud

- The Microsoft Research Blog [writes about how Bing uses semantic search with Azure Cognitive Services](https://www.microsoft.com/research/blog/the-science-behind-semantic-search-how-ai-from-bing-is-powering-azure-cognitive-search/).
- Luis Cabrera-Cordon [introduces semantic search for Azure Cognitive Services](https://techcommunity.microsoft.com/t5/azure-ai/introducing-semantic-search-bringing-more-meaningful-results-to/ba-p/2175636).
- Graham Else [automates appointment reminders in C# using AWS Lambda](https://www.twilio.com/blog/appointment-reminders-csharp-aws-lambda-cloudwatch).
- Abhishek Gupta [autoscales Redis apps on Kubernetes](https://dev.to/azure/autoscaling-redis-applications-on-kubernetes-4gog).
- Daniel Krzyczkowski [controls access to his API with Azure API Management](https://daniel-krzyczkowski.github.io/Cars-Island-API-Access-With-API-Management/).
- Paul Michaels [defers messages with the Azure Service Bus](https://www.pmichaels.net/2021/02/27/deferred-messages-in-azure-service-bus/).
- Damien Bowden [uses Azure AD group authorization in ASP.NET Core for Azure Blob Storage](https://damienbod.com/2021/03/01/using-azure-ad-groups-authorization-in-asp-net-core-for-an-azure-blob-storage/).
- Ryan Palmer continues [writing about SAFE authentication with Azure AD](https://www.compositional-it.com/news-blog/safe-stack-authentication-with-active-directory-part-2/).

## 📔 Languages

- Jason Roberts [works with static local functions in C#](http://dontcodetired.com/blog/post/ICYMI-C-8-New-Features-Prevent-Bugs-with-Static-Local-Functions).
- Konrad Jokosa [writes about the 8 most missing features in C#](https://tooslowexception.com/the-8-most-missing-features-in-c/).
- Jesse Liberty [updates his C# coding standards](http://jesseliberty.com/2021/03/02/c-coding-standards-updated/).
- Kuba Ciechowski [uses collections with the MemberData attribute in F#](https://blog.ciechowski.net/using-collections-with-memberdata-attribute-in-f).

## 🔧 Tools

- Brian Douglas [writes about 4 things you can do with GitHub Actions](https://github.blog/2021-03-04-4-things-you-didnt-know-you-could-do-with-github-actions/).
- Khalid Abuhakmeh [writes raw SQL queries with EF Core 5](https://khalidabuhakmeh.com/raw-sql-queries-with-ef-core-5), and also [compares streamed results and buffered results with EF Core 5](https://khalidabuhakmeh.com/streaming-vs-buffered-results-with-entity-framework-core-5).
- Niels Swimberghe [writes about better Twilio authentication with Twilio API keys](https://www.twilio.com/blog/better-twilio-authentication-csharp-twilio-api-keys).
- Giorgi Dalakishvili [updates LINQPad.QueryPlanVisualizer for LINQPad6 and Entity Framework Core](https://www.giorgi.dev/miscellaneous/updating-linqpad-queryplanvisualizer-for-linqpad6-and-entity-framework-core/).
- Anne Gao [writes about the Intelligent Visual Studio Search Service](https://devblogs.microsoft.com/visualstudio/intelligent-visual-studio-search-service).
- James Newton-King [writes about IntelliSense support for appsettings.json](https://devblogs.microsoft.com/aspnet/intellisense-for-appsettings-json).
- Peter McKee [writes about how to use your own Docker registry](https://www.docker.com/blog/how-to-use-your-own-registry-2/).
- Nitya Narasimhan [introduces a visual guide to Dapr](https://blog.dapr.io/posts/2021/03/02/a-visual-guide-to-dapr/).
- Jon Gallant [uses the Azure REST APIs with Insomnia](https://blog.jongallant.com/2021/02/azure-rest-apis-insomnia/).

## 🏗 Design, testing, and best practices

- ThoughtWorks writes about [the required mindset shift for Kubernetes adoption](https://www.thoughtworks.com/insights/blog/mindset-shift-needed-kubernetes-adoption-part-1).
- Jimmy Bogard [crosses the generics divide](https://jimmybogard.com/crossing-the-generics-divide/).

## 🎤 Podcasts

- The .NET Rocks podcast [talks with Steve Gordon about Elasticsearch for .NET](https://www.dotnetrocks.com/default.aspx?ShowNum=1729).
- The Merge Conflict podcast [looks back on 3 years of FuGet](https://www.mergeconflict.fm/243).
- Software Engineering Radio [talks about starting your own software company](https://www.se-radio.net/2021/02/episode-448-starting-your-own-software-company/).

## 🎥 Videos

- JetBrains hosted [a webinar with Dennis Doomen about Fluent Assertions](https://blog.jetbrains.com/dotnet/2021/03/03/oss-power-ups-fluent-assertions-webinar-recording/).
- On .NET [dives deep into Microsoft Orleans](https://www.youtube.com/watch?v=R0ODfwU6MzQ) and also [talks to Scott Addie about secretless .NET apps](https://www.youtube.com/watch?v=f8Hf-YUrC10).
