---
title: .NET Core Console Apps (Twilio API)
subtitle: Our first open source .NET Core utility and deployment gotcha's
date: 2016-11-02
tags: ["twilio","console",".net core","dapper","csharp"]
categories:
    - ".NET Core"
    - "Twilio"
    - "VoIP"
---

### In the spirit of open source (and to pay it something better than lip service) 

NandoTech has published our [first open source utility](https://github.com/nandotechinc/TwilioCallsImporter) that we are using in-house.  The application in and of itself is nothing special and doesn't do anything particuarly complicated, but it solves an important problem for us while having the added benefit of possibly helping someone else with a similar problem.

| [NandoTech](https://www.nandotech.com)        | [Twilio](https://www.twilio.com)                |
| --------------------------------------------- | ----------------------------------------------- |
| ![NandoTech](https://i.imgur.com/P0ccWI9.gif) | ![Twilio Logo](https://i.imgur.com/XApo9Wc.png) |

The utility we've released is a [Twilio Call Importer](https://github.com/nandotechinc/TwilioCallsImporter) which essentially just goes out and grabs all SIP call logs from [Twilio](https://www.twilio.com) and then saves them in batches of 1,000 to [SQL Server](), but there are serveral guides showing how you can configure Dapper for other databases very easily around the web.  There's guides for [MySQL](https://github.com/mysql-net/MySqlConnector), [Postgres](http://techbrij.com/asp-net-core-postgresql-dapper-crud) and [MongoDB](http://www.dotnetcurry.com/aspnet-mvc/1267/using-mongodb-nosql-database-with-aspnet-webapi-core) that you could easily adapt for your needs.

I think what this has really done is actually inspired me.  Currently, there is no [.NET Core](http://dot.net) Twilio API either officially from Twilio nor unofficial.  I think I may take a crack at writing a _relatively_ complete wrapper for the entire Twilio API, preferably using 1. their new API version and also 2. following conventions similar to their previous .NET API or their current new ones.

Before I finish going completely off track: there is a huge "gotcha" in publishing .NET Core Console applications that I was completely unaware of and actually had a bit of trouble googling.  When you `dotnet publish` or publish a core Console app from Visual Studio, you do not get any `.exe` or other executable file from your build.  You do get a `.dll` which contains your program, but counterintuitively `dotnet run` does not work and results in errors.

So as I mentioned, I have a fairly basic console application.  To be clear, ![Proj Screenshot](http://i.imgur.com/Fork9W6.png) 

and that's it.  You can [see the code](https://github.com/nandotechinc/TwilioCallsImporter) really doesn't have a ton of stuff in it.  As of this writing, I didn't even bother adding in any logging that could be helpful and basically comes for free. As for the `project.json`:

_project.json_
```json
{
  "version": "1.0.0-*",
  "buildOptions": {
    "emitEntryPoint": true
  },

  "dependencies": {
    "Microsoft.Extensions.Configuration": "1.0.0",
    "Microsoft.Extensions.Configuration.Abstractions": "1.0.0",
    "Microsoft.Extensions.Configuration.FileExtensions": "1.0.0",
    "Microsoft.Extensions.Configuration.Json": "1.0.0",
    "Microsoft.Extensions.DependencyInjection": "1.0.0",
    "Dapper": "1.50.2",
    "Microsoft.NETCore.App": {
      "type": "platform",
      "version": "1.0.1"
    },
    "Microsoft.Extensions.Configuration.EnvironmentVariables": "1.0.0"
  },

  "frameworks": {
    "netcoreapp1.0": {
      "imports": "dnxcore50"
    }
  }
}
```

When we publish this app, whether you follow Visual Studio prompts or the command line, you will end up with this in your folder. 

![CLI commands](http://i.imgur.com/rID1iFK.png)

![Folder Dir](http://i.imgur.com/9r0Gx55.png)

So...since there's no `.exe`... `dotnet run myproj.dll` right?  Immediately, you'll get an error about missing `project.json` and `appsettings.json`.  I found no documentation on this, however I personally had to manually copy the `.json` files directly to my server where this application ran. So we try again.  

![Huh? .NET Core CLI error](http://i.imgur.com/HxuR8OC.png)

What gives?  After a bit of googling, I came across [this GitHub issue](https://github.com/dotnet/core/issues/77).  Although the issue is closed, the "error" still persists and you have to continue using the workaround described there.

That workaround (after manually copying `appsettings.json` and `project.json` and manually `dotnet restore`), is to use the command `dotnet yourapp.dll`. So in the case of our application, `dotnet TwilioCallsImporter.dll`.

And that is all.  Your console app should be off and running. Just for posterity, see our completed console app run after pulling down our last 7 days of calls from Twilio:

![Twilo Calls Imported by .NET Core Console](http://i.imgur.com/9Ci3D6v.png)

Pretty cool, right?