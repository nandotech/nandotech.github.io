---
title: .NET Core Curated Resources List
subtitle: And Links From Around the Web
date: 2016-11-21
bigimg: /img/dotnet.png
tags: [".net core","resources"]
categories:
    - ".NET Core"
    - "Resources"
---

## Next week I hope to continue our series on the [NancyFX](http://www.nancyfx.org) framework for building web applications.  For our next post I hope to cover _Routing_ as well as passing arguments via query string, body and how that all works within `Nancy`.

As a quick break, this week I've decided to put together a compilation of [.NET Core resources](http://dot.net) that may help anyone at any .NET skill level jump in and find the information they are looking for.  I'll try to cover and include all types of projects, scenarios as well as include a few full-fledged production .NET Core applications that are currently out in the wild [and open-source](http://github.com).

That said, there was **big** news on the `Nancy` front this week, with [the announcement](https://twitter.com/NancyFx/status/798953256273772544) coming out that they've joined the [.NET Foundation](https://www.dotnetfoundation.org/). This is _HUGE_ and important as it will help push more adoption of the NancyFX library as it provides a level of "official"-ness with being supported by the foundation.  Otherwise, the .NET Foundation does help projects out some with organization as well as cloud resources, so hopefully this is great news all the way around.

## This week also brought us `.NET Core 1.1`, [announced on the blog](https://blogs.msdn.microsoft.com/dotnet/2016/11/16/announcing-net-core-1-1/). 

Lots of updates and further support added for the runtime, along with some more announcements of groups like [Google Cloud](https://cloudplatform.googleblog.com/2016/11/Google-Cloud-to-join-NET-Foundation-Technical-Steering-Group.html) joining the .NET Foundation.

## All that said, on to the resources:

#### First and foremost: this list borrowed heavily from the [awesome-dotnet-core](https://github.com/thangchung/awesome-dotnet-core) repository available on github right now.

That curated list is maintained by [Thang Chung](https://github.com/thangchung) and since I have found many tools and links from that list which is constantly updated, I wanted to provide proper credit.

Here, I will only include some services, resources & frameworks I am extremely familiar with only, rather than every package possible.

-------------------------

## General
   - [Microsoft .NET Core Website](https://dot.net)
   - [.NET Core Documentation](https://docs.microsoft.com/en-us/dotnet/articles/welcome)
   - [ASP.NET Core Documentation](https://docs.asp.net/en/latest/)
   - [.NET Standard Repository](https://github.com/dotnet/standard)

## People to follow
  - [Steve Smith](http://ardalis.com/blog)
  - [Scott Hanselman](http://www.hanselman.com/blog/)
  - [Damian Edwards](https://twitter.com/DamianEdwards)
  - [David Fowler](https://twitter.com/DavidFowl)
  - [Rick Strahl](https://weblog.west-wind.com/)
  - [Jonathan Channon](http://blog.jonathanchannon.com/)
  - Along with many, many more I've missed, this is just a cursory list off the top of my head.

## Application Frameworks 
  - [ASP.NET MVC / WebAPI](https://docs.microsoft.com/en-us/aspnet/core/) - ASP.NET Core MVC is a rich framework for building web apps and APIs using the Model-View-Controller design pattern.
  - [NancyFX](https://www.nancyfx.org) - Nancy is a lightweight, low-ceremony, framework for building HTTP based services on .NET Framework/Core and Mono. 
  - [ServiceStack](https://www.servicestack.net) - Thoughtfully architected, obscenely fast, thoroughly enjoyable web services for all https://servicestack.net
  - [Orchard vNext](https://github.com/OrchardCMS/Orchard2) - Orchard 2, a re-implementation of Orchard CMS in ASP.NET Core.
  - [Building an (awesome) API with NancyFX 2.0 + Dapper](http://blog.nandotech.com/post/2016-10-25-nancyfx-webapi-dapper/) - Plus, dependency injection explained and everything you need to get up and running.
  - [Exploring ServiceStack's simple and fast web services on .NET Core](http://www.hanselman.com/blog/ExploringServiceStacksSimpleAndFastWebServicesOnNETCore.aspx)
  - [Exploring a Minimal WebAPI with .NET Core and NancyFX](http://www.hanselman.com/blog/ExploringAMinimalWebAPIWithNETCoreAndNancyFX.aspx)


## Authentication & Authorization
  - [ASP.NET Identity](https://github.com/aspnet/Identity) - ASP.NET Core Identity is the membership system for building ASP.NET Core web applications, including membership, login, and user data. ASP.NET Core Identity allows you to add login features to your application and makes it easy to customize data about the logged in user.
  - [Identity Server 4](https://identityserver4.readthedocs.io/en/release/) - IdentityServer4 is an OpenID Connect and OAuth 2.0 framework for ASP.NET Core.
  - [OpenIdDict](https://github.com/openiddict/openiddict-core) - Easy-to-use OpenID Connect server for ASP.NET Core
  - [OpenIdDict Samples](https://github.com/openiddict/openiddict-samples) - Sample projects using OpenIdDict

## ORM & Database Access Utilities
  - [Dapper Dot Net](https://github.com/StackExchange/dapper-dot-net) - Dapper - a simple object mapper for .Net
  - [Entity Framework](https://docs.microsoft.com/en-us/ef/core/index) - Entity Framework (EF) Core is a lightweight, extensible, and cross-platform version of the popular Entity Framework data access technology.
  - [Simple.Data Core](https://github.com/RendleLabs/Simple.Data.Core) - Simple.Data, the next generation
  - [Using Dapper ORM With ASP.NET Core](http://www.talkingdotnet.com/use-dapper-orm-with-asp-net-core/) - Blog post detailing how to create API with ASP.NET/Dapper
  - [Cross platform database walk-through using ASP.NET MVC and EF Core](http://www.blinkingcaret.com/2016/11/01/cross-platform-database-walk-through-using-asp-net-mvc-and-entity-framework-core/) - This post is a walk-through of how to set up a simple ASP.NET MVC Core project using three available options with Entity Framework Core: Sqlite, MySql and Postgres.
  - [.NET Core Data Access](https://blogs.msdn.microsoft.com/dotnet/2016/11/09/net-core-data-access/) - Blog post from .NET team, very thoroughly covers all types of data access.

## Logging
 - [Built-in Logging Provider](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging) - ASP.NET Core has built-in support for logging, and allows developers to easily leverage their preferred logging framework's functionality as well. Implementing logging in your application requires a minimal amount of setup code. Once this is in place, logging can be added wherever it is desired.
 - [Serilog](https://serilog.net/) - like many other libraries for .NET, Serilog provides diagnostic logging to files, the console, and elsewhere. It is easy to set up, has a clean API, and is portable between recent .NET platforms.
 - [NLog](http://nlog-project.org/) - NLog is a free logging platform for .NET (works with Core)
 - [Sample Code using Built-In Provider](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/sample)
 - [Step by Step: Serilog with ASP.NET Core](https://carlos.mendible.com/2016/09/19/step-step-serilog-asp-net-core/)


## Testing
  - As far as I know, at this point Microsoft officially adopted [xUnit](https://xunit.github.io/). I do know they are still working on MSTest and there may be others, but I'm only really familiar with xUnit.
  - [xUnit](https://xunit.github.io/) - xUnit.net is a free, open source, community-focused unit testing tool for the .NET Framework
  - [Getting Started with xUnit and ASP.NET Core](https://xunit.github.io/docs/getting-started-dotnet-core.html)
  - [Unit Testing with Dotnet test](https://docs.microsoft.com/en-us/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
  
## Dependency Injection
  - Here is another case, where MS has provided a built in [Inversion of Control](https://msdn.microsoft.com/en-us/library/ff921087.aspx) container, but you are free to use what you prefer.
  - [Built-In Provider](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/dependency-injection) - ASP.NET Core is designed from the ground up to support and leverage dependency injection. ASP.NET Core applications can leverage built-in framework services by having them injected into methods in the Startup class, and application services can be configured for injection as well. The default services container provided by ASP.NET Core provides a minimal feature set and is not intended to replace other containers.
  - [Autofac](https://github.com/autofac/Autofac)
  - [DryIoc](https://bitbucket.org/dadhi/dryioc) - DryIoc is fast, small, full-featured IoC Container for .NET
  - [StructureMap](https://github.com/structuremap/StructureMap.Microsoft.DependencyInjection) - The package contains a single, public extension method, Populate. It's used to populate a StructureMap container using a set of ServiceDescriptors or an `IServiceCollection`.
  - [TinyIoC](https://github.com/grumpydev/TinyIoC) - IoC container built-in to [NancyFX](https://www.nancyfx.org), not sure about standard .NET Core support.
  - [Sample Project using Built-In DI Provider](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/sample)
  - [Getting Started with Autofac and MVC6 + ASP.NET Core](http://benjii.me/2016/01/getting-started-autofac-and-asp-net-core/) - May have slightly outdated bits
  - [Building an (awesome) API with NancyFX 2.0 + Dapper](http://blog.nandotech.com/post/2016-10-25-nancyfx-webapi-dapper/) - Plus, dependency injection explained and everything you need to get up and running.

## Miscellaneous Links
 - [.NET Blog](https://blogs.msdn.microsoft.com/dotnet/) - Your authoritative place for all .NET news and information
 - [.NET Foundation](https://dotnetfoundation.org)
 - [Awesome-Dot-Net](https://github.com/thangchung/awesome-dotnet-core) - Comprehensive listing of .NET Core libraries
 - [Shawn Wildermuth's Blog](http://wildermuth.com/)
 - [Julie Lerman's Blog](http://thedatafarm.com/blog/)

## Sample .NET Core Projects (Production Web Apps)
 - [HTBox AllReady](https://github.com/HTBox/allReady)
 - [WilderBlog](https://github.com/shawnwildermuth/wilderblog/)
 - [Core Code Camp](https://github.com/shawnwildermuth/CoreCodeCamp)
 - [Practical ASP.NET Core](https://github.com/dodyg/practical-aspnetcore)

## That's all I got for now folks.

Tried to provide a relatively short list of very useful utilities that I have personally used and/or come across in my short time dealing with ASP.NET Core, Nancy and other related frameworks. Also, given the fact that the [more comprehensive listing](https://github.com/thangchung/awesome-dotnet-core) of resources isn't going anywhere--this works well as a nice, curated list of libraries to use.

Eventually, there may be a part 2 and/or an update to this post where we add a ton of categories & links that we've come across since this time. There are several categories I left off just for the sake of brevity and also not wanting to provide descriptions for another 100 libraries.

Nonetheless, I hope this serves as a helpful resource!  Please comment below if you wish or reach out to us directly if you have any questions or concerns.

Happy Coding!