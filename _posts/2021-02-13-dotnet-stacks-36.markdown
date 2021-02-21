---
date: "2021-02-13"
title: "The .NET Stacks #36: ⚡ Azure Functions and some Microsoft history"
tags: [dotnet-stacks]
comments: false
image: /assets/img/stacks-36-card.png 
description: This week, we talk about Azure Functions, Blazor, and some Microsoft history.
---

Welcome to another week. If you're seeing temperatures above above 0° F, please send some my way. Talk about a cold start—Azure Functions should be jealous.

This week:

- OpenAPI support for Azure Functions
- Achieving inheritance with Blazor CSS isolation
- Ignoring calls from Bill Gates

## Open API support for Azure Functions

When you create a new ASP.NET Core Web API in .NET 5, you may have noticed that Open API (more commonly to you, Swagger UI) is supported out of the box—and there's a handy check mark in Visual Studio to guide you along. You might be wondering: will Open API support come to Azure Functions?

The answer is yes. While it's very much in preview, the team's [been talking about](https://twitter.com/bradygaster/status/1356706555735887876) releasing Open API bindings for Functions. This idea [isn't anything new](https://devkimchi.com/2019/02/02/introducing-swagger-ui-on-azure-functions/), but the bindings look promising.

With bindings, you'll be able to [decorate your functions with bindings like](https://github.com/Azure/azure-functions-openapi-extension/blob/main/docs/openapi-core.md#expose-endpoints-to-open-api-document):

- `[OpenApiOperation("addStuff", "stuff")]`
- `[OpenApiRequestBody("application/json", typeof(MyRequestModel))]`
- `[OpenApiResponseBody(HttpStatusCode.OK, "application/json", typeof(MyResponseModel))]`

I'm seeing [JSON.NET decorators](https://github.com/Azure/azure-functions-openapi-extension/blob/main/docs/openapi-core.md#supported-jsonnet-decorators) (hat tip to the team for not pushing `System.Text.Json` on us), [optional metadata configuration](https://github.com/Azure/azure-functions-openapi-extension/blob/main/docs/openapi-core.md#open-api-metadata-configuration), and [security and authentication support](https://github.com/Azure/azure-functions-openapi-extension/blob/main/docs/openapi-core.md#openapisecurityattribute). It's early, so if you do try it out be patient and report issues as you come across them.

## Achieving inheritance with Blazor CSS isolation

After [writing](https://daveabrock.com/2020/09/10/blazor-css-isolation) [about](https://docs.microsoft.com/aspnet/core/blazor/components/css-isolation?view=aspnetcore-5.0) Blazor CSS isolation over the last few months I've received a common question: how do I achieve inheritance with CSS isolation? I [wrote about it this week](https://daveabrock.com/2021/01/31/blazor-css-iso-inheritance-scopes).

The concept is a little weird—isolation scopes CSS to your components, and "inheriting" or sharing styles across components seems to go against a lot of those advantages. Even so, in some limited scenarios, it can help maintenance when dealing with similarly grouped components.

From the post (quoting myself in my newsletter deserves a trip to the therapist, I must admit):

>If we’re honest, with Blazor CSS isolation, the “C” (cascading) is non-existent. There’s no cascading going on here—isolating your styles is the opposite of cascading. With Blazor CSS isolation, you scope your CSS component styles. Then, at build time, Blazor takes care of the cascading for you. When you wish to inherit styles and share them between components, you’re losing many of the advantages of using scoped CSS.

>In my opinion, using inheritance with CSS isolation works best when you want to share styles across a small group of similar components. If you have 500 components in your solution and you want to have base styles across them all, you may be creating more problems for yourself. In those cases, you should stick with your existing tools.

With those caveats aside, how do you do it? The key here is that Blazor CSS isolation is *file*-based, not class-based, so it doesn't know about your object graph. This is done with grouping custom scope identifiers, which you can add to your project file. I've included these updates [in this week's blog post](https://daveabrock.com/2021/01/31/blazor-css-iso-inheritance-scopes) and in the official Microsoft ASP.NET Core doc.

## Ignoring calls from Bill Gates

I've really been enjoying reading Steven Sinofsky. A former Microsoft veteran—with stories to share from building out Windows in the antitrust years—he's writing a serialized book on Substack, *[Hardcore Software](https://hardcoresoftware.learningbyshipping.com/)*. It'll be split into 15 chapters and an epilogue. In his first chapter he writes about joining Microsoft. He never returned calls from Bill Gates because he thought his friends were pranking him. When he finally did, [he was met with some hilarious frankness](https://hardcoresoftware.learningbyshipping.com/p/001-becoming-a-microsoftie-chapter):

>“Hi, Steve, this is Bill Gates.” ... “Hello. Thank you for calling, and so sorry for the confusion. I thought a friend of mine...”

>“So, David gave me this list of like ten people and I’m supposed to call all of them and convince them to work at Microsoft. You should come work at Microsoft. Do you have any questions?”

> “Well, why haven’t you accepted yet? You have a good offer.”

>“I’m considering other things. I have been really interested in government service.”

>“Government? That’s for when you’re old and stupid.”

This is a fun history lesson and I'm looking forward to reading it all.

## 🌎 Last week in the .NET world

### 🔥 The Top 4

- Andrew Lock [finds all routable components in a Blazor app](https://andrewlock.net/finding-all-routable-components-in-a-webassembly-app/).
- Oren Eini continues writing a social media platform as he [deals with the past](https://ayende.com/blog/193061-A/building-a-social-media-platform-without-going-bankrupt-part-ix-dealing-with-the-past?Key=1e50fc85-8fba-42d7-9db8-3236e5f396fc), [tags and searches](https://ayende.com/blog/193060-A/building-a-social-media-platform-without-going-bankrupt-part-viii-tagging-and-searching?Key=c69d9fb8-89e6-4d4c-a4a8-2b3b37386c36), [works with edits and deletions](https://ayende.com/blog/193058-A/building-a-social-media-platform-without-going-bankrupt-part-vi-dealing-with-edits-and-deletions?Key=107bae9f-a2f2-49bb-bc49-4a4338156e98), [counts replies and likes](https://ayende.com/blog/193059-A/building-a-social-media-platform-without-going-bankrupt-part-vii-counting-views-replies-and-likes), and [handles the timeline](https://ayende.com/blog/193057-A/building-a-social-media-platform-without-going-bankrupt-part-v-handling-the-timeline?Key=f24ec06d-5d24-49ba-9a90-2809f6b55c3f).
- Damien Bowden [implements app role authorization with Azure AD and ASP.NET Core](https://damienbod.com/2021/02/01/implement-app-roles-authorization-with-azure-ad-and-asp-net-core/).
- Stephen Cleary [continues writing about asynchronous messaging](https://blog.stephencleary.com/2021/02/asynchronous-messaging-5-miscellaneous-considerations.html).

### 📢 Announcements

- David Ortinau [provides a MAUI update](https://devblogs.microsoft.com/xamarin/the-new-net-multi-platform-app-ui-maui).
- Scott Addie [provides a rundown of what's new in the ASP.NET Core docs](https://docs.microsoft.com/aspnet/core/whats-new/2021-01?view=aspnetcore-5.0).
- Azure Event Grid [is now in public preview for Azure Cache for Redis](https://azure.microsoft.com/updates/azure-event-grid-is-now-in-public-preview-for-azure-cache-for-redis).
- Dapr [v1.0.0-rc.3 is now available](https://blog.dapr.io/posts/2021/02/03/dapr-v1.0.0-rc.3-is-now-available/).
- The January 2021 [release of VS Code is out](https://code.visualstudio.com/updates/v1_53).
- Redgate announces [they are committing to the future of PASS](https://www.red-gate.com/blog/redgate-events/redgate-pass).
- Microsoft released [PowerToys 0.31.1, with a better FancyZone editor](https://www.onmsft.com/news/powertoys-0-31-1-is-now-available-with-better-fancyzone-editor).

### 📅 Community and events

- If you didn't know, GitHub discussions area thing. Now that you know they are a thing, the ASP.NET Core team [has stopped using them](https://github.com/dotnet/aspnetcore/issues/29935).
- Microsoft Ignite [registration is open](https://t.co/eKAF5ApaZH?amp=1).
- JetBrains [released 2020.1 EAP for Rider](https://blog.jetbrains.com/dotnet/2021/02/01/rider-2021-1-eap/) and also [2020.1 EAP for ReSharper](https://blog.jetbrains.com/dotnet/2021/02/01/resharper-2021-1-starts-eap/).
- For community standups, we've got Xamarin [talking about Community Toolkit updates](https://www.youtube.com/watch?v=n12JoSn31Ac), Machine Learning [talking about Jupyter and .NET Interactive](https://www.youtube.com/watch?v=JeX7BhBPoUE), and ASP.NET [talks about Dapr](https://www.youtube.com/watch?v=VkJQR854S1s).
- The .NET Docs Show [automates kombucha availability with .NET](https://www.youtube.com/watch?v=XBp4pNc1Q4g).

### 🌎 Web development

- Dave Brock [writes about how to achieve style inheritance with Blazor CSS isolation](https://daveabrock.com/2021/01/31/blazor-css-iso-inheritance-scopes).
- Niels Swimberghe [deploys Blazor WASM apps to Netlify](https://swimburger.net/blog/dotnet/how-to-deploy-blazor-webassembly-to-netlify).
- Imar Spaanjars [builds and auto-deploys an ASP.NET Core app](https://imar.spaanjaars.com/619/building-and-auto-deploying-an-aspnet-core-application-part-2-creating-the-web-application).
- David Grace [starts writing about Blash, a Twitter dashboard](https://www.roundthecode.com/dotnet/blazor/blash-twitter-dashboard-using-blazor-web-api-signalr-twitter-api).
- Brady Gaster [creates discoverable APIs with ASP.NET Core 5 Web API](https://devblogs.microsoft.com/aspnet/creating-discoverable-http-apis-with-asp-net-core-5-web-api).
- Marinko Spasojevic [works on Facebook auth in Blazor WebAssembly hosted applications](https://code-maze.com/facebook-authentication-in-blazor-webassembly-hosted-applications/), and also [works with Google auth as well](https://code-maze.com/google-authentication-in-blazor-webassembly-hosted-applications/).
- Daniel Jimenez Garcia [writes about Kubernetes for ASP.NET Core developers](https://www.dotnetcurry.com/aspnet-core/kuberenetes-for-developers).
- Eve Turzillo [writes about strategies for debugging common website issues](https://www.telerik.com/blogs/how-to-effectively-debug-three-common-website-issues-using-web-debugging-proxy-tool).
- Khalid Abuhakmeh [works with IOptions in ASP.NET Core](https://khalidabuhakmeh.com/aspnet-core-ioptions-configuration).
- Tobias Ahlin [continues writing about GitHub's new homepage](https://github.blog/2021-01-29-making-githubs-new-homepage-fast-and-performant/).

### 🥅 The .NET platform

- Alexandre Zollinger Chohfi [writes about command line parsing in .NET 5](https://devblogs.microsoft.com/pax-windows/command-line-parser-on-net5).
- David Ramel [writes about .NET 6 desktop dev options](https://visualstudiomagazine.com/articles/2021/02/03/net-6-desktop.aspx).
- Peter Mbanugo [introduces gRPC in .NET Core and .NET 5](https://www.telerik.com/blogs/introduction-to-grpc-dotnet-core-and-dotnet-5).
- Konrad Kokosa [continues his series on .NET GC internals](https://tooslowexception.com/net-gc-internals-the-concurrent-mark-phase/).
- Khalid Abuhakmeh [writes about common files in a .NET solution](https://khalidabuhakmeh.com/common-files-dotnet-solution).
- Nick Randolph [writes about WinUI 3.0 misconceptions](https://nicksnettravels.builttoroam.com/winui-3-misconceptions/).

### ⛅ The cloud

- Richard Seroter [compares cloud shells across the various cloud providers](https://seroter.com/2021/02/03/lets-compare-the-cloud-shells-offered-by-aws-microsoft-azure-and-google-cloud-platform/).
- The Azure SDK folks [write about the state of the SDK](https://devblogs.microsoft.com/azure-sdk/state-of-the-azure-sdk-2021).
- ErikEJ [finishes his series on advanced automated deployments of Azure SQL with Azure DevOps](https://erikej.github.io/sqlserver/2021/02/01/azure-sql-advanced-deployment-part4.html).
- Jonathan George [writes about configuration in Azure Functions](https://endjin.com/blog/2021/02/configuration-in-azure-functions-part-1-whats-in-the-box.html).
- Leonard Lobel [works with transactions in Azure Cosmos DB with the .NET SDK](https://lennilobel.wordpress.com/2020/10/04/transactions-in-azure-cosmos-db-with-the-net-sdk/).

### 📔 Languages

- Michael Shpilt [writes about what he learned about C# from job interviews](https://michaelscodingspot.com/what-i-learned-about-c-from-job-interviews/).
- Szymon Kulec [writes about his mental model of Span, Memory, and ReadOnlySequence in .NET](https://blog.scooletz.com/2021/02/03/spans-memory-mental-model).
- Patrick Smacchia [includes IL offsets into production exception stack traces](https://blog.ndepend.com/include-il-offset-into-production-exception-stack-traces/).
- Jason Roberts [writes about using declarations in C# 8](http://dontcodetired.com/blog/post/ICYMI-C-8-New-Features-Write-Less-Code-with-Using-Declarations).

### 🔧 Tools

- Paul Vick [writes about recent Visual Studio improvements that help with working with large .NET 5 solutions](https://devblogs.microsoft.com/visualstudio/working-with-large-net-5-solutions-in-visual-studio-2019-16-8).
- Huang Qiangxiong [analyzes gRPC messages with Wireshark](https://grpc.io/blog/wireshark/).
- Matthew MacDonald [writes about the new Windows Terminal](https://medium.com/young-coder/if-you-code-on-windows-you-deserve-a-better-terminal-94e52014d848).
- Rick Strahl [opens an admin Windows Terminal from Visual Studio](https://weblog.west-wind.com/posts/2021/Feb/04/Opening-an-Admin-Windows-Terminal-from-Visual-Studio).
- Lee Richardson [looks back on lessons learned when using Cake](http://www.leerichardson.com/2021/02/he-one-thing-i-wish-id-known-before.html).
- Matt Lacey [updates his VS extension to search Stack Overflow](https://www.mrlacey.com/2021/02/here-i-made-this-to-search.html).
- David Ramel [writes about tips for working with VS Code](https://visualstudiomagazine.com/articles/2021/01/29/vs-code-tips.aspx).
- Johnny Reilly [works with ASP.NET, Serilog, and Application Insights](https://blog.johnnyreilly.com/2021/01/aspnet-serilog-and-application-insights.html).

### 📱 Xamarin

- Steven Thewissen [handles the new UIDatePicker](https://www.thewissen.io/handling-the-new-uidatepicker-in-xamarin-forms/).
- James Montemagno [creates a RadioButton control](https://devblogs.microsoft.com/xamarin/beautiful-custom-radiobutton-with-xamarin-forms-5).

### 🎤 Podcasts

- The .NET Rocks show [talks to Jeff Fritz about Blazor Static Web App](https://www.dotnetrocks.com/default.aspx?ShowNum=1725).
- The 6-Figure Developer podcast [talks to Jeremy D. Miller about Marten DB](https://6figuredev.com/podcast/episode-181-marten-db-with-jeremy-d-miller/).
- The Adventures in .NET podcast [talks to Maarten Balliauw about developing an IDE](https://devchat.tv/adventures-in-dotnet/net-054-how-do-you-develop-an-ide-jetbrains-rider-with-maarten-balliauw/).
- The Merge Conflict podcast [talks about planning an app release](https://www.mergeconflict.fm/239).

### 🎥 Videos

- The ON.NET Show [talks with Reuben Bond about Orleans](https://channel9.msdn.com/Shows/On-NET/Building-real-applications-with-Orleans), [deploys Orleans apps to Kubernetes](https://www.youtube.com/watch?v=da9u5tqw-eo), and [runs PHP and WordPress sites on .NET with PeachPie](https://www.youtube.com/watch?v=HVI7l9qgum4).
- The Xamarin Show [walks through the Xamarin Community Toolkit](https://channel9.msdn.com/Shows/XamarinShow/Xamarin-Community-Toolkit-TabView).
