---
date: "2018-11-12"
title: "Share Blazor Components with Shared Class Libraries"
excerpt: Use shared class libraries to share Blazor components across projects.
---

(Some context: Blazor is an experimental .NET web framework using C# and HTML in the browser. See the [preview documentation](https://blazor.net/) for more details on the project and how to get started.)

As developers, we often take advantage of the benefits of sharing common code in a specific project. That way, it can be shared and maintained in a centralized way - accessed easily whether it is your team's source control repository or, in the .NET world, a NuGet package.

This is no different with Blazor. When we use shared libraries, we can easily share components across projects. When we share components, all our target project needs to do is reference the shared project and add the component - no style imports necesssary!

In this article, we will demonstrate the power of Blazor shared libraries using a simple example with the default scaffolded Blazor application.

## Prerequisites

Before getting started, make sure you have the following minimum prerequisites installed:

* [.NET Core 2.1 SDK](https://go.microsoft.com/fwlink/?linkid=873092), *2.1.402 or later*
* [Visual Studio 2017 15.8 or later](https://go.microsoft.com/fwlink/?linkid=873093), with the *ASP.NET and web development* workload
* The [Blazor Language Services Extension](https://go.microsoft.com/fwlink/?linkid=870389) for Visual Studio
* The Blazor templates from the command line by running `dotnet new -i Microsoft.AspNetCore.Blazor.Templates`

## Create a Blazor application

Let's create a Blazor application.

1. From Visual Studio 2017, click **File** > **New** > **Project** and select the **Visual C#** > **.NET Core** > **ASP.NET Core Web Application**.
2. Give it a name, like **MySharedLibDemo**, and click **OK**.
3. Select the **Blazor** template and click **OK**. This will scaffold a default SPA application that is integrated with WebAssembly.
4. After the scaffolding completes, click **Debug** > **Start Without Debugging** to run the application. Leave this running so we can save the application in Visual Studio and reload it. Next, we'll add a shared Blazor library. At this time, a Blazor shared library is not available in Visual Studio. We'll use the .NET Core command-line interface (CLI) to accomplish this.
5. Right-click the solution and select **Open Command Line**. From your preferred command line utility, enter `dotnet new` to see all the .NET Core templates available to you. We will be adding the **Blazor Library** template to our project.
6. From your prompt, enter `dotnet new blazorlib -o MySharedBlazorLibrary`. This will add the `MySharedBlazorLibrary` project in your directory.
7. Right-click your solution and click **Add** > **Existing Project**. Browse to your library, select the *MySharedBlazorLibrary.csproj* file, and click **Open**. Your project structure will now resemble the following.
8. Finally, reference the shared project. From your main project, right-click **Dependencies** > **Add Reference...** Then, select your newly created project and click **OK**.

## Update the ViewImports.cshtml file

For Blazor to use your shared project, add the following line to the `_ViewImports.cshtml` file that sits at the root of your main project.

`@addTagHelper *, MySharedBlazorLibrary`

This is a temporary solution. The Blazor team hopes to have this resolved. Until then, this is a required step.

## Add shared component to your main project

Now, all you need to do is add the component to your project. If you remember, the shared project includes a `Component1.cshtml` file that includes a styled component. We will now add this to our main project.

From your `Pages/Index.cshtml` file, below the `SurveyPrompt` component, add the `Component1` component. As you begin typing, you can use IntelliSense.

![Autocomplete](/images/Autocomplete.png)

Your `Index.cshtml` component should now look like this:

```html
@page "/"

<h1>Hello, world!</h1>

Welcome to your new app.

<SurveyPrompt Title="How is Blazor working for you?" />

<Component1 />
```

Notice you don't even need a `@using` statement in your view to reference your component!

## View your changes

After you save your changes, reload the page to see your new component in action.

   ![PageWithComponent-1](/images/PageWithComponent-1.png)

You have just referenced a component from a shared library with minimal effort. By merely importing the library, you were able to add a component and its styles quite easily.
