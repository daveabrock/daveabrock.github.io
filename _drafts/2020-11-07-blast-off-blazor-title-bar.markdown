---
date: "2020-11-07"
title: "Blast Off with Blazor: Dynamically update the HTML head from a component"
excerpt: "In the latest post, we'll add our favicons and update our title bar dynamically."
tags: [blazor, aspnet-core]
header:
    overlay_image: /assets/images/404-card.png
    overlay_filter: 0.8
---

So far in this series, we've [walked through a project intro](https://daveabrock.com/2020/10/26/blast-off-blazor-intro) and also [got our feet wet with your first component](https://daveabrock.com/2020/10/28/blast-off-blazor-404-page).

It's time to update our branding. While I love the official NASA icon in the header—and people might start thinking I work for NASA, which has always been a dream of mine—I probably should take that down. Additionally, my favicon is the boring default so we'll make that a little more exciting. Since we're dealing with the browser's title bar already, it's a good time to show off a welcome addition with .NET 5: the ability to dynamically update your HTML head from a component. Let's dig in.

This post contains the following content.

- [Why should I need to update my HTML head dynamically?](#why-should-i-need-to-update-my-html-head-dynamically)
- [Update header icon](#update-header-icon)
- [Add favicons to our app](#add-favicons-to-our-app)
- [Dynamically update the HTML head](#dynamically-update-the-html-head)
  - [Add a package reference](#add-a-package-reference)
  - [Reference the project](#reference-the-project)
  - [Update page title dynamically](#update-page-title-dynamically)
- [But what about the tests, Dave?](#but-what-about-the-tests-dave)
- [Wrap up](#wrap-up)

# Why should I need to update my HTML head dynamically?

When I speak of the "HTML head" I'm referring to the contents of the `<head>` tag in your HTML tree. 

Before .NET 5, you'd have to use JavaScript interopability to pull this off. It wouldn't be too hard but a pain nonethetheless.

You'd likely drop the following JavaScript at the bottom of your HTML in question (likely `index.html`):

```javascript
<script>
    function SetTitle(title) {
        document.title = title;
    }
</script>
```

Then, you'd have to inject JavaScript interop and probably create a special component for it:

```csharp
@inject IJSRuntime JSRuntime

// markup here...

@code {
    protected override async Task OnAfterRenderAsync(bool firstRender)
    {
        await JSRuntime.InvokeVoidAsync("SetTitle", Value);
    }
}
```

Then, you'd drop the component on your page...

```html
<PageTitle Value="@GetPageTitle" />
```

...which calls your logic to get the title.

```csharp
@code {

    private int GetYear(DateTime date)
    {
        return date.Year;
    }

    private string GetPageTitle()
    {
        return image is null ? "Blasting off..." : $"New! From {GetYear(image.Date)}!";
    }
}
```


# Update header icon

Did you know the .NET team has a [branding GitHub repository](https://github.com/dotnet/brand) where you can look at logos, presentation templates, wallpapers, and a bunch of illustrations of the purple .NET bot? For our example, we'll use the [bot using a jetpack](https://github.com/dotnet/brand/blob/master/dotnet-bot-illustrations/dotnet-bot/dotnet-bot_jetpack-faceing-right.png) because—obviously. We're blasting off, after all. 

After dropping the file in our `wwwroot/images` [directory](https://github.com/daveabrock/NASAImageOfDay/tree/main/Client/wwwroot/images), we can edit our `NavBar` component in our `Shared` directory to include our new file.

```html
<nav class="flex items-center justify-between flex-wrap bg-black p-6 my-bar">
    <div class="flex items-center flex-shrink-0 text-white mr-6">
        <img src="images/dotnet-bot-jetpack.png">
        <span class="font-semibold text-xl tracking-tight">Blast Off with Blazor</span>
    </div>
    <! -- more html left out for brevity -->
</nav>
```

I feel better and, as a bonus, the fine folks at NASA don't have to worry about asking me to take their official logo down.

# Add favicons to our app

When you create a Blazor app (our any ASP.NET Core app, for that matter), a bland `favicon.ico` file is dropped in your `wwwroot` directory. (For the uninitiated, [a favicon](https://en.wikipedia.org/wiki/Favicon) is the small icon in the top-left corner of your browser tab, right next to the page title.) This might lead you to believe that if you want to use favicons, you can either use that file or overwrite it with one of your own—then move on with your life.

While you *can* do that, you probably shouldn't. These days you're dealing with different browser requirements and multiple platforms (Windows, iOS, Mac, Android) where just one *favicon.ico* will not get the job done. For example, iOS users can pin a site to their homescreen—and iOS will grab your favicon  icon as the "app" image. 

Thanks to Andrew Lock's [wonderful post on the topic](https://andrewlock.net/adding-favicons-to-your-asp-net-core-website-with-realfavicongenerator/), I went over to the [RealFaviconGenerator](https://realfavicongenerator.net/) to lend a hand. (If you want more details on the process, like how to create your own, check out his post.)

To get all our required favicons, we go to 
1. From the [RealFaviconGenerator](https://realfavicongenerator.net/) site, upload your image
2. At the next page, adjust any device-specific settings, then click **Generate your Favicons and HTML code**
3. The next page contains instructions on how to add the favicons to your app. Thanks to Andrew's post, they have an "ASP.NET Core" section.

In `index.html` in the `wwwroot` directory, add the following inside the `<head>` tag:

```html
<link rel="apple-touch-icon" sizes="76x76" href="/apple-touch-icon.png">
<link rel="icon" type="image/png" sizes="32x32" href="/favicon-32x32.png">
<link rel="icon" type="image/png" sizes="16x16" href="/favicon-16x16.png">
<link rel="manifest" href="/site.webmanifest">
<link rel="mask-icon" href="/safari-pinned-tab.svg" color="#5bbad5">
<meta name="msapplication-TileColor" content="#da532c">
<meta name="theme-color" content="#ffffff">
```

While the instructions recommend putting the favicons in an isolated partial view, I was fine with putting it right into my deployed static `index.html` file—it's a one-time step and this is the only file that needs it.

# Dynamically update the HTML head

Before you work with this feature, you'll need to add a package reference, include it in your project, and add a script reference.

## Add a package reference

From our `Client` directory of our project, open your terminal and add a package reference using the `dotnet` CLI.

```bash
dotnet add package Microsoft.AspNetCore.Components.Web.Extensions --prerelease
```

## Reference the project

We can use one of two options to reference this so our component can use it. Our first option is in our `Index.razor` component:

```html
@using Microsoft.AspNetCore.Components.Web.Extensions.Head
```

Or, we can include it in our `_Imports.razor` file. The `_Imports.razor` file, which sits at the root of our `Client` project, allows us to put all our imports in one spot. That way, we won't have to manually add it to all our components that need it. With my new addition, here's what my `_Imports.razor` file looks like now:

```html
@using System.Net.Http
@using System.Net.Http.Json
@using Microsoft.AspNetCore.Components.Forms
@using Microsoft.AspNetCore.Components.Routing
@using Microsoft.AspNetCore.Components.Web
@using Microsoft.AspNetCore.Components.Web.Virtualization
@using Microsoft.AspNetCore.Components.WebAssembly.Http
@using Microsoft.JSInterop
@using Client
@using Client.Shared
@using Data
@using Microsoft.AspNetCore.Components.Web.Extensions.Head
```

Either method works, but I'm going with the latter option for reusability.

## Update page title dynamically

We're going to update your page title dynamically. While we're waiting for the API call, the page title reads *Blasting off...* When we get a response back, the page title will change to *New! From YEAR*, where the year is the year the image was taken—grabbed from the `Date` property of the returned object.

In the `@code` block, we'll add a new method to parse the year from our date (nothing that will break your brain):

```csharp
private int GetYear(DateTime date)
{
    return date.Year;
}
```

Then, we'll write a ternary statement to decide which message to display in the title bar.

```csharp
private string GetPageTitle()
{
    return image is null ? "Blasting off..." : $"New! From {GetYear(image.Date)}!";
}
```

Now, all that's left is to add a `<Title>` component. We'll pass our method as a parameter to the component. At the top of our component, right under the `@page` and `@inject` directives, add this:

```html
<Title Value="@GetPageTitle()"></Title>
```

(I've added a `Thread.Sleep(2000)` after the API call for demonstration purposes, but it obviously isn't recommended.)

# But what about the tests, Dave?

I've placed an emphasis on [testing our components](https://daveabrock.com/2020/10/28/blast-off-blazor-404-page#test-our-component-with-the-bunit-library) with the bUnit library. We'll work on testing this, and the rest of our `Index` component, in the next post. We need to mock a few things, like `HttpClient`, and I'm currently researching the best way to do so. Our next post will focus on testing Blazor components with `HttpClient` dependencies.

# Wrap up

In this post, we updated our site image and also showed how to include favicons in a Blazor WebAssembly project. Next, we dynamically updated the HTML head by setting the page title based on the image. 

Stay tuned for the next post where we work on writing tests for our `Index` component.










