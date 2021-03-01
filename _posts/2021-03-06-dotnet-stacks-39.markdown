---
date: "2021-02-27"
title: "The .NET Stacks #39: 🔥 Is Dapr worth the hype?"
tags: [dotnet-stacks]
comments: false
image: /assets/img/stacks-38-card.png 
description: This week, is Dapr for real?
---

![Newsletter image](https://daveabrock.com/THE .NET STACKS.png)

Buckle up! It's been a busy few weeks. With [Ignite this week](https://myignite.microsoft.com/), things aren't slowing down anytime soon. I've got you covered. Let's get to it.

- **One big thing**: Is Dapr worth the hype?
- **The little things**: Blazor Desktop, .NET 5 on Azure Functions, ASP.NET Core health checks
- Last week in the .NET world

# One big thing: Is Dapr worth the hype?

I mentioned Dapr [in passing last week](https://daveabrock.com/2021/02/27/dotnet-stacks-38), but its release competed with [the release of .NET 6 Preview 1](https://daveabrock.com/2021/02/27/dotnet-stacks-38#one-big-thing-net-6-gets-preview-1). I've been spending the last week trying to understand what exactly it is, and the *Dapr for .NET Developers* e-book [has been mostly helpful](https://docs.microsoft.com/dotnet/architecture/dapr-for-net-developers/?WT.mc_id=-blog-scottha). With Ignite kicking off on Tuesday, you're going to start hearing a lot more about it. (If you don't work with microservices, feel free to scroll to the next section.)

Dapr is a runtime for your microservice environment—in .NET terms, you could call it a "BCL for the cloud." It provides abstractions for you to work with the complexities of distributed applications. Dapr calls these pluggable components. Think about all the buckets your distributed systems have: authentication/authorization, pub/sub messaging, state management, invoking services, observability, secrets management, and so on. Your services can call these pluggable components directly, and Dapr deals with calling all these dependencies on your behalf. For example, to call Redis you call the Dapr state management API. You can call Dapr from its native REST or gRPC APIs and also language-specific SDKs. I *love* the idea of calling pub/sub over HTTP and not haggling with specific message broker implementations.

Dapr uses a sidecar architecture, enabling it to run in a separate memory process. Dapr says this provides isolation—Dapr can connect to a service but isn't dependent on it. Each service can then have its own runtime environment. It can run in your existing environment, the edge, and Kubernetes (it's built for containerized environments). While shepherded for Microsoft, it's pretty agnostic and isn't only built for Azure (but that might be lost in the Microsoft messaging). With Dapr, services communicate through encrypted channels, service calls are automatically retried when transient errors occur, and automatic service discovery reduces the amount of configuration needed for services to find each other.

While Dapr is service mesh-*like*, it is more concerned with handling distributed application features and is not dedicated network infrastructure. Yes, Dapr is a proxy. But if you're in the cloud, breaking news: you're using proxies whether you like it or not.

Dapr promises to handle the tricky parts for you through a consistent interface. Sure, you can do retries, proxies, network communication, and pub/sub on your own, but it's probably a lot of duct tape and glue if you have a reasonably complex system.

With the [release of Dapr v1.0](https://blog.dapr.io/posts/2021/02/17/announcing-dapr-v1.0/), it's production-ready. Will this latest "distributed systems made easy" offering solve all your problems? Of course not. Dapr uses the calls over the highly-performant gRPC, but [that's a lot of network calls](https://docs.microsoft.com/dotnet/architecture/dapr-for-net-developers/dapr-at-20000-feet#dapr-performance-considerations). The line *To increase performance, developers can call the Dapr building blocks with gRPC* needs some unpacking. The team discusses low latency but will REST be enough for all its chatter? Are you ready to hand your keys to yet another layer of abstraction? Would you be happy with a cloud within your cloud? Does your app have scale and complexity requirements that make this a necessity? Are you worried about leaky abstractions?

There's a lot going on here, and I plan to explore more. If you wanted to learn more about Dapr, I hope this gets the ball rolling for you.

***

# The little things: Blazor Desktop, .NET 5 on Azure Functions, health checks

With the release of [.NET 6 Preview 1 last week](https://devblogs.microsoft.com/dotnet/announcing-net-6-preview-1/), one of the most interesting takeaways was the mention of Blazor desktop apps. (It wasn't part of Preview 1, but a preview of what's to come for .NET 6.) As if we didn't have enough desktop dev options to worry about—WPF, UWP, WinUI, .NET MAUI, WinForms, and so on–where does this even fit?

Here's [what Richard Lander wrote](https://devblogs.microsoft.com/dotnet/announcing-net-6-preview-1/#blazor-desktop-apps):

> Blazor has become a very popular way to write .NET web apps. We first supported Blazor on the server, then in the browser with WebAssembly, and now we’re extending it again, to enable you to write Blazor desktop apps. Blazor desktop enables you to create hybrid client apps, which combine web and native UI together in a native client application. It is primarily targeted at web developers that want provide rich client and offline experiences for their users.

Initially, Blazor Desktop will not utilize WebAssembly. Built on top of the .NET MAUI platform coming with .NET 6, it'll use that stack to use native containers and controls. You can choose to use Blazor for your entire desktop app or only for targeted functionality—in the blog post, Lander mentions a Blazor-driven user profile page integrated with an otherwise native WPF app.

It appears this will work similarly to Electron. There will be a WebView control responsible for rendering content from an embedded Blazor web server, which can serve both Blazor components and other static assets. If you're hoping to see one unifying platform for native development, I wouldn't hold your breath—but if you like working with Blazor (especially if you're apprehensive of XAML), it'll be worth a try.

***

In this week's shameless plug, I [wrote about using Azure Functions with .NET 5](https://daveabrock.com/2021/02/24/functions-dotnet-5). A new out-of-process model is in preview. Here's the story behind it:

>Traditionally, .NET support on Azure Functions has been tied to the Azure Functions runtime. You couldn’t just expect to use the new .NET version in your Functions as soon as it was released. Because .NET 5 is not LTS, and Microsoft needs to support specific releases for extended periods, they can’t upgrade the host to a non-LTS version because it isn’t supported for very long (15 months from the November 2020 release). This doesn’t mean you can’t use .NET 5 with your Azure Functions. To do so, the team has rolled out a new out-of-process model that runs a worker process along the runtime. Because it runs in a separate process, you don’t have to worry about runtime and host dependencies. Looking long-term: it provides the ability to run the latest available version of .NET without waiting for a Functions upgrade.

As its in early preview, it does take some work to get going with it, but ultimately it's great news for getting Azure Functions to support new .NET releases much sooner.

***

At work, we had a hackathon of sorts to build an authentication API to validate Azure runbooks, and roll it out to production in two weeks (a *mostly* fun exercise). I hadn't built out an ASP.NET Core Web API from scratch in awhile, and [implemented health checks](https://docs.microsoft.com/aspnet/core/host-and-deploy/health-checks?view=aspnetcore-5.0).

Of course, I knew you could add an endpoint in your middleware for a simple check:

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseRouting();
    app.UseEndpoints(endpoints =>
    {
        endpoints.MapHealthChecks("/api/healthcheck");
    });
}
```

There's a [ton of configuration options](https://docs.microsoft.com/aspnet/core/host-and-deploy/health-checks?view=aspnetcore-5.0#health-check-options) at your disposal, and after reading Kevin Griffin's [timely blog post](https://consultwithgriff.com/monitoring-aspnet-core-application-health-with-health-checks/), I learned about the `AspNetCore.Diagnostics.HealthChecks` library. Did you [know about this](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks)? Where have I been? You can plug into health check packages for widely used services like Kubernetes, Redis, Postgres, and more. There's even a [UI you can leverage](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks#HealthCheckUI). In case you weren't aware—maybe you were—I hope you find it helpful.

***

# 🌎 Last week in the .NET world

## 🔥 The Top 4

- Andrew Lock [uses source generators with a custom attribute to generate a menu component in a Blazor app](https://andrewlock.net/using-source-generators-with-a-custom-attribute--to-generate-a-nav-component-in-a-blazor-app/).
- Khalid Abuhakmeh [writes about EF Core 5 pitfalls and ideas](https://blog.jetbrains.com/dotnet/2021/02/24/entity-framework-core-5-pitfalls-to-avoid-and-ideas-to-try/).
- Jon Galloway [generates API clients using Visual Studio Connected Services](https://devblogs.microsoft.com/aspnet/generating-http-api-clients-using-visual-studio-connected-services).
- Brady Gaster [builds apps with Azure API Management, Functions, Power Apps, and Logic Apps](https://devblogs.microsoft.com/aspnet/app-building-with-azure-api-management-functions-power-apps-and-logic-apps).

## 📢 Announcements

- The nuget.org repository [signing certificate will be updated as soon as March 15](https://devblogs.microsoft.com/nuget/the-nuget-org-repository-signing-certificate-will-be-updated-as-soon-as-march-15th-2021).
- Jimmy Bogard [releases the extensions for OpenTelemetry 1.0](https://jimmybogard.com/opentelemetry-1-0-extensions-released/).

## 📅 Community and events

- Microsoft Ignite [starts on Tuesday](https://myignite.microsoft.com/home).
- Check out the [entire stream](https://www.youtube.com/watch?v=mZRNjixZEMg) of this week's .NET Conf ("Focus on Windows").
- The .NET Docs Show talks to [Mika Dumont about Roslyn analyzers](https://www.youtube.com/watch?v=qY2u1mWgt_I).
- For standups this week, ASP.NET [talks about building HTTP APIs](https://www.youtube.com/watch?v=Mpf0fCO6NrU) and EF [talks about performance tuning](https://www.youtube.com/watch?v=VgNFFEqwZPU).
- The .NET team [wants you to fill out a "State of .NET" survey](https://devblogs.microsoft.com/dotnet/survey-library-open-source).
- The .NET Foundation [provides a January/February 2021 update](https://dotnetfoundation.org/blog/2021/02/24/blog/posts/net-foundation-january-february-2021-update).
- Uno Platform [is sponsoring the .NET Foundation](https://platform.uno/blog/uno-platform-sponsors-net-foundation/).
- The .NET Foundation [rolls out a speakers directory](https://dotnetfoundation.org/blog/2021/02/19/blog/posts/announcing-the-dot-net-foundation-speakers-directory).

## 🌎 Web development

- The Uno Platform [migrates Silverlight apps to WinUI + Uno Platform](https://platform.uno/blog/migrating-silverlight-apps-to-winui-uno-platform-xaml-in-the-browser/).
- David Grace [writes about implementing dependency injection in ASP.NET Core](https://www.roundthecode.com/dotnet/how-to-implement-dependency-injection-in-asp-net-core).
- Kevin W. Griffin [monitors ASP.NET Core applications with health checks](https://consultwithgriff.com/monitoring-aspnet-core-application-health-with-health-checks/).
- Marinko Spasojevic [uses streams with HttpClient to improve performance](https://code-maze.com/using-streams-with-httpclient-to-improve-performance-and-memory-usage/), and also [uses HttpClient to send HTTP PATCH requests in ASP.NET Core](https://code-maze.com/using-httpclient-to-send-http-patch-requests-in-asp-net-core/).
- Matthew MacDonald [writes about Blazor Desktop](https://medium.com/young-coder/blazor-desktop-the-electron-for-net-ecdcf5c30027), and [so does David Ramel](https://visualstudiomagazine.com/articles/2021/02/25/app-model.aspx).
- Thomas Ardal [implements two-factor implementation in ASP.NET Core](https://blog.elmah.io/how-to-implement-two-factor-authentication-with-asp-net-core/).
- Camilo Reyes [integrates the Create React app with .NET 5](https://www.red-gate.com/simple-talk/dotnet/net-tools/integrate-create-react-app-with-net-core-5/).
- Matt Watson [shows off how to monitor performance in ASP.NET](https://stackify.com/asp-net-performance-tools-you-need-to-know/).
- Imar Spaanjaars [continues building and auto-deploying an ASP.NET Core app](https://imar.spaanjaars.com/620/building-and-auto-deploying-an-aspnet-core-application-part-3-dealing-with-change).
- Niels Swimberghe [deploys Blazor WebAssembly to AWS Amplify](https://swimburger.net/blog/dotnet/how-to-deploy-blazor-webassembly-to-aws-amplify) and [also Heroku](https://swimburger.net/blog/dotnet/how-to-deploy-blazor-webassembly-to-heroku).

## 🥅 The .NET platform

- Szymon Kulec [writes about .NET performance investigations](https://blog.scooletz.com/2021/02/23/performance-investigations).
- Hannes Du Preez [writes about how Microsoft Win32 APIs have become more .NET compatible](https://www.developer.com/net/net/microsoft-win32-apis-become-more-.net-compatible.html).
- Steve Gordon [writes about DI and the IServiceProvider](https://www.stevejgordon.co.uk/aspnet-core-dependency-injection-what-is-the-iserviceprovider-and-how-is-it-built).
- Bruno Capuano [configures a single-file app in .NET 6](https://elbruno.com/2021/02/24/net6-single-file-apps-improved-for-windows-and-mac/).
- Nick Randolph [builds a roadmap for Windows development](https://nicksnettravels.builttoroam.com/windows-developer-roadmap/).
- Mark Heath [writes about porting versus interop for .NET](https://markheath.net/post/porting-interop-and-webassembly).
- Joe Mayo [writes about getting .NET 6 (for Mac)](https://joemayo.medium.com/getting-net-6-8776d03798ec).
- Elton Stoneman [experiments with .NET 5 and 6 using Docker containers](https://blog.sixeyed.com/experimenting-with-net-5-and-6-using-docker-containers/).

## ⛅ The cloud

- Thiago Custódio [talks with Jeffrey Richter about the Azure .NET SDKs](https://www.infoq.com/news/2021/02/richter-azure-net-sdk/).
- Brandon Minnick [creates Azure Functions with .NET 5](https://techcommunity.microsoft.com/t5/apps-on-azure/preview-creating-azure-functions-using-net-5/ba-p/2156846), and [so does Dave Brock](https://daveabrock.com/2021/02/24/functions-dotnet-5).
- Daniel Krzyczkowski [integrates his app with the Graph SDK and Azure AD B2C](https://daniel-krzyczkowski.github.io/Cars-Island-Azure-Functions-Integration-With-Microsoft-Graph/).
- Gregor Suttie [troubleshoots App Services in Azure](https://gregorsuttie.com/2021/02/22/troubleshooting-app-services-in-azure/).
- Anthony Chu [builds and deploys to Azure Static Web Apps with Stackbit](https://techcommunity.microsoft.com/t5/apps-on-azure/build-and-deploy-to-azure-static-web-apps-with-stackbit/ba-p/2116687).

## 📔 Languages

- Yacoub Massad [works through F# language features using a Toc-Tac-Toe example](https://www.dotnetcurry.com/fsharp/fsharp-features-part-1).
- Thomas Claudius Huber [writes about switch expression pattern matching in C# 9](https://www.thomasclaudiushuber.com/2021/02/25/c-9-0-pattern-matching-in-switch-expressions/).
- Jason Roberts [simplifies array access and range code in C#](http://dontcodetired.com/blog/post/ICYMI-C-8-New-Features-Simplify-Array-Access-and-Range-Code).
- Nikos Vaggalis [explains design patterns in C# with food](https://www.i-programmer.info/news/231-methodology/14358-design-patterns-explained-with-food-in-c.html).

## 🔧 Tools

- Michał Białecki [works on static refactoring with Visual Studio regular expressions](https://www.michalbialecki.com/2021/02/23/static-refactoring-with-visual-studio-regular-expressions/).
- Jon P. Smith [offers five levels of performance tuning for an EF Core query](https://www.thereformedprogrammer.net/five-levels-of-performance-tuning-for-an-ef-core-query/).
- David Ramel [writes about Dapr](https://visualstudiomagazine.com/articles/2021/02/22/dapr1.aspx).
- Khalid Abuhakmeh [writes about EF Core 5 value converters](https://khalidabuhakmeh.com/entity-framework-core-5-value-converters).
- Jason Gaylord [adds a pipeline expression value to an Azure DevOps variable](https://www.jasongaylord.com/blog/2021/02/25/add-expression-in-azure-devops-variable).
- Khalid Abuhakmeh [installs Tailwind CSS with ASP.NET Core](https://khalidabuhakmeh.com/install-tailwind-css-with-aspnet-core).
- The Edge team [writes about six time-saving tips when using the DevTools Console](https://blogs.windows.com/msedgedev/2021/02/23/six-time-saving-tips-edge-devtools-console).

## 📱 Xamarin

- Gerald Versluis [writes about building beautiful tabs with the Xamarin Community Toolkit](https://devblogs.microsoft.com/xamarin/xamarin-community-toolkit-tabview).
- Leomaris Reyes [uses push notifications in Xamarin Forms](https://www.telerik.com/blogs/how-to-use-push-notifications-xamarin-forms), and also  [replicates a food delivery UI](https://askxammy.com/replicating-food-delivery-ui-in-xamarin-forms/).

## 🎤 Podcasts

- The Adventures in .NET podcast [talks with Jimmy Bogard](https://devchat.tv/adventures-in-dotnet/net-057-open-source-mediatr-and-automapper-with-jimmy-bogard/).
- The 6-Figure Developer podcast [talks to Faheem Memon and Facundo Gauna about going cloud-native](https://6figuredev.com/podcast/episode-184-cloud-native-with-facundo-and-faheem/).
- The .NET Rocks podcast [talks to Mark Rendle about migrating WCF](https://www.dotnetrocks.com/default.aspx?ShowNum=1728).

## 🎥 Videos

- The ASP.NET Monsters [avoid SSL expirations](https://www.youtube.com/watch?v=hHLZqBXuIZE).
- The ON .NET Show talks [about Dapr with Ryan Nowak](https://www.youtube.com/watch?v=kIfmwmJHNMs).