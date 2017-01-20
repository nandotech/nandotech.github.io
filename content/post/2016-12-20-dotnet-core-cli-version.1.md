---
title: Choosing your .NET Core Version CLI
subtitle: Use your global.json file to specify framework version
date: 2016-12-20
description: "How to specify your .NET Core Version before scaffolding your app from the CLI"
tags: ["cli",".net core"]
categories:
    - ".NET Core"
    - "CLI"
---

## I recently downloaded and upgraded to `.NET Core 1.1.0` which comes along with updated CLI tools, `1.0.0-preview3-004056`.

This new version reflects the CLI tools that are being used for the new `.csproj` project system which does away with `project.json` that many of us grew to know and love.

Of course, this has been a long time coming and the announcement that Microsoft would be moving back to `.csproj` was announced [way back in March or so.](https://docs.microsoft.com/en-us/dotnet/articles/core/tutorials/target-dotnetcore-with-msbuild)

Anyway--so if you're like me and you've downloaded [.NET Core 1.1](https://dot.net), then this is likely what you see:

![.NET Core CLI 1.0 Preview 3](http://i.imgur.com/Ok6Angi.png)

Based on this, you might think that you're stuck on the newest version.  Luckily for us, each version of `.NET Core` is capable of running side by side and install into different folders for us.

![.NET SDK Folders](http://i.imgur.com/ywygDfI.png)

As I wondered about this myself, I came across [Scott Hanselman's](https://shanselman.com) post about deploying a `.NET Core` [app to Azure where he had to roll back the SDK (seems Azure knew about a version he didn't have).](http://www.hanselman.com/blog/PublishingASPNETCore11ApplicationsToAzureUsingGitDeploy.aspx)  This was the key.

So, following much the same process that Scott details on his blog, we are completely capable of telling the `.NET Core CLI` tools exactly which version of the SDK we would like it to build our project from. In order to do this, all we must do is create a root directory with a `global.json` file where we tell the framework what to look for.

Also, so you can see the difference, I will first simply scaffold a `Web` project and then I will add our `global.json` to demonstrate how `.NET Core` detects `version` and then how we can let the framework know what version of the tooling we need for our project.

Remember _(as shown above)_ that locally we currently have `.NET Core` runtime versions `1.0.0`, `1.0.1`, and now also `1.1.0` (as well as another one that I probably missed).  On top of that, each and every install came with its very own `1.0.0-preview` version of the [.NET Core CLI](https://github.com/dotnet/cli/). 

In case that was unclear for anyone: **don't worry,** because if you do think about it too much, it is actually **INCREDIBLY** confusing and until you just _accept it_ you will live out your remaining days in misery. Jokes aside, this is literally just a nightmare back and forth of version numbers that hardly _appear_ to make any sense ([semantic versioning, anyone?](https://www.semver.org)). Hidden just below the surface in plain sight lies understanding, however. 

Before I go off on that tangent though: our `dotnet --version` shows we are on `1.0.0-preview3-004056`. This is our current version of the [_.NET Command Line Tools (a.k.a. the CLI/SDK)_](https://github.com/dotnet/cli/). This is the new, updated `.csproj` build system, as we can see here:

![.NET Core csproj build](http://i.imgur.com/vZptRhk.png)

Just by taking a look at the folder and file structure here, we can very clearly see we are missing any `project.json` file and have inherited a `new.csproj` that if we inspect, we can see that it contains everything our `project.json` file used to.

That said, I wasn't quite ready to make the jump--maybe it's denial, maybe I'm hoping the `.json` project style will stay around forever--the moral of the story is I needed to tell `.NET Core` that I did not want to use the newest version of the `cli`.

Turns out, this is incredibly easy. Here I am in the same `Powershell` window I was in before. Created a new directory for our new project, and then what we need is a `global.json` to tell the framework which version of the `cli` we want.

_global.json_

```json
{
  "projects": [ "src", "test" ],
  "sdk": {
    "version": "1.0.0-preview2-1-003177"
  }
}
```

Here we are telling `.NET Core` to look for our projects in the directories `src` and `test` and then we specify the `sdk` version of `1.0.0-preview2-1-003177` which shipped with version `1.0.1` of the runtime. Now we simply create the `src` directory and scaffold our project as per usual.

![.NET Core Web Project Preview 2 Tooling](http://i.imgur.com/hi6gvIF.png)

If you look in our window carefully, we've added the `global.json` file shown above into the `root` directory which allowed us to specify the correct `SDK` version for our project.

And that's it! You may now create projects using whichever version of the `dotnet cli` makes your heart content or makes you feel most comfortable. As a final observation, you may also notice the significant difference in the number of folders and files showing from the `Web` project using the exact same command, that is, `dotnet new -t Web`.  

In `preview2` we got a nice fully fleshed out application with stubbed out code for an `SmsService`, `EmailService` as well as all the required boilerplate and code for simple `Accounts`/`Users` in the system. `Preview3` yanks a lot of this extra boilerplate out and leaves us with just a very barebones web app, only containing a `HomeController.cs` under `Controllers`.

Personally, depending on the project, sometimes I want the extra boilerplate code already there for me--other times I may not.  Either way, more often than not, I end up pulling these things in manually or copying them anyway.

Hope this helps someone out who may have been a little confused regarding the versioning and/or encountered any issues during deployment to Azure or anywhere else.

Feel free to leave any feedback and come back for more soon!