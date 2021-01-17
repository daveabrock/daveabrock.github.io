---
date: "2021-01-17"
title: "Dev Discussions: Steve Sanderson"
tags: [dotnet-stacks, dev-discussions]
subtitle: We talk to Steve Sanderson, the creator of Blazor, about what's next.
tags: [dotnet-stacks, dev-discussions]
share-img: /assets/img/sanderson-card.png
---

This is the full interview from my discussion with Steve Sanderson in my weekly (free!) newsletter, *The .NET Stacks*. Consider [subscribing today](https://dotnetstacks.com)!
{: .box-note}

It seems like forever ago when, [at NDC Oslo in 2017](https://youtu.be/MiLAE6HMr10?t=1612), Steve Sanderson talked about a fun project he was working on, called .NET Anywhere. In the demo, he was able to load and run C# code—*ConsoleApp1.dll*, specifically—in the browser, using Web Assembly. C# in the browser! In the talk, he called it "*an experiment, something for you to be amused by*."

Of course, this amusing experiment has grown [into Blazor](https://dotnet.microsoft.com/apps/aspnet/web-apps/blazor), a robust system for writing web UIs in C#. I was happy to talk to Steve Sanderson about his passions for the front-end web, how far Blazor has come, and what's coming to Blazor in .NET 6.

![Steve Sanderson profile photo]({{ site.url }}{{ site.baseurl }}/assets/img/steve-sanderson.jpg)

> What geared you toward the front-end web, over other tech?

It’s not that I’m personally more motivated by front-end web over other tech. I’m just as motivated by all kinds of other technology, whether that’s backend web stuff, or further out fields like ML, graphics, or games—even things like agriculture automation.

What I have found though is that my professional life has had more impact in front-end web than in other fields. I’m not certain why, but suspect it’s been an under-focused area. When I started my first software job in 2003, being able to do anything in a browser with JS was considered unusual, so it was pretty easy to exceed the state of the art then.

> My front-end experience started with Knockout.js, [your MVVM front-end framework](https://knockoutjs.com/). We've come a long away since then, for sure. Have any of your experience and learnings from Knockout impacted your work on Blazor?

Certainly. Knockout was my first critical-mass open source project, so that’s where I was forced to design APIs to account for how developers get confused and do things contrary to their own best interests, what to expect from the community, and where to draw the line about what features should be in or out of a library/framework.

> Years ago, you probably envisioned what Blazor could be. Has it met its potential, or are there other areas to focus on?

We’re not there yet. If you go on YouTube and find the [first demo I ever did of Blazor at NDC Oslo in 2017](https://youtu.be/MiLAE6HMr10?t=1612), you’ll see my original prototype had near-instant live reloading while coding, and the download size was really tiny. I still aspire to get the real version of Blazor to have those characteristics. Of course, the prototype had the advantage of only needing to do a tiny number of things—creating a production-capable version is 100x more work, which is why it hasn’t yet got there, but has of course exceeded the prototype vastly in more important ways.

Good news though is that in .NET 6 [we expect to ship](https://github.com/dotnet/aspnetcore/issues/5456) an even better version of live-updating-while-coding than I had in that first prototype, so it’s getting there!

> The Blazor use case for .NET devs is quite clear. Do you see Blazor reaching a lot of folks in the JavaScript community (I'm thinking of Angular, where the learning curve is steep), or do you see Blazor's impact limited to the .NET ecosystem? 

Longer term I think it depends on the fundamentals: download size and perf. With .NET 5, Blazor WebAssembly’s main selling point is the .NET code, which easily makes it the best choice of framework for a lot of .NET-centric teams, but on its own that isn’t enough to win over a JS-centric team.

If we can get Blazor WebAssembly to be faster than JS in typical cases (via AoT compilation, which is very achievable) and somehow simultaneously reduce download sizes to the point of irrelevance, then it would be very much in the interests of even strongly JS-centric teams to reconsider and look at all the other benefits of C#/.NET too.

> When looking at AOT, you'll see increased performance but a larger download size. Do you see any other tradeoffs developers will need to consider?

The mixed-mode flavour of AOT, in which some of your code is interpreted and some is AOT, allows for a customizable tradeoff between size and speed, but also includes some subtleties like extra overhead when calling from AOT to interpreted code and vice-versa.

Also, when you enable AOT, your app’s publish time may go up substantially (maybe by 5-10 minutes, depending on code size) because the whole [Emscripten toolchain](https://emscripten.org/) just takes that long. This wouldn’t affect your daily development flow on your own machine, but likely means your CI builds could take longer.

> It's still quite impressive to see the entire .NET runtime run in the browser for Blazor Web Assembly. That comes with an upfront cost, as we know. I know that the Blazor team has done a ton of work to help lighten the footprint and speed up performance. With the exception of AOT, do you envision more work on this? Do you see a point where it'll be as lightweight as other leading front-end frameworks, or will folks need to understand it's a cost that comes with a full framework in the browser?

The size of the .NET runtime isn’t ever going to reduce to near-zero, so JS-based microframeworks (whose size could be just a few KB) are always going to be smaller. We’re not trying to win outright based on size alone—that would be madness. Blazor WebAssembly is aimed to be maximally productive for developers while being small enough to download that, in very realistic business app scenarios, the download size shouldn’t be any reason for concern.

That said, it’s conceivable that new web platform features like [Signed HTTP Exchanges](https://developers.google.com/web/updates/2018/11/signed-exchanges) could let us smartly pre-load the .NET WebAssembly runtime in a browser in the background (directly from some Microsoft CDN) while you’re visiting a Blazor WebAssembly site, so that it’s instantly available at zero download size when you go to other Blazor WebAssembly sites. Signed HTTP Exchanges allow for a modern equivalent to the older idea of a cross-site CDN cache. We don’t have a definite plan about that yet as not all browsers have added support for it.

> I know that you aren't a Xamarin expert, but I'm intrigued by Blazor Mobile Bindings. Do you see there being convergence between MAUI and Blazor, or is this a "do what works for you" platform decision?

Who says I’m not a Xamarin expert, huh? Well, OK, I admit it—I’m not.

Our ideas around MAUI are pretty broad and allow for a lot of different architecture and syntax choices, without having a definite confirmation yet about what exactly are the most first-class built-in options. So I don’t think any conclusion exists here yet.

> Speaking more broadly from the last question: do you see Blazor as the future of native app development in .NET, for mobile and desktop apps?

My guess is there will always be variety. .NET has always supported many different UI programming models (WinForms, WebForms, WPF, UWP, Xamarin, MVC, Razor Pages, Blazor, Unity). The idea that everything would converge on a single one true framework seems unlikely, because different customer groups have different goals and demands.

Blazor is perhaps the option that gives the widest reach across device types, as it’s obviously web-native, but can also target desktop and mobile apps via either web or native rendering within native app shells.

> When looking ahead to .NET 6, in the next year or so, what's coming with Blazor?

See our [published roadmap](https://github.com/dotnet/aspnetcore/issues/27883). 😊

*[Ed. Note: Fair enough. The heavy hitters include [AOT compilation](https://github.com/dotnet/aspnetcore/issues/5466), [hot reload](https://github.com/dotnet/aspnetcore/issues/5456), [global exception handling](https://github.com/dotnet/aspnetcore/issues/13452), and [required parameters for components](https://github.com/dotnet/aspnetcore/issues/11815).]*

> Does Blazor hold up from a load testing perspective? Would it support Amazon-like scale?

Blazor Server or Blazor WebAssembly?

Blazor WebAssembly has the benefit of imposing no per-client runtime cost on the server—from the server’s perspective, it’s just some static content to transmit. So in that sense it inherently scales near-infinitely. But for a public-facing shopping cart app, you most likely want server-rendered HTML to maximise SEO and minimize the risk that any potential customers fail to use the site just because of the initial page load time.

Blazor Server is a more obvious choice for a public-facing shopping cart app. We know it scales well, and a typical cloud server instance can comfortably manage 20k very active concurrent users—the primary limitation is RAM, not CPU. I don’t personally know how many front-end servers are involved in serving Amazon’s pages or how many concurrent users they are dealing with. However I do know that virtually everybody building business web apps is operating at a vastly smaller scale than Amazon, and can likely estimate (to within an order of magnitude) how many concurrent users they want to plan for and thus what server capacity is needed.

> What is your one piece of programming advice?

When something isn’t working or behaves differently than you expected, don’t just keep changing things until it seems to work, as a lot of developers do. Make sure you figure out why it was doing what it was doing, otherwise you’re not really advancing your skills.

*You can connect with Steve Sanderson [on Twitter](https://twitter.com/stevensanderson).*