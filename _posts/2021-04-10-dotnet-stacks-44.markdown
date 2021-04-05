---
date: "2021-04-10"
title: "The .NET Stacks #44: 🐦 APIs that are light as a feather"
tags: [dotnet-stacks]
image: /assets/img/stacks-44-card.png 
description: This week, we discuss the FeatherHttp project and Azure Static Web Apps.

---

![Newsletter image](https://daveabrock.com/THE .NET STACKS.png)

Happy Monday! Here's what we're talking about this week:

- **One big thing**: Looking at the FeatherHttp project
- **The little things**: Azure Static Web apps with DevOps, new OSS badges, coding tip
- Last week in the .NET world

***

# One big thing: Looking at the `FeatherHttp` project

We've talked in the past about ASP.NET Core MVC APIs and their role in the .NET Core ecosystem. While MVC has received performance improvements and does what it sets out to do, it carries a lot of overhead and is often reminiscent of the "here you go, have it all" reputation of .NET Framework. It's a robust solution that allows you to build complex APIs but comes with a lot of ceremony. With imperative frameworks like Go and Express, you can get started immediately and with little effort. No one has said the same about writing APIs in ASP.NET Core. It's a bad look on the framework in general, especially when folks want to try out .NET for the first time.

Last year, Dapr pushed cross-platform samples for the major frameworks, and this tweet shows a common theme with MVC:

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">It does a *REALLY* bad job at showing off C#. The example has 20+ files while all the others have 2-5 🤦‍♂️</p>&mdash; Kristian Hellang (@khellang) <a href="https://twitter.com/khellang/status/1198530726100164608?ref_src=twsrc%5Etfw">November 24, 2019</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

Can we have solutions that allow you to get started quickly and avoid this mess? What if you aren't a fan of controllers? You could use external libraries like [MediatR](https://github.com/jbogard/MediatR) or [API Endpoints](https://github.com/ardalis/ApiEndpoints), or a [framework like Nancy](https://nancyfx.org/)—but it still feels like the native runtime deserves better. The ASP.NET Core team has [thought about this](https://twitter.com/davidfowl/status/1198870972679372800) for a while.

The [route-to-code alternative](https://docs.microsoft.com/aspnet/core/web-api/route-to-code?view=aspnetcore-5.0) is a great start, which [I wrote about](https://daveabrock.com/2020/12/04/migrate-mvc-to-route-to-code). It allows you to write simple JSON APIs, using an API endpoints model—with some helper methods that lend a hand.

Here's a quick example:

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapGet("/hello/{name:alpha}", async context =>
    {
        var name = context.Request.RouteValues["name"];
        await context.Response.WriteAsJsonAsync(new { message = $"Hello {name}!" });
    });
});
```

It's great, but Microsoft will tell you it's only for simple APIs. Their docs [clearly state](https://docs.microsoft.com/aspnet/core/web-api/route-to-code?view=aspnetcore-5.0#notable-missing-features-compared-to-web-api): *Route-to-code is designed for basic JSON APIs. It doesn't have support for many of the advanced features provided by ASP.NET Core Web API.* This begs the question: will .NET ever have a modern, lightweight API solution that has a low barrier to entry and also scales? Can I have a lightweight API that starts small and allows me to add complex features as I need them?

That is the goal of [FeatherHttp](https://github.com/featherhttp/framework), a project from ASP.NET Core architect David Fowler. Triggered by Kristian's tweet, Fowler says the repo has three key goals: to be built on the same primitives on .NET Core, to be optimized to build HTTP APIs quickly, and having the ability to take advantage of existing .NET Core middleware and frameworks. According to a [GitHub comment](https://github.com/featherhttp/framework/issues/3#issuecomment-564403379), the solution uses 90% of ASP.NET Core and changes the Startup pattern to be more lightweight. You can also [check out a tutorial](https://github.com/featherhttp/tutorial) that walks you through building the backend of a React app with basic CRUD APIs and some serialization.

Here's an example:

```csharp
using System.Threading.Tasks;
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Http;

var app = WebApplication.Create(args);

app.MapGet("/", async http =>
{
    await http.Response.WriteAsync("Hello World");
});

await app.RunAsync();
```

Is this a fun experiment (currently at version *0.1.82-alpha*) or will it make its way into ASP.NET Core? I'm not a mind reader, my friends, but I do know two things: (1) David Fowler is the partner architect on ASP.NET Core, and (2) big objectives for .NET 6 are [to appeal to new folks](https://github.com/dotnet/core/issues/5465) and to [improve the developer inner-loop experience](https://github.com/dotnet/core/issues/5510). I suspect we'll be hearing a lot more about this. Stay tuned.

***

# The little things: Azure Static Web apps with DevOps, new OSS badges, code complexity tip

Last week, Microsoft announced that Azure Static Web Apps supports deployment through Azure DevOps YAML pipelines. I [wrote about it as well](https://daveabrock.com/2021/04/01/static-web-apps-azure-pipelines). It opens doors for many corporate customers who aren't ready to move to GitHub yet—while they've made great strides, GitHub still has some work to do to match the robust enterprise capabilities of Azure DevOps.

I was able to move one of my projects over seamlessly. Unlike GitHub Actions, Azure DevOps handles PR triggers for you automatically, so my YAML is pretty clean:

```yaml
trigger:
  - main

pool:
  vmImage: 'ubuntu-latest'

steps:
  - task: AzureStaticWebApp@0
    inputs:
      app_location: "BlastOff.Client"
      api_location: "BlastOff.Api"
      output_location: "wwwroot"
    env:
      azure_static_web_apps_api_token: $(deployment_token)
```

While it isn’t as streamlined and elegant as the GitHub experience—you need to configure your deployment token manually, and you don’t get automatic staging environments—this should help improve adoption. If you're wondering whether to use Azure Static Web Apps with Azure DevOps or GitHub, [I've got you covered](https://daveabrock.com/2021/04/01/static-web-apps-azure-pipelines#should-i-use-azure-static-web-apps-with-azure-devops-or-github).

***

The ASP.NET Core team [has introduced](https://twitter.com/ben_a_adams/status/1376587282413711364) "good first issue" and "Help wanted" GitHub badges. If you've ever wanted to contribute to ASP.NET Core but didn't know where to start, this might help.

***

Sometimes, it seems that processing a collection of objects is half of a developer's job. It can often be a subject of abuse, especially when it comes to nested loops and their poor performance.

Here's an example of me iterating through a list of `Blogger` objects, checking if a `Url` exists, and adding it to a list. (I'm using [records](https://daveabrock.com/2020/07/06/c-sharp-9-deep-dive-records) and [target-typed expressions](https://daveabrock.com/2020/07/14/c-sharp-9-target-typing-covariants#improved-target-typing) for brevity.)

```csharp
using System.Collections.Generic;

var result = new List<string>();
var bloggers = new List<Blogger>
{
    new("Dave Brock", "https://daveabrock.com", true),
    new("James Clear", "https://jamesclear.com", false)
};

foreach (var blogger in bloggers)
{
    if (blogger.IsTechBlogger)
    {
        var url = blogger.Url;
        if (url is not null)
            result.Add(url);
    }
}

record Blogger(string Name, string Url, bool IsTechBlogger);
```

Instead, try a [LINQ collection pipeline](https://www.martinfowler.com/articles/refactoring-pipelines.html):

```csharp
using System.Collections.Generic;
using System.Linq;

var urlList = new List<string>();
var bloggers = new List<Blogger>
{
    new("Dave Brock", "https://daveabrock.com", true),
    new("James Clear", "https://jamesclear.com", false)
};

urlList = bloggers.Where(b => b.IsTechBlogger)
                  .Select(u => u.Url)
                  .Where(u => u is not null).ToList();

record Blogger(string Name, string Url, bool IsTechBlogger);
```

***

# 🌎 Last week in the .NET world

## 🔥 The Top 4

- Nish Anil [writes about monitoring and observability in cloud-native ASP.NET Core apps](https://devblogs.microsoft.com/aspnet/monitoring-and-observability-in-cloud-native-asp-net-core-apps).
- Dave Brock [uses Azure Static Web Apps with Azure DevOps pipelines](https://daveabrock.com/2021/04/01/static-web-apps-azure-pipelines).
- Michael Shpilt [maximizes the power of logs as applications scale](https://michaelscodingspot.com/maximizing-the-power-of-logs-as-your-application-scales/).
- Vladimir Khorikov asks: [are static methods evil?](https://enterprisecraftsmanship.com/posts/static-methods-evil/)

## 📢 Announcements

- Azure Static Web Apps [now supports deployment with Azure DevOps](https://azure.microsoft.com/updates/public-preview-azure-static-web-apps-now-supports-deployment-with-azure-devops).
- GitHub Desktop [now supports cherry-picking](https://github.blog/2021-03-30-github-desktop-now-supports-cherry-picking/).
- Alvin Ashcraft [has released his book on WinUI 3.](https://mailchi.mp/e7c5133c9388/learn-winui-3-book-and-ebook-now-available?e=0eceb6f972&_lrsc=3fe8d675-aafa-4cf2-a5af-e2b4de71c460).
- Nicholas Blumhardt [announces Serilog Expressions 2.0](https://nblumhardt.com/2021/03/serilog-expressions-2/).
- PostSharp 6.9 is live [with Visual Studio Tooling performance improvements](https://blog.postsharp.net/post/postsharp-6-9-visual-studio-tooling-performance-improvements.html).
- Andrew Clinick [announces Project Reunion 0.5](https://blogs.windows.com/windowsdeveloper/2021/03/29/announcing-project-reunion-0-5).
- .NET Standard 1.3 is [no longer supported in AWS SDK for .NET version 3.7](https://aws.amazon.com/blogs/developer/net-standard-1-3-is-no-longer-supported-in-aws-sdk-for-net-version-3-7/).
- David Guida's [OpenSleigh project](https://www.opensleigh.net/) has been [sponsored by JetBrains](https://twitter.com/DavideGuida82/status/1376896675579449351).

## 📅 Community and events

- Matt Lacey [builds a Visual Studio extension to navigate between ANY files](https://www.mrlacey.com/2021/03/here-i-made-way-to-navigate-between-any.html).
- For community standups: Xamarin [previews .NET 6 updates](https://www.youtube.com/watch?v=su3ntRjEN1I) and ASP.NET [talks about 12-factor apps](https://www.youtube.com/watch?v=xRlDuXJ3t08).
- Because NuGet stores download counts in Int32, in [80 weeks the NewtonSoft.Json package will overflow it](https://twitter.com/JamesNK/status/1377400586950172672).
- Microsoft Build [will occur on May 25-27](https://www.zdnet.com/article/its-official-microsoft-build-will-be-may-25-to-27/).

## 🌎 Web development

- Matthew Jones [continues writing a Tetris game in Blazor](https://exceptionnotfound.net/tetris-in-blazor-part-2-cells-the-grid-and-the-game-state/).
- Mukesh Murugan builds a chat application [with Blazor, Identity, and SignalR](https://codewithmukesh.com/blog/realtime-chat-application-with-blazor/).
- Niels Swimberghe [deploys Blazor WebAssembly apps to DigitalOcean](https://swimburger.net/blog/dotnet/how-to-deploy-blazor-webassembly-to-digitalocean-app-platform).
- Sundaram Subramanian [writes about using the MudBlazor library for breadcrumbs](https://www.c-sharpcorner.com/article/breadcrumbs-for-blazor-using-mudblazor/).
- David Hayden [works with ASP.NET Core user secrets and the Secret Manager tool](https://www.davidhayden.me/blog/asp-net-core-user-secrets-and-secret-manager-tool).
- Claudio Bernasconi [writes a Blazor dialog component in .NET 5](https://www.claudiobernasconi.ch/2021/03/26/blazor-modal-dialog-component/), and also [handles CSS in Blazor](https://www.claudiobernasconi.ch/2021/03/31/blazor-css-handling/).
- Marinko Spasojevic [works with Material UI in Blazor](https://code-maze.com/blazor-material-ui-configuration-and-theme-customization/).

## 🥅 The .NET platform

- Khalid Abuhakmeh [handles exceptions with ASP.NET Core ExceptionHandlerMiddleware
](https://khalidabuhakmeh.com/handling-aspnet-core-exceptions-with-exceptionhandler-middleware).
- Matthew MacDonald [writes about the future of Windows apps](https://medium.com/young-coder/the-future-of-windows-apps-demystified-17d1f9e325d1).
- Peter Vogel [writes about Project Reunion](https://www.telerik.com/blogs/project-reunion-why-desktop-developers-care).

## ⛅ The cloud

- Paul Michaels [writes about auto-deleting Azure Service Bus messages when idle](https://www.pmichaels.net/2021/03/27/azure-service-bus-auto-delete-on-idle/).
- Khalid Abuhakmeh [builds mono-repos with GitHub Actions](https://khalidabuhakmeh.com/building-mono-repositories-with-github-actions).
- Jon Gallant [works with Azure REST APIs with Postman's OAuth 2.0 Provider](https://blog.jongallant.com/2021/03/azure-rest-apis-postman-oauth2/).
- Aaron Powell [writes about simplifying auth for Azure Static Web App APIs](https://www.aaron-powell.com/posts/2021-03-30-making-auth-simpler-for-static-web-app-apis/).

## 📔 Languages

- Tom Deseyn [writes about C# 9 top-level programs and target-typed expressions](https://developers.redhat.com/blog/2021/03/30/c-9-top-level-programs-and-target-typed-expressions/).
- Shawn Wildermuth [demystifies bitwise operators in C#](http://wildermuth.com/2021/03/28/Coding-Shorts-Demystifying-Bitwise-Operators-in-C).

## 🔧 Tools

- Clyde D'Souza [explores GitHub Actions fundamentals](https://codeburst.io/fundamentals-of-github-actions-d35dc3102447).
- Richard Reedy [writes about the .NET Upgrade Assistant](https://www.telerik.com/blogs/jump-starting-migration-dotnet-core-with-upgrade-assistant).
- Scott Hanselman [writes about recent updates to the Windows Terminal](https://www.hanselman.com/blog/the-windows-terminal-made-better-with-the-command-palette-plus-multiple-actions-in-one-command).
- Julio Sampaio [stress tests .ET apps with Apache JMeter](https://www.red-gate.com/simple-talk/dotnet/software-testing/load-stress-testing-net-apps-with-apache-jmeter/).
- Yannick Reekmans [adds the Developer PowerShell and Developer Command Prompt for Visual Studio to Windows Terminal](https://techcommunity.microsoft.com/t5/microsoft-365-pnp-blog/add-developer-powershell-and-developer-command-prompt-for-visual/ba-p/2243078).
- James Montemagno [explores code Generation from XAML in Visual Studio](https://montemagno.com/code-generation-from-xaml-in-visual-studio/).

## 📱 Xamarin

- Luis Matos [handles global errors ](https://luismts.com/global-error-handling-xamarin-forms/).
- Sam Basu [provides another MAUI update](https://www.telerik.com/blogs/sands-of-maui-issue-2).
- James Montemagno [sets Up an M1 Mac for Xamarin development](https://montemagno.com/setting-up-an-m1-mac-for-xamarin-development/).

## 🏗 Design, testing, and best practices

- Jason Farrell [addresses a misconception that serverless is good for APIs](https://jfarrell.net/2021/03/28/common-misconception-2-serverless-is-good-for-apis/).
- James Shore [writes about TDD](https://www.jamesshore.com/v2/books/aoad2/test-driven_development).
- Oleksii Holubiev [writes about .NET authentication](https://hackernoon.com/net-authentication-security-notes-a64w35fg?source=rss).
- Genevieve Michaels [writes about providing feedback to your manager without being a jerk](https://blog.trello.com/how-to-give-your-manager-feedback).
- Derek Comartin [discusses the pitfalls of using REST APIs for microservices](https://codeopinion.com/rest-apis-for-microservices-beware/).
- Patrick Smacchia [writes about 8 books for .NET developers](https://blog.ndepend.com/8-books-to-improve-as-a-net-developer/).
- Rossitza Fakalieva [unit tests some C# 8 and 9 features](https://www.telerik.com/blogs/lets-give-some-unit-testing-love-to-csharp-8-9-features).
- James Hickey writes about [scaling your auth system](https://fusionauth.io/learn/expert-advice/identity-basics/making-sure-your-auth-system-scales/).

## 🎤 Podcasts

- The Adventures in .NET podcast [talks about application monitoring](https://devchat.tv/adventures-in-dotnet/net-062-innocent-application-performance-monitoring-with-innocent-bindura-from-raygun/).
- The 6-Figure Developer podcast [talks about reactive DDD with Vaughn Vernon](https://6figuredev.com/podcast/episode-189-reactive-ddd-with-vaughn-vernon/).
- The .NET Core Podcast [discusses C# and .NET for beginners with Vijesh Salian](https://dotnetcore.show/episode-73-c-sharp-and-net-for-beginners-with-vijesh-salian/).
- The Complete Developer podcast [talks about side hustle failures](https://completedeveloperpodcast.com/side-hustle-fails/).

## 🎥 Videos

- Over at Ask the Expert, [a discussion with Dr. Brian Kernighan, the inventor of "Hello, World."](https://channel9.msdn.com/Shows/Ask-the-Expert/Ask-the-Expert-Hello-Brian-A-conversation-with-Dr-Brian-Kernighan-creator-of-hello-world)
- The On .NET Show [integrates PowerApps with .NET Web APIs](https://channel9.msdn.com/Shows/On-NET/Integrating-PowerApps-with-NET-Web-APIs), [discusses the .NET Upgrade Assistant](https://www.youtube.com/watch?v=L5Fy1-CTCLo), and [integrates PowerApps with .NET Web APIs](https://www.youtube.com/watch?v=VLqdRpwniwk).
- The ASP.NET Monsters [discuss Microsoft Identity with Christos Matskas](https://www.youtube.com/watch?v=LgNc-IA8d1g).
- The AI Show [extracts information from documents with Form Recognizer](https://channel9.msdn.com/Shows/AI-Show/Extract-information-from-your-documents-with-new-capabilities-in-Form-Recognizer).
