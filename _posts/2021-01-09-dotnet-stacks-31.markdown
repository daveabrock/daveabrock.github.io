---
date: "2021-01-09"
title: "The .NET Stacks #31: 🥳 10 things to kick off '21"
tags: [dotnet-stacks]
comments: false
share-img: /assets/img/10-things-2021.png 
subtitle: This week, we kick off 2021 with a busy issue.
---

![Newsletter image](https://daveabrock.com/THE .NET STACKS.png)

*Note: This is the published version of my free, weekly newsletter, The .NET Stacks. It was originally sent to subscribers on January 4, 2021. Subscribe at the bottom of this post to get the content right away!*

Well, friends, our dream has finally come true: 2020 is now over.

I hope your 2021 is off to a great start, and also hope you and your families were able to enjoy the holidays. I was lucky enough to take a break. I kept busy: hung out with the kids, watched a bunch of bad TV, worked out (which included shoveling snow three times; thanks, Wisconsin), tried out streaming, hacked around on some side projects, and generally just had fun with the time off.

This week, let's play catch up. Let's run through 10 things to get us ready for what's to come in 2021. Ready?

___

With .NET 5 out to the masses, we're beginning to get past a lot of the novelty of source generators—there's some great use cases out there. (If you aren't familiar, source generators are a piece of code that runs during compilation and can inspect your app to produce additional files that are compiled together from the rest of your code.)

To that end, Tore Nestenius writes about a source generator that automatically generates an API for a system that [uses the MediatR library and the CQRS pattern](https://www.edument.se/en/blog/post/net-5-source-generators-mediatr-cqrs). What a world.

Expect to see a lot of innovation with source generators this year.

___

In my *Blast Off with Blazor* blog series, I've been using Tailwind CSS to mark up my components—you can see it in action where I [build a responsive image gallery](https://daveabrock.com/2020/12/16/blast-off-blazor-responsive-gallery). Instead of Bootstrap, which can give you previously styled UI components, it's a utility-first library that allows you to style classes in your markup. You can avoid the hassle of overriding stylesheets when things aren't to your liking.

To be clear, you may cringe at first when you see this. (I know I did.)

```html
<div class="m-6 rounded overflow-hidden shadow-lg">
    <img class="w-full h-48 object-cover" src="@ImageDetails.Url" alt="@ImageDetails.Title" />
    <div class="p-6">
        <div class="flex items-baseline">
            <!-- stuff -->
        </div>
        <h3 class="mt-1 font-semibold text-2xl leading-tight truncate">@ImageDetails.Title</h3>
    </div>
</div>
```

Once you get used to it, though, you'll begin to see its power. Adam Wathan, the creator of Tailwind, [wrote a piece](https://adamwathan.me/css-utility-classes-and-separation-of-concerns/) that helps you get over the initial visceral reaction.

___

Over the weekend, I [asked this question](https://twitter.com/daveabrock/status/1345413697028685824):

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">What would you be doing for a living if computers or software didn&#39;t exist? I literally have no idea.</p>&mdash; Dave Brock (@daveabrock) <a href="https://twitter.com/daveabrock/status/1345413697028685824?ref_src=twsrc%5Etfw">January 2, 2021</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

This led to entertaining answers. A lot of people talked about going professional with their hobbies, like with music and art. I also found it interesting how many answers had parallels to programming, like being a cook, architecting buildings, and building and maintaining things.

___

What *can't* you do with GitHub Actions? David Pine wrote an Action that [performs machine-translations from Azure Cognitive Services for .NET localization](https://devblogs.microsoft.com/dotnet/localize-net-applications-with-machine-translation/). Tim Heuer [used GitHub Actions for bulk resolving](https://timheuer.com/blog/use-github-actions-for-bulk-resolve-issues/). Of course, you [could go on and on](https://github.com/sdras/awesome-actions).

The main use case is clear: workflows centered around your GitHub deployments, which goes hand-in-hand with it being [the long-term solution to the Azure DevOps platform](https://daveabrock.com/2020/08/15/dotnet-stacks-12). My prediction is that in 2021, the feature gap between Azure DevOps and GitHub Actions will narrow significantly.

The success of GitHub Actions is sure to blur the lines between all the Microsoft solutions—between Azure Functions, Actions, and WebJobs, for example. While the main use case for Actions is deployment workflows, it's hard to beat an app backed by a quick YAML file.

___

We've talked about `System.Text.Json` quite a few times before. The gist from those posts: `System.Text.Json` is a new-ish native JSON serialization library for .NET Core. It's fast and performant but isn't as feature-rich as `Newtonsoft.Json` ([this table gets worse](https://docs.microsoft.com/dotnet/standard/serialization/system-text-json-migrate-from-newtonsoft-how-to?pivots=dotnet-5-0#table-of-differences-between-newtonsoftjson-and-systemtextjson) the more you scroll)—so if you're already using Newtonsoft, stick with it unless you've got advanced performance considerations.

A few days after Christmas, Microsoft's Layomi Akinrinade [provided an update](https://devblogs.microsoft.com/dotnet/whats-next-for-system-text-json/) on `System.Text.Json`. .NET 5 was a big release for the library, as they pushed a lot of updates that brought more Newtonsoft-like functionality: [you'll see](https://devblogs.microsoft.com/dotnet/whats-next-for-system-text-json/#new-features) the ability to deserialize paramterized constructors, conditionally ignoring properties (thank you), C# record types, and constructors that take serialization defaults.

With .NET 6, Microsoft plans to "address the most requested features that help drive `System.Text.Json` to be a viable choice for the JSON stack in more .NET applications." This is after writing (in the same post) that in .NET Core 3.0, "we made `System.Text.Json` the default serializer for ASP.NET Core because we believe it’s good enough for most applications." So, that's interesting. With Newtonsoft expected to [top the NuGet charts](https://www.nuget.org/packages) for the foreseeable future, it's nice to see Microsoft understanding there's much work to do, still, to encourage folks to move over to their solution.

___

Egil Hansen, the man behind bUnit—the Blazor component testing library—describes how component vendors can make their components easily testable with a one-liner. Developers can now call an extension method based on bUnit's `TestContext` that users can call before kicking off tests. The extension method can then be published as a NuGet package with a version matching the compatible library.

Check out [the Twitter thread for details](https://twitter.com/egilhansen/status/1343169295593963521).

___

Did you know that you can target attributes in C# 9 records using a prefix of `property:` or `field:`? You do now.

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">The always smart <a href="https://twitter.com/jnm236?ref_src=twsrc%5Etfw">@jnm236</a> pointed out that you can target attributes on <a href="https://twitter.com/hashtag/csharp?src=hash&amp;ref_src=twsrc%5Etfw">#csharp</a> <a href="https://twitter.com/hashtag/records?src=hash&amp;ref_src=twsrc%5Etfw">#records</a> using a prefix of “property:” or “field:”<br><br>This is a pretty cool feature of records! <br><br>Also, follow <a href="https://twitter.com/jnm236?ref_src=twsrc%5Etfw">@jnm236</a>. <a href="https://t.co/wUvD2Dca6k">pic.twitter.com/wUvD2Dca6k</a></p>&mdash; Khalid 🛸 (@buhakmeh) <a href="https://twitter.com/buhakmeh/status/1341483189613813762?ref_src=twsrc%5Etfw">December 22, 2020</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

___

The .NET Foundation released their November/December 2020 update [just before the holidays](https://dotnetfoundation.org/blog/2020/12/15/blog/posts/net-foundation-november-december-2020-update). There's a community survey you can fill out, [that's available until the end of March](https://dotnetfoundation.org/about/survey?utm_source=dotnetfdn&utm_medium=newsletter). They've also launched a [speaker directory](https://dotnetfoundation.org/community/speakers).

___

Speaking of the .NET Foundation, tonight I watched board member Shawn Wildermuth's new film, *Hello World*. It begins as a love letter to our industry, then takes a sobering look at our diversity and inclusion issues. It's a wonderful film that everyone should see. It's a cheap rental ([check out the site to see where to rent it](https://helloworldfilm.com/))—and once there you can also help support organizations that are helping push our industry to where it should be.

___

As you may or may not have noticed, the Azure SDKs have gotten a facelift. During our break, Jeffrey Richter [wrote about its architecture](https://devblogs.microsoft.com/azure-sdk/architecture/).

Crucial to the SDK design—and most importantly, crucial to *me*—is that retry and cancellation mechanisms are built in. Additionally, you can implement your own policies in their pipeline (think ASP.NET Core middleware) for things like caching and mocking. Right on cue, Pavel Krymets [writes about unit testing and mocking with the .NET SDKs](https://devblogs.microsoft.com/azure-sdk/unit-testing-and-mocking/).

___

# 🌎 Last week in the .NET world

## 🔥 The Top 3

* Tore Nestenius [uses source generators with C# 9 and .NET 5 for MediatR and CQRS](https://www.edument.se/en/blog/post/net-5-source-generators-mediatr-cqrs).
* Niels Swimberghe [pre-renders Blazor Web Assembly at build time for SEO](https://swimburger.net/blog/dotnet/pre-render-blazor-webassembly-at-build-time-to-optimize-for-search-engines).
* Andrew Lock [compares using self-contained or framework-dependent publishing in Docker images](https://andrewlock.net/should-i-use-self-contained-or-framework-dependent-publishing-in-docker-images/).

## 📢 Announcements

* Nat Friedman [writes how GitHub removed all cookie banners from GitHub](https://github.blog/2020-12-17-no-cookie-for-you/).
* The Uno Platform [writes about the developments to look forward to in 2021](https://platform.uno/blog/top-10-uno-platform-developments-to-look-forward-to-in-2021/).
* Tobias Ahlin [discusses how the GitHub globe was built](https://github.blog/2020-12-21-how-we-built-the-github-globe/).
* Google Chrome [is testing larger cache sizes for performance](https://www.bleepingcomputer.com/news/google/google-chrome-is-testing-larger-cache-sizes-to-increase-performance/).
* Microsoft Learn has [learning path for creating serverless applications](https://docs.microsoft.com/learn/paths/create-serverless-applications).
* Layomi Akinrinade [writes about what's next for System.Text.Json](https://devblogs.microsoft.com/dotnet/whats-next-for-system-text-json).

## 📅 Community and events

* Maarten Balliauw [recaps all the (free!) JetBrains community webinars](https://blog.jetbrains.com/dotnet/2020/12/17/catch-up-with-2020-s-net-community-webinars/).
* Matthew MacDonald [writes about .NET's third-party software problem](https://medium.com/young-coder/net-has-a-third-party-software-problem-45d24cdc30c9).
* The .NET Foundation [provides a November/December 2020 update](https://dotnetfoundation.org/blog/2020/12/15/blog/posts/net-foundation-november-december-2020-update).
* Telerik [pulled together a Q&A from a recent .NET 5 webinar](https://www.telerik.com/blogs/the-state-of-dotnet-qa-compilation).

## 🥅 The .NET platform

* Josef Ottosson [writes about efficient file uploads with .NET](https://josef.codes/efficient-file-uploads-with-dotnet/).
* Kristoffer Strube [adds the User-Agent header to HttpClient](https://blog.elmah.io/how-to-add-user-agent-header-to-httpclient-in-net/).
* Immo Landwerth [writes about transitioning from .NET Standard to .NET 5](https://www.codemag.com/article/2010022).

## 🌎 Web development

* Sam Walpole [optimizes background tasks using Hangfire and ASP.NET Core](https://hackernoon.com/how-to-optimize-background-tasks-using-hangfire-and-aspnet-core-5j2331ax?source=rss).
* Ed Charbeneau [writes about 10 Blazor features you might not know about](https://www.telerik.com/blogs/10-blazor-features-you-probably-didnt-know).
* Dave Brock [builds a responsive image gallery in Blazor](https://daveabrock.com/2020/12/16/blast-off-blazor-responsive-gallery), and [prerenders a Blazor Web Assembly application](https://daveabrock.com/2020/12/27/blast-off-blazor-prerender-wasm).
* Sean Franklin [uses Blazor Server to store encrypted session data in the browser](https://www.c-sharpcorner.com/article/blazor-server-how-to-use-local-browser-storage/).
* Wael Kdouh [writes about microfrontends with Blazor Web Assembly](https://medium.com/@waelkdouh/microfrontends-with-blazor-webassembly-b25e4ba3f325).
* Niels Swimberghe [interacts with JavaScript objects using the new IJsObjectReference in Blazor](https://swimburger.net/blog/dotnet/interacting-with-javascript-objects-using-the-new-ijsobjectreference-in-blazor), [deploys Blazor Web Assembly to Firebase Hosting](https://swimburger.net/blog/dotnet/how-to-deploy-blazor-webassembly-to-firebase-hosting), and [fixes Blazor Web Assembly PWA integrity checks](https://swimburger.net/blog/dotnet/fix-blazor-webassembly-pwa-integrity-checks).
* David Grace [writes about client-side UI events and other common queries in Blazor](https://www.telerik.com/blogs/client-side-ui-events-and-other-common-queries-blazor).
* Marinko Spasojevic [writes about 2-step verification with Angular and ASP.NET Core Identity](https://code-maze.com/2-step-verification-with-angular-and-aspnet-identity/).
* Mitchel Sellers [writes about real world localization with ASP.NET Core 5](https://www.mitchelsellers.com/blog/article/real-world-localization-implementation-asp-net-core-5).

## ⛅ The cloud

* David Pine [writes a GitHub Action to perform machine translation with Azure Cognitive Services](https://devblogs.microsoft.com/dotnet/localize-net-applications-with-machine-translation).
* Jeffrey Richter [walks us through the Azure SDK architecture](https://devblogs.microsoft.com/azure-sdk/architecture).
* David Ellis [offers tips on how to save when running .NET on Azure](https://azure.microsoft.com/blog/5-ways-to-save-costs-by-running-net-apps-on-azure).
* Tim Heuer [uses GitHub Actions for bulk resolving](https://timheuer.com/blog/use-github-actions-for-bulk-resolve-issues/).
* Damian Brady [uses GitHub Actions for Azure Machine Learning](https://devblogs.microsoft.com/devops/using-azure-machine-learning-from-github-actions).
* Paul Michaels [uses Managed Identity with Azure Key Vault](https://www.pmichaels.net/2020/12/19/using-managed-identity-with-azure-keyvault), and [receives messages through the Azure Service Bus](https://www.pmichaels.net/2020/12/26/receiving-messages-in-azure-service-bus).
* Samson Amaugo [makes a CRUD API using Azure Functions and Azure Cosmos DB](https://auth0.com/blog/making-crud-api-with-azure-functions/).

## 📔 Languages

* Khalid Abuhakmeh [writes about C# 9 covariant return types](https://khalidabuhakmeh.com/csharp-9-covariant-return-types), and [warns about C# 9 record gotchas](https://khalidabuhakmeh.com/avoid-csharp-9-record-gotchas).
* Patrick Smacchia [writes about the proper usages of exceptions in C#](https://blog.ndepend.com/the-proper-usages-of-exceptions-in-c/).
* Steve Gordon [plays with C# 9 top-level programs, records, and Elasticsearch.NET](https://www.stevejgordon.co.uk/playing-with-csharp-9-top-level-programs-records-and-elasticsearch-dotnet).
* John Reilly [writes about nullable reference types in C#](https://blog.johnnyreilly.com/2020/12/nullable-reference-types-csharp-strictnullchecks.html).
* Eric Potter [writes about init-only setters in C# 9](http://humbletoolsmith.com/2020/12/18/upgrading-old-csharp-to-csharp-9-init-only-setters/).
* Thomas Levesque [shows off how you can use C# 9 records with EF Core](https://thomaslevesque.com/2020/12/23/csharp-9-records-as-strongly-typed-ids-part-4-entity-framework-core-integration/).
* Mark Heath [weighs performance and readability when storing coordinates in C#](https://markheath.net/post/coord-performance-versus-readability).
* Reed Copsey [binds with F#](http://reedcopsey.com/2020/12/17/f-basics-result).
* Riccardo Terrell [builds a recommendation engine with ML.NET and F#](http://www.rickyterrell.com/?p=214).
* Jamie Dixon [looks at the SARS-CoV-2 genome with F#](https://jamessdixon.com/2020/12/26/looking-at-sars-cov-2-genome-with-f/).

## 🔧 Tools

* Mark Heath [configures SQL Server dependencies in ASP.NET microservices with Project Tye](https://markheath.net/post/sql-container-with-tye).
* Sam Walpole [shows off Automapper, Mediatr, Swagger, and Fluent Validation](https://hackernoon.com/the-top-4-nuget-packages-according-to-this-software-developer-r53c3150?source=rss).
* Charles Flatt [writes about interesting behaviors with NuGet 5.7+](https://www.softwaremeadows.com/posts/nuget_5_7+_ignores_nuspec_replacement_tokens_-_and_other_weird_behavio/).
* John Reilly [prettifies C# with dotnet-format and lint-staged](https://blog.johnnyreilly.com/2020/12/prettier-your-csharp-with-dotnet-format-and-lint-staged.html).
* Scott Hanselman [provides a 2021 update for his Developer and Power Users Tool List for Windows](https://www.hanselman.com/blog/scott-hanselmans-2021-ultimate-developer-and-power-users-tool-list-for-windows).

## 🔐 Security

* Giorgi Dalakishvili [uses WebAuthn with C#](https://developer.okta.com/blog/2020/12/18/how-to-use-webauthn-csharp-dotnet).
* Gergely Sinka [supports .NET Core SameSite + OAuth apps on Linux](https://developer.okta.com/blog/2020/12/15/okta-linux-dotnet-server-support).
* Andrea Chiarelli [uses C# extension methods for Auth0 authentication](https://auth0.com/blog/using-csharp-extension-methods-for-auth0-authentication/).

## 👍 Design, architecture, and testing

* Derek Comartin [scales a monolith horizontally](https://codeopinion.com/scaling-a-monolith-horizontally).
* Steve Smith [writes about data-deficient messages](https://ardalis.com/data-deficient-messages/).
* Pavel Krymets [unit tests and mocks with the Azure .NET SDK](https://devblogs.microsoft.com/azure-sdk/unit-testing-and-mocking/).
* Sean Killeen [utilizes the Bogus library for better mocks](https://seankilleen.com/2020/12/utilizing-the-builder-pattern-and-bogus-for-better-supporting-test-objects/).
* Kevin Avignon [writes about guidelines to improve your .NET software design skills](https://kevinavignon.com/2020/12/23/guidelines-to-improve-your-software-design-skills-with-net-part-i/).

## 📱 Xamarin

* Charlin Agramonte [writes about multi-binding in Xamarin Forms](https://xamgirl.com/understanding-multi-binding-in-xamarin-forms/).
* James Montemagno [introduces a cadence sensor display for iOS](https://montemagno.com/introducing-my-cadence-for-ios-a-simple-cadence-sensor-display/).
* Leomaris Reyes [debugs on a real Android device using Xamarin for Visual Studio](https://www.telerik.com/blogs/how-to-debug-on-real-android-device-using-xamarin-visual-studio) and also [replicates a Christmas shopping UI](https://askxammy.com/replicating-christmas-shopping-ui-in-xamarin-forms/).
* Suthahar [deploys Xamarin.iOS app to iOS devices without an Apple Developer account](https://www.msdevbuild.com/2020/01/deploy-xamarinios-app-to-ios-device.html) and also [works with emotion recognition in Xamarin](https://www.msdevbuild.com/2020/02/microsoft-ai-xamarin-emotion-recognition-Cognitive-Services.html).

## 🎤 Podcasts

* .NET Rocks [builds a flight simulator in C# with Laura Laban](https://www.dotnetrocks.com/default.aspx?ShowNum=1718), and [talks to Scott Hunter about .NET 5](https://www.dotnetrocks.com/default.aspx?ShowNum=1719).
* The 6-Figure Developer Podcast [talks to Dave Glick about Statiq and open source](https://6figuredev.com/podcast/episode-175-dave-glick-statiq-sites-and-open-source/), and [talks about REST with Irina Scurtu](https://6figuredev.com/podcast/episode-176-rest-apis-with-irina-scurtu/).
* The Azure DevOps Podcast [talks to Maddy Leger about Xamarin in a .NET 5 world](http://azuredevopspodcast.clear-measure.com/maddy-leger-on-xamarin-in-a-net-5-world-episode-120), and [talks with Kendra Havens about Codespaces](http://azuredevopspodcast.clear-measure.com/kendra-havens-on-codespaces-episode-121).

## 🎥 Videos

* Gerald Versluis [accesses battery and energy saver info with Xamarin.Essentials](https://www.youtube.com/watch?v=D1O_xkNgBXU).
* The Loosely Coupled Show [talks with Fran Méndez about AsyncAPI](https://www.youtube.com/watch?v=6XbBJ6xF-uY).
* At Technology and Friends, [David Giard talks to Ed Charbeneau about Blazor testing](http://davidgiard.com/2020/12/28/EdCharbeneauOnBlazorTesting.aspx).
* David Ortinau [validates navigation in Xamarin.Forms](https://www.youtube.com/watch?v=gXLRIy1_284), and also [adds a header to the flyout menu in Xamarin](https://www.youtube.com/watch?v=VZxV5lAzDZ0).
* Jeremy Likness and Arthur Vickers give a tour of EF Core 5 in [two](https://www.youtube.com/watch?v=p0UJdoBj-Lc) [videos](https://www.youtube.com/watch?v=b2klBzcALJc).
