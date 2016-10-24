---
title: Building an (awesome) API with NancyFX 2.0 + Dapper
subtitle: Plus, dependency injection explained and everything you need to get up and running.
date: 2016-10-24
bigimg: /img/nancyhome.png
---

## NancyFX & Dapper are so hot right now. Naturally, I had to put them together.

#### I am a huge fan and follower of [.NET Core](https://dot.net) and the entire open source movement insinde Microsoft. 

[ASP.NET Core](http://asp.net) looks extremely promising, but I was always a fan of NancyFX and wanted to try it out. Subsequently, I was extremely excited when I found out that [NancyFX](http://nancyfx.org) would be compatible with Core, and I basically gave myself a reason to use it.

That reason is a rather small Web API project that I very simply needed to accept `POST` requests from two separate web servers/services and provide `GET` routes that would be accessed via a SPA (probably [Angular 2](http://angular.io)) with some relevant business information that would be coming in from the API's.

Moral of the story is that this was incredibly easy to accomplish, especially if you stay on the "[Super Duper Happy Path](https://github.com/NancyFx/Nancy/wiki/Introduction)" as the developers like to put it.  We will only cover the back end service here, possibly saving the Angular 2 portion for another post.

With Core, we have a few options as it pertains to tooling, it really comes down to personal preference.  You may generate and do everything necessary from the [command line](https://github.com/dotnet/cli) and utilize your text editor of choice (in this case, I prefer [VS Code](http://code.visualstudio.com/)).  My personal preference is actually still to use the full version of Visual Studio for the C#/.NET coding, though I do like some of the cli templates better.

Generally, I like to take some parts of ASP.NET web templates to help take care of some boilerplate, but there is some value in doing everything from scratch. Below, we'll explore a few options.

So, why not both?

| **CLI/Code** | **Visual Studio** |
| ------------ | ----------------- |
| ![.NET Core CLI](http://i.imgur.com/PhJP72m.png) | ![VS Web API](http://i.imgur.com/HoGGd07.png)
| ![VS Code](http://i.imgur.com/VVHHhQG.png) | ![Bare WebAPI Proj](http://i.imgur.com/69T7KsR.png)

From here, the direction are basicallly identical regardless which IDE or setup you are attempting to use. I will try to point out differences wherever possible. Also of note, from the `dotnet` cli you may use the command `dotnet new -t web` to get a full ASP.NET application.  Alternatively, you can also use the [Yeoman](http://yeoman.io/) tool that provides several different templates that can be utilized typing `yo aspnet` ([assuming you've installed yeoman & the aspnet generators](https://docs.asp.net/en/latest/client-side/yeoman.html)).

Regardless which route you go, next step here is our config files.  Every Core project has a `project.json` and we will also add an `appsettings.json` file where we will inject some static data from as well as database connection string.

_project.json_

```json
{
  "dependencies": {
    "Microsoft.NETCore.App": {
      "version": "1.0.1",
      "type": "platform"
    },
    "Microsoft.AspNetCore.Diagnostics": "1.0.0",
    "Microsoft.AspNetCore.Server.IISIntegration": "1.0.0",
    "Microsoft.AspNetCore.Server.Kestrel": "1.0.1",
    "Microsoft.Extensions.Logging.Console": "1.0.0",
    "Microsoft.AspNetCore.Owin": "1.0.0",
    "Nancy": "2.0.0-barneyrubble",
    "Dapper": "1.50.2",
    "Microsoft.Extensions.Configuration.FileExtensions": "1.0.0",
    "Microsoft.Extensions.Configuration.Json": "1.0.0"
  },
  "tools": {
    "Microsoft.AspNetCore.Server.IISIntegration.Tools": "1.0.0-preview2-final"
  },
  "frameworks": {
    "netcoreapp1.0": {}
  },
  "buildOptions": {
    "debugType": "portable", 
    "emitEntryPoint": true
  },
  "runtimeOptions": {
    "configProperties": {
      "System.GC.Server": true
    }
  },
  "publishOptions": {
    "include": [
      "wwwroot",
      "web.config"
    ]
  },
  "scripts": {
    "postpublish": [ "dotnet publish-iis --publish-folder %publish:OutputPath% --framework %publish:coreclr%" ]
  }
}
```

_appsettings.json_

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Data Source=(localdb)\\MSSQLLocalDB;Initial Catalog=DemoDb;Integrated Security=True;Connect Timeout=30;Encrypt=False;TrustServerCertificate=True;ApplicationIntent=ReadWrite;MultiSubnetFailover=False"
  },
    "Greeting": "A configurable Hello!"
}
```

Here, we'll use `DefaultConnection` as our DB connection string and will be injecting `Greeting` via Dependency Injection as a simple API string response and in a Nancy view, mainly just to show that we can do that. :)

Depending which app template you used, you may or may not already have a `Startup.cs` file in the root of your project. Either way, we want to edit that file to look like so:

_Startup.cs_

```csharp
    public class Startup
    {
        // This method gets called by the runtime. Use this method to add services to the container.
        public void ConfigureServices(IServiceCollection services)
        {
            // All dependency injection will be done in NancyBootstrapper
        }
        // This method gets called by the runtime.
        // We will configure this to use Nancy middleware.
        public void Configure(IApplicationBuilder app)
        {
            // Creating our request pipeline--strictly Nancy middleware
            app.UseOwin(x => x.UseNancy());
        }
    }
```

At this point, if we define a `HomeModule` which is Nancy's default route, equivalent to the `HomeController` in MVC, then you will have an API that can handle requests already. While I like to split up these different types of code files into folders, it is not in any way required.

Below is a very basic module with a "Hello World" message at the `/` route at `localhost:5000`. You can also see Nancy returning your current OS (or whatever is hosting the app) by visiting `localhost:5000/os`.  Below code also demonstrates how you may write your route definitions in-line (for very short routes) and also within blocks (curly braces) allowing you to write as much code as you need.

_HomeModule.cs_

```csharp
 public class HomeModule : NancyModule
    {
        public HomeModule()
        {
            Get("/", args => "Hello World");

            Get("/os", x =>
            {
                return System.Runtime.InteropServices.RuntimeInformation.OSDescription;
            });
        }
    }
```

We will experiment with injecting dependencies using Nancy's built in `TinyIoC` and also build a few other modules. We'll include very cursory, somewhat contrived examples, but you will get to also see how `async` modules look along with the use of `Before` and `After` request pipelines in Nancy modules. The same applies for Nancy views, though I will try and provide some explanation and/or guidance with links to point you in the right direction.

_...To be completed_
