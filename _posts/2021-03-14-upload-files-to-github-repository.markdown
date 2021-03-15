---
date: "2021-03-14"
title: "Use C# to upload files to a GitHub repository"
description: "In this post, we use C# and Octokit to upload files to GitHub."
tags: [csharp, github]
image: /assets/img/upload-file-github.png
---

If you need to upload a file to GitHub, [you can do it easily](https://docs.github.com/en/github/managing-files-in-a-repository/adding-a-file-to-a-repository) from *github.com*. However, that doesn't scale. When it comes to doing it on a repeated basis or facilitating automation, you'll want to take a programmatic approach. While GitHub [does have a REST API](https://docs.github.com/en/rest), C# developers can take advantage of [the Octokit library](https://github.com/octokit/octokit.net). This library saves you a lot of time—you can take advantage of dynamic typing, get data models automatically, and not have to construct `HttpClient` calls yourself.

This post will show you how to use the Octokit library to upload a Markdown file to a GitHub repository.

Before we get started, download the Octokit library from NuGet. You can do this in several different ways, but the simplest is using the `dotnet` CLI. From the path of your project, execute the following from your favorite terminal:

```bash
dotnet add package Octokit
```

## Create the client

To get started, you'll need to create a client so that Octokit can connect to GitHub. To do that, you can instantiate a new `GitHubClient`. The `GitHubClient` takes a `ProductHeaderValue`, which can be any string—it's so GitHub can identify your application. GitHub doesn't allow API requests without a `User-Agent` header, and Octokit uses this value to populate one for you.

```csharp
//using Octokit;

var gitHubClient = new GitHubClient(new ProductHeaderValue("MyCoolApp"));
```

With this single line of code, you can now access public GitHub information. For example, you can use a web browser to get a user's details. Here's what you get for `api.github.com/users/daveabrock`:

```json
{
    "login": "daveabrock",
    "id": 275862,
    "node_id": "MDQ6VXNlcjI3NTg2Mg==",
    "avatar_url": "https://avatars.githubusercontent.com/u/275862?v=4",
    "gravatar_id": "",
    "url": "https://api.github.com/users/daveabrock",
    "html_url": "https://github.com/daveabrock",
    "followers_url": "https://api.github.com/users/daveabrock/followers",
    "following_url": "https://api.github.com/users/daveabrock/following{/other_user}",
    "gists_url": "https://api.github.com/users/daveabrock/gists{/gist_id}",
    "starred_url": "https://api.github.com/users/daveabrock/starred{/owner}{/repo}",
    "subscriptions_url": "https://api.github.com/users/daveabrock/subscriptions",
    "organizations_url": "https://api.github.com/users/daveabrock/orgs",
    "repos_url": "https://api.github.com/users/daveabrock/repos",
    "events_url": "https://api.github.com/users/daveabrock/events{/privacy}",
    "received_events_url": "https://api.github.com/users/daveabrock/received_events",
    "type": "User",
    "site_admin": false,
    "name": "Dave Brock",
    "company": null,
    "blog": "daveabrock.com",
    "location": "Madison, WI",
    "email": null,
    "hireable": null,
    "bio": "Software engineer, Microsoft MVP, speaker, blogger",
    "twitter_username": "daveabrock",
    "public_repos": 55,
    "public_gists": 2,
    "followers": 63,
    "following": 12,
    "created_at": "2010-05-13T20:05:05Z",
    "updated_at": "2021-03-13T19:05:32Z"
}
```

(Fun fact: it's fun to look at the `id` values to see how long a user has been with GitHub—I was the 275,862nd registered user, a few years after [GitHub co-founder Tom Preston-Warner](https://api.github.com/users/mojombo), who has an `id` of `1`.)

To get this information programmatically, you can use the `User` object:

```csharp
//using Octokit;

var gitHubClient = new GitHubClient(new ProductHeaderValue("MyCoolApp"));
var user = await gitHubClient.User.Get("daveabrock");
Console.WriteLine($"Woah! Dave has {user.PublicRepos} public repositories.");
```

That was quick and easy, but not very much fun. To modify anything in a repository you'll need to authenticate.

## Authenticate to the API

You can authenticate to the API using one of two ways: a basic username/password pair or an OAuth flow. Using OAuth (from a generated personal access token) is almost always a better approach, as you won't have to store a password in the code, and you can revoke it as needed. What happens when a password changes? Bad, bad, bad.

Instead, connect by [passing in a personal access token (PAT)](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token). From your profile details, navigate to **Developer settings** > **Personal access tokens**, and create one. You'll want to include the `repo` permissions. After you create it, copy and paste the token somewhere. We'll need it shortly.

![Generate GitHub personal access token]({{ site.url }}{{ site.baseurl }}/assets/img/github-pat.jpg)

With the generated PAT, here's how you authenticate to the API. (**Note**: Be careful with your token. In real-world scenarios, make sure to store it somewhere safe and access it from your configuration.)

```csharp
//using Octokit;

var gitHubClient = new GitHubClient(new ProductHeaderValue("MyCoolApp"));
gitHubClient.Credentials = new Credentials("my-new-personal-access-token");
```

## Add a new file to the repository

With that in place, I'm using a `StringBuilder` to create a simple Markdown file. Here's what I have so far:

```csharp
//using Octokit;

var gitHubClient = new GitHubClient(new ProductHeaderValue("MyCoolApp"));
gitHubClient.Credentials = new Credentials("my-new-personal-access-token");

var sb = new StringBuilder("---");
sb.AppendLine();
sb.AppendLine($"date: \"2021-05-01\"");
sb.AppendLine($"title: \"My new fancy post\"");
sb.AppendLine("tags: [csharp, azure, dotnet]");
sb.AppendLine("---");
sb.AppendLine();

sb.AppendLine("# The heading for my first post");
sb.AppendLine();
```

Because I need to pass in a string to create my file, I'll use `sb.ToString()`, to pass it in. I can call the `CreateFile` method to upload a file now.

```csharp
//using Octokit;

var gitHubClient = new GitHubClient(new ProductHeaderValue("MyCoolApp"));
gitHubClient.Credentials = new Credentials("my-new-personal-access-token");

var sb = new StringBuilder("---");
sb.AppendLine();
sb.AppendLine($"date: \"2021-05-01\"");
sb.AppendLine($"title: \"My new fancy updated post\"");
sb.AppendLine("tags: [csharp, azure, dotnet]");
sb.AppendLine("---");
sb.AppendLine();

sb.AppendLine("The heading for my first post");
sb.AppendLine();

var (owner, repoName, filePath, branch) = ("daveabrock", "daveabrock.github.io", 
        "_posts/2021-05-02-my-new-post.markdown", "main");

await gitHubClient.Repository.Content.CreateFile(
     owner, repoName, filePath,
     new CreateFileRequest($"First commit for {filePath}", sb.ToString(), branch));    
```

You can fire and forget in many cases, but it’s good to note this method returns a `Task<RepositoryChangeSet>` which gives you back commit and content details.

## Update an existing file

If you try to execute `CreateFile` on an existing file, you'll get an error that the SHA for the commit doesn't exist. You'll need to fetch the SHA from the result first.

```csharp
var sha = result.Commit.Sha;

await gitHubClient.Repository.Content.UpdateFile(owner, repoName, filePath,
    new UpdateFileRequest("My updated file", sb.ToString(), sha));
```

In many scenarios, you won't be editing the file right after you created it. In these cases, get the file details first:

```csharp
var fileDetails = await gitHubClient.Repository.Content.GetAllContentsByRef(owner, repoName,
    filePath, branch);

var updateResult = await gitHubClient.Repository.Content.UpdateFile(owner, repoName, filePath,
    new UpdateFileRequest("My updated file", sb.ToString(), fileDetails.First().Sha));
```

## What about base64?

The `CreateFile` call also has a `convertContentToBase64` boolean flag, if you'd prefer. For example, I can pass in an image's base64 string and set `convertContentToBase64` to `true`.

```csharp
string imagePath = @"C:\pics\headshot.jpg";
string base64String = GetImageBase64String(imagePath);

var result = await gitHubClient.Repository.Content.CreateFile(
     owner, repoName, filePath,
     new CreateFileRequest($"First commit for {filePath}", base64String, branch, true));

static string GetImageBase64String(string imgPath)
{
    byte[] imageBytes = System.IO.File.ReadAllBytes(imgPath);
    return Convert.ToBase64String(imageBytes);
}
```

## Wrap up

I showed you how to use the Octokit C# library to upload a new file to GitHub in this post. We also discussed how to update existing files and pass base64 strings to GitHub as well. Thanks for reading, and have fun!
