---
date: "2020-10-17"
title: "Increase rendering performance with Blazor component virtualization"
excerpt: "Use Blazor component virtualization to increase perceived performance of components that work with large data sets."
tags: [blazor, aspnet-core]
header:
    overlay_image: /assets/images/virtualization-card.png
    overlay_filter: 0.8
---

This is an introduction.

## Prerequisites

To work with Blazor component virtualization, you'll need to be using .NET 5 RC1 and greater. Head on over to the [.NET 5 SDK downloads](https://dotnet.microsoft.com/download/dotnet/5.0), or have Visual Studio 2019 Preview 3 (v16.8) or greater installed.

## Our application

To illustrate the need for virtualization, let's take it to the extremes. In the Blazor app's `FetchData` component, let's make a bunch of objects in memory when the page loads (in `OnInitializedAsync`). In the `@code` block of the component, we'll populate a list of 10,000 cars to display on the page. (To state the obvious, we should never *actually* do this.)

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

![Elements changing on scroll]({{ site.url }}{{ site.baseurl }}videos/twitter.mp4)

## Wrap up

## References
