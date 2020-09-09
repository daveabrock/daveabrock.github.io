---
date: "2020-09-08"
title: "Use CSS isolation in your Blazor projects"
tags: [aspnet-core]
header:
    overlay_image: /assets/images/css-isolation-card.png
    overlay_filter: 0.8
excerpt: We talk about how to isolate your CSS to your Blazor components—all without a stylesheet reference.
---

weather FORECAST

give table a unique class, go to global site.css

frustrating = combining many different concepts into large file

add new style file to project - FetchData.razor.css
Add same files we had before -> is not manually referenced anywhere

don't need custom class names anymore and you're not interfering with style names

compiler has generated a magic attribute -> components with scoped css (does not set a class but an attribute) - this is what Vue does

Uses in Razor class libraries

other preprocessors
    scoped SASS file (pick SASS file) (Counter.razor.scss)
    OOB -> no SASS support but is extensible, can use mechanisms 
    Delegate.SassBuilder

webpack watch

can set up rules globally and components will override it

use ::deep h1 to cover child elements - travel to child component

Uses attributes so each component and styles only apply to elements with that string - requires an ancestor to inherit





Hey, this is a thing. This is another thing.

Issue: https://github.com/dotnet/aspnetcore/issues/10170

blazorstyled.io, https://github.com/alexandrereyes/BlazorScopedCss

As we wrote in the design summary above, we're keeping this decoupled from any particular CSS preprocessors. You can wire up your own SCSS/SASS compiler if you want. You just need to make sure it transforms MyComponent.razor.sass into MyComponent.razor.css before the Razor SDK goes looking for the .razor.css file. We're not implementing anything for this; it's up to you to plug in your chosen preprocessor.

Is no one else using {pagename}.razor.scss with nested selectors and a unique root to avoid collisions? The .scss file tucks nicely under the .razor file and we get full Intellisense in the .scss file as well as hints in the .razor file itself (it recognizes and suggests css info in class attributes).

The above pattern was based on our usage of Angular css isolation and it works without any issues. You are of course responsible for uniqueifying the root and it doesn't auto-generate some goofy hash so there's that but unless I'm missing a key requirement, you might want to look into that.


Blazor now supports defining CSS styles that are scoped to a given component. Component specific CSS styles make it easier to reason about the styles in your app and to avoid unintentional side effects from global styles. You define component specific styles in a .razor.css file the matches the name of the .razor file for the component.

Since this is a build-time action, each change to an isolated CSS file will require you to recompile your project to see the change.

For example, let’s say you have a component MyComponent.razor file that looks like this:

MyComponent.razor

<h1>My Component</h1>

<ul class="cool-list">
    <li>Item1</li>
    <li>Item2</li>
</ul>
You can then define a MyComponent.razor.css with the styles for MyComponent:

MyComponent.razor.css

h1 {
    font-family: 'Comic Sans MS'
}

.cool-list li {
    color: red;
}
The styles in MyComponent.razor.css will only get applied to the rendered output of MyComponent; the h1 elements rendered by other components, for example, are not affected.

To write a selector in component specific styles that affects child components, use the ::deep combinator.

.parent ::deep .child {
    color: red;
}
By using the ::deep combinator, only the .parent class selector is scoped to the component; the .child class selector is not scoped, and will match content from child components.

Blazor achieves CSS isolation by rewriting the CSS selectors as part of the build so that they only match markup rendered by the component. Blazor then bundles together the rewritten CSS files and makes the bundle available to the app as a static web asset at the path _framework/scoped.styles.css.

While Blazor doesn’t natively support CSS preprocessors like Sass or Less, you can still use CSS preprocessors to generate component specific styles before they are rewritten as part of the building the project.