---
date: "2021-02-27"
title: "The .NET Stacks #38: 📢 I hope you like announcements"
tags: [dotnet-stacks]
comments: false
image: /assets/img/stacks-38-card.png 
description: This week, both .NET 6 Preview 1 and Dapr v1.0 are released.
---

![Newsletter image](https://daveabrock.com/THE .NET STACKS.png)

It was very busy last week! Let's get right to it.

- One big thing: .NET 6 gets Preview 1
- The little things: Dapr v1.0 is released, Azure Static Web Apps, more SolarWinds findings
- Dev Discussions: Cecil Phillip
- Last week in the .NET world

# One big thing: .NET 6 gets Preview 1

It seems like just yesterday .NET 5 went live. (It was November, for the record.) This week, [the .NET team announced](https://devblogs.microsoft.com/dotnet/announcing-net-6-preview-1/) .NET 6 Preview 1, to be officially released in November 2021. You can also check out the [ASP.NET Core](https://devblogs.microsoft.com/aspnet/asp-net-core-updates-in-net-6-preview-1/) and [EF Core 6](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-6-0-preview-1/) announcements. .NET 6 will be [an enterprise-friendly LTS release](https://github.com/dotnet/core/blob/master/release-policies.md), meaning it will be supported for three years.

As expected, a main focus will be integrating Xamarin into the "One .NET" model by way of the [.NET Multi-Platform App UI (MAUI)](https://devblogs.microsoft.com/dotnet/announcing-net-6-preview-1/#net-multi-platform-app-ui). With Preview 1, Xamarin developers can develop Android and iOS with MAUI. Future previews will address support for macOS and Windows. Another big goal is improving the inner loop experience. You can [check out *themesof.net*](https://themesof.net/) to see what's being prioritized. AOT support is not rolled out yet, but you can [head over to the NativeAOT repo](https://github.com/dotnet/runtimelab/tree/feature/NativeAOT) to get up to speed.

For ASP.NET Core 6, they're prioritizing work on [hot reload](https://github.com/dotnet/aspnetcore/issues/18486), [micro APIs](https://github.com/dotnet/aspnetcore/issues/27724), [AoT compilation](https://github.com/dotnet/aspnetcore/issues/5466), [updated SPA support](https://github.com/dotnet/aspnetcore/issues/27887), and [HTTP/3](https://github.com/dotnet/aspnetcore/issues/15271). For Preview 1, the team now supports `IAsyncDisposable` in MVC, offers a new `DynamicComponent` for Blazor to render a component based on type, and applying more nullability annotations.

For EF Core 6, the team is busy with [support for SQL Server sparse columns](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-6-0-preview-1/#support-for-sql-server-sparse-columns), required property validations for [not null for the in-memory database](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-6-0-preview-1/#support-for-sql-server-sparse-columns), improved SQL Server translation for `IsNullOrWhitespace`, a [Savepoints API](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-6-0-preview-1/#savepoints-api), and much more.

It's sure to be a busy eight months. Stay tuned.

# The little things: Dapr v1.0 is released, Azure Static Web Apps, more SolarWinds findings

This week, Dapr [hit GA with v1.0](https://blog.dapr.io/posts/2021/02/17/announcing-dapr-v1.0/). The next in a long line of technologies that promise to make distributed systems easier, we'll see if this one sticks. While it's hard for folks to pin down what exactly Dapr is—[no, seriously](https://news.ycombinator.com/item?id=26173566)—it uses pluggable components to remove the complexity of low-level plumbing involved with developing distributed applications.

Here's more detail from the new [*Dapr for .NET Developers* e-book](https://docs.microsoft.com/dotnet/architecture/dapr-for-net-developers/):

> It provides a dynamic glue that binds your application with infrastructure capabilities from the Dapr runtime. For example, your application may require a state store. You could write custom code to target Redis Cache and inject it into your service at runtime. However, Dapr simplifies your experience by providing a distributed cache capability out-of-the-box. Your service invokes a Dapr building block that dynamically binds to Redis Cache component via a Dapr configuration. With this model, your service delegates the call to Dapr, which calls Redis on your behalf. Your service has no SDK, library, or direct reference to Redis. You code against the common Dapr state management API, not the Redis Cache API.

--

In my [talk last week](https://www.meetup.com/Bournemouth-ASP-NET-Blazor-Meetup-Group/events/275962302/) on Azure Static Web Apps, it was nice to see Anthony Chu attend. He's the PM for Azure Functions and Azure Static Web Apps. We asked what's new with Azure Static Web Apps—he talked about a new tier, a CLI for a better local development experience, root domain support (right now it supports custom DNS with *https://mysite.com* but not *https://www.mysite.com*), an SLA, and more. There's no firm dates on these things, but it looks like improvements are on the way before Microsoft takes it out of preview.

--

We heard a little more about how the SolarWinds hack [hit Microsoft](https://www.reuters.com/article/idUSL1N2KO3ME):

> Microsoft said its internal investigation had found the hackers studied parts of the source code instructions for its Azure cloud programs related to identity and security, its Exchange email programs, and Intune management for mobile devices and applications. Some of the code was downloaded, the company said, which would have allowed the hackers even more freedom to hunt for security vulnerabilities, create copies with new flaws, or examine the logic for ways to exploit customer installations ... Microsoft had said before that the hackers had accessed some source code, but had not said which parts, or that any had been copied.

Microsoft [blogged](https://msrc-blog.microsoft.com/2021/02/18/microsoft-internal-solorigate-investigation-final-update/) their "final update," so that's all we'll hear about it. It looks like their defenses held up.

# Dev Discussions: Cecil Phillip

I recently had a chance to catch up with Cecil Phillip, a Senior Cloud Advocate for Microsoft. He's a busy guy: you might have seen him co-hosting [The .NET Docs Show](https://dotnet.microsoft.com/live/dotnet-docs) or the [ON.NET Show](https://channel9.msdn.com/Shows/On-NET). Cecil also does a lot with with microservices and distributed systems. I wanted to pick his brain on topics like Dapr, YARP, and Service Fabric. I hope you enjoy it.

![Cecil Phillip profile photo]({{ site.url }}{{ site.baseurl }}/assets/img/cecil-phillip.png)

> I'd love to hear about how you got in this field and ended up at Microsoft.

I’ll try to give you a condensed version. Back in Antigua, we didn’t have computers in school at the time. One day, my dad brought home a [Compaq Presario](https://en.wikipedia.org/wiki/Compaq_Presario) to upgrade from using a typewriter that he used for working on his reports. Initially, I wasn’t allowed to “experiment” with the machine, so I’d explore it when he wasn’t around. I was so curious about what this machine could do, I wanted to know what every button did—and eventually, I got better at it than he was.

I went to college and got my CS degrees. I didn’t know I wanted to be a programmer. I didn’t know many examples of what CS folks did at the time. I got my first job working on ASP.NET Web Forms applications. After I got my green card, I spent a few years exploring different industries like HR, Finance, and Education. I taught some university courses in the evenings, and it was at that point I realized how much I loved teaching kids. Then one day, I saw a tweet about a new team at Microsoft, and I thought, “Why not?” I didn’t think I’d get the job, but I’d give it a try.

> When it comes to microservices, there's been a lot of talk about promoting the idea of "loosely coupled monoliths" when the overhead of microservices might not be worth it. What are your thoughts?

Somewhere along the way, having your application get labeled as a monolith became a negative thing. In my opinion, we’ve learned a lot of interesting patterns that we can apply to both monoliths and microservices. For some solutions and teams, having a monolith is the right option. We can now pair that with the additional benefits of modern patterns to get some additional resiliency for a stable application.

> I've been reading and learning about YARP, a .NET reverse proxy. Why would I use this over something like nginx?

If you’re a .NET developer and want the ability to write custom rules or extensions for your reverse proxy/load balancer using your .NET expertise, then [YARP](https://microsoft.github.io/reverse-proxy/articles/getting_started.html) is a great tool.

> Azure Front Door seems reverse-proxy*ish*—why should I choose YARP over this?

[Azure Front Door](https://azure.microsoft.com/services/frontdoor/) is a great option if you’re running large-scale applications across multiple regions. It’s a hosted service with load balancing features, so you get things like SSL management, layer-7 routing, health checks, URL rewriting, and so on. YARP, on the other hand, is middleware for ASP.NET Core that implements some of those same reverse proxy concerns, but you’re responsible for your infrastructure and have to write code to enable your features. YARP would be more akin to something like [HAProxy](http://www.haproxy.org/) or [nginx](https://nginx.org/en/).

> A lot of enterprises, like mine, use nginx for an AKS ingress controller. Will YARP ever help with something like that?

Maybe. 🙂

> I know you've also been working with Dapr. I know it simplifies event management for microservices, but can you illustrate how it can help .NET developers and what pain points it solves?

The biggest selling point for [Dapr](https://dapr.io/) is that it provides an abstraction over common needs for microservice developers. In the ecosystem today, there are tons of options to choose from to get a particular thing done— but with that comes tool-specific SDKs, APIs, and configuration. Dapr allows you to choose the combination of tools you want without needing to have your code rely on tool-specific SDKs. That includes things like messaging, secrets, service discovery, observability, actors, and so on.

> How does Dapr compare with something like Service Fabric? Is there overlap, and do you see Dapr someday replacing it?

I look at Dapr and [Service Fabric](https://azure.microsoft.com/en-us/services/service-fabric/) as two completely different things. Dapr is an open-source, cross-platform CLI tool that provides building blocks for making microservice development easier. Service Fabric is a hosted Azure service that helps with packaging and deploying distributed applications. Dapr can run practically anywhere—on any cloud, on-premises, and Kubernetes. You can even run it on Service Fabric clusters since Service Fabric can run processes. It can run containers and even contains a programming model that applications can use as a hard dependency. Dapr itself is a process that would run beside your application’s various instances to provide some communication abstractions.

> What is your one piece of programming advice?

I think it’s important to spent time reading code. We should actually read more code than we write. There’s so we can learn and be inspired by exploring the techniques used by the developers that have come before us.

*You can connect with Cecil Phillip [on Twitter](https://twitter.com/cecilphillip).*

# 🌎 Last week in the .NET world

## 🔥 The Top 3

- Niels Swimberghe [uses Project Tye to host Blazor WASM and ASP.NET Web API on a single origin to avoid CORS](https://swimburger.net/blog/dotnet/use-project-tye-to-host-blazor-wasm-and-aspdotnet-web-api-on-a-single-origin) and also [deploys Blazor WebAssembly to AWS Amplify](https://swimburger.net/blog/dotnet/how-to-deploy-blazor-webassembly-to-aws-amplify).
- Andrew Lock [uses source generators to generate a menu component in a Blazor app](https://andrewlock.net/using-source-generators-to-generate-a-nav-component-in-a-blazor-app/).
- Steve Gordon [writes about the IServiceCollection and dependency injection](https://www.stevejgordon.co.uk/aspnet-core-dependency-injection-what-is-the-iservicecollection).

## 📢 Announcements

- .NET 6 preview 1 is out: Richard Lander [has the main announcement](https://devblogs.microsoft.com/dotnet/announcing-net-6-preview-1), Sourabh Shirhatti [talks about ASP.NET Core updates](https://devblogs.microsoft.com/aspnet/asp-net-core-updates-in-net-6-preview-1), and Jeremy Likness [announces what's new with EF Core 6](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-6-0-preview-1)
- Tara Overfield [releases the .NET Framework monthly cumulative update](https://devblogs.microsoft.com/dotnet/net-framework-february-2021-cumulative-update-preview-for-net-framework).
- The Azure SDK folks [provide their monthly update](https://devblogs.microsoft.com/azure-sdk/february-release-2021).
- Dapr v1.0 [released this week](https://blog.dapr.io/posts/2021/02/17/announcing-dapr-v1.0/), and the Dapr folks [talk about the community effort to deliver Dapr v1.0](https://cloudblogs.microsoft.com/opensource/2021/02/17/the-community-effort-that-delivered-dapr-v1-0). (.NET 6 Preview 1 stole the show this week, and I'll address this in detail next week.)
- GitHub Enterprise Server 3.0 [is generally available](https://github.blog/2021-02-16-github-enterprise-server-3-0-is-now-generally-available/).

## 📅 Community and events

- Chris Sainty [hits a million Blazored downloads](https://chrissainty.com/blazored-hits-1-000-000-downloads-on-nuget/).
- For standups this week: .NET Tooling talks ... [tools](https://www.youtube.com/watch?v=sGkW9AkuJag), Machine Learning [looks at ML.NET APIs](https://www.youtube.com/watch?v=Vmgv_HJmJkE), and ASP.NET [discusses flexible HTTP APIs](https://www.youtube.com/watch?v=xoYkk5jk8d0).
- The .NET Docs Show [talks with Daniel Krzyczkowski](https://www.youtube.com/watch?v=IGJY2FW-gy0) about the Microsoft Identity Platform, but I really wonder: does he work inside of a GitHub contribution graph?

## 🌎 Web development

- David Jones [writes about using .Includes in EF with a Blazor Server app](https://davidjones.sportronics.com.au/blazor/Blazor_Helpers_App-More_on_Includes_with_Selections-blazor.html).
- Josef Ottosson [offers his thoughts on the Result class](https://josef.codes/my-take-on-the-result-class-in-c-sharp/).
- Sam Basu [writes about how to make Blazor web apps for Desktop](https://www.telerik.com/blogs/blazor-on-desktop).
- Dave Brock (hi!) [builds a Blazor 'Copy to Clipboard' component](https://daveabrock.com/2021/02/18/copy-to-clipboard-markdown-editor).
- Damien Bowden [works on requiring user password verification with ASP.NET Core identity to access Razor Pages](https://damienbod.com/2021/02/19/require-user-password-verification-with-asp-net-core-identity-to-access-razor-page/).
- Shawn Wildermuth [offers a mea culpa on hosting SPAs in ASP.NET Core](http://wildermuth.com/2021/02/15/I-Was-Wrong---Hosting-SPAs-in-ASP-NET-Core).
- David Grace [continues work on his Twitter dashboard .NET 5 app](https://www.roundthecode.com/dotnet/blazor/blash-twitter-dashboard-net-5-app-source-code-twitter-api).
- Marinko Spasojevic [works on fetching data and content negotiation with HttpClient in ASP.NET Core](https://code-maze.com/fetching-data-with-httpclient-in-aspnetcore/).

## 🥅 The .NET platform

- Khalid Abuhakmeh [fixes .NET dependencies with monkey patching](https://khalidabuhakmeh.com/fix-dotnet-dependencies-with-monkey-patching).
- Nick Randolph [builds an Android app with .NET 6](https://nicksnettravels.builttoroam.com/android-net-6/).
- Mitchel Sellers [writes about deterministic builds and source linking in .NET 5](https://www.mitchelsellers.com/blog/article/net-5-deterministic-builds-source-linking).

## ⛅ The cloud

- Justin Yoo [writes about bulk managing Key Vault secret rotation](https://dev.to/azure/keyvault-secrets-rotation-management-in-bulk-5fl1).
- Tim Sander [writes about new ways to use composite indexes in Azure Cosmos DB](https://devblogs.microsoft.com/cosmosdb/new-ways-to-use-composite-indexes).
- Gregor Suttie [uses Azure Durable Functions to place phone calls when alerts trigger](https://gregorsuttie.com/2021/02/15/azure-durable-functions-support-caller/).
- Adam Storr [controls logging levels in Azure Functions](https://adamstorr.azurewebsites.net/blog/controlling-the-logging-levels-in-azure-functions).
- Damien Bowden [adds ASP.NET Core authorization for an Azure Blob Storage and Azure AD users for role assignments](https://damienbod.com/2021/02/16/adding-asp-net-core-authorization-for-an-azure-blob-storage-and-azure-ad-users-using-role-assignments/).
- Dominique St-Amand [has some FAQs and tips for Azure Functions in C#](https://www.domstamand.com/common-azure-functions-in-csharp-quick-faqs-and-tips/).
- Brandon Minnick [creates Azure Functions using .NET 5](https://codetraveler.io/2021/02/12/creating-azure-functions-using-net-5/).

## 📔 Languages

- Thomas Claudius Huber [writes about improved pattern matching with C# 9](https://www.thomasclaudiushuber.com/2021/02/18/c-9-0-improved-pattern-matching/).
- Michał Białecki [writes about C# tuples](https://www.michalbialecki.com/2021/02/13/returning-more-than-one-result-from-a-method-tuple/).
- Jason Roberts [works on C# nested switch expressions](http://dontcodetired.com/blog/post/ICYMI-C-8-New-Features-Nested-Switch-Expressions).
- Khalid Abuhakmeh [plays the Super Mario Bros. theme with C#](https://khalidabuhakmeh.com/playing-the-super-mario-bros-theme-with-csharp).

## 🔧 Tools

- Scott Hanselman [writes about DotNet Boxed for prescriptive templates for .NET Core](https://www.hanselman.com/blog/dotnet-boxed-includes-prescriptive-templates-for-net-core).
- Nick Randolph [works on WinUI 3 debugging tooling with Visual Studio](https://nicksnettravels.builttoroam.com/winui-3-0-debugging-tooling-with-visual-studio/).
- Thomas Ardal [exports data from Google Analytics with .NET](https://blog.elmah.io/export-data-from-google-analytics-with-net/).
- Jonathan Bowman [installs Docker on WSL without Docker Desktop](https://dev.to/bowmanjd/install-docker-on-windows-wsl-without-docker-desktop-34m9).

## 📱 Xamarin

- Mohammed Ismail Sameer Mohamed Saleem [helps you avoid common mistakes in Xamarin development](https://www.syncfusion.com/blogs/post/10-tips-to-avoid-common-mistakes-in-xamarin-forms-app-development.aspx).
- Luis Matos [creates a scalable style library in Xamarin Forms](https://luismts.com/style-library-xamarin-forms/).
- Denys Fiediaiev [works on pop-up validation in Xamarin with MvvmCross](https://prin53.medium.com/pop-up-validation-in-xamarin-with-mvvmcross-b035f0193ba4).
- Leomaris Reyes [adds simple navigation in Xamarin Forms](https://www.telerik.com/blogs/applying-simple-navigation-xamarin-forms).

## 🏗 Design, testing, and best practices

- Derek Comartin [explains event sourcing](https://codeopinion.com/event-sourcing-example-explained-in-plain-english/).
- Steve Smith [compares DTOs and POCOs](https://ardalis.com/dto-or-poco/).

## 🎤 Podcasts

- The .NET Core Podcast [talks to Barry Luijbregts about picking the right Azure resources](https://dotnetcore.show/episode-70-picking-the-right-azure-resources-with-barry-luijbregts/).
- Serverless Chats [talk to Jeff Hollan about Azure Functions](https://www.serverlesschats.com/88/).
- The Adventures in .NET podcast [discusses HotChocolate and StrawberryShake](https://devchat.tv/adventures-in-dotnet/net-056-how-does-hotchocolate-and-strawberryshake-relate-to-net-and-graphql/).
- The 6-Figure Developer podcast [discusses developer velocity with Amanda Silver](https://6figuredev.com/podcast/episode-183-developer-velocity-with-amanda-silver/).

## 🎥 Videos

- Data Exposed [talks about how Azure SQL enables real-time analytics](https://channel9.msdn.com/Shows/Data-Exposed/How-Azure-SQL-Enables-Real-time-Operational-Analytics-HTAP-Part-1).
- Leslie Richardson [talks to Beth Massi about the upcoming .NET Conf](https://channel9.msdn.com/Shows/Visual-Studio-Toolbox/NET-Conf-Focus-on-Windows).
- AzureFunBytes [introduces Dapr on Azure](https://devblogs.microsoft.com/devops/azurefunbytes-dapr-on-azure).
- The Xamarin Show [talks about the MultiConverter and the VariableMultiValueConverter](https://channel9.msdn.com/Shows/XamarinShow/Xamarin-Community-Toolkit-MultiConverter--VariableMultiValueConverter).
- At Technology and Friends, [David Giard talks with Julie Lerman](https://www.davidgiard.com/2021/02/15/JulieLermanOnEntityFrameworkCore5.aspx).
- Azure Friday [builds modern hybrid apps with Azure Arc and Azure Stack](https://channel9.msdn.com/Shows/Azure-Friday/Building-modern-hybrid-applications-with-Azure-Arc-and-Azure-Stack).
- The ASP.NET Monsters [diagnose startup issues in Azure App Service](https://www.youtube.com/watch?v=1xLlUlmLgYs).
- The ON.NET Show [creates GraphQL APIs with Hot Chocolate](https://www.youtube.com/watch?v=LfPc0sitoR4).

