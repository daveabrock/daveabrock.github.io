---
date: "2021-02-24"
title: "Use Azure Functions with .NET 5"
description: "In this post, we upgrade a Functions app to .NET 5."
tags: azure, functions]
image: /assets/img/functions-dotnet-5.png
readtime: true
---

A little while ago, I [wrote a post](https://daveabrock.com/2020/11/16/httprepl-openapi-swagger-netcoreapis) showing off how to use Open API and HttpRepl with ASP.NET Core 5 Web APIs. Using a new out-of-process model, you can now use .NET 5 with Azure Functions.

Traditionally, .NET support on Azure Functions has been tied to the Azure Functions runtime. You couldn't just expect to use the new .NET version in your Functions as soon as it was released. As [Anthony Chu writes](https://techcommunity.microsoft.com/t5/apps-on-azure/net-5-support-on-azure-functions/ba-p/1973055), an updated version of the host is also required. Because .NET 5 is not LTS, and Microsoft needs to support specific releases for extended periods, they can't upgrade the host to a non-LTS version because it isn't supported for very long (15 months from the November 2020 release).

This doesn't mean you can't use .NET 5 with your Azure Functions. To do so, the team has rolled out a new out-of-process model that runs a worker process along the runtime. Because it runs in a separate process, you don't have to worry about runtime and host dependencies. Looking long-term: it provides the ability to run the latest available version of .NET without waiting for a Functions upgrade.

There's now an [early preview of the .NET 5 worker](https://github.com/Azure/azure-functions-dotnet-worker-preview), with plans to be generally available in "early 2021." I used the repository from my previous post to see how it works. 

**Note**: Before embarking on this adventure, you'll need the [.NET 5 SDK](https://dotnet.microsoft.com/download/dotnet/5.0) installed as well as [Azure Functions Core Tools](https://github.com/Azure/azure-functions-core-tools) (a version >= `3.0.3160`).

# Update `local.settings.json`

Update `local.settings.json` and change the `FUNCTIONS_WORKER_RUNTIME` to `dotnet-isolated`. This is likely a short-term fix, as the worker will be targeted to future .NET versions.

```json
{
  "IsEncrypted": false,
  "Values": {
    "FUNCTIONS_WORKER_RUNTIME": "dotnet-isolated",
    "AzureWebJobsStorage": ""
}
```

# Update project file

You'll need to update your project file to the following:

```xml
<PropertyGroup>
  <TargetFramework>net5.0</TargetFramework>
  <LangVersion>preview</LangVersion>
  <AzureFunctionsVersion>v3</AzureFunctionsVersion>
  <OutputType>Exe</OutputType>
  <_FunctionsSkipCleanOutput>true</_FunctionsSkipCleanOutput>
</PropertyGroup>
```

You might be wondering why the `OutputType` is `exe`. A .NET 5 Functions app is actually a .NET console app executable running in a separate process. Save that for Trivia Night. You'll also note it uses the `v3` host and uses a `_FunctionsSkipCleanOutput` flag to preserve important files.

You'll also need to update or install quite a few NuGet packages. It'll probably be quickest to copy and paste these into your project file, save it, and let the NuGet restore process take over.

```xml
<ItemGroup>
    <PackageReference Include="Microsoft.Azure.Functions.Worker" Version="1.0.0-preview4" />
    <PackageReference Include="Microsoft.Azure.Functions.Worker.Sdk" Version="1.0.0-preview4" OutputItemType="Analyzer" />
    <PackageReference Include="Microsoft.Azure.WebJobs.Extensions.Http" Version="3.0.12" />
    <PackageReference Include="Microsoft.Azure.WebJobs.Extensions.Storage" Version="4.0.3" />
    <PackageReference Include="Microsoft.Azure.WebJobs.Script.ExtensionsMetadataGenerator" Version="1.2.0" />
    <PackageReference Include="System.Net.NameResolution" Version="4.3.0" />
</ItemGroup>
```

Lastly, you'll need to include the following so the worker behaves correctly on Linux.

```xml
<Target Name="CopyRuntimes" AfterTargets="AfterBuild" Condition=" '$(OS)' == 'UNIX' ">
    <!-- To workaround a bug where the files aren't copied correctly for non-Windows platforms -->
    <Exec Command="rm -rf $(OutDir)bin/runtimes/* &amp;&amp; mkdir -p $(OutDir)bin/runtimes &amp;&amp; cp -R $(OutDir)runtimes/* $(OutDir)bin/runtimes/" />
</Target>
```

# Add code to use EF in-memory database

In my previous post, I used the Entity Framework in-memory database to quickly get going. I can add it just as easily in my Azure Function.

In my `Data` folder, I have an `ApiModels.cs` file to store my models as C# 9 records, a `SampleContext`, and a `SeedData` class.

The `ApiModels.cs`:

```csharp
using System.ComponentModel.DataAnnotations;

namespace FunctionApp.Data
{
    public class ApiModels
    {
        public record Band(int Id, [Required] string Name);
    }
}
```

My `SampleContext.cs`:

```csharp
using Microsoft.EntityFrameworkCore;

namespace FunctionApp.Data
{
    public class SampleContext : DbContext
    {
        public SampleContext(DbContextOptions<SampleContext> options)
            : base(options)
        {
        }

        public DbSet<ApiModels.Band> Bands { get; set; }
    }
}
```

And here's the `SeedData` class:

```csharp
using System.Linq;

namespace FunctionApp.Data
{
    public class SeedData
    {
        public static void Initialize(SampleContext context)
        {
            if (!context.Bands.Any())
            {
                context.Bands.AddRange(
                    new ApiModels.Band(1, "Led Zeppelin"),
                    new ApiModels.Band(2, "Arcade Fire"),
                    new ApiModels.Band(3, "The Who"),
                    new ApiModels.Band(4, "The Eagles, man")
                );

                context.SaveChanges();
            }
        }
    }
}
```

# Enjoy a better middleware experience

With the new worker, I can enjoy a better dependency injection experience—I can finally do away with all that `[assembly: FunctionsStartup(typeof(Startup))]` business. It feels more natural, like I'm working in ASP.NET Core.

In `Program.cs`, I added a function to seed my database as startup time:

```csharp
private static void SeedDatabase(IHost host)
{
    var scopeFactory = host.Services.GetRequiredService<IServiceScopeFactory>();

    using var scope = scopeFactory.CreateScope();
    var context = scope.ServiceProvider.GetRequiredService<SampleContext>();

    if (context.Database.EnsureCreated())
    {
        try
        {
            SeedData.Initialize(context);
        }
        catch (Exception ex)
        {
            var logger = scope.ServiceProvideGetRequiredService<ILogger<Program>>();
            logger.LogError(ex, "A database seeding error occurred.");
        }
    }
}
```

In the `Main` method, I can inject services and work with my middleware. (More on that `Debugger.Launch()` later.)

```csharp
using System;
using System.Threading.Tasks;
using Microsoft.Extensions.Configuration;
using System.Diagnostics;
using FunctionApp.Data;
using Microsoft.Extensions.Hosting;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Azure.Functions.Worker.Configuration;
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.Logging;

static async Task Main(string[] args)
{
#if DEBUG
    Debugger.Launch();
#endif
    var host = new HostBuilder()
        .ConfigureAppConfiguration(c =>
        {
            c.AddCommandLine(args);
        })
        .ConfigureFunctionsWorker((c, b) =>
        {
            b.UseFunctionExecutionMiddleware();
        })
        .ConfigureServices(s =>
        {
            s.AddSingleton<IHttpResponderService, DefaultHttpResponderService>();
            s.AddDbContext<SampleContext>(options =>
                options.UseInMemoryDatabase("SampleData"));
        })
        .Build();
    SeedDatabase(host);

    await host.RunAsync();
}
```

# Write the function

With all that out of the way, I can write my Function. I started with a get, but all the other endpoints from the previous post should follow the same pattern.

```csharp
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;
using FunctionApp.Data;
using Microsoft.Azure.Functions.Worker;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;

namespace FunctionApp
{
    public class BandsFunction
    {
        private readonly SampleContext _context;

        public BandsFunction(SampleContext context)
        {
            _context = context;
        }

        [FunctionName("BandsGet")]
        public async Task<List<ApiModels.Band>> Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)]
            HttpRequestData req) =>
                _context.Bands.ToList();
    }
}
```

This is a simple example, but as you dig a little more you'll notice that in .NET 5 you'll replace `HttpRequest` with `HttpRequestData` and `ILogger` with `FunctionExecutionContext`.

# About the tooling

As with early preview capabilities, the tooling isn't that far along. For example, right now you can't expect to hit F5 in Visual Studio to debug your Function. To run the sample locally, you'll need to use Azure Functions Core Tools from the command line.

From the prompt `cd` into the project directory and execute the following:

```bash
func host start --verbose
```

To debug in Visual Studio, uncomment the `Debugger.Launch()` lines in `Program.cs` then run the command.

For VS Code, you'll need to install the Functions extension, select the `Attach to .NET Functions` launch task, then start debugging.

# Wrap up

In this post, I introduced the new Functions worker, which allows you to use .NET 5 with your Azure Functions—and will allow more immediate release support in the future. We updated our project files, learned about the easier DI experience, and walked through the tooling experience. It takes some manual work to get this going, but tooling should improve throughout time.

Next, you can look into [deploying your app to Azure](https://github.com/Azure/azure-functions-dotnet-worker-preview#deploying-to-azure). Right now, the team is warning you can [only do this for Windows plans and it isn't completely optimized yet](https://github.com/Azure/azure-functions-dotnet-worker-preview#deploying-to-azure).

Take a look at the GitHub repo for more details and for a sample app.

# References

- [.NET 5 Support on Azure Functions](https://techcommunity.microsoft.com/t5/apps-on-azure/net-5-support-on-azure-functions/ba-p/1973055) (Anthony Chu)
- [Azure Functions .NET 5 Support](https://github.com/Azure/azure-functions-dotnet-worker-preview) (GitHub)
- [(Preview) Creating Azure Functions using .NET 5](https://codetraveler.io/2021/02/12/creating-azure-functions-using-net-5/) (Brandon Minnick)