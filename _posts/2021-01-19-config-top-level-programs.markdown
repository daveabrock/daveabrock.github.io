---
date: "2021-01-19"
title: "How to use configuration with C# 9 top-level programs"
tags: [csharp,csharp-9]
image: /assets/img/top-level-config.png 
description: In this post, let's use C# 9 top-level programs to read from configuration.
---

I've been working with [top-level programs in C# 9](https://daveabrock.com/2020/07/09/c-sharp-9-top-level-programs) quite a bit lately. When writing simple console apps in .NET 5, it allows you to remove the ceremony of a namespace and a `Main(string[] args)` method. It's very beginner-friendly and allows developers to get going without worrying about learning about namespaces, arrays, arguments, and so on. While I'm not a beginner—although I feel like it some days—I enjoy using top-level programs to prototype things quickly.

With top-level programs, you can work with normal functions, use `async` and `await`, access command-line arguments, use local functions, and more. For example, here's me working with some arbitrary strings and getting a random quote from the [Ron Swanson Quotes API](https://github.com/jamesseanwright/ron-swanson-quotes#ron-swanson-quotes-api):

```csharp
using System;
using System.Net.Http;

var name = "Dave Brock";
var weekdayHobby = "code";
var weekendHobby = "play guitar";
var quote = await new HttpClient().GetStringAsync("https://ron-swanson-quotes.herokuapp.com/v2/quotes");

Console.WriteLine($"Hey, I'm {name}!");
Console.WriteLine($"During the week, I like to {weekdayHobby} and on the weekends I like to {weekendHobby}.");
Console.WriteLine($"A quote to live by: {quote}");
```

# Add configuration to a top-level program

Can we work with configuration with top-level programs? (Yes, *should we* is a different conversation, of course.)

To be clear, there are [many, many ways to work with configuration](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/?view=aspnetcore-5.0) in .NET. If you're used to it in ASP.NET Core, for example, you've most likely done it from constructor dependency injection, wiring up a `ServiceCollection` in your middleware, or using the `Options` pattern—so you may think you won't be able to do it with top-level programs.

Don't overthink it. Using the `ConfigurationBuilder`, you can easily use configuration with top-level programs.

Let's create an `appsettings.json` file to replace our hard-coded values with configuration values.

```json
{
    "Name": "Dave Brock",
    "Hobbies": {
        "Weekday": "code",
        "Weekend": "play guitar"
    },
    "SwansonApiUri": "https://ron-swanson-quotes.herokuapp.com/v2/quotes"
}
```

Then, make sure your project file has the following packages installed, and that the `appSettings.json` file is being copied to the output directory:

```xml
  <ItemGroup>
    <PackageReference Include="Microsoft.Extensions.Configuration" Version="5.0.0" />
    <PackageReference Include="Microsoft.Extensions.Configuration.Json" Version="5.0.0" />
    <PackageReference Include="Microsoft.Extensions.Configuration.EnvironmentVariables" Version="5.0.0" />
  </ItemGroup>

  <ItemGroup>
    <None Update="appsettings.json">
      <CopyToOutputDirectory>Always</CopyToOutputDirectory>
    </None>
  </ItemGroup>
```

In your top-level program, create a `ConfigurationBuilder` with the appropriate values:

```csharp
var config = new ConfigurationBuilder()
                 .SetBasePath(Directory.GetCurrentDirectory())
                 .AddJsonFile("appsettings.json")
                 .Build();
```

With a `config` instance, you're ready to simply read in your values:

```csharp
var name = config["Name"];
var weekdayHobby = config.GetSection("Hobbies:Weekday");
var weekendHobby = config.GetSection("Hobbies:Weekend");
var quote = await new HttpClient().GetStringAsync(config["SwansonApiUri"]);
```

And here's the entire top-level program in action:

```csharp
using Microsoft.Extensions.Configuration;
using System;
using System.IO;
using System.Net.Http;

var config = new ConfigurationBuilder()
                 .SetBasePath(Directory.GetCurrentDirectory())
                 .AddJsonFile("appsettings.json")
                 .Build();

var name = config["Name"];
var weekdayHobby = config.GetSection("Hobbies:Weekdays");
var weekendHobby = config.GetSection("Hobbies:Weekends");
var quote = await new HttpClient().GetStringAsync(config["SwansonApiUri"]);

Console.WriteLine($"Hey, I'm {name}!");
Console.WriteLine($"During the week, I like to {weekdayHobby.Value}" +
        $" and on the weekends I like to {weekendHobby.Value}.");
Console.WriteLine($"A quote to live by: {quote}");
```

# Review the generated code

When throwing this in the [ILSpy decompilation tool](https://github.com/icsharpcode/ILSpy), you can see there's not a lot of magic here. The top-level program is merely wrapping the code in a `Main(string[] args)` method and replacing our implicit typing:

```csharp
using System;
using System.IO;
using System.Net.Http;
using System.Runtime.CompilerServices;
using System.Threading.Tasks;
using Microsoft.Extensions.Configuration;

[CompilerGenerated]
internal static class <Program>$
{
  private static async Task <Main>$(string[] args)
  {
    IConfigurationRoot config = new ConfigurationBuilder().SetBasePath(Directory.GetCurrentDirectory()).AddJsonFile("appsettings.json").Build();
    string name = config["Name"];
    IConfigurationSection weekdayHobby = config.GetSection("Hobbies:Weekday");
    IConfigurationSection weekendHobby = config.GetSection("Hobbies:Weekend");
    string quote = await new HttpClient().GetStringAsync(config["SwansonApiUri"]);
    Console.WriteLine("Hey, I'm " + name + "!");
    Console.WriteLine("During the week, I like to " + weekdayHobby.Value + " and on the weekends I like to " + weekendHobby.Value + ".");
    Console.WriteLine("A quote to live by: " + quote);
  }
}
```

# Wrap up

In this quick post, I showed you how to work with configuration in C# 9 top-level programs. We showed how to use a `ConfigurationBuilder` to read from an `appsettings.json` file, and we also reviewed the generated code.
