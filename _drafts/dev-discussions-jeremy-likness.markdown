---
date: "2020-07-18"
title: "Dev Discussions - Jeremy Likness"
excerpt: We talk with Jeremy Likness!
tags: [dotnet-stacks, dev-discussions]
header:
    overlay_image: /assets/images/shahed-card.png
    overlay_filter: 0.8
---

This is the full interview from my discussion with Jeremy Likness in my weekly (free!) newsletter, *The .NET Stacks*. Consider [subscribing today](https://dotnetstacks.com/register) to get this content right away!
{: .notice--success}

(intro here)

**Can you walk me through your path to Microsoft, and what you do at Microsoft now?**

My desire to be a programmer started when I was 7 years old back in 1981. At home and bored, I found a manual for our TI-99/4A computer that ran at a blazing 3 MHz (nope, not GHz) with 16 kilobytes (nope, not a typo) of memory. I typed in a BASIC program, ran it, and was blown away. It felt like magic and I realized that this was something I could master.

I pursued a Computer Science degree in college but ended up dropping out for mental health reasons. I believed what everyone told me: that without a degree I might as well give up on a career, so I focused on fast food, retail, hospitality, and even worked in a pool hall for a year.

My personal break came when I interviewed for a job at an insurance company and mentioned that I speak Spanish. They hired me as a Spanish-speaking claims representative. I was always competitive so I tried to close the most claims in the department, but the “green screen” software running on an AS/400 (now called iSeries) would often crash. We’d call tech support, they’d come over and basically break into a command line and restart the app. I decided to close the loop and fix it myself, and when IT figured out what I was doing, they called me up. I expected to get disciplined but instead was offered a job in IT. It was a night shift position mostly focused on reloading printer cartridges in refrigerator-sized printers, but that gave me time to study the language they used, RPG, and eventually earn a spot on the programming team.

To make a short story boring, I worked at various positions and had job titles ranging from Technical Team Lead to Director of IT. I even owned my own company for a few years. It was an online fitness business that I scaled up doing online coaching, delivering seminars and selling CDs and books. 

I sold the company when I had the opportunity to join a startup focused on custom hotspot portals back in 2006. My first day there, the CEO took me to Ikea, bought a desk then dropped me off at a loft in downtown Atlanta. “Find a spot you like, assemble the desk and get to work.” The company later pivoted to mobile device management. After five years of startup mode working 12 hour days and most weekends, I had to make a change. I left the company with nothing vested and they were acquired a few years later for $1.5 billion. I was very proud of the infrastructure and product I helped build to position them for the acquisition, but also extremely happy to have my health, time, and sanity back.

I went into consulting at a company named Wintellect that was entirely remote. I went from not being home for weeks at a time to being home all the time, and working over 80 hours a week to 40-hour weeks with bonuses if I had to work more. This strengthened my relationship with my wife and daughter and taught me the importance of balance. I was invited by another local Atlanta company to build their application consulting practice and joined as their director of application development. Over three years I built up a multi-million dollar practice and had the opportunity to hire and mentor some amazing developers.

I was very happy in that role and regularly ignored recruiting emails, but one came in from Microsoft and I couldn’t ignore it. I watched the positive cultural transformation under Satya Nadella over social media and just a week earlier had told my wife, “Some great things are happening at Microsoft.” The timing was perfect, so I thought, “It can’t hurt to try.” 

I replied and started a series of interviews. I waited a few weeks to receive the feedback that I interviewed well, but they were looking for someone with a lot more open source contributions. My work at the time was largely proprietary enterprise line of business apps. I decided it wasn’t meant to be and moved on… only to receive a call a week later about another role. I interviewed for that role and felt good about it, but didn’t hear back for several weeks. I was finally told that my interview went great but due to budget adjustments, the position was no longer open. 

Clearly I was not meant to work at Microsoft, right? Then I received a third call about a new developer advocate role. The first call was all about why I thought I would be a good fit and why I was willing to leave a role as Director of Application Development to become an individual contributor. I explained it was always a dream of mine to work at Microsoft and that the role felt custom fit for me – after all, I was already advocating to raise awareness about our app dev services at my current company. “Great, we’ll schedule an interview.”

At the time I planned a trip to Italy with my wife and some close friends to celebrate our 20-year anniversary. Microsoft wanted two days to interview me. I reported directly to the CEO and knew it would be strange to request two extra days off at the last minute. I had one extra day after the trip to adjust to the time zone change, so I asked the recruiter if they could squeeze the interviews into a single day and make it happen on that specific day. To my surprise, they said, “Yes.” 

I flew back to Atlanta from Italy, helped my wife get to her shuttle to head home, then went back into the airport to fly to Seattle. I landed at midnight, was in my hotel by 1 am and back up at 5 am to start a day of interviews. It was the best decision I ever made because I landed the role! I was renewed for my 8th year as an MVP just 8 days before my start date on July 10th, 2017.

(If anyone is interested, a fun post spanning my career is “30 years of Hello, World.” https://blog.jeremylikness.com/blog/2016-02-28_30-years-of-hello-world/ I also wrote a post about my recent transition to the PM role here: https://blog.jeremylikness.com/blog/new-role-dotnet-data-pm/)

**As a developer at heart (your blog is called "Developer for Life", after all) what kind of projects have you been tinkering with?**

My first consulting projects at Wintellect involved XAML via WPF then Silverlight. I was a strong advocate of Silverlight because I had built some very complex web apps using JavaScript and managing them across browsers was a nightmare. It is a common misconception that JavaScript was “different”, when in reality it was the implementation of the HTML interface or DOM that caused us grief in the earlier days. 

Silverlight raised productivity orders of magnitude and XAML made it possible to work in parallel with designers. It was a game-changer and I dedicated time to learn it inside and out. Silverlight was an implicit promise to write C# code that could run anywhere, and for many reasons probably more political than technical, the promise was not fulfilled. 

I believe it has been realized with .NET Core and more specifically, Blazor. I resisted Blazor when it came out due to the existence of mature web frameworks available like Angular, React, Vue.js and Svelte, but colleagues convinced me to give it a spin and I was blown away by two things: first, how productive I could be and produce so much in a short period of time. Second, how many existing packages work with it and run inside the browser “as is”. 

Want to build a markdown engine? No worries, take your pick of libraries to pull in. For personal learning (and as a result, education for others) I’ve been building a lot of Blazor apps to explore different protocols, ways of interacting with data, application of the MVVM pattern and more. I’m working to publish some reference applications that show how to use Blazor with Entity Framework Core [and have published seven articles](https://blog.jeremylikness.com/series/blazor-and-ef-core/).

I am also diving into expressions. I [published a blog post](https://blog.jeremylikness.com/blog/dynamically-build-linq-expressions/) about parsing JSON and turning it into an expression tree to evaluate. Now I’m working on the inverse: parsing an `IQueryable` LINQ expression to pull out the various pieces. Imagine you have a queryable source that you send to a component that then applies ordering, filtering, and sorting. How do you inspect the result to verify how the query was manipulated? I’m working on that and plan to write some blog posts soon.

My other projects are related to products I work with. I’m working on reference samples for using EF Core with Blazor, WPF, and WinForms. I’m also starting to explore big data and will be looking at projects that use .NET for Spark.

**To be honest, when I think of your role as the "Sr. PM of .NET Data," I mostly think of "Sr. PM of Entity Framework." Does anything else fall under that umbrella?**

Entity Framework is certainly a large part of what I do, but the role is really about all of the ways that .NET developers interact with data. 
In addition to EF Core, I also deal with big data and manage .NET for Spark. I don’t just limit my scope to products I’m directly responsible for, but also focus on things like OData, gRPC and GraphQL for connecting to data over APIs.

I’m interested in improving the experience for the products we own at Microsoft as well as supporting community projects like Hot Chocolate, GraphQL .NET and Dapper just to name a few. I work closely with the team responsible for the SQL client. I work with Azure SQL and Cosmos DB. 

I’m focused on an initiative to reorganize our documentation to have a landing page for .NET Data so that developers can find what they need with just a few clicks. It will focus on topics ranging from storage and NoSQL to relational databases, cache, APIs, and more.  I encourage anyone reading this to participate and share feedback using [the GitHub issue](https://github.com/dotnet/docs/issues/19029). Our goal is to provide the best possible experience for .NET developers working with data, so you can imagine there is a lot of surface area to consider.

That’s actually something that applies to the EF Core team as well. They are branded based on that product, but are really passionate about all things data-related. The team “owns” `System.Data`.

**I know that when EF Core first rolled out, many features from EF 6 weren't ported over (I'm thinking of 'Always On' encryption, for example. Coming into .NET 5, can we say that with progress over the last few years, EF Core can handle virtually any production need?**

Specific to your example, I’m fairly certain that “Always On” encryption was an issue with lack of support in SqlClient and had nothing to do with Entity Framework. More generally, “any production need” covers a lot of ground.

Both EF 6 and EF Core are very mature and widely used and customers are employing both versions in production. For the common case of a data access layer for a web app, I believe that EF Core 5.0 will be “feature complete” for the majority of scenarios developers are looking to address. We are building support and documentation for end-to-end scenarios across all supported platforms, from Xamarin and Blazor to WinForms, WPF, and of course Azure.

Looking at the cloud, we are growing the set of features available for our Cosmos DB provider. The SQL Provider works with Azure SQL with nothing more than a connection string change. We are investigating how features like encryption and sharding work with EF Core to provide guidance where needed and consider features if gaps exist.

One caveat is that EF Core is not a micro-ORM and there is not a goal to support that use case. We don’t consider it the tool to solve all data access needs. In fact, some customers use a lightweight solution like Dapper for queries and use EF Core for the create/update/delete side of things. This allows them to take advantage of the features that make EF Core shine, like advanced field-mapping with shadow properties, concurrency resolution, and the built-in change trackers and proxies, while having a go-to for lightweight data transfer objects and complex queries using the micro-ORM.

**EF has definitely had its fair share of demanding customers about when features would be developed. This is just me speculating here, but previously I think EF wasn't as community-focused as it could have been, and the community didn't have a lot of insights about the team and how decisions were made. We're all seeing some nice community involvement with roadmaps, documentation, and community standups (in which someone commented about the tremendous productivity for a team so small!). Can you talk about how you are engaging with the community today, and the different ways we can learn, stay up to speed, offer feedback, and understand what's coming?**

I can’t speak to the full history of the EF team, but my impression is they have been very community-focused for quite some time. They’ve been posting weekly updates for several years now and you can see open discussions of issues dating back longer than that. One thing that resonated with me when I joined the team is their focus and desire to be completely open. Some members came over to Microsoft after being contributors, and most of the engineering team have worked on the same product for years – some over a decade! 

We do everything in the open. Everything is tracked via our issues and/or discussions. We prioritize features based on community feedback, including upvotes and comments. We use issues and discussions for design decisions. The roadmap is publicly available and updated whenever priorities shift. There are a few ways to keep track with what’s new and to interface directly with the team. First, most of the team is active on Twitter. You can use [the alias to find profiles and ask questions directly](https://aka.ms/efteam-twitter). Second, we are active on GitHub. Perhaps the most useful link is [the issue that is pinned to the issues list and updated weekly with progress](https://github.com/dotnet/efcore/issues/19549). You can access [discussions here](https://github.com/dotnet/efcore/discussions): For major releases, [keep an eye on the .NET blog](https://devblogs.microsoft.com/dotnet/author/jeremy-likness/)  - it’s not just a list of what’s new, but also an opportunity to recognize our many community contributors. You might be amazed to see how many people are involved in code and documentation updates for EF Core 5.0!

The other way to keep in touch, and one I am very excited about, is our EF Core Community Standup. We host these every other Wednesday at 10am PT (17:00 UTC). During the standup, we highlight updates, community blog posts, projects, and contributions. It is a live stream and we encourage live Q&A so we can interact directly and answer the community questions that come up. We also take suggestions for shows and bring on guests when we can. You can find the standup schedules and watch previous recorded standups at *[live.dot.net](https://live.dot.net/)*. 

**I've been noticing how, in preparation for .NET 5, the EF Core preview releases seem to be in lock-step with ASP.NET Core. Is this intentional? Are you working on a more predictable release cycle and coordination with other teams?**

Yes. We have intentionally aligned EF Core releases with .NET Core releases. ASP.NET Core follows the same cadence. Due to the way all three interoperate, it just makes sense to align them together. 

That way we can build a release that potentially takes advantage of new runtime, framework, and SDK features that is released the same time, and likewise ASP. NET Core for example can pull in our releases for things like templates that provide identity management that relies on EF Core. Finally it provides a consistent cadence for messaging. It makes it very clear when customers can acquire and test preview releases and when to expect the final release.

**Speaking of, while I can easily see what you've been delivering, what are some of the best upcoming feature improvements for EF Core you're most proud of?**

Many of the features are ready for preview. I’m excited that the team has tackled some of the most requested items for this release. 

The many-to-many implementation that makes it possible to define relationships in a fluid way – for example, teams and individuals might have relationships, but down the road you want to add metadata about the relationship so you go from `Teams` has `Person` and `Person` belongs to `Teams` to a `TeamsPerson` domain object – that will be fully baked into EF Core 5.0. 

Split includes provide flexibility over the way you handle sub-collections, i.e. subquery vs. separate SQL call to fetch the collection. There has been significant effort put into improving the experience for managing schemas via migrations, from additional command line options and support to enhanced documentation. A huge feature is table-per-type mapping, and another is filtered includes that provide far more control over the data sets you return. 

Honestly I’m excited about [everything that’s coming out](https://aka.ms/efcore5). I’ve personally been working on end-to-end scenarios and will be very happy to see clear guidance for using EF Core in Xamarin (should be ready soon), Blazor, WinForms, WPF, UWP, and other platforms in addition to the documentation we already have.

**What have you discovered about things like EF Core that you weren't aware of, before you joined the team?**

I was surprised just how popular and useful the Cosmos DB provider is. I was skeptical because the SDK is great, so why put EF Core in front of it? After joining the team and talking to customers I realized there is a huge ecosystem that looks at EF Core as a common interface to data. They want to use it in mobile (Xamarin), and in serverless Azure Functions, and to talk to NoSQL databases in addition to relational databases.

It was really eye-opening to discover just how much development time is saved when we’re able to support these scenarios and provide that common strategy for data access. Just something as simple as managing multiple types in the same collection with automatic discriminators makes a huge difference. 

Some scenarios like Xamarin have been very customer-driven. Adoption in Blazor is huge, too, so we’re focused on providing appropriate guidance for all platforms and closing gaps that might exist.

I also am really impressed with the community collaborations the team has built. I sat on a call with a third-party database team that is building a .NET LINQ provider for their own SDK (not part of EF Core) and the EF Core team was happy to provide guidance, share gotchas and lessons learned and offer support from our experience building EF Core.

The team has great relationships across the board – we have conversations with the Dapper team, for example, and on the mobile side are happy to suggest SQLite-Net when it makes sense as a native ORM solution for mobile. It’s really a team focused on solving problems in the best way possible, rather than trying to make their own product the solution for everything.

**What is your one piece of programming advice?**

After decades of building software my biggest piece of advice is that your first step shouldn’t be to find the library or framework or tool, but to solve the problem.

Too often people add frameworks or tools or patterns because they’re recommended, rather than actually determining if they add value. Many times the overhead outweighs the benefit. I’m often told, “That solution isn’t right because you don’t have a business logic layer.” My response is, “So?” It’s not that I don’t see value in that layer for certain implementations, but that it’s not always necessary.

Have you worked on that project that forced an architecture so that one change involves updating five projects and the majority of them are just default “pass through” implementations? I am a fan of solving for the solution, and only then if you find some code is repeated, refactor. Don’t over-engineer or complicate. One of my favorite starting points for a solution is to consider, “What is the ideal way I’d like to code for this?”

For example, I am building a Blazor MVVM implementation. For testing the filtering and sorting that the view model applies, I would love to pass an `IQueryable<Entity>` then do something like this: `Assert.That(query.CalledMethod(nameof(Enumerable.Take)).WithValue(5))` to verify that the view model applied the right extensions for paging. I start with what is easy to read and understand, then work backwards to provide an API that satisfies it.

As another example, when you raise property change notifications, you often have dependencies. If I have `Quantity` and `CostPerItem`, then `TotalCost` changes whenever `Quantity` or `CostPerItem` does. I’d love to say `TotalCost.DependsOn(CostPerItem).AndAlso(Quantity)` … so I start with that, then build the appropriate interfaces to make it work. That in my opinion leads to readable, maintainable code.