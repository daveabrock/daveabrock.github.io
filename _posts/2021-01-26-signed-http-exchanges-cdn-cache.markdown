---
date: "2021-01-26"
title: "Signed HTTP Exchanges: A path for Blazor WebAssembly instant runtime loading?"
tags: [csharp,blazor]
image: /assets/img/signed-exchanges.png 
description: We explore Signed HTTP Exchanges, which may help when loading the .NET runtime in Blazor WebAssembly apps.
---

Before you sit down and write the next great Blazor application, you first need to think about [hosting](https://docs.microsoft.com/aspnet/core/blazor/hosting-models?view=aspnetcore-5.0): Blazor Server or Blazor WebAssembly? I've [written about this quite a bit](https://daveabrock.com/2020/10/26/blast-off-blazor-intro#hosting-models), but I'll provide a condensed version for you.

Blazor Server executes your app on the server from an ASP.NET Core app. JS calls, event handling, and UI updates are managed over a persistent SignalR connection. In this case, the app loads much faster, you can enjoy .NET Core APIs on the server, and your apps work with browsers that don't support WebAssembly.

For Blazor WebAssembly, there's no server-side dependency, you can leverage client capabilities, and it works great for serverless deployment scenarios. For example, [in my *Blast Off with Blazor series*](https://daveabrock.com/tags#blast-off-blazor), it's served using Blazor WebAssembly and Azure Functions—I only have to pay for compute.

The biggest drawback to Blazor WebAssembly, of course, is the runtime download size. With no runtime dependency, users need to wait for the .NET runtime to load in the browser. While it's getting smaller thanks to IL trimming and other techniques, it still is a major factor to consider. You can definitely help with initial load time—thanks to [server prerendering](https://daveabrock.com/2020/12/27/blast-off-blazor-prerender-wasm) and [other techniques](https://www.meziantou.net/optimizing-a-blazor-webassembly-application-size.htm)—but the download size will always be a factor. Or will it?

In a recent interview [with Blazor creator Steve Sanderson](https://daveabrock.com/2021/01/17/dev-discussions-steve-sanderson), I posed this question: do we see a point where it'll ever be as lightweight as leading front-end frameworks, or will we need to understand it's a cost that comes with a full framework in the browser?

Here's what he said:

>The size of the .NET runtime isn’t ever going to reduce to near-zero, so JS-based microframeworks (whose size could be just a few KB) are always going to be smaller. We’re not trying to win outright based on size alone—that would be madness. Blazor WebAssembly is aimed to be maximally productive for developers while being small enough to download that, in very realistic business app scenarios, the download size shouldn’t be any reason for concern.

>That said, it’s conceivable that new web platform features like Signed HTTP Exchanges could let us smartly pre-load the .NET WebAssembly runtime in a browser in the background (directly from some Microsoft CDN) while you’re visiting a Blazor WebAssembly site, so that it’s instantly available at zero download size when you go to other Blazor WebAssembly sites. [Signed HTTP Exchanges](https://developers.google.com/web/updates/2018/11/signed-exchanges) allow for a modern equivalent to the older idea of a cross-site CDN cache. We don’t have a definite plan about that yet as not all browsers have added support for it.

Interesting! I've read about Signed HTTP Exchanges before but not in the context of a cross-site CDN cache. In this post, I'd like to unpack the concept of a cross-site CDN cache, describe Signed HTTP Exchanges, and how this new technology *might* possibly help with downloading the .NET runtime in the browser for Blazor WebAssembly apps.

To be clear, I'm not saying that Microsoft plans on using Signed HTTP Exchanges to assist with Blazor WebAssembly browser load time. I'm exploring the technology here to understand how it all works.
{: .box-warning}

# The value of a "cross-site CDN cache"

Before we discuss the values of a cross-site CDN cache, let's briefly discuss a few big reasons why you might want to use a Content Delivery Network (CDN) to store your resources instead of handling it yourself:

- **Better content availability** - because CDNs are typically distributed worldwide, they can handle more traffic easily. You can trust that something hosted by a Google or a Microsoft will withstand the pressures of the modern web.
- **Faster load times** -  because CDNs are typically globally distributed, you can serve content to users closer to their location
- **Cost reduction** - through *caching* and other optimization techniques, CDNs can reduce the data footprint

It's the *caching* we want to focus on here. How does this relate to cross-site caching? This is definitely not a new idea. Let's think about the [jQuery library](https://jquery.com/). (While it isn't as popular as it once was, it's still [all over the web](https://trends.builtwith.com/javascript/jQuery), including in your latest ASP.NET Core project templates.)

The idea of cross-site CDN caching is simple. If folks all over the world are accessing a ubiquitous library like jQuery from its official CDN, the odds are high that they already have a cached script file in their browser from a visit to another website. This significantly speeds up script load time when users make their way to your site.

Will Blazor ever be as ubiquitous across the web as jQuery once was? Time will tell. But it's a reasonable strategy to address how to load the .NET runtime in the browser. It isn't as simple as asking Microsoft to stand up a new CDN, as this pattern [invites tracking abuse](https://www.stefanjudis.com/notes/say-goodbye-to-resource-caching-across-sites-and-domains/) and potential security problems. We need a modern approach. Let's see if Signed HTTP Exchanges can help.

# Enter Signed HTTP Exchanges

Here's an elevator pitch for Signed HTTP Exchanges, [taken straight from Google](https://developers.google.com/web/updates/2018/11/signed-exchanges):

>Signed HTTP Exchange (or "SXG") ... enables publishers to safely make their content portable, i.e. available for redistribution by other parties, while still keeping the content’s integrity and attribution. Portable content has many benefits, from enabling faster content delivery to facilitating content sharing between users, and simpler offline experiences.

This especially helps with serving content from third-party caches. If Microsoft has a CDN to host the Blazor WebAssembly runtime bits, they could leverage SXG to make this possible.

How does this work? When a publisher [signs an HTTP exchange](https://wicg.github.io/webpackage/draft-yasskin-http-origin-signed-responses.html#rfc.section.3) (a request/response pair) from any cached server, the content can be published and referenced elsewhere on the web without a dependency on a server, connection, or even a hosting service. Feel free to [geek out on the juicy details](https://web.dev/signed-exchanges/). (If you're a publisher, you enable this by generating a certificate key to generate a signature with a special [CanSignHttpExchanges extension](https://wicg.github.io/webpackage/draft-yasskin-http-origin-signed-responses.html#cross-origin-cert-req).)

As the main use case is delivering a page's main document, sites could leverage it by using an `a` or `link` tag:

```html
<a href="https://myexample.com/sxg">
```

```html
<link rel="prefetch" as="document" href="https://myexample.com/sxg">
```

In our case, Microsoft could use SXG preloading to download the runtime in the background, cache it, then allowing for a lightning-quick experience as you navigate other Blazor WebAssembly sites. As we think about how we load resources today, it's done over a `<script>` tag. This approach in SXG—called subresource loading—is [currently not recommended](https://web.dev/signed-exchanges/#loading-sxgs), but it may change over time.

# Browser support

Over at *caniuse.com*, you can see [how browser support looks](https://caniuse.com/sxg)—at the time of this writing it enjoys around 67% support for users worldwide. Edge, Chrome, and Opera rolled out support for it around the spring of 2020, and Firefox does not support it yet.

![Browser support for SXG]({{ site.url }}{{ site.baseurl }}/assets/img/can-i-sxg.png)

In addition to this, SXGs support advanced content negotiation. This means you can serve both SXG and non-SXG versions of the same content, [depending on if a browser supports it](https://web.dev/signed-exchanges/#content-negotiation).

# Wrap up

In this post, we talked about Signed HTTP Exchanges and how it might help with loading the .NET runtime in the browser. It's always scary writing about a new topic that's subject to change—so please let me know if you have any suggestions or corrections.
