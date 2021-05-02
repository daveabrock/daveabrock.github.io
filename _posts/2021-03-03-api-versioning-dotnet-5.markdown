---
date: "2021-03-03"
title: "Use API versioning in ASP.NET Core 5"
description: "In this post, we discuss API versioning and how to use it in ASP.NET Core 5."
tags: [csharp, aspnet-core]
image: /assets/img/functions-dotnet-5.png
---

`Microsoft.AspNetCore.Mvc.Versioning` install

https://localhost:44302/v1.0/bands/4

```csharp
services.AddApiVersioning(opts =>
{
    opts.ReportApiVersions = true;
    opts.DefaultApiVersion = new ApiVersion(1, 0);
    opts.AssumeDefaultVersionWhenUnspecified = true;
});
```

`api-supported-versions` response header

Pass `X-Api-Version` 1.0

```csharp
opts.ApiVersionReader = new HeaderApiVersionReader("X-API-Version");
```



APIs are UIs for developers

When you ship an API, your consumers depend on you. Most importantly, they depend on you to not break their existing code. trust you  for one thing: don't break my shit. Don't break my shit, OK? Example, example, example.

Here's an example of me calling an API with an endpoint of `https://mycoolapi.com/api/bands/4`, which returns a specific `Band` with an `id` of `4`:

```json
{
    "id": 4,
    "name": "The Eagles, man"
}
```

Now, let's say I decide to update my API schema with a new property. I want to return a `FoundedYear`:

```json
{
    "id": 4,
    "name": "The Eagles, man",
    "foundingYear": 1971
}
```

You likely won't break too many clients here. It's a new field, so existing apps might happily ignore it. This should be documented in The Long Run, for sure, but it certainly isn't a breaking change.

Let's say you want the `name` to instead be a collection of `names` associated with the band, like this:

```json
{
    "id": 4,
    "names": [
        "The Eagles, man",
        "The Eagles"
    ],  
    "foundingYear": 1971
}
```

You introduced a *breaking change*. Your consumers expected a string value, and now you're returning a collection of strings. When consumers call the API, your client's applications will not work as intended and your users are not going to Take It Easy.

While breaking changes should be kept to a minimum, they are inevitable. To better communicate breaking changes to consumers of your API, you should introduce some kind of API versioning.

In this post, I'm going to review common API versioning approaches. Then, I'll show you how to implement them in your ASP.NET Core APIs.

## URI versioning

tightly couples client and server

It should use web standards where they make sense
It should be friendly to the developer and be explorable via a browser address bar
It should be simple, intuitive and consistent to make adoption not only easy but pleasant
It should provide enough flexibility to power majority of the Enchant UI
It should be efficient, while maintaining balance with the other requirements

Likewise, minor and patch versions shouldn’t go into other names, such as media types or link relations. There is a train of thought that you don’t need to have numeric major versions at all, since you can call the first one “foo” and the second one “bar” – but that’s just a matter of taste.

Second, it’s also intermingling the version into identifiers. Because URIs are used in the Web as the fundamental identifier by so many things – caches, spiders, forms, and so on – embedding information that’s likely to change into them make them unstable, thereby reducing their value. For example, a cache invalidates content associated with a URL when a POST request flows through it; if the URL is different because there are different versions floating around, it doesn’t work.

THEY SHOULD REPRESENT THE ENTITY BUT IT'S SO EASY TO USE!
You should try to think of the URL as being a path to the concept you would like - not how you want the item returned
Keeping backwards compatibility forever is often cost-prohibitive and/or very difficult.

With URI versioning, you add a version number to the URI for each of your resources. For example, I can move my `names` change to a new version, and include it in `https://mycoolapi.com/v2/api/bands/4`

```json
{
    "id": 4,
    "names": [
        "The Eagles, man",
        "The Eagles"
    ],  
    "foundingYear": 1971
}
```

https://stripe.com/docs/api/versioning

There are a lot of mixed opinions as to whether the API consumer should create links or whether links should be provided to the API. RESTful design principles specify HATEOAS which roughly states that interaction with an endpoint should be defined within metadata that comes with the output representation and not based on out-of-band information.

Although the web generally works on HATEOAS type principles (where we go to a website's front page and follow links based on what we see on the page), I don't think we're ready for HATEOAS on APIs just yet. When browsing a website, decisions on what links will be clicked are made at run time. However, with an API, decisions as to what requests will be sent are made when the API integration code is written, not at run time. Could the decisions be deferred to run time? Sure, however, there isn't much to gain going down that route as code would still not be able to handle significant API changes without breaking. That said, I think HATEOAS is promising but not ready for prime time just yet. Some more effort has to be put in to define standards and tooling around these principles for its potential to be fully realized.

For now, it's best to assume the user has access to the documentation & include resource identifiers in the output representation which the API consumer will use when crafting links. There are a couple of advantages of sticking to identifiers - data flowing over the network is minimized and the data stored by API consumers is also minimized (as they are storing small identifiers as opposed to URLs that contain identifiers).

Also, given this post advocates version numbers in the URL, it makes more sense in the long term for the API consumer to store resource identifiers as opposed to URLs. After all, the identifier is stable across versions but the URL representing it is not!

This is very common in the "real world"—due to its simplicity, even though it's dependent on the server routing correctly. As you can imagine, it can also become a maintenance headache if you roll out a lot of new versions. If you are a REST purist, you would say, *"Why is the URI different when I'm getting at the same resource?"* 

Also, if you implement HATEOAS, you need to include the version number in *every* URI.

## Query string versioning

Rather than dealing with managing a bunch of versioned URIs, you can specify a resource version with query string parameters. In my example, I would return the latest data using `https://mycoolapi.com/v2/api/bands/4?version=2.0`. With this approach, you need to think about default behavior. Where will users go if you leave off the query string?

This satisfies the purists since the same resource is retrieved using the same URI. You'll need to have code to handle the request that parses the query string. You'll have the same HATEOAS considerations.

## Header versioning

THIS SUCKS BECAUSE IT ISN'T A SEMANTIC WAY OF DESCRIVING THE RESOURCE. WHY REPRODUCE THE ACCEPT HEADER?

For something completely different, you can choose to implement a custom header that indicates the version of the resource. In this case, the client application sends along a custom request header (and, of course, you'll need to have default behavior if none is specified).

In this case, here's how your header would look:

```json
GET https://mycoolapi.com/bands/4 HTTP/1.1
X-Api-Version: api-version=2.0

{
    "id": 4,
    "names": [
        "The Eagles, man",
        "The Eagles"
    ],  
    "foundingYear": 1971
}
```

As with the other approaches, you'll also need to worry about maintaining HATEOAS links with the appropriate headers.

## Media type versioning

THESE SUCK BECAUSE THEY ARE SO HARD TO TEST - CAREFULLY CONSTRUCT THE REQUEST AND CONFIGURE THE ACCEPT HEADER

DON'T BREAK THE WHOLE STABLE CONTRACT GOAL


When a client passes a request to you, it'll use an `Accept` header to dictate the format of the request it can handle. These days, the most common `Accept` value is `application/json` (but you can also pass along a myriad of other formats).

You can also use media type versioning here, where the client can define with version of your API it will accept.

For example, let's evaluate this header.

```
GET https://mycoolapi.com/bands/4 HTTP/1.1
Accept: application/vnd.my-cool-api.v2+json
```

The `vnd.my-cool-api.v2` element notes we're using `v2` of our resource and the `.json` means it's accepting—you guessed it—JSON. Here, our code honors the `Accept` header the best we can, and then we can confirm the data format in the `Content-Type` response header as well.

```
HTTP/1.1 200 OK
Content-Type: application/vnd.my-cool-api.v2+json; charset=utf-8
```

This approach is a REST purist's dream and is very HATEOAS-friendly. You can return the MIME type of related data in the resource links.

# References

- [Web API design](https://docs.microsoft.com/azure/architecture/best-practices/api-design) (Microsoft Docs)

