---
date: "2020-11-14"
title: "Use OpenAPI, Swagger UI, and HttpRepl in ASP.NET Core 5 to supercharge your API development"
excerpt: "In the latest post, we'll do a thing, then another thing, and then yet another fing."
tags: [aspnet-core]

---

When developing APIs in ASP.NET Core, there are many tools at your disposal. Long gone are the days when you run your app from Visual Studio and call your `localhost` endpoint over Postman. Over the last few years, a lot of tools have emerged that use the OpenAPI specification—most commonly, [Swagger](https://github.com/swagger-api). Many ASP.NET Core API developers are familiar with Swagger UI, which in .NET uses a Swashbuckle library that builds a wonderful UI experience from your `swagger.json` file.

Additionally, I've been impressed by [the HttpRepl project](https://github.com/dotnet/HttpRepl), which allows you explore your APIs from the command-line. Different from utilities [like curl](https://curl.se/), it allows you to explore APIs from the command-line where you would with files—all from a simple and lightweight interface.

While these utilities are not new with ASP.NET Core 5, with this version it's much easier to get started. This post will discuss how to get up and running with these tools in .NET 5.

Before you get started, you can have an API ready to use. Or, you can use mine.

This post contains the following content.

- [What's the difference between OpenAPI and Swagger?](#whats-the-difference-between-openapi-and-swagger)
- [OpenAPI now shipped with ASP.NET Core 5 templates by default](#openapi-now-shipped-with-aspnet-core-5-templates-by-default)
- [Wrap up](#wrap-up)
- [Resources](#resources)

# What's the difference between OpenAPI and Swagger?

If you've heard OpenAPI and Swagger used interchangeably, you might be wondering what the difference is. 

In short, OpenAPI is a specification used for documenting the capabilities of your API. Swagger is a set of tools from SmartBear (both open-source and commercial) that use the OpenAPI specification (like Swagger UI).

# OpenAPI now shipped with ASP.NET Core 5 templates by default



With Swagger being donated to the OpenAPI Initiative in 2015, 

With ASP.NET TCore, you'll see these things...

In your project file, a `Swashbuckle.AspNetCore` package reference is added:

```xml
<ItemGroup>
    <PackageReference Include="Swashbuckle.AspNetCore" Version="5.6.3" />
</ItemGroup>
```

In `Startup.cs` in the `ConfigureServices` method, you'll see:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddControllers();
    services.AddSwaggerGen(c =>
    {
        c.SwaggerDoc("v1", new OpenApiInfo { Title = "HttpReplApi", Version = "v1" });
    });
}
```

And, in `Configure`:

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
        app.UseSwagger();
        app.UseSwaggerUI(c =>
        {
            c.SwaggerEndpoint("/swagger/v1/swagger.json", "HttpReplApi v1");
            c.RoutePrefix = string.Empty;
        });
    }

    // other services removed for brevity
}
```

In `launchSettings.json`:

```json
{
  "profiles": {
    "HttpReplApi": {
      "commandName": "Project",
      "dotnetRunMessages": "true",
      "launchBrowser": true,
      "launchUrl": "swagger",
      "applicationUrl": "https://localhost:5001;http://localhost:5000",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    }
  }
}
```




# Wrap up

# Resources
