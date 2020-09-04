---
date: "2020-09-04"
title: "NDepend: Boost Your Team's Code Quality"
tags: [tools]
---
Have you used NDepend before? If you've been working on .NET for some time, you may have either used it or heard of it—I believe it's been around since 2007 or so.

If you aren't familiar, NDepend is a robust static code analysis tool that allows you to gain a concrete understanding of your project's complexity, technical debt, associations, dependencies, and more. You can easily integrate NDepend into your daily workflow with a Visual Studio extension and it also has extensions for your build pipelines as well.

I gave NDepend 2020.1 a test spin and have been using it the last few weeks. I'll spend the rest of the post writing about my experiences.

I couldn't possibly go through **all** the features and functionality that NDepend offers, so this post will show off what caught my eye. You can [take a look at the docs for the full treatment](https://www.ndepend.com/docs/getting-started-with-ndepend).

*Disclaimer*: I am reviewing NDepend with a free review license.
{: .notice--info}

## Attach NDepend to your .NET solution

Once you install NDepend, the easiest way to get started is to attached NDepend to an existing .NET solution in Visual Studio.

In your version of Visual Studio, enter **ndepend** in your global search bar at the top, and click **Analyze a VS solution**. Browse to a solution file you want NDepend to analyze, then click **OK**. At the next screen, select the assemblies you want analyzed, and let NDepend loose.

The first time you analyze, a beginner-friendly screen will ask you how you want to proceed. Click **View NDepend Dashboard**.

![go to NDepend dashboard]({{ site.url }}{{ site.baseurl }}/images/ndepend-what-to-do.png)

## Review the NDepend dashboard

The dashboard gives you a nice view of overall tech debt, quality gates, and issue and rule estimation. My sample app needs some work but could definitely be worse!

![go to NDepend dashboard]({{ site.url }}{{ site.baseurl }}/images/dashboard.png)

You can drill into the dashboard to get details about any violations. Here's a full view from my Visual Studio.

![go to NDepend violations in Visual Studio]({{ site.url }}{{ site.baseurl }}/images/ndepend-violations.png)

### Technical debt severity levels

If you're wondering about the NDepend severity levels, here's some guides:

* **Info** - a small improvement can be made
* **Minor** - a type of warning that won't have a significant impact if not addressed
* **Major** - should be fixed quickly but can wait until the next run
* **Critical** - these issues shouldn't move to production
* **Blocker** - it must be fixed

Once you improve your code, you can rerun the analysis again and NDepend will show you how you improved from the last baseline.

## Visualizing complexity

It had been a few years since I worked with NDepend, and a welcome new addition I noticed is the visualization of cyclomatic complexity. While it doesn't tell the whole story, high complexity numbers are typically bad news for code quality and testability.

Here you'll see a nice visualization view where can hover over "trouble spots" in your application. In the top-left of the image, you'll see details about the offending method (I hovered over the redness).

![visualize code complexity]({{ site.url }}{{ site.baseurl }}/images/visualizer.png)

This capability isn't just for cyclomatic complexity—you can do it for documentation (comments), test coverage, and more.

## Understand dependencies

NDepend ships with a dependencies view that allows you to visualize the relationships between your projects.

![visualize code complexity]({{ site.url }}{{ site.baseurl }}/images/graph.png)

## Query code with the CQLinq syntax

NDepend offers Code Query LINQ (CQLinq) to query .NET code through the use of LINQ queries. If you want greater control of the metrics, or want a one-off result, it's a great capability.

Let's say I want to find any methods where the cyclomatic complexity is greater than 10.

The first thing I notice is the Intellisense-like previews as I type:

![cqlinq]({{ site.url }}{{ site.baseurl }}/images/intellisense.png)

Now, I can execute the query like this:

```csharp
from m in Methods where m.CyclomaticComplexity > 10
select m
```

I don't need to click a Run button or anything—the results will populate automatically.

![cqlinq]({{ site.url }}{{ site.baseurl }}/images/cqlinq-results.png)

I can save my queries for later use.

## CI integration

To make the greatest impact to your team, you can use NDepend with your CI tools—it supports any major CI tool like Azure DevOps/TFS, TeamCity, Jenkins, SonarQube, and more. You will need to purchase a [Build Machine Edition *or* Azure DevOps/TFS Edition license for this capability](https://www.ndepend.com/editions).

If you are using something like Azure DevOps, you can use an extension that uses a build task. This will produce the dashboard at a moment's notice, just like we saw in our local Visual Studio.

Much like you can enforce quality gates in your CI tooling for code coverage and coding conventions, you can also do for NDepend. The extension ships with a dozen quality gates for things such as technical debt amount or issues with a particular severity—for example, you could enforce a gate where developers don't push any code with Major or Critical issues.

On top of enforcing code quality across the team, you have the advantage of the metrics in one place (and not scattered across dev machines) and also can report progress up the chain, if needed.

This is a powerful capability and if you're paying for NDepend, you should give this serious consideration. This will morph your use of NDepend from "we should really fix those issues" to "we've set a standard to not introduce anything to our codebase with these issues"—a huge difference.

Take a look at the [NDepend CI documentation](https://www.ndepend.com/docs/azure-devops-tfs-vsts-integration-ndepend) for more details.

## Wrap up

I was happy to get acquainted with NDepend once again and take a look at the improvements it has made over the years, and see that it's still the best-in-class tool for .NET static analysis.

If you have the budget for it, 