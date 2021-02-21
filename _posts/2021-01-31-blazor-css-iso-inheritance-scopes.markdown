---
date: "2021-01-31"
title: "How to achieve style inheritance with Blazor CSS isolation"
tags: [csharp,blazor]
image: /assets/img/iso-inherit.png 
description: We explore how to achieve traditional style inheritance with Blazor CSS isolation.
---

Are you familiar with Blazor CSS isolation? Last year, when it was released, I [wrote an introductory post](https://daveabrock.com/2020/09/10/blazor-css-isolation) and the [official ASP.NET Core doc](https://docs.microsoft.com/aspnet/core/blazor/components/css-isolation?view=aspnetcore-5.0). If you want a quick primer, here goes: Blazor CSS isolation allows you to scope styles to a specific component. With CSS isolation, you can package your styles to your components, and you also don't need to worry about the inevitable headaches when working with vendor styles and global CSS.

As I've spoken to a few user groups about CSS isolation, I always get the same question: *how can I use this to inherit styles between components*? Good question. This post will show you how.

When I say *inherit styles*, I'm *not* referring to how you can [pass the same styles down to child components](https://daveabrock.com/2020/09/10/blazor-css-isolation#how-to-work-with-child-components), such as when you want all of a component's children to have the same `h1` style declaration. I'm referring to when you want a base set of styles shared across a set of components. You might be doing this without CSS isolation. For example, if you define most of your styles using a library like Bootstrap, you lean on the library for most of your styles—then you have a custom stylesheet (like `custom.css`) that overrides those when appropriate.

If we're honest, with Blazor CSS isolation, the "C" (cascading) is non-existent. There's no cascading going on here—isolating your styles is the opposite of cascading. With Blazor CSS isolation, you scope your CSS component styles. Then, at build time, Blazor takes care of the cascading for you. When you wish to inherit styles and share them between components, you're losing many of the advantages of using scoped CSS.

In my opinion, using inheritance with CSS isolation **works best when you want to share styles across a small group of similar components**. If you have 500 components in your solution and you want to have base styles across them all, you may be creating more problems for yourself. In those cases, you should stick with your existing tools.

With that knowledge in place, let's understand how to use inheritance with Blazor CSS isolation.

## Inherit styles using CSS isolation

I'm using a newly created Blazor WebAssembly app (it'll work for Blazor Server, too). If you've worked with Blazor, it's from the template with `Index`, `Counter`, and `FetchData` components. To create an app, run this from the `dotnet` CLI and run it to confirm it works:

```bash
dotnet new blazorwasm -o "CSSIsoInheritance"
cd blazorwasm
dotnet run
```

In your `Pages` folder, create the following files:

- `BaseComponent.razor` 
- `BaseComponent.razor.css`
- `FetchData.razor.css`
- `Counter.razor.css`

In `BaseComponent.razor.css` we'll share an `h1` and `p` style across your components.

```css
h1 {
    color: red;
}

p {
    text-decoration-line: underline;
}
```

## Your key to inheritance: scope identifiers

As mentioned previously, Blazor CSS isolation is a compile-time, file-based solution. Out of the box, it'll look for a `MyComponent.razor.css` file that matches a `MyComponent.razor` file. At compile time, it'll assign a scope identifier to your elements. It's assigned for you and looks like this (your identifier will differ):

![A lot of unused styles]({{ site.url }}{{ site.baseurl }}/assets/img/scoped-css.png)

You can't expect the solution to know about inherited classes. I say this repeatedly because it's important: *Blazor CSS isolation is a file-based solution*. To accomplish inheritance, you apply a custom scope identifier across your impacted components. From your project file, you can add the following `<ItemGroup>`:

```xml
<ItemGroup>
    <None Update="Pages/BaseComponent.razor.css" CssScope="inherit-scope" />
    <None Update="Pages/Counter.razor.css" CssScope="inherit-scope" />
    <None Update="Pages/FetchData.razor.css" CssScope="inherit-scope" />
</ItemGroup>
```

To save some lines, you can also use the wildcard `*` operator to include any files that end with `razor.css`:

```xml
<ItemGroup>
    <None Update="Pages/*.razor.css" CssScope="inherit-scope" />
</ItemGroup>
```

If you browse to the `Counter` and `FetchData` components, you'll see the styles are applied. Here's how the `Counter` component looks.

![The inheritance works]({{ site.url }}{{ site.baseurl }}/assets/img/counter-inherit.png)

If you open your Developer Tools, you'll see the scope identifier is applied correctly. If you're wondering, the `b-lib7l0qa43` identifier belongs to the `MainLayout.razor` file.

![The inheritance works]({{ site.url }}{{ site.baseurl }}/assets/img/inherit-scope.jpg)

Since we didn't apply a custom scope identifier to our `Index` component, it's left alone.

![The inheritance works]({{ site.url }}{{ site.baseurl }}/assets/img/hey-world.jpg)

## Solution drawbacks

As I mentioned before, you should do this carefully and cautiously, so you don't introduce more headaches. Aside from that, the solution isn't elegant, because:

- Every derived component **must** have a `razor.css` file, even if it's empty
- In our case (and in many cases), the base component markup is empty
- You need to explicitly assign an identifier in your project file (or name impacted components similarly)

Out of the box, the solution is meant for you to isolate and not share styles. However, if this fits your use case and makes sense to you, you can use this to lower your maintenance costs.

## Wrap up

In this post, we talked about how to use inheritance with CSS isolation. We talked about how to implement inheritance as well as the benefits and drawbacks. As always, feel free to share your experiences in the comments, and check out the references below for more details on Blazor CSS isolation.

- [Use CSS isolation in your Blazor projects](https://daveabrock.com/2020/09/10/blazor-css-isolation) (Me)
- [ASP.NET Core Blazor CSS isolation](https://docs.microsoft.com/aspnet/core/blazor/components/css-isolation?view=aspnetcore-5.0) (ASP.NET Core Docs)