---
date: "2021-04-08"
title: "Working with the Blazor DynamicComponent"
description: "In this post, we discuss how to dynamically render components in Blazor using the new DynamicComponent."
tags: [blazor,csharp]
image: /assets/img/dynamic-card.png
---

Blazor is a joy when you know what you're rendering—this typically involves knowing your types at compile time. But what happens when you want to render your components dynamically, when you don't know your types ahead of time?

You can do it in a variety of ways: you can iterate through components and use complex conditionals, use reflection, [declare a bunch of `RenderFragment`s](https://www.davidguida.net/how-to-render-a-dynamic-component-with-blazor/), or even [build your own render tree](https://jonhilton.net/blazor-dynamic-components/). It can get complicated when dealing with parameters and complex data graphs, and none of these solutions are any good, really.

With .NET 6 Preview 1, the ASP.NET Core team introduced a built-in Blazor component, DynamicComponent, that [allows you to render a component specified by type](https://devblogs.microsoft.com/aspnet/asp-net-core-updates-in-net-6-preview-1/#dynamiccomponent). When you bring in the component, you specify the `Type` and optionally a dictionary of `Parameters`.

You would declare the component like this:

```html
<DynamicComponent Type="@myType" Parameters="@myParameterDictionary" />
```

The `DynamicComponent` has a variety of applications. I find it to be valuable when working with form data. For example, you can render data based on a selected value without having to iterate through possible types.

In this post, I'll walk through how to use the `DynamicComponent` when a user selects a list from a drop-down list. I've got the repo [out on GitHub](https://github.com/daveabrock/DynamicComponentDemo) for reference.

(Also, a shout-out to Hasan Habib's [nice video on the topic](https://www.youtube.com/watch?v=Wcc14aoylME), which helped me think through some scenarios.)

## Our use case

For a quick use case, I'm using a silly loan application. I want to gather details about where someone lives now (to determine how much they pay and whatnot). The drop-down has five possible values, and I've got some custom components in my `Shared` directory that render based on what a user selects.

- Default state: [DefaultDropdownComponent.razor](https://github.com/daveabrock/DynamicComponentDemo/blob/master/Shared/DefaultDropdownComponent.razor)
- Rent: [RentComponent.razor](https://github.com/daveabrock/DynamicComponentDemo/blob/master/Shared/RentComponent.razor)
- Own house: [OwnHouseComponent.razor](https://github.com/daveabrock/DynamicComponentDemo/blob/master/Shared/OwnHouseComponent.razor)
- Own condo or townhouse: [OwnCondoComponent.razor](https://github.com/daveabrock/DynamicComponentDemo/blob/master/Shared/OwnCondoComponent.razor)
- I'm Dave's roommate (fall-through condition): [DaveRoommate.razor](https://github.com/daveabrock/DynamicComponentDemo/blob/master/Shared/DaveRoommate.razor)

![]({{ site.url }}{{site.baseurl}}/assets/img/loanapp.jpg)

## Generate drop-down

To generate my drop-down list, I'll use my component name as the `option value`. Here, I can use [the `nameof` keyword](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/nameof), which returns component names as constant strings.

Here's the markup of `Index.cshtml` so far:

```html
@page "/"

<h1>Loan application</h1>

<div class="col-4">
    What is your current living situation?
    <select class="form-control">
        <option value="@nameof(DefaultDropdownComponent)">Select a value</option>
        <option value="@nameof(RentComponent)">Rent</option>
        <option value="@nameof(OwnHouseComponent)">Own house</option>
        <option value="@nameof(OwnCondoComponent)">Own condo or townhouse</option>
        <option value="@nameof(DaveRoommate)">I'm Dave's roommate</option>
    </select>
</div>
```

## Write change event

We now need to decide what to do when a drop-down value is selected. To get the drop-down value, we work with [the `ChangeEventArgs` type](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.components.changeeventargs?view=aspnetcore-5.0) to get the value of the raised event—in this case, it's the changed drop-down selection. 

Before `DynamicComponent` came along, this is where you'd have logic to determine which component to render. Maybe you'd have a boolean flag, then use that in Razor markup. Instead, use `Type.GetType` to get the specific component to render.

```csharp
Type selectedType = typeof(DefaultDropdownComponent);

public void OnDropdownChange(ChangeEventArgs myArgs)
{
  selectedType = Type.GetType($"DynamicComponentDemo.Shared.{myArgs.Value}");
}
```

Once I'm done with that, I can bind the drop-down's `onchange` event to my `OnDropdownChange` method. Whenever the drop-down value changes, the method will trigger and determine which type it needs.

```html
<select @onchange="OnDropdownChange" class="form-control">
  <option value="@nameof(DefaultDropdownComponent)">Select a value</option>
  <option value="@nameof(RentComponent)">Rent</option>
  <option value="@nameof(OwnHouseComponent)">Own house</option>
  <option value="@nameof(OwnCondoComponent)">Own condo or townhouse</option>
  <option value="@nameof(DaveRoommate)">I'm Dave's roommate</option>
</select>
```

Finally, I can render my `DynamicComponent`. I'll place it right under my drop-down list.

```html
<DynamicComponent Type="selectedType" />
```

Now we can see the page change using my `DynamicComponent`.

![]({{ site.url }}{{site.baseurl}}/assets/img/dynamic-component.gif)


## Optional: Pass in parameters

If your components have parameters, you can optionally pass them into your `DynamicComponent`. It takes a `Dictionary<string, object>`. The `string` is the name of your parameter, and the `object` is its value.

As a quick example, I can define `ComponentMetadata` through a quick class:

```csharp
class ComponentMetadata
{
  public Type ComponentType { get; set; }
  public Dictionary<string, object> ComponentParameters { get; set; }
}
```

Then, I can create a dictionary for my components like this (only one component has a parameter):

```csharp
private Dictionary<string, ComponentMetadata> paramsDictionaries = new()
{
  {
    "DefaultDropdownComponent",
    new ComponentMetadata { ComponentType = typeof(DefaultDropdownComponent)}
  },
  {
    "RentComponent",
    new ComponentMetadata { ComponentType = typeof(RentComponent)}
  },
  {
    "OwnCondoComponent",
     new ComponentMetadata { ComponentType = typeof(OwnCondoComponent)}
  },
  {
    "OwnHouseComponent",
    new ComponentMetadata { ComponentType = typeof(OwnCondoComponent)}
  },
  {
    "DaveRoommate",
    new ComponentMetadata
    {
      ComponentType = typeof(OwnCondoComponent),
      ComponentParameters = new Dictionary<string, object>()
      {
        { "CustomText", "Ooh, no." }
      }
    }
  }
};
```

Then, I could have logic that filters and passes in a `ComponentParameters` instance to the `DynamicComponent`, depending on what type I'm passing in. There's a lot of power here—you could pass in data from an API or a database as well or even a function, as long as it returns a `Dictionary<string, object>`. 

You might be asking: Why not use the catch-all approach?

```html
<DynamicComponent Type="@myType" MyParameter="Hello" MySecondParameter="Hello again" />.
```

According to Blazor architect Steve Sanderson, [he says](https://github.com/dotnet/aspnetcore/pull/28082#issue-525829150):

>If we do catch-all parameters, then every explicit parameter on DynamicComponent itself - now and in the future - effectively becomes a reserved word that you can't pass to a dynamic child. It would become a breaking change to add any new parameters to DynamicComponent as they would start shadowing child component parameters that happen to have the same name ... It's unlikely that the call site knows of some fixed set of parameter names to pass to all possible dynamic children. So it's going to be far more common to want to pass a dictionary.

## Wrap up

In this post, we walked through the new `DynamicComponent`, which allows you to render components when you don't know your types at runtime. We were able to render a component based on what a user selects from a drop-down list. We also explored how to pass in parameters to the `DynamicComponent` as well.

## References

- [Add DynamicComponent component](https://github.com/dotnet/aspnetcore/issues/26781) (GitHub issue)
- [DynamicComponent source code](https://github.com/dotnet/aspnetcore/blob/c925f99cddac0df90ed0bc4a07ecda6b054a0b02/src/Components/Components/src/DynamicComponent.cs) (GitHub)
- [ASP.NET Core updates in .NET 6 Preview 1](https://devblogs.microsoft.com/aspnet/asp-net-core-updates-in-net-6-preview-1/#dynamiccomponent) (Sourabh Shirhatti)
- [Introduction to Dynamic Components in Blazor](https://www.youtube.com/watch?v=Wcc14aoylME) (Hassan Habib)