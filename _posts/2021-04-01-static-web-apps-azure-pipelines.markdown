---
date: "2021-04-01"
title: "Use Azure Static Web Apps with Azure DevOps pipelines"
description: "In this post, we discuss how to use Azure Static Web Apps from a pipeline in Azure DevOps."
tags: [azure]
image: /assets/img/static-apps-pipelines.png
---

Last year, Microsoft released [Azure Static Web Apps](https://azure.microsoft.com/services/app-service/static/), a great way to bundle your static app with a serverless Azure Functions backend. If you have a GitHub repository, Azure Static Web Apps has you covered. You create an instance in Azure, select a GitHub repository, and Azure creates a GitHub Actions CI/CD pipeline for you that'll automatically trigger when you merge a pull request into your `main` branch. It's still in preview, but a GA release isn't too far off.

To borrow [from a famous Henry Ford quote](https://www.goodreads.com/quotes/23494-any-customer-can-have-a-car-painted-any-colour-that): *you can have it from any repo you want, so long as it's GitHub*.

That has changed. Azure Static Web Apps now provides Azure DevOps support. If you have a repository in Azure DevOps, you can wire up an Azure Pipelines YAML file that builds and deploys your app to Azure Static Web Apps. While it isn't as streamlined and elegant as the GitHub experience—you need to configure your deployment token manually, and you don't get automatic staging environments—it sure beats the alternative for Azure DevOps customers (that is, no Azure Static Web Apps support at all).

Since last fall, I've been experimenting with Azure Static Web Apps for my *Blast Off with Blazor* [blog posts](https://daveabrock.com/tags/#blast-off-blazor) and [accompanying repository](https://github.com/daveabrock/NASAImageOfDay). In this post, I'll deploy my repository's code from Azure DevOps to try out the support.

## Prerequisites

Before we begin, I'll assume you have an active Azure DevOps project. If you don't, [check out the docs](https://docs.microsoft.com/en-us/azure/devops/organizations/projects/create-project?view=azure-devops-2020&tabs=preview-page). 

If you need to import a Git repository from your local machine, as I did, perform the following steps:

  1. Navigate to your Azure DevOps repository.
  2. Select **Import**.
  3. Enter the URL of your Git repository.
  4. Select **Import** again.

## Create an Azure Static Web Apps resource

To get started, create a new Azure Static Web App resource from the Azure Portal.

1. Navigate to the Azure Portal at *[portal.azure.com](https://portal.azure.com)*.
2. From the global search box at the top, search for and select **Azure Static Web Apps (Preview)**.
3. Select **Create**.
4. Enter details for your subscription, resource group, name, and region.  Under *Deployment details*, click **Other**. This option bypasses the automated GitHub flow and tells Azure you don't need it.

 ![]({{ site.url }}{{site.baseurl}}/assets/img/create-static-app-resource.jpg)

Once you're done, click **Review + Create**, then **Create**. When the deployment completes, navigate to your new resource. You'll now need to grab the deployment token to use with Azure DevOps.

## Access deployment token

Since you clicked **Other** when you created your resource, you'll need to do some manual work to get Azure Static Web Apps working with Azure DevOps. To do so, select **Manage deployment token**.

![]({{ site.url }}{{site.baseurl}}/assets/img/manage-deployment-token-option.jpg)

From there, copy your token to a text editor so you can use it later. (Or, if you're fancy, you can use your operating system's [clipboard history features](https://www.onmsft.com/how-to/how-to-turn-on-clipboard-history-on-windows-10-to-save-time).)

## Create Azure Pipelines task

Now, we'll need to create an Azure Pipelines task in Azure DevOps.

From your Azure DevOps repository, select **Set up build**.

 ![]({{ site.url }}{{site.baseurl}}/assets/img/set-build-button.jpg)

From *Configure your pipeline*, select **Starter pipeline**.

 ![]({{ site.url }}{{site.baseurl}}/assets/img/starter-pipeline.jpg)

In your pipeline, replace the existing YAML content with a new `AzureStaticWebApp` task. Pay special attention to the `inputs` section:

- The `app_location` specifies the root of your application code. For me, this sits in a Blazor client directory at `Blazor.Client`.
- The `api_location` is the location of your Azure Functions code. **Pay attention here!** If no API is found in the specified location, the build assumes it doesn't exist and silently fails. (Trust me ... I know this personally.)
- The `output_location` includes the build output directory relative to `app_location`. For me, my static files reside in `wwwroot`.

We'll work on the `azure_static_web_app_api_token` value soon.

Here's my YAML file. (You'll notice it's a bit cleaner than the [GitHub Action](https://github.com/daveabrock/NASAImageOfDay/blob/main/.github/workflows/azure-static-web-apps-jolly-ground-0bdb93d10.yml).)

```yaml
trigger:
  - main

pool:
  vmImage: 'ubuntu-latest'

steps:
  - task: AzureStaticWebApp@0
    inputs:
      app_location: "BlastOff.Client"
      api_location: "BlastOff.Api"
      output_location: "wwwroot"
    env:
      azure_static_web_apps_api_token: $(deployment_token)
```

Before we kick off the pipeline, we need to add our Azure Static Web Apps deployment token as an Azure Pipelines variable. To do so, select **Variables** and create a variable called **deployment_token** (assuming that's what you named the value of `azure_static_web_apps_api_token`). Finally, select **Keep this value secret**, then click **OK**.

![]({{ site.url }}{{site.baseurl}}/assets/img/new-variable.jpg)

Select **Save**, then **Save and run**. With a little luck, it might pass on the first try. Congratulations! You now have an Azure Static Web Apps site built from an Azure DevOps repository.

## Side note: The minimal Azure Pipelines YAML file

I can't help but notice the brevity of the Azure Pipelines YAML file. Here's another look at the file:

```yaml
trigger:
  - main

pool:
  vmImage: 'ubuntu-latest'

steps:
  - task: AzureStaticWebApp@0
    inputs:
      app_location: "BlastOff.Client"
      api_location: "BlastOff.Api"
      output_location: "wwwroot"
    env:
      azure_static_web_apps_api_token: $(deployment_token)
```

Compare that to the look of the GitHub Actions YAML file that Azure Static Web Apps created for me:

```yaml
name: Azure Static Web Apps CI/CD

on:
  push:
    branches:
      - main
  pull_request:
    types: [opened, synchronize, reopened, closed]
    branches:
      - main

jobs:
  build_and_deploy_job:
    if: github.event_name == 'push' || (github.event_name == 'pull_request' && github.event.action != 'closed')
    runs-on: ubuntu-latest
    name: Build and Deploy Job
    steps:
      - uses: actions/checkout@v2
        with:
          submodules: true
      - name: Build And Deploy
        id: builddeploy
        uses: Azure/static-web-apps-deploy@v0.0.1-preview
        with:
          azure_static_web_apps_api_token: ${{ secrets.AZURE_STATIC_WEB_APPS_API_TOKEN_BLACK_DESERT_00CF9D310 }}
          repo_token: ${{ secrets.GITHUB_TOKEN }} 
          action: "upload"
          app_location: "BlastOff.Client"
          api_location: "BlastOff.Api"
          output_location: "wwwroot"

  close_pull_request_job:
    if: github.event_name == 'pull_request' && github.event.action == 'closed'
    runs-on: ubuntu-latest
    name: Close Pull Request Job
    steps:
      - name: Close Pull Request
        id: closepullrequest
        uses: Azure/static-web-apps-deploy@v0.0.1-preview
        with:
          azure_static_web_apps_api_token: ${{ secrets.AZURE_STATIC_WEB_APPS_API_TOKEN_BLACK_DESERT_00CF9D310 }}
          action: "close"
```

While the deploy step is similar between the two, the GitHub Actions file has a lot of noise related to managing pull requests. As such, a common question might be: *Do I need to add PR trigger steps in my Azure Pipelines file?* No! In my testing, I successfully merged a pull request to `main` and kick off a successful build and deploy without updating the YAML file manually.

## Should I use Azure Static Web Apps with Azure DevOps or GitHub?

With two ways to deploy your Azure Static Web Apps (with the possibility of more), you might be wondering which option to choose.

Microsoft will not be supporting competing code management/DevOps solutions long-term. Azure DevOps isn't being phased out anytime soon, but the long-term investment will be in GitHub. If you are building new and don't have a dependency on Azure DevOps, Azure Static Web Apps with GitHub is the way to go. However, if you are using Azure DevOps—for many, the robust enterprise capabilities of Azure DevOps make it hard to leave until GitHub matures in that space—it's a nice solution.

## Wrap up

In this post, we demonstrated how to deploy an Azure Static Web App using an Azure DevOps pipeline. We created an Azure Static Web Apps resource, retrieved a deployment token, created an Azure Pipelines task, and explored why you would want to use GitHub or Azure DevOps for your Azure Static Web Apps.
