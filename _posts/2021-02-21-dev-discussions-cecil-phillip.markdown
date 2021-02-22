---
date: 2021-02-21
title: 'Dev Discussions: Cecil Phillip'
tags: [dotnet-stacks, dev-discussions]
description: We talk to Cecil.
image: "/assets/img/sanderson-card.png"

---
*This is the full interview from my discussion with Steve Sanderson in my weekly (free!) newsletter, The .NET Stacks. Consider [subscribing today](https://dotnetstacks.com)!*

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
