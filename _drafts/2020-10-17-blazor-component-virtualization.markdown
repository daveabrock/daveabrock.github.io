---
date: "2020-10-19"
title: "Increase rendering performance with Blazor component virtualization"
excerpt: "Use Blazor component virtualization to increase perceived performance of components that work with large data sets."
tags: [blazor, aspnet-core]
header:
    overlay_image: /assets/images/virtualization-card.png
    overlay_filter: 0.8
---

This is an introduction.

We'll cover the following content in this post:

- [Prerequisites](#prerequisites)
- [The "up and running in 30 seconds" solution](#the-up-and-running-in-30-seconds-solution)
- [Our application](#our-application)
- [Deep dive: understand the internal code](#deep-dive-understand-the-internal-code)
- [Wrap up](#wrap-up)
- [References](#references)

# Prerequisites

To work with Blazor component virtualization, you'll need to be using .NET 5 RC1 and greater. Head on over to the [.NET 5 SDK downloads](https://dotnet.microsoft.com/download/dotnet/5.0), or have Visual Studio 2019 Preview 3 (v16.8) or greater installed.

# The "up and running in 30 seconds" solution

Assume you have a collection called `people` that's a list of `Person` objects with properties like `FirstName`, `LastName`, and so on. Replace your traditional for-each loop...

```csharp
@foreach (var person in people)
{
    <p>
        @person.FirstName @person.LastName is only fun on Fridays.
    </p>
}
```

... with the `Virtualize` component, and pass the collection into the `Items` parameter and use `context` to access your object's properties:

```html
<Virtualize Items="@people">
    <p>
        @context.FirstName @context.LastName is only fun on Fridays.
    </p>
</Virtualize>
```

Additionally, you could explicitly specify a context in your component:

```html
<Virtualize Context="person" Items="@people">
    <p>
        @person.FirstName @person.LastName is only fun on Fridays.
    </p>
</Virtualize>
```

That's really how easy it is, and will cover a majority of your use cases. Keep reading to understand the component in greater detail with a sample project, and learn how you can customize it even further.

# Our application

To further illustrate the need for virtualization, let's take it to the extremes. In the Blazor app's `FetchData` component, let's make a bunch of objects in memory when the page loads (in `OnInitializedAsync`). In the `@code` block of the component, we'll populate a list of 10,000 cars to display on the page. (To state the obvious, we should never *actually* do this.)

```csharp
private List<Car> cars;

protected override async Task OnInitializedAsync()
{
  cars = await MakeTenThousandCars();
}

private async Task<List<Car>> MakeTenThousandCars()
{
  List<Car> myCarList = new List<Car>();

  for (int i = 0; i < 10000; i++)
  {
    var car = new Car()
    {
      Id = Guid.NewGuid(),
      Name = $"Car {i}",
      Cost = i * 100
    };

    myCarList.Add(car);
  }  
  return await Task.FromResult(myCarList);
}

public class Car
{
  public Guid Id { get; set; }
  public string Name { get; set; }
  public int Cost { get; set; }
}
```

Then, in the markup: when we get all our cars, we'll lay them out in a single table. If you've ever worked with Razor, you should be familiar.

```html
@if (cars == null)
{
  <p><em>Loading...</em></p>
}
else
{
  <table class="table">
    <thead>
      <tr>
        <th>Id</th>
        <th>Name</th>
        <th>Cost</th>
      </tr>
    </thead>
    <tbody>
      @foreach (var car in cars)
      {
        <tr>
            <td>@car.Id</td>
            <td>@car.Name</td>
            <td>@car.Cost</td>
        </tr>
       }
    </tbody>
  </table>
}
```

Launch your app, fire up your favorite dev tools, and head over to the **Fetch data** link at `http://localhost:xxxx/fetchdata`.

According to my dev tools with the cache disabled, load ranges anywhere from 2.5 to 4 seconds (I refreshed ten times) and quite a bit of lag—just imagine if each row had button components, that can make the page lag even more.

What I can do is replace my for-loop with a `<Virtualize>` component. This component takes two parameters:

* Items
* Context

Replace the for-loop with the following, then re-launch your application. 

```html
<Virtualize Items="cars" Context="car">
  <tr>
    <td>@car.Id</td>
    <td>@car.Name</td>
    <td>@car.Cost</td>
  </tr>
</Virtualize>
```

Now, we're looking at 1.2 to 1.9 seconds uncached, about twice the speed.

This completes all you'll need to know to use Blazor component virtualization. If you really want to geek out, though, keep reading.

# Deep dive: understand the internal code

The code for the `<Virtualize>` component [is out on GitHub if you really want to understand *everything*](https://github.com/dotnet/aspnetcore/blob/master/src/Components/Web/src/Virtualization/Virtualize.cs). 

Obviously, you don't need to review and understand this code in order to use the component—the beauty of an out-of-the-box component is to provide a level of abstraction for you—but if you really want to see how everything works, let's dive in.

At the beginning of `Virtualize.cs`, you'll see the component can take several parameters (these are noted, as they are in your own components, with the `[Parameter]` annotation).

* `RenderFragment<TItem>? ChildContent` - gets or sets the item template for the list (as you can see, it is optional and nullable)
* `RenderFragment<TItem>? ItemContent` - gets or sets the item template for the list (as you can see, it is optional and nullable)
* `RenderFragment<PlaceholderContext>? Placeholder` - gets or sets the template for items that have not yet been loaded in memory (optional and nullable)
* `float ItemSize` - gets size of each item in memory (defaults to 50px)
* `ItemsProviderDelegate<TItem>? ItemsProvider` - gets or sets the function providing items to the list
* `ICollection<TItem>? Items` - gets or sets the fixed item source
* `int OverscanCount` - gets or sets a value that determines how many additional items will be rendered before and after the visible region (the default is 3). This helps to reduce the frequency of rendering during scrolling. However, higher values mean that more elements will be present in the page.







# Wrap up

# References
