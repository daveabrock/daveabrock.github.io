---
date: "2021-01-14"
title: "Blast Off with Blazor: Build a search-as-you-type box"
subtitle: "In this post, we build a quick search box that filters our images."
tags: [aspnet-core, blast-off-blazor]
share-img: /assets/img/search-type-card.png
readtime: true
---

So far in our series, we've [walked through the intro](https://daveabrock.com/2020/10/26/blast-off-blazor-intro), [wrote our first component](https://daveabrock.com/2020/10/28/blast-off-blazor-404-page), [dynamically updated the HTML head from a component](https://daveabrock.com/2020/11/08/blast-off-blazor-update-head), [isolated our service dependencies](https://daveabrock.com/2020/11/22/blast-off-blazor-service-dependencies), worked on [hosting our images over Azure Blob Storage and Cosmos DB](https://daveabrock.com/2020/12/13/blast-off-blazor-cosmos), [built a responsive image gallery](https://daveabrock.com/2020/12/16/blast-off-blazor-responsive-gallery), and [implemented prerendering](https://daveabrock.com/2020/12/27/blast-off-blazor-prerender-wasm).

With a full result set from Cosmos DB, in this post we'll build a quick search box. While I was originally thinking I should execute queries against our database, I then thought: *You dummy, you already have the data, just filter it*. My inner voice doesn't always offer sage advice, but it did this time.

Anyway, here's how it'll look.

![Our search box]({{ site.url }}{{ site.baseurl }}/assets/img/search-box.png)

You've likely noticed the absence of a search button. This will be in the "search-as-you-type" style, as results will get filtered based on what the user types. It's a lot less work than you might think.

This post covers the following content.

- [The "easy" part: our filtering logic](#the-easy-part-our-filtering-logic)
- [Understand data binding](#understand-data-binding)
  - [Understand how `@bind` and `@bind-value` work](#understand-how-bind-and-bind-value-work)
  - [Put it together](#put-it-together)
- [Wrap up](#wrap-up)

# The "easy" part: our filtering logic

In very simplistic terms, here's what we'll do: a user will type some text in the search box, and we'll use a LINQ query to filter it. Then, we'll display the filtered results.

In `Images.razor.cs`, we'll add `SearchText`. This field captures what the user enters. We'll take that string, and see if we have any items that have a part of the string in the `Title`. We'll want to make sure to initialize it to an empty string. That way, if nothing is entered, we'll display the whole result set.

```csharp
public string SearchText = "";
```

Then, I'll include a `FilteredImages` field. This will return a filtered `List<Image>` that uses the LINQ query in question to filter based on the `SearchText`.

```csharp
List<Image> FilteredImages => ImageList.Where(
    img => img.Title.ToLower().Contains(SearchText.ToLower())).ToList();
```

This part is relatively simple. We can do this in any type of app—Blazor, Web API, MVC, whatever. The power of Blazor is how we can leverage data binding to "connect" the data with our components to synchronize them and update our app accordingly. Let's first understand data binding as a general concept.

# Understand data binding

Data binding is not unique to Blazor. It's essential to virtually all single-page application (SPA) libraries and frameworks. You can't get away from displaying, updating, and receiving data.

The simplest example is with *one-way binding*. As inferred from the name, this means data updates only flow in one direction. In these cases, the application is responsible for handling the data. The gist: you have some data, an event occurs, and the data changes. There's no need for a user to control the data yet.

In terms of events, by far the most common is a button click in Blazor:

```csharp
<p>@TimesClicked</p>

<button @onclick="UpdateTimesClicked">Update Click Count</button>

@code {
    private int TimesClicked { get; set; } = 1;

    private void UpdateTimesClicked()
    {
        TimesClicked++;
    }
}
```

In this example the `TimesClicked` is bound to the component with the `@` symbol. When a user clicks a button, the value changes—and, because an event handler was executed, Blazor triggers a re-render for us. Don't let the simplicity of one-way binding fool you: in many cases, it's all you need.

Whenever we need input from a user, we need to track that data and also bind it to a field. In our case, we need to know what the user is typing, store it in `SearchText`, and execute filtering logic as it changes. Because it needs to flow in both directions, we need to use *two-way binding*. For ASP.NET Core in particular, [data binding involves](https://docs.microsoft.com/aspnet/core/blazor/components/data-binding?view=aspnetcore-5.0) ... binding ... a `@bind` HTML element with a field, property, or Razor expression.

## Understand how `@bind` and `@bind-value` work

In the ASP.NET Core [documentation for Blazor data binding](https://docs.microsoft.com/aspnet/core/blazor/components/data-binding?view=aspnetcore-5.0), this line should catch your eye:

>When one of the elements loses focus, its bound field or property is updated.

In this case `@bind` works with an `onchange` handler after the input loses focus (like when a user tabs out). However, that's not what we want. We want updates to occur as a user is typing and the ability to control which event triggers an update. After all, we don't want to wait for a user to lose focus (and patience). Instead, we can use `@bind-value`. When we use `@bind-value:event="event"`, we can specify a valid event like `oninput`, `keydown`, `keypress`, and so on. In our case, we'll want to use `oninput`, or whenever the user types something.

Don't believe me? That's fine, we're only Internet friends, I understand. You can go check out the compiled components in your project's `obj/Debug/net5.0/Razor/Pages` directory. Here's how my `BuildRenderTree` method looks with `@bind-value` (pay attention to the last line):

```csharp
protected override void BuildRenderTree(Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder __builder)
{
    __builder.OpenElement(0, "div");
    __builder.AddAttribute(1, "class", "text-center bg-blue-100");
    __builder.AddAttribute(2, "b-bc0k7zrx0q");
    __builder.OpenElement(3, "input");
    __builder.AddAttribute(4, "class", "border-4 w-1/3 rounded m-6 p-6 h-8\r\n               border-blue-300");
    __builder.AddAttribute(5, "placeholder", "Search by title");
    __builder.AddAttribute(6, "value", Microsoft.AspNetCore.Components.BindConverter.FormatValue(SearchText));
    __builder.AddAttribute(7, "oninput", Microsoft.AspNetCore.Components.EventCallback.Factory.CreateBinder(this, __value => SearchText = __value, SearchText));
    // and more ...
}
```

## Put it together

When we set `@bind-value` to `SearchText` and `@bind-value:event` to `oninput`, we'll be in business. Here's how we'll wire up the search box:

```html
<div class="text-center bg-blue-100">
    <input class="border-4 w-1/3 rounded m-6 p-6 h-8
               border-blue-300" @bind-value="SearchText"
           @bind-value:event="oninput" placeholder="Search by title" />
</div>
```

Finally, as we're iterating through our images, pass in `FilteredImages` instead:

```csharp
@foreach (var image in FilteredImages)
{
    <ImageCard ImageDetails="image" />
}
```

So now, here's the entire code for our `Images` component:

```csharp
@page "/images"
<div class="text-center bg-blue-100">
    <input class="border-4 w-1/3 rounded m-6 p-6 h-8
               border-blue-300" @bind-value="SearchText"
           @bind-value:event="oninput" placeholder="Search by title" />
</div>

@if (!ImageList.Any())
{
    <p>Loading some images...</p>
}
else
{
    <div class="p-2 grid grid-cols-1 sm:grid-cols-1 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-3">
        @foreach (var image in FilteredImages)
        {
            <ImageCard ImageDetails="image" />
        }
    </div>
}
```

And the `Images` partial class:

```csharp
using BlastOff.Shared;
using Microsoft.AspNetCore.Components;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;

namespace BlastOff.Client.Pages
{
    partial class Images : ComponentBase
    {
        public IEnumerable<Image> ImageList { get; set; } = new List<Image>();

        public string SearchText = "";

        [Inject]
        public IImageService ImageService { get; set; }

        protected override async Task OnInitializedAsync()
        {
            ImageList = await ImageService.GetImages(days: 70);
        }

        List<Image> FilteredImages => ImageList.Where(
            img => img.Title.ToLower().Contains(SearchText.ToLower())).ToList();
    }
}

```

Here's the search box in action. Look at us go.

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">It&#39;s so fast! Trying to remember the pains I&#39;ve had in my career implementing typeahead, and in <a href="https://twitter.com/hashtag/Blazor?src=hash&amp;ref_src=twsrc%5Etfw">#Blazor</a> it&#39;s super simple with data binding on change. <a href="https://t.co/zRjOmeUXCu">pic.twitter.com/zRjOmeUXCu</a></p>&mdash; Dave Brock (@daveabrock) <a href="https://twitter.com/daveabrock/status/1349882727621865473?ref_src=twsrc%5Etfw">January 15, 2021</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

# Wrap up

In this post, I wrote how to implement a quick "type-as-you-go" search box in Blazor. We wrote filtering logic, understood how data binding works, compared `@bind` and `@bind-value` in Blazor, and finally put it all together.

There's certainly more we can do here—things like debouncing come in handy when we want to control (and sometimes delay) how often searches are executed. I'm less concerned because this is all executed on the client, but would definitely come into play when we want to limit excessive trips to the database.
