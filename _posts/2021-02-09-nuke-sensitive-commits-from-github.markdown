---
date: "2021-02-09"
title: "How to nuke sensitive commits from your GitHub repository"
tags: [github]
image: /assets/img/sensitive-commits.png 
description: Let's learn how to hit Ctrl+Z on a regretful commit.
---

We all make mistakes. Let's talk about one of my recent ones.

I created a GitHub repository that tweets old posts of mine, which was heavily ~~stolen~~ inspired by [Khalid Abuhakmeh's solution](https://khalidabuhakmeh.com/automate-your-blog-with-github-actions). (Yes, the repo [is called TweetOldPosts](https://github.com/daveabrock/TweetOldPosts).) It is scheduled through [a GitHub Action](https://github.com/daveabrock/TweetOldPosts/blob/master/.github/workflows/main.yml).

To connect to the [Tweetinvi API](https://github.com/linvi/tweetinvi), I had to pass along a consumer key, consumer secret, access token, and access token secret. Like a responsible developer, I stored these as [encrypted secrets in GitHub](https://docs.github.com/en/actions/reference/encrypted-secrets). Before I did that, though, I hardcoded these values to *get it working*. I pushed these changes to GitHub. Oops.

![My bad commit]({{ site.url }}{{ site.baseurl }}/assets/img/github-secrets-bad.jpg)

After refreshing the keys—which you should do immediately, should you find yourself in the same situation— I was wondering how I could pretend this never happened. This post shows you how to delete files with sensitive data from your commit history.

To resolve this issue, you could use `filter-branch`, if the [warning in the documentation](https://git-scm.com/docs/git-filter-branch) doesn't scare you:

>git filter-branch has a plethora of pitfalls that can produce non-obvious manglings of the intended history rewrite (and can leave you with little time to investigate such problems since it has such abysmal performance). These safety and performance issues cannot be backward compatibly fixed and as such, its use is not recommended. Please use an alternative history filtering tool such as git filter-repo. If you still need to use git filter-branch, please carefully read SAFETY (and PERFORMANCE) to learn about the land mines of filter-branch, and then vigilantly avoid as many of the hazards listed there as reasonably possible.

Eek. Scary. Luckily, there's a simpler and faster alternative: the [open-source BFG Repo Cleaner](https://rtyley.github.io/bfg-repo-cleaner/). I wanted something quick and simple, because this is the only time I'll need to do this. Stop laughing.

## Remove appsettings.json from commit history

As for me, I accidentally pushed an `appsettings.json` file. I don't need that file at all. So, for me, once I [downloaded the BFG JAR file](https://repo1.maven.org/maven2/com/madgag/bfg/1.13.2/bfg-1.13.2.jar), my task was pretty simple.

Here's what to do:

1. Clone a fresh copy of your impacted repository with the `--mirror` flag, like this:

   ```bash
   git clone --mirror https://github.com/{user}/{repo}.git
   ```

   The `--mirror` flag clones a bare repository, which is a full copy of your Git database—but it contains no working files. If you've ever looked into a `.git` folder, it'll look familiar. **Make sure you create a backup of this repository.**

   ![My bare repo]({{ site.url }}{{ site.baseurl }}/assets/img/bare.jpg)

2. Assuming you have the Java runtime installed, run the following command. (You can also set an alias for `java -jar bfg.jar` to just call `bfg`.) This deletes the file from your history (but not the current version, although I don't care).

   ```bash
   java -jar bfg.jar --delete-files appsettings.json
   ```

   With this command, BFG cleans my commits and branches to remove `appsettings.json`.

   Here's the response I get back:

    ```bash
    Using repo : C:\code\TweetOldPosts.git

    Found 8 objects to protect
    Found 2 commit-pointing refs : HEAD, refs/heads/master

    Protected commits
    -----------------

    These are your protected commits, and so their contents will NOT be altered:

    * commit xxxxxxxx (protected by 'HEAD')

    Cleaning
    --------

    Found 17 commits
    Cleaning commits:       100% (17/17)
    Cleaning commits completed in 182 ms.

    Updating 1 Ref
    --------------

            Ref                 Before     After
            ---------------------------------------
            refs/heads/master | xxxxxxxx | xxxxxxxx

    Updating references:    100% (1/1)
    ...Ref update completed in 14 ms.

    Commit Tree-Dirt History
    ------------------------

            Earliest      Latest
            |                  |
            .Dm Dmmmmm mmmmm mmm

            D = dirty commits (file tree fixed)
            m = modified commits (commit message or parents changed)
            . = clean commits (no changes to file tree)

                                    Before     After
            -------------------------------------------
            First modified commit | xxxxxxxx | xxxxxxxx
            Last dirty commit     | xxxxxxxx | xxxxxxxx

    Deleted files
    -------------

            Filename           Git id
            -----------------------------------
            appsettings.json | xxxxxxxx (308 B)


    In total, 20 object ids were changed. Full details are logged here:

            C:\code\TweetOldPosts.git.bfg-report\2021-02-09\21-26-32
   ```

3. While the Git data is updated, nothing has been deleted yet. After you confirm the repo is updated correctly, run the following two commands to do so. **Again, make sure you have a backup.**

    ```bash
    git reflog expire --expire=now --all
    git gc --prune=now --aggressive
    ```

4. Finally, do a `git push` to push your changes up to GitHub. My file is no longer in my commit history. You're ready to do a fresh `git clone` and pretend that it never happened (unless you decided to write about it).

## Resources

- [Removing sensitive data from a repository](https://docs.github.com/en/github/authenticating-to-github/removing-sensitive-data-from-a-repository) (GitHub)
- [BFG Repo-Cleaner Docs](https://rtyley.github.io/bfg-repo-cleaner/)
- [BFG Repo-Cleaner Repo](https://github.com/rtyley/bfg-repo-cleaner)
