---
date: "2020-12-27"
title: "Blast Off with Blazor: Prerender a Blazor Web Assembly application"
subtitle: "In this post, we speed up initial load time by prerendering our Blazor Web Assembly application."
tags: [aspnet-core]
share-img: /assets/img/prerender-card.png
readtime: true
---

So far in our series, we've [walked through the intro](https://daveabrock.com/2020/10/26/blast-off-blazor-intro), [wrote our first component](https://daveabrock.com/2020/10/28/blast-off-blazor-404-page), [dynamically updated the HTML head from a component](https://daveabrock.com/2020/11/08/blast-off-blazor-update-head), [isolated our service dependencies](https://daveabrock.com/2020/11/22/blast-off-blazor-service-dependencies), worked on [hosting our images over Azure Blob Storage and Cosmos DB](https://daveabrock.com/2020/12/13/blast-off-blazor-cosmos), and [built a responsive image gallery](https://daveabrock.com/2020/12/16/blast-off-blazor-responsive-gallery).

{: .box-note}
I did some housekeeping with the code that I won't mention here, such as renaming projects. Check out [the GitHub repo](https://github.com/daveabrock/NASAImageOfDay) for the latest.

Now, we need to discuss how to mitigate the biggest drawback of using Blazor Web Assembly: the initial load time. While the Web Assembly solution allows us to enjoy a serverless deployment scenario and fast interactions, the initial download can take awhile. Users need to sit around while the browser downloads the .NET runtime and all your project's DLLs.

While the ASP.NET Core team has made a lot of progress in improving the .NET IL linker and making framework libraries more linkable, the initial startup time of Blazor Web Assembly apps is still quite noticeable. In my case, an initial, uncached download takes *7 seconds*. With prerendering, I was able to cut this down considerably. That is the focus of this post.

{: .box-warning}
With ahead-of-time (AoT) compilation [coming with .NET 6](https://github.com/dotnet/aspnetcore/issues/5466), you might be inclined to treat it as a fix to this problem. AoT will not help decrease the app download size because Web Assembly uses an Intermediate Language (IL) interpreter and isn't a JIT-based runtime. In terms of performance, AoT can speed up runtime performance but will mean an even *larger* download size. See [Dan Roth's comment for details](https://github.com/dotnet/aspnetcore/issues/5466#issuecomment-639890420).

This post covers the following content.

- [What is prerendering?](#what-is-prerendering)
- [Configure the `BlastOff.Server` project](#configure-the-blastoffserver-project)
- [Share the service layer](#share-the-service-layer)
- [Check our progress](#check-our-progress)
- [Wrap up](#wrap-up)
- [References](#references)

{: .box-note}
This post builds off the wonderful content from [Chris Sainty](https://chrissainty.com/prerendering-a-client-side-blazor-application/), [Jon Hilton](https://jonhilton.net/blazor-wasm-prerendering/), and the [ASP.NET Core documentation](https://docs.microsoft.com/aspnet/core/blazor/components/prerendering-and-integration?view=aspnetcore-5.0&pivots=webassembly).

# What is prerendering?

Without prerendering, your Blazor Web Assembly app parses the `index.html` file it receives, then fetches all your dependencies. Then, it'll load and launch your application—leaving you with a less-than-ideal UX as users are sitting and waiting. With prerendering, the initial load occurs on the server with the download occurring in the background. By the time the page quickly loads and the download completes, your users are ready to interact with your site—with a big Blazor Web Assembly drawback out of the way. 

When we say *prerendering*, we really mean *prerendering components*. We do this by integrating our app using Razor Pages. The cost here: you're kissing goodbye to a simple static file deployment in favor of faster startup times.

{: .box-note}
This is not simply converting your app to [a Blazor Server solution](https://docs.microsoft.com/aspnet/core/blazor/hosting-models?view=aspnetcore-5.0#blazor-server). While Blazor Server executes your application from a server-side ASP.NET Core app, this is compiling your code and serving static assets from a server project. You're still running your app in a browser as you were before.

To get started, we need to create and configure a `Server` project.

# Configure the `BlastOff.Server` project

To get started, you'll need to create a new `BlastOff.Server` project and add it to the solution. The easiest and simplest way is using the `dotnet` CLI.

To create a new project, I executed the following from the root of the project (where the solution file is):

```bash
dotnet new web -o BlastOff.Server
```

Then, I added it to my existing solution:

```bash
dotnet sln add BlastOff.Server/BlastOff.Server.csproj
```

In the new `BlastOff.Server` project, I headed over to `Startup.cs` and updated the `Configure` method to the following:

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    public void Configure(IApplicationBuilder app)
    {
        // some code removed for brevity

        app.UseBlazorFrameworkFiles();
        app.UseStaticFiles();

        app.UseRouting();

        app.UseEndpoints(endpoints =>
        {
            endpoints.MapRazorPages();
            endpoints.MapControllers();
            endpoints.MapFallbackToPage("/_Host");
        });
    }
}
```

With the `IApplicationBuilder` interface, we'll call `UseBlazorFrameworkFiles()` that will allow us to serve Blazor Web Assembly files from a specified path. The `UseStaticFiles()` call enables us to serve static files.

After enabling routing, we use the endpoint middleware to work with Razor Pages, which is the secret sauce behind this magic. Then, in `MapFallbackToPage()`, we're serving a `/_Host.cshtml` file in the `Server` project's `Pages` directory.

This `_Host.cshtml` file will replace our `Client` project's `index.html` file. Therefore, we can copy the contents of our `index.html` here.

You'll notice below that we:

- Set the namespace to `BlastOff.Server.Pages`
- Set a `@using` directive to the `BlastOff.Client` project
- Make sure the `<link href>` references resolve correctly
- Update the [Component Tag Helper](https://docs.microsoft.com/aspnet/core/mvc/views/tag-helpers/built-in/component-tag-helper?view=aspnetcore-5.0) to include a `render-mode` of `WebAssemblyPrerendered`. This option prerenders the component into static HTML and includes a flag for the app to make the component interactive when the browser loads it
- Ensure the Blazor script points to the client script at `_framework/blazor.webassembly.js`

Here's the `_Host.cshtml` file in its entirety:

```html
@page "/"
@namespace BlastOff.Server.Pages
@using BlastOff.Client

<html>
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Blast Off With Blazor</title>
    <base href="~/" />
    <link rel="stylesheet" href="css/bootstrap/bootstrap.min.css" />
    <link href="css/site.css" rel="stylesheet" />
</head>
<body>
    <app>
        <component type="typeof(App)" render-mode="WebAssemblyPrerendered" />
    </app>

    <script src="_framework/blazor.webassembly.js"></script>
</body>
</html>
```

With this in place, I headed over to the `BlastOff.Client` project to delete `wwwroot/index.html` and this line from `Program.Main`. It felt both nice and weird.

```csharp
builder.RootComponents.Add<App>("#app");
```

# Share the service layer

With prerendering, we need to think about how our calls to Azure Functions work. Previously, our API calls were from the `BlastOff.Client` project. With prerendering, it'll first need to make these calls from the server. As a result, it'll make better sense to move our service layer out of the `BlastOff.Client` project and to a new `BlastOff.Shared` project.

Here's our `ImageService` now (I also moved the `Image` model here for good measure):

```csharp
using Microsoft.Extensions.Logging;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Net.Http;
using System.Net.Http.Json;
using System.Threading.Tasks;

namespace BlastOff.Shared
{
    public interface IImageService
    {
        public Task<IEnumerable<Image>> GetImages(int days);
    }

    public class ImageService : IImageService
    {
        readonly IHttpClientFactory _clientFactory;
        readonly ILogger<ImageService> _logger;

        public ImageService(ILogger<ImageService> logger, IHttpClientFactory clientFactory)
        {
            _clientFactory = clientFactory;
            _logger = logger;
        }

        public async Task<IEnumerable<Image>> GetImages(int days)
        {
            try
            {
                var client = _clientFactory.CreateClient("imageofday");
                var images = await client.GetFromJsonAsync
                    <IEnumerable<Image>>($"api/image?days={days}");
                return images.OrderByDescending(img => img.Date);
            }
            catch (Exception ex)
            {
                _logger.LogError(ex.Message, ex);
            }

            return default;
        }
    }
}
```

We can thank ourselves for [isolating our service dependency a few posts ago](https://daveabrock.com/2020/11/22/blast-off-blazor-service-dependencies)–turning a potential headache to some copying and pasting. The only cleanup in our `BlastOff.Client` project is to bring in the `BlastOff.Shared` project to reference our service in `Program.cs`:

```csharp
using System;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Components.WebAssembly.Hosting;
using Microsoft.Extensions.DependencyInjection;
using BlastOff.Shared;

namespace BlastOff.Client
{
    public class Program
    {
        public static async Task Main(string[] args)
        {
            var builder = WebAssemblyHostBuilder.CreateDefault(args);
            builder.Services.AddHttpClient("imageofday", iod =>
            {
                iod.BaseAddress = new Uri("http://localhost:7071" ?? builder.HostEnvironment.BaseAddress);
            });
            builder.Services.AddScoped<IImageService, ImageService>();

            await builder.Build().RunAsync();
        }
    }
}
```

If we run the app, it loads much quicker. When we browse to `/images` it loads fast too. But if we hit reload, we'll see a nasty error because we didn't register the `ImageService` in our `Server` project. In the DI container logic in `Startup.ConfigureServices`, then, add the `HttpClient` and `ImageService` references:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddRazorPages();

    services.AddHttpClient("imageofday", iod =>
    {
        iod.BaseAddress = new Uri("http://localhost:7071");
    });

    services.AddScoped<IImageService, ImageService>();
}
```

# Check our progress

Check out how much better everything looks now. Much better.

![Our slow site]({{ site.url }}{{ site.baseurl }}/assets/img/Prerender.gif)

# Wrap up

In this post, we worked through how to prerender our Blazor Web Assembly application. We created a new server project, shared our dependencies in a new project, and understood how prerendering can significantly improve initial load times.

# References

- [Prerendering a Client-side Blazor Application](https://chrissainty.com/prerendering-a-client-side-blazor-application/) (Chris Sainty)
- [Prerendering your Blazor WASM application with .NET 5](https://jonhilton.net/blazor-wasm-prerendering/) (Jon Hilton)
- [Prerender and integrate ASP.NET Core Razor components](https://docs.microsoft.com/aspnet/core/blazor/components/prerendering-and-integration?view=aspnetcore-5.0&pivots=webassembly) (Microsoft Docs)
