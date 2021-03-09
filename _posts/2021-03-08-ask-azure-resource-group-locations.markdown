---
date: "2021-03-08"
title: "Ask About Azure: Why do resource groups need a location?"
description: "In this post, we discuss why resource groups need to be bound to a location."
tags: [azure, ask-azure]
image: /assets/img/rg-loc-card.png
readtime: true
---

*Ask About Azure is a weekly-ish beginner-friendly series that answers common questions on Azure development and infrastructure topics.*

In Azure, a [resource group](https://docs.microsoft.com/azure/azure-resource-manager/management/manage-resource-groups-portal#what-is-a-resource-group){:target="_blank"} is a logical container that you use to hold related resources. When you organize related resources together, it makes it easier to perform common operations on them. For example, you might have a resource group to host resources for a singular app like the web front end, APIs, and so on. You should deploy, monitor, update, and delete resources in a resource group together. Think of it as a family: you'll have your ups and downs, but life is a lot easier when you do it together. (Mostly.)

A resource group is just a *logical container*, and the resources in your resource group can belong to various locations. The answer is pretty straightforward: you may want to have specific locations because of user requirements or maybe that certain Azure resources are only available in certain regions. The more interesting question is: if resource groups are logical and you can specify various locations for their resources, why do you need a specific location for your resource group?

If you try to create a resource group and leave out the location, you can't create it.

If you create a resource group in the Azure Portal, you'll see a required `Region` field:

![Create resource group in the portal]({{ site.url }}{{ site.baseurl }}/assets/img/create-rg-portal.jpg)

If you go the scripting route, you'll see it's required in the [Azure CLI](https://docs.microsoft.com/azure/azure-resource-manager/management/manage-resource-groups-cli#create-resource-groups){:target="_blank"}:

```bash
az group create --name MyResourceGroup --location centralus
```

In [PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/management/manage-resource-groups-powershell#create-resource-groups){:target="_blank"}, it's required as well:

```bash
New-AzResourceGroup -Name MyResourceGroup -Location centralus
```

So why?

The resource group itself stores *metadata* about the resources inside it. If you're a .NET developer, think of something like a Visual Studio solution (*.sln*) file: it has information about the project in the solution, the Visual Studio version you're using, and so on. Instead of projects in a solution file, think of resources in a resource group.

Anyway, this metadata resides in the location you specified when you created the resource group. Your location is important when it comes to compliance. Your company or government may have specific rules, regulations, or laws about where you must store certain data. This metadata benefits *you*, too: it allows you to better manage your resources for cost management and security and access controls. For example, you can assign allocation tags to your resource group or orchestrate deployments for ARM templates. In terms of security, it's common for teams to have their access scoped to a specific resource group to prevent impacting other resource groups with their changes. Information like this can reside in your resource group's metadata.

How does availability play into this? If my resource group is in `Central US`, but my Azure Function app is in `East US 2`, an `East US 2` failure means the application won't be available (unless you've made failure considerations, which you should). But what if `Central US` has an outage?

When this happens, your Function app in `East US 2` will still be online, but you won't be able to deploy new resources until `Central US` goes online. As adding resources (or a "project") definitely would force a change to the metadata, you won't be able to do it when there's an outage. Typically, updates to existing resources shouldn't impact you. In these situations, you could wait until the outage clears. Alternately, create your resource in a new resource group, then move it to the existing resource group when the outage clears.

# References

- [Azure Resource Manager overview](https://docs.microsoft.com/azure/azure-resource-manager/management/overview){:target="_blank"} (Microsoft Docs)
- [Manage resource groups](https://docs.microsoft.com/azure/azure-resource-manager/management/manage-resource-groups-portal#what-is-a-resource-group){:target="_blank"} (Microsoft Docs)